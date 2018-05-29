# Variable Rate Shipping [](id=by-weight-shipping-method)

Variable rate shipping uses a formula to calculate shipping costs on the basis
of three factors: the order's total weight, it's subtotal (cost before shipping
and taxes), and any fixed price you choose to impose. To what degree each factor
is weighted---if at all---is up to you.

You can create multiple different shipping methods with variable rates. For
example, you might create "Standard Ground" method with a relatively low cost
per unit of weight, as well as "Two-Day Air" method that calculates costs in the
same way but results in a higher price.

## Creating a Variable Rate Shipping Method

Variable-rate shipping costs are determined by the following formula: shipping
costs = [fixed price] + ([order total weight] x [rate unit weight price])
+ ([order subtotal] x [rate percentage])

Follow these steps:

1.  Go to *Site Menu* &rarr; *Commerce* &rarr; *Settings* and click on the
    *Shipping Method* tab. Click on the *Variable Rate* method and then on the
    *Shipping Options* tab.

2.  Click the ![Add](../../images/icon-add.png) button and fill in the
    following fields:

    **Name:** this is the name that buyers will see when they are prompted to
    select a shipping option.

    **Description:** Information about the method---delivery time, guarantees,
    insurance and the like---should go in this field.

    **Priority:** Sets the order in which methods will be displayed. Lower
    numbers come first.

3.  Click *Save*. Then click the *Shipping Option Settings* tab.

4.  Click the ![Add](../../images/icon-add.png) button and fill in the following
    fields. Then click *Save*.

**Shipping Option:** Select the shipping option to which this setting should be
applied. The first time through, you should select the option you named in step
2.

**Warehouse:** Select a warehouse if you want this method to apply only to
shipments from one location. Leave blank to use this method for all warehouses.

**Country, Region, Zip:** Make an entry if you want to restrict the use of this
shipping method to the area you define.

**Weight From, Weight To:** Enter a weight range for orders that can use this
option.

**Fixed Price:** An entry in this field sets an effective minimum price
and contributes the fixed component of the shipping cost formula.

**Rate Unit Weight Price:**





















## Measurement Units [](id=measurement-units)

In order to use product weight to calculate shipping costs, the catalog must
specify each product's weight (see
[Configuration](/web/liferay-emporio/documentation/-/knowledge_base/7-1/configuration#shipping)).
First, however, you may need to create measurement units.

@commerce@ uses a handful of commonly used measurement units--meters,
millimeters, feet, inches, kilograms, grams, 

If you built your store using a site initializer, measurement units probably
came prepackaged. However, you may need to make make changes if you want to
change the primary unit, or use units are relatively obscure.



There's a little bit of housekeeping to take care of before you can ship by
weight. Of course, every product in your catalog will need to specify a weight
[link to catalog/SKUs]. Before you do that, however, you may need to create
measurement units.

If you built you store from a site template, measurement units will already be
in place. But if you built your site from scratch or prefer archaic units of
measure, go to *Site Menu* &rarr; *Commerce* &rarr; *Settings* and click on the
*Measurement Units* tab. Then select either the *Dimension* or *Weight* tab and
click ![add](../../images/icon-add.png).

![Figure 1: This is where you choose between using a traditional system of measurement or the much easier (quicker, more seductive) metric system.](../../images/measurement-units.png).

The *key* field  will auto-fill when the unit's
name is entered.

If you intend for the unit to be use to compute shipping costs, check the
*Primary* box and the *Ratio to Primary* field will disappear. Otherwise, enter
the number of new units that are equal to a single primary unit. For example, if
the primary unit is *ounces*, then ratio to primary for the unit *pounds* would
be 0.0625. If the primary unit is *kilograms*, then the ratio for the unit
*grams* would be a nice, even 1000 (seductive, isn't it?).

Once your measurement units are in place and you've entered the weight of your
products, you can activate the By Weight shipping method just as you activated
the fixed rate method [link to details, above]. But the shipping options for By
Weight are a little different.

## Shipping Options [](id=shipping-options)

To define a shipping option for By Weight Shipping, go to *Site Menu* &rarr;
*Commerce* &rarr; *Settings* &rarr; *Shipping Methods* tab &rarr; *By Weight*
and select the *Shipping Options* tab. Name and describe your option as above
[link to fixed/shipping options] and move on to the *Shipping Option Settings*
tab.

<!--[Figure 3: This is where it can get a little math-y. Relax, the computer
will handle the actual arithmetic.](../../images/shipping-formula.png)-->

By Weight Shipping calculates a rate by the formula displayed at the top of the
page. This allows to base shipping costs on three factors: a fixed (or minimum)
cost, a cost expressed as set price per unit of weight, and a cost expressed as
a percentage of the order subtotal. To use these factors, fill in the following
fields.

**Shipping Option**: Select the option to which this option setting applies.

**Warehouse**: Make a selection here to apply this setting only to shipments
from a particular warehouse. Leave blank to apply to all warehouses.

**Country, Region, Zip**: Entries to these fields limit the use of this setting
to deliveries to the selected locations.

**Weight From, Weight To**: Entries to these fields limit the use of this
setting to products that fall within the weight range.

**Fixed Price**: A price in this field sets an effective minimum price and
contributes the fixed component of the shipping cost formula.

**Rate Unit Weight Price**: Sets the cost to ship per unit of weight.

**Rate percentage**: A number in this field imposes a shipping cost
as a percentage of the total purchase price of the items in an order.

The total shipping cost will be the sum of the fixed price, the rate unit weight
price (times the weight of order, or course) and the rate percentage (times the
order subtotal). If you don't want to use one or more of these components, leave
the corresponding field blank.

The next section covers FedEx shipping.
