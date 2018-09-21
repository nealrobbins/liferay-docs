# Adding Regions to @commerce@'s Country List

@commerce@ stores a country list that is used wherever a user is prompted to
select a country (typically to enter an address). @commerce@ also stores lists of
regions (states or provinces) for each country, but out of the box, only the
region lists for Italy and the USA are populated. Administrators can populate
these lists manually, (see
[Countries](web/emporio/documentation/-/knowledge_base/1-0/countries))
but a developer can populate them automatically by implementing the
`CommerceRegionsStarter` interface.

Follow these steps:

1.  Create a new module and add dependencies on `commerce-api` and
   `commerce-product-api` to its `build.gradle` file.

2.  Implement the `com.liferay.commerce.starter.CommerceRegionsStarter` interface.

3.  Implement your logic in the interface's `start` method.

First, add dependencies to `build.gradle`. It should look like this:

    sourceCompatibility = "1.8"
    targetCompatibility = "1.8"

    dependencies {
        provided group: "com.liferay.portal", name: "com.liferay.portal.kernel", version: "3.5.0"
        provided group: "org.osgi", name: "org.osgi.service.component.annotations", version: "1.3.0"
        provided project(":private:apps:commerce:commerce-api")
    }

<!--Note that the above code contains a dependency on commerce-api, but not on
commerce-product-api--> Then create a component to implement the interface. In
the example below, the class `ItalyCommerceRegionsStarter` contains the the
constant `ITALY_NUMERIC_ISO_CODE`. This constant is used to 
