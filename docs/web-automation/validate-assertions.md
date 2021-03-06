---
layout: default
title:  "Validate Assertions"
excerpt: "Learn how to use BELLATRIX Validate assertion methods."
date:   2018-06-22 06:50:17 +0200
parent: web-automation
permalink: /web-automation/validate-assertions/
anchors:
  example: Example
  explanations: Explanations
---
Example
-------
```java
@Test
public void assertValidateCartPageFields() {
    app().navigate().to("http://demos.bellatrix.solutions/?add-to-cart=26");

    app().navigate().to("http://demos.bellatrix.solutions/cart/");

    TextField couponCodeTextField = app().create().byId(TextField.class, "coupon_code");

    couponCodeTextField.validatePlaceholderIs("Coupon code");

    Button applyCouponButton = app().create().byValueContaining(Button.class, "Apply coupon");

    applyCouponButton.validateIsVisible();

    Div messageAlert = app().create().byClassContaining(Div.class, "woocommerce-message");

    messageAlert.validateNotVisible();

    Button updateCart = app().create().byValueContaining(Button.class, "Update cart");

    updateCart.validateIsDisabled();

    Span totalSpan = app().create().byXPath(Span.class, "//*[@class='order-total']//span");

    totalSpan.validateTextIs("120.00€");
}
```

Explanations
------------
```java
// Assert.assertEquals(couponCodeTextField.getPlaceholder(), "Discount code");
couponCodeTextField.validatePlaceholderIs("Coupon code");
```
We can assert the default text in the coupon text field through the BELLATRIX element getPlaceholder method. The different BELLATRIX web elements classes contain lots of these properties which are a representation of the most important HTML element attributes. The biggest drawback of using vanilla assertions is that the messages displayed on failure are not meaningful at all. This is so because most unit testing frameworks are created for much simpler and shorter unit tests. There is information in next chapter how BELLATRIX solves If the commented assertion fails the following message is displayed:
```
Expected :Discount code
Actual   :Coupon code
```
You can guess what happened, but you do not have information which element failed and on which page. If we use the **validate** methods, BELLATRIX waits some time for the condition to pass. After this period if it is not successful, a beautified meaningful exception message is displayed:
```
The component's placeholder should be 'Discount code' but was 'Coupon code'. The test failed on URL: http://demos.bellatrix.solutions/cart/
```

```java
Button applyCouponButton = App.ElementCreateService.CreateByValueContaining<Button>("Apply coupon");

// Assert.assertTrue(applyCouponButton.isVisible());
applyCouponButton.validateIsVisible();
```
Assert that the apply coupon button exists and is visible on the page. On fail the following message is displayed:
```
Expected :true
Actual   :false
```
Cannot learn much about what happened.
Now if we use the **validateIsVisible** method and the check does not succeed the following error message is displayed:
```
The control should be visible but wasn't. The test failed on URL: http://demos.bellatrix.solutions/cart/
```
To all exception messages, the current URL is displayed, which improves the troubleshooting.
```java
Div messageAlert = app().create().byClassContaining(Div.class, "woocommerce-message");
messageAlert.validateNotVisible();
```
Since there are no validation errors, verify that the message div is not visible.
```java
Button updateCart = app().create().byValueContaining(Button.class, "Update cart");
updateCart.validateIsDisabled();
```
No changes are made to the added products so the update cart button should be disabled.
```java
Span totalSpan = app().create().byXPath(Span.class, "//*[@class='order-total']//span");
totalSpan.validateTextIs("120.00€");
```
Check the total price contained in the order-total span HTML element. By default, all Validate methods have 5 seconds timeout. However, you can specify a custom timeout and sleep interval (period for checking again)

BELLATRIX provides you with a full BDD logging support for **validate** assertions and gives you a way to hook your logic in multiple places.