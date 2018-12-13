# Customizing New Catalog Rule Types

Catalog rules manage the catalog's visibility, determining exactly what products
each user segment is able to see. Out of the box, there are two types of catalog
rules:

**All Products:** A rule of this type enables users to see every product in the
catalog.

**Categories:** This enables users to see products only in categories specified
by an administrator.

Since the *Categories* rule type may not meet every use case, @commerce@ exposes
an extension point to allow custom types to be developed, grouping products
using whatever criteria is desired. This involves implementing the `CPRuleType`
interface.

Follow these steps:

1.  Create a new module and include a dependency on
    `com.liferay.commerce.product.api` in its `build.gradle`.

2.  Create a new component and implement the
    `com.liferay.commerce.product.catalog.rule.CPRuleType` interface.

3.  Extend the administrative screen for your rule type to the Product Catalog Rule
    portlet. The portlet uses the
    [Screen Navigation Framework](/develop/tutorials/-/knowledge_base/7_1/screen-navigation-framework),
    so you can extend it with a another component that contributes an appropriate JSP.

First, create a new module. It's `build.gradle` should look like this:

    sourceCompatibility = "1.8"
    targetCompatibility = "1.8"

    dependencies {
        compileOnly group: "com.liferay.commerce", name: "com.liferay.commerce.product.api", version: "2.0.0"
        compileOnly group: "com.liferay.portal", name: "com.liferay.portal.kernel", version: "3.5.0"
        compileOnly group: "org.osgi", name: "org.osgi.service.component.annotations", version: "1.3.0"
        compileOnly group: "javax.servlet", name: "javax.servlet-api", version: "3.0.1"
        compileOnly group: "javax.portlet", name: "portlet-api", version: "3.0.0"
    }

Next, create a component and implement the interface. The example that follows
enables returns only products whose product name matches text entered by an
administrator while creating the rule.


    import java.util.Locale;
    import java.util.Map;
    import java.util.ResourceBundle;

    import javax.servlet.http.HttpServletRequest;

    import org.osgi.service.component.annotations.Component;

    import com.liferay.commerce.product.catalog.rule.CPRuleType;
    import com.liferay.commerce.product.model.CPDefinition;
    import com.liferay.commerce.product.model.CPRule;
    import com.liferay.portal.kernel.exception.PortalException;
    import com.liferay.portal.kernel.language.LanguageUtil;
    import com.liferay.portal.kernel.search.Document;
    import com.liferay.portal.kernel.search.Field;
    import com.liferay.portal.kernel.search.filter.BooleanFilter;
    import com.liferay.portal.kernel.util.ParamUtil;
    import com.liferay.portal.kernel.util.ResourceBundleUtil;
    import com.liferay.portal.kernel.util.UnicodeProperties;

    @Component(
        immediate = true,
        property = {
            "commerce.product.rule.type.key=" + NameMatcherCPRuleTypeImpl.KEY,
            "commerce.product.rule.type.order:Integer=300"
        },
        service = CPRuleType.class
    )
    public class NameMatcherCPRuleTypeImpl implements CPRuleType {

        public static final String KEY = "name-matching";

Note that the `commerce.product.rule.type.key` property matches the class name.
The `commerce.product.rule.type.order:Integer` property locates your rule type
on the admin screen alongside other types.

Next, include the methods required by the `CPRuleType` interface. 

    @Override
    public void contributeDocument(Document document, CPDefinition cpDefinition)
        throws PortalException {
    }

The `contributeDocument` method provides the information required by the search
indexer document in order for the appropriate products to be returned. While the
method is required by the interface, it this case it takes no action as the
`cpDefinition` name is already indexed.

Add the `getKey` method, which identifies your rule type using the `KEY`
constant when called:

    @Override
    public String getKey() {
        return KEY;
    }

The `getLabel` method retrieves the language value associated with the `KEY`
constant from the module's appropriate `Language.properties` file, which must be
placed in your module's `src/main/resources/content` folder. The language value
provides the type's label on the frontend.

    @Override
    public String getLabel(Locale locale) {
        ResourceBundle resourceBundle = ResourceBundleUtil.getBundle(
            "content.Language", locale, getClass());

        return LanguageUtil.get(resourceBundle, KEY);
    }

Include the `getTypeSettingsProperties` method. This method is called when an
administrator saves a new catalog rule and allows you to use the `typesettings`
field save relevant information for which there is no dedicated database field.
In this case, the `nameSubstring` string is taken from the request---the
string's value is entered by the administrator on the frontend---then placed in
the `typesettings` database field and returned wrapped in the
`typeSettingsProperties` key-value store.

    @Override
    public UnicodeProperties getTypeSettingsProperties(
        HttpServletRequest httpServletRequest) {

        UnicodeProperties typeSettingsProperties = new UnicodeProperties(true);

        String nameSubstring = ParamUtil.getString(
            httpServletRequest, "nameSubstring");

        typeSettingsProperties.put("nameSubstring", nameSubstring);

        return typeSettingsProperties;
    }

The `isSatisfied` method determines whether a given product (`cpDefinition`)
meets the rule's requirement. In this case, the method gets the product's name
from the `cpDefinition` and checks to see whether the `nameSubstring` string is
contained in it.

    @Override
    public boolean isSatisfied(CPDefinition cpDefinition, CPRule cpRule)
        throws PortalException {

        UnicodeProperties typeSettingsProperties =
            cpRule.getTypeSettingsProperties();

        Map<Locale, String> nameMap = cpDefinition.getNameMap();

        for (String name : nameMap.values()) {
            if (name.contains(typeSettingsProperties.get("nameSubstring"))) {
                return true;
            }
        }

        return false;
    }

Include the `postProcessContextBooleanFilter` method. This method contains the
same logic as `isSatisfied`, but instead of checking a single product is passed
in, it modifies the indexer query `booleanFilter` to include `nameSubstring` in
the query, so that only matching products will be returned.

    @Override
    public void postProcessContextBooleanFilter(
            BooleanFilter booleanFilter, CPRule cpRule)
        throws PortalException {

        UnicodeProperties typeSettingsProperties =
            cpRule.getTypeSettingsProperties();

        booleanFilter.addRequiredTerm(
            Field.NAME, typeSettingsProperties.get("nameSubstring"));
    }

Include the `update` method. This method should provide any other business logic
that needs to be executed when a rule of this type is saved:

     @Override
        public void update(CPRule cpRule, HttpServletRequest httpServletRequest) {
        }

    }

