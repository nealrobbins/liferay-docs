# Customizing Cart Validation[](id=customizing-cart-validation)

Whenever a buyer adds a product to his cart or attempts to checkout, @commerce@
checks to make sure that the transaction is permissible. This tutorial
describes how to inject custom behavior into the validation check.

In each product's
[*Configuration*](/web/emporio/documentation/-/knowledge_base/1-0/configuration)
page, there are several settings which administrators can adjust to govern the
conditions under which the product can be purchased---minimum order quantities
can be set, out-of-stock items may or may not be backordered, etc. Out of the
box, @commerce@ performs a validation check on each transaction to ensure these
conditions are met. An initial check is made when an item is added to the cart,
and again as the buyer checks out, revalidating the cart at each step in the
checkout process. This ensures that conditions have not changed (for example, a
product my have run out of stock) between the first validation and order
placement.

Cart validation is handled by `DefaultCommerceOrderValidatorImpl`, which
implements `CommerceOrderValidator`. To impose new conditions on purchases, you
can customize the validator by creating a new component implementing the same
interface, adding its functionality on top of
`DefaultCommerceOrderValidatorImpl`.

Follow these steps:

1.  Create a new module an add a dependencies on `com.liferay.commerce.api` and
    `com.liferay.commerce.product.api` to the `build.gradle` file.

2.  Create a new component to implement the `CommerceOrderValidator` interface.

3.  Include your custom rules in the interface's methods.

First, add dependencies to `build.gradle`. The build script should look like
this:


    sourceCompatibility = "1.8"
    targetCompatibility = "1.8"

    dependencies {
        compileOnly group: "com.liferay.commerce", name: "com.liferay.commerce.api", version: "2.0.0"
        compileOnly group: "com.liferay.commerce", name: "com.liferay.commerce.product.api", version: "2.0.0"
        compileOnly group: "com.liferay.portal", name: "com.liferay.portal.kernel", version: "3.5.0"
        compileOnly group: "org.osgi", name: "org.osgi.service.component.annotations", version: "1.3.0"
    }

Then implement the interface:

    @Component(
        immediate = true,
        property = {
            "commerce.order.validator.key=" + SampleCommerceOrderValidatorImpl.KEY,
            "commerce.order.validator.priority:Integer=10"
        },
        service = CommerceOrderValidator.class
    )
    public class SampleCommerceOrderValidatorImpl
        implements CommerceOrderValidator {

        public static final String KEY = "sample-validator";

        @Override
        public String getKey() {
            return KEY;
        }

The interface requires two more methods after the `getKey` method. These
methods contain the actual logic of the validating check---this is where you'll
want to insert your own code. The following method is called whenever a buyer
procedes to a new step in the checkout process, and validates products that are
already in the cart. It then returns the `CommerceOrderValidatorResult` object
to display the outcome to the user:

    @Override
    public CommerceOrderValidatorResult validate(
            CommerceOrderItem commerceOrderItem)
        throws PortalException {
        
        if (commerceOrderItem.getQuantity() != 100) {

            return new CommerceOrderValidatorResult(
                commerceOrderItem.getCommerceOrderItemId(), false,
                "quantity-is-not-valid");
        }

        return new CommerceOrderValidatorResult(true);
    }

The interface's second `validate` method makes the same check as the first. In
this case, however, the method checks products as they are added to the cart:

    @Override
        public CommerceOrderValidatorResult validate(
                CPInstance cpInstance, int quantity)
            throws PortalException {

            if (cpInstance == null) {
                return new CommerceOrderValidatorResult(false);
            }

    if (quantity != 100) {

            return new CommerceOrderValidatorResult(false, "quantity-is-not-valid");
        }
            return new CommerceOrderValidatorResult(true);
        }
    }

In this is example, the validator checks whether an an order's quanitity is 100
and rejects any other quanitity. For a real-world example see
`DefaultCommerceOrderValidatorImpl.java`.
