# Displaying Category Pages [](id=displaying-product-categories)

Users have two main options for displaying the contents of a category on a page.
You can manually create a category display page for each category, or you can
create a single page to display content dynamically depending on which category
a customer selects.

There are advantages to both approaches: creating pages individually has the
obvious benefit of permitting greater customization, but using a single default
page is significantly less labor intensive. Fortunately, you don't have to
choose: you can use a default page to set certain standards for appearance and
consistency, and then override the default with a custom page for any categories
on which you want to lavish more attention.

Once your categories have pages attached to them, the Category Navigation widget
can be used to help buyers easily find the products they want.

## Creating a Default Category Page

Follow these steps to create a single generic page to display category content:

1.  [Create a page.][discover/portal/-/knowledge_base/7-1/creating-and-managing-pages] 

2.  Go to *Site Menu* &rarr; *Build* &rarr; *Pages* and click on the
    ![Options](../../images/icon-options.png) button next to the page you just
    created. Select *View*.

3.  Add a *Category Content* widget to the page.

The category content widget does two things. First, it identifies the page you
just created as the default category page. When a link in a *Commerce Category
Navigation* widget is clicked, the navigation widget will find the first page in
the site with a category content widget and open that page. Second, the category
content widget displays the category's
[image](web/liferay-emporio/documentation/-knowledge_base/7-1/product-categories#images).

You can add other content and applications to the page: for instance,
a *Commerce Search Results* widget will automatically display products from the
appropriate category when the page is viewed. Related widgets, such as *Option
Facets* or *Specification Option Facets*, give users the ability to filter those
search results.

+$$$

**Note:** The commerce category navigation widget needs to be configured to link
to the category page. Once the widget is on a page, open its *Configuration*
dialog. From the *Vocabulary* drop-down, select the vocabulary that contains
your product category tree. See
[Product Categories](web/liferay-emporio/documentation/-/knowledge_base/7-1/product-categories)
for more detail.

$$$

## Creating an Individual Category Page 

To create a unique page for a single category, follow these steps:

1.  [Create a page.][discover/portal/-/knowledge_base/7-1/creating-and-managing-pages] 

2.  Go to *Site Menu* &rarr; *Categorization* &rarr; *Categories* and find the
    category you want to create a page for. Then click
    ![Option](../../images/icon-app-options) &rarr; *Edit* and select the
    *Category Display Page* tab.

3.  Click *Choose*, select the page you just created, and click *Done*.

4.  Change the *Friendly URL* if you want to be something other than the
    default.

5.  Click *Save*.

You can put whatever content and applications you want on this page, but unlike
the generic page it does *not* need a category content widget. When a user
clicks on a category, the commerce category navigation widget first checks to
see whether a unique page has been designated for the selected category. If no
such page has been designated, it then checks for a category content widget.

An individual category page will automatically override the generic page; no
further action is required than to create the page and assign it to a category.
