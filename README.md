### **BUG**: "Failed to retrieve shipping label refund status: Unexpected server error."

**Plugin Versions**: 1.25.12 & 1.25.11

I have minimal PHP experience, but think I have this tracked down to the version / commit where this has broken.

Please ping me with resources to get started and I will jump into fixing this issue.

#### The Problem / How To Reproduce
The shop owner received multiple orders and on a order with multiple products they select 'Create Shipping Label' on the following URLs page by going to the left menu 'Woocommerce' > 'orders' and clicking into a order with multiple products on the right

**URL:** `https://shopowner.com/wp-admin/post.php?post=9124&action=edit`

The Shop owner goes through the process of buying the label, but is returned to the above URL afterwards and an error is displayed **Failed to retrieve shipping label refund status: Unexpected server error.**

No 'shipment tracking' section has been populated on this page like would be normally be seen and the the 'Create Shipping Label' button is still present for the order though charges are happening and email receipts are being sent to the shop owner.

#### What exactly is breaking? 

When I pop into a browser console and watch the network tab on the above URL I see the following endpoint fails with error 403  in plugin version 1.25.11 and 1.25.12.

`https://shopowner.com/wp-json/wc/v1/connect/label/9124/3707992,3707991,3707980,3707979,3707972,3707971`

when I switch to plugin version 1.25.10 the endpoint works but is queried in a different way where the endpoint is queried one time for each label id instead of just once like in plugin 1.25.11 and 1.25.12 .

`https://shopowner.com/wp-json/wc/v1/connect/label/9124/3707992` 
`https://shopowner.com/wp-json/wc/v1/connect/label/9124/3707980`

#### What Changed To Break This? 

In the plugin version 1.25.11 – 2021-04-06 the following change log was recorded. 

`Tweak – Update the shipping label status endpoint to accept and return multiple ids.`

I believe the following commit might have something to do with this: [ Click For The Commit ](https://github.com/Automattic/woocommerce-services/commit/0a357daed1b1b2c504fa75de43e995443993c140)

#### How does The Store Owner Bypass This Problem? 

Install '**WP Rollerback Plugin**' and down grade the '**WooCommerce Shipping & Tax**' to version - **1.25.10**

