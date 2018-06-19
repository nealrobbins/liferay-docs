# Settings

Several functions are accessed through the *Site Menu* &rarr; *Commerce* &rarr;
*Settings*. Many of these are described elsewhere: see 
[Orders](web/liferay-emporio/documentation/-knowledge_base/7_1/orders),
[Payment Methods](web/liferay-emporio/documentation/-knowledge_base/7_1/payment-methods),
[Warehouses](web/liferay-emporio/documentation/-knowledge_base/7_1/warehouses),
[Shipping Methods](web/liferay-emporio/documentation/-knowledge_base/7_1/shipping-methods),
and [Taxes](web/liferay-emporio/documentation/-knowledge_base/7_1/taxes)
for more details.

The remaining functions are described here.

## Availability Estimates

When an item is out of stock, you need communicate to buyers when it will be
available again. By entering an availability estimate, you can provide that
information just by selecting a time frame from a drop-down menu.

Follow these steps to create an availability estimate:

1.  Go to *Site Menu* &rarr; *Commerce* &rarr; *Settings* and click on the
    *Availability Estimates* tab. Click on the ![Add](../../images/icon-add.png)
    button at the top right.

2.  Enter the estimate in the *Title* field. For example, "2-3 weeks," or "4-6
    days."

    Enter a priority to establish the order in which estimates will appear in
    the drop-down. Lowest numbers come first.

3.  Click *Save*. Repeat steps 1-3 if you need to create different estimates for
    different products.

To apply an availability estimate to a product, go to the product's
*Configuration* tab and make a selection from the *Availability Estimate*
drop-down. See
[Configuration](web/liferay-emporio/documentation/-knowledge_base/7_1/configuration#inventory)
for more information. The estimate will be displayed automatically whenever the
product is out of stock.

## Countries

Go to *Site Menu* &rarr; *Commerce* &rarr; *Settings* and click on the
*Countries* tab to view the countries in the system. The countries on this list
are available to choose from anywhere a user is prompted to enter a country
(typically to enter a shipping or billing address). On this page you can edit
the countries in this list, or---if geopolitical events warrant---add or delete
them.

You can also determine to which countries you will permit billing and shipping.
Follow these steps:

1.  To edit a country, click on its name, or else on
    ![Options](../../images/icon-options) &rarr; *Edit*.

2.  Check or uncheck the *Billing Allowed* and *Shipping Allowed* boxes.

3.  Click *Save*.

When editing a country you can also change its identifying information (name,
ISO codes), determine whether billing and shipping to that country will be
permitted, and select a priority (priority determines where in a menu a country
will be located. Lowest numbers come first). You can also deactivate a country
by unchecking the *Active* box. This removes the country from all menus in the
site where it could otherwise be selected.

### Regions

You can add regions to countries to allow users to make a selection from a menu
when entering an address. By default, regions are already in place for the USA
and for Italy. Follow these steps to add a new region:

1.  Click on a country's name and then on the *Regions* sub-tab.

2.  Click on the ![Add](../../images/icon-add.png) button, enter a name, an ISO
    code, and priority.

3.  Check the *Active* box and hit *Save*.

## Currencies

Go to *Site Menu* &rarr; *Commerce* &rarr; *Settings* and click on the
*Currencies* tab to view the currencies in the system.

To choose your store's primary currency, click on the
![Options](../../images/icon-options.png) button &rarr; Set as Primary.

To add a new currency, follow these steps:

1.  Click on the ![Add](../../images/icon-add.png) button and fill in the *Add
    Currency* form.

2.  Click Save.

The *Add Currency* form includes the following fields:

**Name:** Enter the currency's name.

**Code:** Enter the currency's ISO code.

**Format Pattern:** Enter the currency's sign, the number of digits to come
between commas, and which places will be displayed. For example, in the
format $###,##0.00 for US dollars, the places held by *0* will always be
displayed (as 0 if no other digit is present) while the places held by *#*
will only be displayed if occupied. A value of 0.01 in the above format
would be displayed as $0.01.

**Decimal Places:** Set a minimum and maximum number of places to be
displayed to the right of the decimal point.

**Rounding Mode:** Select a 
[rounding mode](https:en.wikipedia.org/wiki/Rounding#Directed_rounding_to_an_integer)
from the drop-down menu.

Check the *Primary* box to make this currency your store's default. Check the
*Active* box unless you want the currency to be saved but not used. Set
a *Priority* to determine where this currency will be positioned in any lists
where it appears---lower numbers come first.

### Exchange Rates

You can choose to allow buyers to purchase products using currencies other than
your store's primary (see [Price
Lists](web/liferay-emporio/documentation/-/knowledge_base/7-1/price-lists)).
When you do, @commerce@ can retrieve exchange rates and convert prices
accordingly.

By default, commerce relies on the European Central Bank for foreign exchange
rates. The ECB updates its exchange rates approximately every 24 hours; for more
timely updates a solution could be customized to use a different source.

To enable automatic currency conversion, follow these steps:

1.  Go to *Control Panel* &rarr; *Configuration* &rarr; *System Settings*. In
    the *Commerce* section click on *Pricing*.

2.  Click the *Store Currency* tab and check the *Enable auto update* box.

3.  Set the update interval in minutes. The default value of 60 is far more
    frequent than the ECB's daily updates, but the bank's updates are not always
    published at exactly the same time each day, so a relatively frequent
    interval ensures that the daily update is not missed.

4.  Click *Save*. Steps 1-4 do not need to repeated to enable currency
    conversion on multiple sites within the same instance.

5.  Go to *Site Menu* &rarr; *Commerce* &rarr; *Settings*. Click on the
    *Currencies* tab and then the *Exchange Rate* sub-tab.

6. Select an exchange rate provider from the drop-down menu (the European
   Central Bank is the only default option) and ensure that the *Auto Update*
   toggle is set to *Yes*. Click *Save*.

When you click save, @commmerce@ immediately retrieves exchange rate data from
the ECB and updates the site. If you make changes after clicking *Save*---for
instance, if you subsequently change your primary currencies or add new
currencies to the site---exchange rates will not be updated again until the end
of the next update interval is reached (or until you return to the *Exchange
Rate* sub-tab and click *Save* again).
