---
layout: article
title: Checkout pixel implementation guide
attribution: 
resource: true
categories: [Resources]
---


Olapic checkout pixel will track detailed data about the order that gets processed on the e-commerce store.

Once implemented, Olapic will be able to grab the information related to each of the product bought (product ID, product price), and attribute it to the specific user who checked out.

The checkout pixel should be implemented on the order confirmation page after the user checkout (explicitly, after the user has submitted the order to be processed).

Example code:

```html
<!-- Photorank advanced checkout code -->
<script type="text/javascript" data="olapic-checkout">
	var olapicProducts = '';

	// Product loop starts. This is where you will store each product purchased info
	olapicProducts += ((olapicProducts === '') ? '' : ',' ) +  '{"id":"'+encodeURIComponent("PRODUCT_ID")+'","price":"'+encodeURIComponent("PRODUCT_PRICE")+'"}';
	// Product loop ends.

	window.onload = function() {
		;(function(w, d){
			olapicProducts = '['+olapicProducts+']';
			var olapicCheckout = d.createElement("script"); olapicCheckout.async = true;
			olapicCheckout.setAttribute("olapicProducts", olapicProducts);
			olapicCheckout.setAttribute("olapicApiKey", encodeURIComponent("UNIQUE_OLAPIC_API_KEY"));
			olapicCheckout.setAttribute("olapicIdentifier", encodeURIComponent("USER_ID"));
			olapicCheckout.setAttribute("olapicAmount", encodeURIComponent("AMOUNT"));
			olapicCheckout.setAttribute("olapicTransactionId", encodeURIComponent("TRANSACTION_ID"));
			olapicCheckout.setAttribute("olapicEmail", encodeURIComponent("EMAIL"));
			olapicCheckout.setAttribute("olapicName", encodeURIComponent("NAME"));
			olapicCheckout.src="//www.photorank.me/static/js/olapic.checkout.js";
			d.body.appendChild(olapicCheckout);
		})(window, document);
	}
</script>
```

## Instructions:

1. Grab your account specific checkout code from the Checkout code tab from Settings page ([link](http://www.photorank.me/admin/settings#tabb_checkout)). This code should be placed in your Thank You / Checkout Confirmation page where the user has completed the purchase.

2. Pay close attention to the products array referenced by `olapicProducts`. This line uses add assignment operator along with a shorthand if / else statement that will append product information into the array. You will have to loop through the available purchased product array within your template and append additional products appropriately to the JavaScript variable assignment.

	Please refer to the product loop using PHP as an example below:

	```markup
    <?php
    foreach ($products as $product => $price) {
        echo "olapicProducts += ((olapicProducts === '') ? '' : ',' ) +  '{\"id\":\"'+encodeURIComponent(\"$product\")+'\",\"price\":\"'+encodeURIComponent(\"$price\")+'\"}';"
    }
    ?>
    ```
    
3. Swap out the variables with each of the appropriate variables from your site.

4. Please refer to the following variables we need placed in the code:

- `PRODUCT_ID` - **required**
	- This is Purchased Product's ID. The unique product ID from your store should be used
- `PRODUCT_PRICE` - **required**
	- This is the price of each purchased product
- `TRANSACTION_ID` - **required**
	- The unique transaction ID from your from your store.
- `AMOUNT` - **required**
	- The total amount of the entire purchase. Net total amount must be used (before shipping & tax).
- `USER_ID` - **required**
	- The unique user ID from your store. This will identify the user that performed the purchase. If you can't pass the unique user ID, please use the user e-mail address as the value.
- `EMAIL` - **OPTIONAL**
	- The unique user e-mail from your store. This will identify the user that performed the purchase. This field is mandatory if you want to use the Post Purchase Email Feature. If you are not using the Post-Purchase email function, you can leave this value as blank.
- `NAME` - **OPTIONAL**
	- User's full name from your store. We use this name in the e-mail template when we send the Post Purchase Email. If you are not using the Post-Purchase email function, you can leave this value as blank.
	
**Note:** All values must be inside the `encodeURIComponent` function in order to keep the data clean.

If product array cannot be populated and you wish to only return total order value data without detailed product data included in the checkout, you can leave the `olapicProducts` as an empty array.