In this example, `update` is only included to satisfy the demands of the
interface---all necessary configuration has already been saved in
`cpRule.typeSettingsProperties`.

## Extending the Admin Portlet

Implementing the `CPRuleType` interface handles the back-end logic of your rule
type, but you still need to provide an interface for your administrators. The
Catalog Rule portlet can be extended using the Screen Navigation Framework, and
@commerce@ gives you a head start with the `CPRuleTypeJSPContributor` interface.

In this example, the Admin screen extension will add a text field for
administrators to provide a value for `nameSubString`. This will require some
additional dependencies in the module's `build.gradle`:

	compileOnly group: "com.liferay", name: "com.liferay.frontend.taglib", version: "3.0.0"
	compileOnly group: "com.liferay", name: "com.liferay.frontend.taglib.soy", version: "2.0.0"
	compileOnly group: "com.liferay.portal", name: "com.liferay.util.taglib", version: "3.0.0"

You'll also need `bnd.bnd` file with a `Web-ContextPath` in order to provide the
JSP contributor with a `ServletContext`. The `bnd.bnd` should look something
like this:

    Bundle-Name: commerce-catalog-rule
    Bundle-SymbolicName: com.liferay.catalog.rule.type
    Bundle-Version: 1.0.0
    Web-ContextPath: /commerce-catalog-rule

To implement this interface, create a new component in the same package as your
`CPRuleType` implementation:

    import com.liferay.commerce.product.catalog.rule.CPRuleTypeJSPContributor;
    import com.liferay.frontend.taglib.servlet.taglib.util.JSPRenderer;

    import javax.servlet.ServletContext;
    import javax.servlet.http.HttpServletRequest;
    import javax.servlet.http.HttpServletResponse;

    import org.osgi.service.component.annotations.Component;
    import org.osgi.service.component.annotations.Reference;

    @Component(
        immediate = true,
        property = "commerce.product.rule.type.jsp.contributor.key=" + NameMatcherCPRuleTypeImpl.KEY,
        service = CPRuleTypeJSPContributor.class
    )
    public class NameMatcherCPRuleTypeJSPContributor
        implements CPRuleTypeJSPContributor {

The interface, requires only one method, `render`:

    @Override
    public void render(
            long cpRuleId, HttpServletRequest httpServletRequest,
            HttpServletResponse httpServletResponse)
        throws Exception {

        _jspRenderer.renderJSP(
            _servletContext, httpServletRequest, httpServletResponse,
            "/contributor/name_matcher.jsp");
    }

This method renders the JSP in your module's `META-INF/resources/contributor`
directory.

Finally, include these @Reference annotations:

        @Reference
        private JSPRenderer _jspRenderer;

        @Reference(
            target = "(osgi.web.symbolicname=com.liferay.catalog.rule.type)"
        )
        private ServletContext _servletContext;

    }

Note that the `symbolicname` in the second reference should match the
`Bundle-SymbolicName` in your module's `bnd.bnd` file.

## Providing the JSPs

You'll need to include at least one JSP in your module. The JSP contributor in
the previous section called for `name_matcher.jsp`:

    <%@ include file="/init.jsp" %>

    <%
    Object cpCatalogRuleDisplayContext = request.getAttribute(WebKeys.PORTLET_DISPLAY_CONTEXT);

    Class<?> displayContextClass = cpCatalogRuleDisplayContext.getClass();

    Method getCPRuleTypeSettingsPropertyMethod = displayContextClass.getMethod("getCPRuleTypeSettingsProperty", String.class);

    String nameSubstring = (String)getCPRuleTypeSettingsPropertyMethod.invoke(cpCatalogRuleDisplayContext, "nameSubstring");
    %>

    <aui:input label="name" name="nameSubstring" type="text" value="<%= nameSubstring %>" />

This JSP yadayadayada... it calls for `init.jsp` to be located one directory up, providing imports:

(include init.jsp). Don't conclude with code.
