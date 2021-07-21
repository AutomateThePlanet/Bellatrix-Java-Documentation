---
layout: default
title:  "Normal Assertions"
excerpt: "Learn how to use normal assertion methods in BELLATRIX tests."
date:   2018-06-22 06:50:17 +0200
parent: web-automation
permalink: /web-automation/normal-assertions/
anchors:
  example: Example
  explanations: Explanations
---
Example
-------
```java
@Test
public void assertCartPageFields() {
    app().navigate().to("http://demos.bellatrix.solutions/?add-to-cart=26");

    app().navigate().to("http://demos.bellatrix.solutions/cart/");

    TextField couponCodeTextField = app().create().byId(TextField.class, "coupon_code");

    Assert.assertEquals(couponCodeTextField.getPlaceholder(), "Coupon code");

    Button applyCouponButton = app().create().byValueContaining(Button.class, "Apply coupon");

    Assert.assertTrue(applyCouponButton.isVisible());

    Div messageAlert = app().create().byClassContaining(Div.class, "woocommerce-message");

    Assert.assertFalse(messageAlert.isVisible());

    Button updateCart = app().create().byValueContaining(Button.class, "Update cart");

    Assert.assertTrue(updateCart.isDisabled());

    Span totalSpan = app().create().byXPath(Span.class, "//*[@class='order-total']//span");

    Assert.assertEquals(totalSpan.getText(), "120.00€");

    SoftAssert multipleAssert = new SoftAssert();
    multipleAssert.assertEquals(totalSpan.getText(), "120.00€");
    multipleAssert.assertTrue(updateCart.isDisabled());
    multipleAssert.assertAll();
}
```

Explanations
------------
```java
Assert.assertEquals(couponCodeTextField.getPlaceholder(), "Coupon code");
```
We can assert the default text in the coupon text fiend through the BELLATRIX component **getPlaceholder** method. The different BELLATRIX web component classes contain lots of these methods which are a representation of the most important HTML element attributes. The biggest drawback of using vanilla assertions is that the messages displayed on failure are not meaningful at all. This is so because most unit testing frameworks are created for much simpler and shorter unit tests. In next chapter, there is information how BELLATRIX solves the problems with the introduction of **validate** methods. If the bellow assertion fails the following message is displayed:
```
Expected :Discount code
Actual   :Coupon code
```
You can guess what happened, but you do not have information which element failed and on which page.
```java
Assert.assertTrue(applyCouponButton.isVisible());
```
Here we assert that the apply coupon button is visible on the page. On fail the following message is displayed:
```
Expected :true
Actual   :false
```
Cannot learn much about what happened.
```java
Div messageAlert = app().create().byClassContaining(Div.class, "woocommerce-message");
Assert.assertFalse(messageAlert.isVisible());
```
Since there are no validation errors, verify that the message div is not visible.
```java
Button updateCart = app().create().byValueContaining(Button.class, "Update cart");
Assert.assertTrue(updateCart.isDisabled());
```
We have not made any changes to the added products so the update cart button should be disabled.
```java
Span totalSpan = app().create().byXPath(Span.class, "//*[@class='order-total']//span");
Assert.assertEquals(totalSpan.getText(), "120.00€");
```
We check the total price contained in the order-total span HTML element.

```java
SoftAssert multipleAssert = new SoftAssert();
multipleAssert.assertEquals(totalSpan.getText(), "120.00€");
multipleAssert.assertTrue(updateCart.isDisabled());
multipleAssert.assertAll();
```
You can execute multiple assertions failing only once viewing all results.

One more thing you need to keep in mind is that normal assertion methods do not include BDD logging and any available hooks. BELLATRIX provides you with a full BDD logging support for **validate** assertions and gives you a way to hook your logic in multiple places.