---
layout: article
title: Olapic Product Feed
attribution: Jae
resource: true
categories: [Resources]
---


The following document will explain the product feed supported by **Olapic**.

## Basics

**Product Feed (PF)** is a list of products in your e-commerce store. Olapic requires this feed to create what we call `streams` for your account so you can start tagging the media to specific products in the Moderation Queue. So having the right product feed format is critical for a correct implementation.

Ideally, we will create one stream per product object / node in your PF.

## XML product feed

We will focus on how to create your PF for Olapic, and explain which fields are necessary for a correct implementation.


### Start your XML file

To start, we only support `UTF-8` encoding, so your XML feed must start with the following line:

```xml
<?xml version="1.0" encoding="utf-8"?>
```

**Note:** Please ensure your feed is in `UTF-8`. If not, please contact your Integration Engineer to check other available options.

As this will be an XML file, we will need a root node. We will use `<Feed>` as the root node as default. Inside the `<Feed>` node, we will have `<Products>` node with children nodes called `<Product>`.

Here is an example:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<Feed>
	<Categories>
				<Category>...</Category>
		</Categories>
		<Products>
				<Product>...</Product>
		</Products>
</Feed>
```

### Product categorization

We support category structure for each product in your e-commerce store. In order to use our *category_based widget*. If you want to use this feature, all you need to do is give us all the categories you will want to create in Olapic as child elements within the `<Categories>` element.

The `<Categories>` element can contain as meny `<Category>` children nodes as you need.

You can build the `<Category>` elements using the following children elements:

| Element Name | Description | Required |
|--------------|-------------|----------|
| Name | The visible name of the category. | YES |
| CategoryUniqueID | A unique ID for the category. | Only if you use *category_based* widget |
| CategoryUrl | A URL where visitors can shop by this category, if you have one. | Only if you use our *category_based* widget |
| CategoryParentID | This is to support sub-category levels. If this category is not a root category and, instead, it's a sub-category of another category, all you need to do is give us that CagetoryUniqueID here. We do the rest! | Only if you need sub-category support |


`Categories` node example:
```xml
<Categories>
	<Category>
		<CategoryUniqueID>cat-111</CategoryUniqueID>
		<Name>My Demo Category</Name>
		<CategoryUrl>http://www.demoshop.com/cotegories/cat-111</CategoryUrl>
		<CategoryParentID></CategoryParentID>
	</Category>
	<Category>
		<CategoryUniqueID>cat-123</CategoryUniqueID>
		<Name>My Demo Sub-Category</Name>
		<CategoryUrl>http://www.demoshop.com/cotegories/cat-123</CategoryUrl>
		<CategoryParentID>cat-111</CategoryParentID>
	</Category>
</Categories>
```
In this example the first `<Category>` (*My Demo Category*) is a root category, it has no parent category. But it has a sub-category called *My Demo Sub-Category*. We can tell this because the second `<Category>` element has as `CategoryParentID` the `CategoryUniqueID` of the first element.

### `<Product>` element

`<Product>` will define product stream you want to import to Olapic. Basically, we will create a `stream` entity for every `<Product>` element.

Here is a list of possible elements you can use under `<Product>` (please note that some of these elements are required):

| Element Name | Description | Required |
|--------------|-------------|----------|
| Name | The visible name of the product in your PDP.| YES |
| ProductUniqueID | This is the unique identifier of the product. We treat this as an *unique* key and your organization will use it to call our widgets in your PDP. | YES |
| ProductUrl | This is the URL we use when you click "Shop this look" in an Olapic viewer. This must take the visitor to a page to purchase the item | YES |
| ImageUrl | This is the URL of the product primery image. The image that most represents your product in your PDP | YES |
| Description | This is a short and plain text description of the product. We use this in your Olapic Admin page, your visitors will not see it. *No HTML elements are recognized in this element*. | NO |
| CategoryID | This is the unique identifier of the category related with this product. We use this in the *category_based* Widget. ***The value here should match the *CategoryUniqueID* of a `<Category>` element.*** | Only if you use our *category_based* widget |
| CategoriesID | Contains at least one `<CategoryID>` element. | Only if you have multiples categories associated with this product |
| EAN | European Article Number, which is used world wide for marking retail goods. Can be a string of digits either 8 or 13 characters long. | NO |
| EANs | Contains at least one `<EAN>` element. | Only if you use `<EAN>` elements or *syndication* |
| UPC | Universal Product Code, which is the 6 - or 12- digit bar code used for standard retail packaging in the United States. The UPC must contain numerals only, with no letters or characters. Further, spaces and hyphens disrupt ***syndication*** matching and must be removed. | NO |
| UPCs | Contains at least one `<UPC>` element. | Only if you use `<UPC>` elements or *syndication* |
| ISBN | International Standard Book Number, which is a unique identifier for books and which is intended for commercial use. The number is either 10 or 13 digits long and consists of four or five parts. The different sections of the number can be of different lengths and are usually separated by hyphens (-) or tildes (~). Spaces and hyphens disrupt ***syndication*** matching and must be removed. | NO |
| ISBNs | Contains at least one `<ISBNs>` element. | Only if you use `<ISBN>` elements or *syndication* |
| Price | This is the most significative price your visitor can see in you PDP. *Do NOT include the currency*. Only include the number with decimals separeted by '.'. Example: 23.99 | No |
| Stock | This is an integer that represents your stock of this product | No |
| Availability | This is a bool representing the current status of this product. Should be consistent with your site. We can set `INACTIVE` galleries dynamically based on this value. *Expected values: {true, false, 0, 1}* | No |
| Color | This is a string with the color name of the product. Useful for color specific products. | No |
| ParentID | This is to support sub-streams levels. If this stream is not a root stream and, instead, it's a sub-stream of another stream, all you need to do is give us that ProductUniqueID here. We do the rest! Example: Color specific stream has as ParentID the non color specific stream | Only if you need sub-stream support |



**Note: Fields marked as `required` must not be empty**

### Attributes elements of `<Product>` element

We want your organization to have full control of the streams, and in order to accomplish we give you the following attributes you can include in a `<Product>` element to be able to mark as `INACTIVE` streams or even delete them.

| Attribute Name | Description | Required |
|--------------|-------------|----------|
| removed | This is a bool you can use to remove products. If you set this to `true` then we will remove the stream associated with this product. Default: false. *Expected values: {true, false, 0, 1}* | No |
| disabled | This is a bool you can use to disable products. If you set this to `true` then we will set the stream associated with this product as INACTIVE. Default: false. *Expected values: {true, false, 0, 1}* | No. |

### XML Feed Example
The following is an example of a valid feed you can provide.
```xml
<?xml version="1.0" encoding="UTF-8"?>
<Feed>
	<Categories>
		<Category>
			<CategoryUniqueID>cat-111</CategoryUniqueID>
			<Name>My Demo Category</Name>
			<CategoryUrl>http://www.demoshop.com/cotegories/cat-111</CategoryUrl>
			<CategoryParentID></CategoryParentID>
		</Category>
		<Category>
			<CategoryUniqueID>cat-123</CategoryUniqueID>
			<Name>My Demo Sub-Category</Name>
			<CategoryUrl>http://www.demoshop.com/cotegories/cat-123</CategoryUrl>
			<CategoryParentID>cat-111</CategoryParentID>
		</Category>
	</Categories>
	<Products>
			<Product>
				<Name>My Exmaple Product</Name>
				<ProductUniqueID>123456789</ProductUniqueID>
				<ProductUrl>http://www.demoshop.com/products/123456789</ProductUrl>
				<ImageUrl>http://www.demoshop.com/images/product-image-123456789.jpg</ImageUrl>
				<Description>This is an example description of my product</Description>
				<CategoryID>cat-123</CategoryID>
				<UPCs>
					<UPC>001122334455</UPC>
				</UPCs>
				<Price>23.99</Price>
				<Stock>15</Stock>
				<Availability>true</Availability>
				<Color>Red</Color>
			</Product>
	</Products>
	<Product disabled="true">
				<Name>My New Product</Name>
				<ProductUniqueID>123456221</ProductUniqueID>
				<ProductUrl>http://www.demoshop.com/products/123456789</ProductUrl>
				<ImageUrl>http://www.demoshop.com/images/product-image-123456789.jpg</ImageUrl>
				<Description>This is an example description of my product</Description>
				<CategoryID>cat-123</CategoryID>
				<UPCs>
					<UPC>001122334455</UPC>
				</UPCs>
				<Price>23.99</Price>
				<Stock>15</Stock>
				<Availability>true</Availability>
				<Color>Green</Color>
				<ParentID>123456789</ParentID>
			</Product>
	</Products>
