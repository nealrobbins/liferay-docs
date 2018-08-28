# Validating the Shopping Cart

In each product's *Configuration* page, there are a number of settings which
administrators can adjust to govern the conditions under which the product can
be purchased---minimum order quantities can be set, out-of-stock items may or
may not be back-ordered, etc. Whenever a customer attempts to make a purchase,
@commerce@ checks to make sure the transaction is permissible. This tutorial
describes how to make custom changes to the validation check.

@commerce@ makes an initial check when an item is added to the cart,
but the cart is validated a second time when the customer checks out. Out of the
box, this ensures that conditions have not changed (for example, a product my
have run out of stock) between the first validation and the second. A custom
solution, however, may make additional use of this two-step validation (for
instance, to ensure that product A can only be purchased alongside product B).

Cart validation is handled by `DefaultCommerceOrderValidatorImpl`, which
implements `CommerceOrderValidator`. To customize your own validator, modify
`DefaultCommerceOrderValidator` or create your own class implementing the same
interface.

Follow these steps:

1.  Add a dependency on `com.liferay.commerce.api` to the `build.gradle` file.

2.  Implement the `CommerceOrderValidator` interface.

3.  Include your custom rules in the interface's methods.

First, add the dependency to `build.gradle`. It should look like this:

    dependencies {
    … 
    compileOnly group: "com.liferay.commerce", name: "com.liferay.commerce.api", version: "1.0.0" 
    … 
    }

Then implement the interface. Out of the box, the implementation looks like
this:

    @Component(
        immediate = true,
        property = {
            "commerce.order.validator.key=" + DefaultCommerceOrderValidatorImpl.KEY,
            "commerce.order.validator.priority:Integer=10"
        },
        service = CommerceOrderValidator.class
    )
    public class DefaultCommerceOrderValidatorImpl
        implements CommerceOrderValidator {

        public static final String KEY = "default";

        @Override
        public String getKey() {
            return KEY;
        }

The interface requires two more methods after the `getKey` method. These methods
contain the actual logic of the validating check---this is where you'll want to
insert your own code. The following method, when called, validates products that
are already in the cart. This example checks that backorders are allowed and
that the product quantities in the order are permissible. It then calls on
`CommerceOrderValidatorResult` to display the outcome to the user:

	@Override
	public CommerceOrderValidatorResult validate(
			CommerceOrderItem commerceOrderItem)
		throws PortalException {

		CPInstance cpInstance = commerceOrderItem.getCPInstance();

		CPDefinitionInventory cpDefinitionInventory =
			_cpDefinitionInventoryLocalService.
				fetchCPDefinitionInventoryByCPDefinitionId(
					cpInstance.getCPDefinitionId());

		CPDefinitionInventoryEngine cpDefinitionInventoryEngine =
			_cpDefinitionInventoryEngineRegistry.getCPDefinitionInventoryEngine(
				cpDefinitionInventory);

		if (cpDefinitionInventoryEngine.isBackOrderAllowed(cpInstance)) {
			return new CommerceOrderValidatorResult(true);
		}

		int minOrderQuantity = cpDefinitionInventoryEngine.getMinOrderQuantity(
			cpInstance);
		int maxOrderQuantity = cpDefinitionInventoryEngine.getMaxOrderQuantity(
			cpInstance);
		String[] allowedOrderQuantities =
			cpDefinitionInventoryEngine.getAllowedOrderQuantities(cpInstance);

		if ((minOrderQuantity > 0) &&
			(commerceOrderItem.getQuantity() < minOrderQuantity)) {

			return new CommerceOrderValidatorResult(
				commerceOrderItem.getCommerceOrderItemId(), false,
				"minimum-quantity-is-x", String.valueOf(minOrderQuantity));
		}

		if ((maxOrderQuantity > 0) &&
			(commerceOrderItem.getQuantity() > maxOrderQuantity)) {

			return new CommerceOrderValidatorResult(
				commerceOrderItem.getCommerceOrderItemId(), false,
				"maximum-quantity-is-x", String.valueOf(maxOrderQuantity));
		}

		if ((allowedOrderQuantities.length > 0) &&
			!ArrayUtil.contains(
				allowedOrderQuantities,
				String.valueOf(commerceOrderItem.getQuantity()))) {

			return new CommerceOrderValidatorResult(
				commerceOrderItem.getCommerceOrderItemId(), false,
				"quantity-is-not-allowed");
		}

        return new CommerceOrderValidatorResult(true); }

The interface's second `validate` method makes the same checks as the first. In
this case, however, the method checks products as they are added to the cart:


	@Override
	public CommerceOrderValidatorResult validate(
			CPInstance cpInstance, int quantity)
		throws PortalException {

		if (cpInstance == null) {
			return new CommerceOrderValidatorResult(false);
		}

		CPDefinitionInventory cpDefinitionInventory =
			_cpDefinitionInventoryLocalService.
				fetchCPDefinitionInventoryByCPDefinitionId(
					cpInstance.getCPDefinitionId());

		CPDefinitionInventoryEngine cpDefinitionInventoryEngine =
			_cpDefinitionInventoryEngineRegistry.getCPDefinitionInventoryEngine(
				cpDefinitionInventory);

		int minOrderQuantity = cpDefinitionInventoryEngine.getMinOrderQuantity(
			cpInstance);
		int maxOrderQuantity = cpDefinitionInventoryEngine.getMaxOrderQuantity(
			cpInstance);
		String[] allowedOrderQuantities =
			cpDefinitionInventoryEngine.getAllowedOrderQuantities(cpInstance);

		if (cpDefinitionInventoryEngine.isBackOrderAllowed(cpInstance) &&
			(quantity >= minOrderQuantity) && (quantity <= maxOrderQuantity)) {

			return new CommerceOrderValidatorResult(true);
		}

		if ((minOrderQuantity > 0) && (quantity < minOrderQuantity)) {
			return new CommerceOrderValidatorResult(
				false, "minimum-quantity-is-x",
				String.valueOf(minOrderQuantity));
		}

		if ((maxOrderQuantity > 0) && (quantity > maxOrderQuantity)) {
			return new CommerceOrderValidatorResult(
				false, "maximum-quantity-is-x",
				String.valueOf(maxOrderQuantity));
		}

		if ((allowedOrderQuantities.length > 0) &&
			!ArrayUtil.contains(
				allowedOrderQuantities, String.valueOf(quantity))) {

			return new CommerceOrderValidatorResult(
				false, "quantity-is-not-allowed");
		}

		return new CommerceOrderValidatorResult(true);
	}
