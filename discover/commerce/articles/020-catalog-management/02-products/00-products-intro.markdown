 # Pricing [](id=pricing)

When all you want to do is assign one price to one SKU, you can handle pricing
in the catalog (see
[SKUs](web/commerce/documentation/-/knowledge_base/1-0/skus#pricing)). 
If you want to do anything more sophisticated, you need to understand
@commerce@'s pricing hierarchy. Specifically, you need to understand the
difference between a *base price*, a *price list*, a *tiered price*, a *promo
price*, and a *discount*.

A **Base Price** is set in a product's SKUs tab and applies to all purchases of
that product, unless it is superseded by another price.

A **Price List** applies prices to specified products for a specified user
segment. It supersedes the base price.

A **Tiered Price** is a price that is applied to orders that meet specified
quantity requirements. It is only available in the context of a price list, and
supersedes the list's price.

A **Promo Price** can be applied to a base price, a price list price, and to
a tiered price, superseding each. When a price is superseded by a price list or
tiered price, its promo is superseded as well.

![Figure 1: The appropriate price will be applied depending on what user segments a buyer belongs to, and on the quantity he purchases.](../../images/price-hierarchy.png)

Once the appropriate price is applied, a discount can be applied. A **Discount**
does not supersede a product's price but instead applies a modifier to it.

The discount is the final component that goes into calculating an order's
subtotal. Then 
[taxes](web/commerce/documentation/-/knowledge_base/1-0/taxes)
are applied and then 
[shipping costs](web/commerce/documentation/-/knowledge_base/1-0/shipping-methods) 
in order to calculate the total.

+$$$

Should you use a price list or a discount to adjust your products' prices?

It depends---and you may want a combination of both. If you want to offer
a special price exclusively to a particular group of customers, and you want to
set the adjusted price for each product individually, you should use a price
list.

If you want to set prices using a currency other than your store's default, or
if you want to increase prices rather than lower them, you should use a price
list.

If you want to offer a reduced rate to bulk buyers, you should use a price list
with tiered pricing.

If you want to apply a blanket modifier to a group of products, however---for
instance, 10% off everything in the catalog, or $10 off everything in
a particular category---you should use a discount. A discount can be applied to
a specific user segment or made available to all customers.

You should use a discount if you want to:

- limit the number of products that can be bought for the adjusted price 
 
- Make special pricing available to customers who enter a coupon code 

- Make special pricing available to customers who spend or have spent
  a specified amount 

$$$
