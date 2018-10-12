# Creating New Shipping Methods

@commerce@ includes three shipping methods out of the box: *fixed*- and
*variable*-rate methods give you a wide range of options for calculating shipping
costs in-house, while the FedEx method provides options for you to integrate
your system with the multinational courier of the same name. Since you may want
a similar solution to provide integration with other shipping companies,
@commerce@ exposes an extension point to allow new shipping methods to be
developed.

Creating a new shipping method requires you to implement the
`CommerceShippingEngine` interface. This creates a new method that
administrators can select from *Site Menu* &rarr; *Commerce* &rarr; *Settings*
&rarr; *Shipping Methods*.

Screenshot? Delivery by UAV?

This tutorial covers the creation of new shipping methods. Business logic to
integrate with any particular shipping company (UPS, DHL, etc.) is the
responsibility of the developer.

Follow these steps:

1.  Create a new module and include dependencies on
    `com.liferay.commerce.product.api` and `com.liferay.commerce.api` in its `build.gradle`
    file.

2.  Create a component implementing the
    `com.liferay.commerce.model.CommerceShippingEngine` interface.

First, add dependencies to `build.gradle`. The build script should look like
this (DXP GA1, Commerce 1.0.0):

    sourceCompatibility = "1.8"
    targetCompatibility = "1.8"

    dependencies {
        compileOnly group: "com.liferay.commerce", name: "com.liferay.commerce.product.api", version: "1.1.0"
        compileOnly group: "com.liferay.portal", name: "com.liferay.portal.kernel", version: "3.5.0"
        compileOnly group: "org.osgi", name: "org.osgi.service.component.annotations", version: "1.3.0"
        compileOnly group: "com.liferay.commerce", name: "com.liferay.commerce.api", version: "1.1.0"
        compileOnly group: "javax.servlet", name: "javax.servlet-api", version: "3.0.1"
    }

Next, create your component class:

    @Component(
        immediate = true,
        property = "commerce.shipping.engine.key=" + SampleCommerceShippingEngine.KEY,
        service = CommerceShippingEngine.class
    )
    public class SampleCommerceShippingEngine implements CommerceShippingEngine {

        public static final String KEY = "sample";

The KEY constant in this example provides a unique identifier for this shipping method.

The `getCommerceShippingOptionLabel` method returns a localized value for the
shipping method's name. In this particular example, the method is never used as
the label is hard-coded in the subsequent method. In a real-world application,
however, a call on `getCommerceShippingOptionLabel` is likely to be used to
provide a name for the shipping method appropriate to a user's language
settings.

    @Override
    public String getCommerceShippingOptionLabel(String name, Locale locale) {
        ResourceBundle resourceBundle = _getResourceBundle(locale);

        return ResourceBundleUtil.getString(resourceBundle, name);
    }

The main business logic of the shipping method is handled by the
`getCommerceShippingOptions` method:


    @Override
        public List<CommerceShippingOption> getCommerceShippingOptions(
                CommerceContext commerceContext, CommerceOrder commerceOrder,
                Locale locale)
            throws CommerceShippingEngineException {

            List<CommerceShippingOption> commerceShippingOptions =
                new ArrayList<>();


            commerceShippingOptions.add(new CommerceShippingOption(“Sample Option”, “Sample Option”, BigDecimal.TEN));
            
                   return commerceShippingOptions;
        }

This method must recieve all the information necessary to calcuate the shipping
cost in order to return a `CommerceShippingOptions` object. In this case, the
object returned has parameters specifying a label and a name (displayed in the
UI and saved to the database, respectively), both of "Sample Option" as well as
a fixed shipping cost of 10 (in your store's primary currency).

The component's final methods provide addtional labelling information:

    @Override
        public String getDescription(Locale locale) {
            ResourceBundle resourceBundle = _getResourceBundle(locale);

            return LanguageUtil.get(resourceBundle, "sample-shipping-description");
        }

        @Override
        public String getName(Locale locale) {
            ResourceBundle resourceBundle = _getResourceBundle(locale);

            return LanguageUtil.get(resourceBundle, "sample-rate");
        }


        private ResourceBundle _getResourceBundle(Locale locale) {
            return ResourceBundleUtil.getBundle(
                "content.Language", locale, getClass());
        }


    }

The `getDescription` method populates the *Description* field of the shipping
method's  admin screen, while `getName` fills the *name* field on the same
screen. The `_getResourceBundle` method provides the appropriate language
bundle to the previous two methods.
