# Customizing new catalog rule types

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

3.  Add an administrative screen for your rule type to the Product Catalog Rule
    portlet. The portlet uses the Screen Navigation Framework (link), so you can
    extend it by contributing an appropriate JSP.<!--this part is a bit fuzzy.
    Check Steve's docs and rework-->

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

Next, create a component and implement the interface:

    import com.liferay.commerce.product.catalog.rule.CPRuleType;
    import com.liferay.commerce.product.model.CPDefinition;
    import com.liferay.commerce.product.model.CPRule;
    import com.liferay.portal.kernel.exception.PortalException;
    import com.liferay.portal.kernel.language.LanguageUtil;
    import com.liferay.portal.kernel.search.Document;
    import com.liferay.portal.kernel.search.filter.BooleanFilter;
    import com.liferay.portal.kernel.util.ParamUtil;
    import com.liferay.portal.kernel.util.ResourceBundleUtil;
    import com.liferay.portal.kernel.util.UnicodeProperties;

    import java.util.Locale;
    import java.util.Map;
    import java.util.ResourceBundle;

    import javax.servlet.http.HttpServletRequest;

    import org.osgi.service.component.annotations.Component;

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

Note that the `order` property




The `contributeDocument` method provides the information required by the search
indexer document in order for the appropriate products to be returned. While the
method is required by the interface, it this case it takes no action as the
`cpDefinition` name is already indexed.

Include the `getKey` method, which identifies your rule type using the `KEY`
constant when called.

The `getLabel` method retrieves the language value associated with the `KEY`
constant from the module's appropriate `Language.properties` file, which must be
placed in your module's `src/main/resources/content` folder.



