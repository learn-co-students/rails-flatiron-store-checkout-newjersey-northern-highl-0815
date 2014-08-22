---
tags: full-application, stripe, oo, intermediate, ajax, WIP
language: ruby, javascript
resources: 2
---

# Flatiron Store on Rails

## Second iteration: Checking out with Stripe

### Refactoring `checkout` method

Last lab, we built a post request method `checkout` that handles a bunch of our app's functionality: creating a new order from a cart, changing the order status, changing the inventory, and destroying the cart session.

Now, we're going to do a bit more and refactor that `checkout` functionality. Our site needs to accept payments from users.

This will be the new flow of our application:

1. You add items to your cart
2. You click checkout
3. You fill out payment info
4. You submit your payment via Stripe
5. Order confirmation page 

### Using Stripe to Handle Payments

We're going to use Stripe to handle accepting payments. We're going to integrate their API into our application, and with doing so, change the functionality of our app.

### Tasks

1. Order and StripePayment functionality
  * Our `checkout` method on our carts controller is going to be done away with and instead that functionality will be handled by methods in the Order class and the orders controller.
  * The `create` action in the orders controller will be where our Stripe functionality is also handled.
  * Read the documentation linked below on how Stripe can be integrated into a Rails app. It's a simple example which we're going to build off of.
  * Create a StripePayment class which will handle working with the Stripe API.
  * A StripePayment belongs to an Order and an Order has_one StripePayment
  * We're going to persist user input via Stripe, so create a migration as well. We want to save the order_id, stripe_customer_id, stripe_email, stripe_default_card, and stripe_charge_id
  * **The model specs should guide your build**
  * Instead of having a "Checkout" link on the carts show page, have a link to paying with stripe. **Pass the features tests**

2. Better Cart UX
  * A user should be able to delete a line_item from the cart, via AJAX. This would be handled by the line_items controller methods `destroy`.
  * The cart total on the page should be updated asynchronously when a line_item is deleted as well

    * Be sure that the stripe iframe knows about the new total, of you will overcharge your user!
  * **Pass the features tests**

### Hints about App Architecture 

* Much of the functionality should be in the Order model, which gets params data from Stripe from the orders controller
* StripePayment is a separate class for sake of organization and separation of concerns; it should handle creating Stripe charges and customers, as well as storing data in the database.
* How does the StripePayment model get data from the Order model? Something to consider:
  * using attr_accessors to hold non-persisted but necessary data
  * method arguments

## Up Next: Third iteration

1. Segment.io analysis of user events / behavior

## Resources
* [Setting up Stripe on Rails](https://stripe.com/docs/checkout/guides/rails)
* [Strip Checkout documentation](https://stripe.com/docs/checkout)