# Creating New Product Types

@commerce@ supports three different [product types]() out of the box, but
additional types can be created. This tutorial covers creating a new product
type and generating admin screens for managing products of that type.

You need three things to create a new product type:

-  An implementation of the `CPType` interface to create the type.

-  An admin screen (or screens) created using @product@'s [Screen Navigation
Framework](/develop/tutorials/-/knowledge_base/7-1/screen-navigation-framework).

-  A file---typically a JSP---containing the UI for the admin screen.

The process to create a new product type is relatively straightforward. Follow
these steps:

1.  Create a new module add dependencies on --)------- to its `build.gradle` file.

    The build script should look like this:

2.  Implement the `com.liferay.commerce.product.type.CPType` interface.

3.  Create your product type's admin screen by implementing the
    `ScreenNavigationCategory` and `ScreenNavigationEntry` interfaces, and
    creating corresponding JSP files.

First, create a module and add dependencies to its `build.gradle`. The build
script should look like this:

--)----------

Then create a new Java class implementing the `CPType` interface. Insert the
following Component annotation before the class declaration:

    @Component(
        immediate = true,
        property = {
            "commerce.product.type.display.order:Integer=5",
            "commerce.product.type.name=sample"
        },
        service = CPType.class
    )

This positions your new type, "sample" 5th in the dropdown menu when an
administrator adds a new product to the catalog:

[Add screenshot?]

Then implement the interface. 

    public class SampleCPType implements CPType {

        @Override
        public void deleteCPDefinition(long cpDefinitionId) throws PortalException {
        }

        @Override
        public String getLabel(Locale locale) {
            return LanguageUtil.get(locale, “sample”);
        }

        @Override
        public String getName() {
            return "sample";
        }

    }