</Feed>
```
It contains only two `<Product>` but typically your organization will include all the products you need. As you can see, the second product will be marked as `INACTIVE`.

Also, the second Product has as parent product the first one, we can tell that because of `<ParentID>123456789</ParentID>`.

## Validate your feed
By now, you should know how to build a correct feed to fully integrate with Olapic. Congratulations!

Please, use the XML Schema Definition we have to validate your feed before sending it to us. You can find our schema here:

[XML Schema Definition for a valid Olapic Feed](http://photorank.me/olapicProductFeedV1_0.xsd)

***Important***
Validate your feed before sending it to use. You can use the tool you want, but we encourage you to do a schema validation using our XSD file above. 

Here some tools:
* [CoreFiling](http://www.corefiling.com/opensource/schemaValidate.html) is an online tool that will ask your XML file and the XSD file (that you can download form above). 
* [XML Validation](http://www.xmlvalidation.com/) is an online tool that will ask you first to paste you XML code or upload it and then you must use the checkbox *"Validate against external XML schema"* and click *"validate"* to go to the next page were you'll be asked to paste your XSD or upload it.
* xmllint (Command Line): you likely already have xmllint installed, which can do validation. At a terminal type: 
```shell
xmllint -noout --schema olapicProductFeedV1_0.xsd my_company_feed.xml
```

## Updating your Product Feed

To keep a good sync between your inventory and Olapic, we need to update the feed on a scheduled basis. In order to do update regularly, we import the feed on a daily basis. The time of feed import varies, however we usually schedule our jobs to run at night or early in the morning. This way, your team can update the feed in the afternoon to have the updates in Olapic the next day.

You can make your PF available to Olapic using one of the following options:

* **SFTP Account**: Request your Integration Engineer to set up an account for your organization and then please upload your feed periodically to that account.
* **FTP Account**: Request your Integration Engineer to set up an account for your organization and then please upload your feed periodically to that account.

We encourage you to update it daily.

## Providing a custom Product Feed
If you plan on giving us a custom feed (existing vendor feeds that are not Olapic specific), please be aware of the following:

* Please keep in mind that some of the product features provided by Olapic **may not** be available on Olapic using a custom feed. Specific parts of Olapic feed schema pertains to product features such as syndication, inventory updates, configurable/simple products, etc (not limited to the features mentioned). 
* It must be in a XML, otherwise it can take 10 or more days to process it and we can't guarantee a correct data import. That's why we work with open and stable standards.
* We require the fields listed as **required**. You can rename them, but they are essential for a good implementation.
* We cannot guarantee a successful import with custom feeds.

## Changing the Product Feed
Please note that if you want to change something in your product feed, we will have to be notified to make sure that the data schema does not break. Please contact your Integration Engineer if you have any changes to the feed scheduled.
