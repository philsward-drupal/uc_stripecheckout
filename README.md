# UC Stripe Checkout

## COMPATIBILITY
Drupal 7.x Ubercart 3.x

## ABOUT

"Stripe Checkout is the quickest way to start accepting payments on Stripe. Send your customers to pay on Stripe's conversion-optimized page instead of building your own."

More:
https://stripe.com/docs/payments/checkout

## INSTALLATION

Install in the usual sites/all/modules folder

If uc_stripecheckout is not enabled on a site: 
* Install, enable, configure uc_stripecheckout as usual.
* Install the Stripe PHP Library in the /sites/all/libraries/stripe folder. Download from: https://github.com/stripe/stripe-php/releases

**Important:** uc_stripe and uc_stripecheckout cannot be used at the same time. Details to follow below.

**Configuration**

Menu: Home » Administration » Store » Configuration » Payment methods

Settings: /admin/store/settings/payment/method/stripecheckout

## USAGE

If uc_stripecheckout is enabled on a site: 
1) Open uc_stripecheckout settings page on a separate tab, so that you can copy-paste keys 
2) Optional: put your site into maintenance mode, so that people do not check out while you configure things 
3) Go to the modules page, disable uc_stripe module, but do not uninstall it, in case you need to switch back to it. 
4) Enable uc_stripecheckout module 
5) Go to the payment config page, disable "Credit card", which is uc_stripe, and "Stripe Checkout" should already be enabled 
6) Go to Stripe Checkout settings page and using the previously opened separate tab, copy-paste keys from uc_stripe to uc_stripecheckout, and modify other settings as needed (e.g. Test mode). 
7) Optional: take site out from maintenance mode 
8) Do a test order or wait until a new order comes in. 

If customers encounter any payment problem, you can disable uc_stripecheckout and enable uc_stripe, then enable "Credit card" payment method on the payment settings page, and everything should work as before. 

## LIMITATIONS

The module sends only a single Stripe line item with title "Order #12345" and with line item price equalling the Ubercart order total:
* if it is configured not to send products as line items
* if order has discounts or any other negative line items 
* if there are more than 100 line items (products), this is a Stripe limitation. 
* if the Ubercart order total does not equal the sum of the Stripe line items (Just in case there is 1 cent difference in case of rounding)

Since the lowest line item price that Stripe accepts is 1 cent, the module does not send products or shipping line items whose price is 0. So "Free Shipping - $0.00" line item cannot and will not appear on the Stripe Checkout page. 

Customer is sent back from the review page to the checkout page: 
* if Stripe has not been properly configured
* if order total is less than $0.50 (Stripe limitation) 

The module works only with USD currency.

This is not designed to work with a module that removes the Review page.
* The Stripe Checkout page is loaded in the background when viewing the Review page. As a result, navigating from Review -> Payment screen issues a webhook that shows up in Stripe as "Incomplete" when looking at All Payments in the dashboard. This is normal.
* Two incompletes in a row indicate the customer most likely clicked the "back" link (most likely), or it could indicate the redirect is not working properly.
