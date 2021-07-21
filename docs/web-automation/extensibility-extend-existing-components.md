---
layout: default
title:  "Extensibility – Extend Existing Components"
excerpt: "Learn how to extend BELLATRIX web components."
date:   2018-06-23 06:50:17 +0200
parent: web-automation
permalink: /web-automation/extensibility-extend-existing-components/
anchors:
  example: Example
  explanations: Explanations
---
Example
-------
```java
public class ExtendExistingComponent extends WebTest {
    @Test
    public void PurchaseRocket() {
        app().navigate().to("http://demos.bellatrix.solutions/");

        Select sortDropDown = app().create().byNameEndingWith(Select.class, "orderby");
        Anchor protonMReadMoreButton = app().create().byInnerTextContaining(Anchor.class, "Read more");
        Anchor addToCartFalcon9 = app().create().byAttributeContaining(Anchor.class, "data-product_id", "28").toBeClickable();
        Anchor viewCartButton = app().create().byClassContaining(Anchor.class, "added_to_cart wc-forward").toBeClickable();
        TextField couponCodeTextField = app().create().byId(TextField.class, "coupon_code");
        Button applyCouponButton = app().create().byValueContaining(Button.class, "Apply coupon");
        NumberInput quantityBox = app().create().byClassContaining(NumberInput.class, "input-text qty text");
        Div messageAlert = app().create().byClassContaining(Div.class, "woocommerce-message");
        Button updateCart = app().create().byValueContaining(Button.class, "Update cart").toBeClickable();
        ExtendedButton proceedToCheckout = app().create().byClassContaining(ExtendedButton.class, "checkout-button button alt wc-forward");
        Heading billingDetailsHeading = app().create().byInnerTextContaining(Heading.class, "Billing details");
        Span totalSpan = app().create().byXPath(Span.class, "//*[@class='order-total']//span");

        sortDropDown.selectByText("Sort by price: low to high");
        protonMReadMoreButton.hover();
        addToCartFalcon9.focus();
        addToCartFalcon9.click();
        viewCartButton.click();
        couponCodeTextField.setText("happybirthday");
        applyCouponButton.click();

        messageAlert.toBeVisible().waitToBe();
        messageAlert.validateTextIs("Coupon code applied successfully.");
        quantityBox.setNumber(0);
        quantityBox.setNumber(2);

        updateCart.click();

        totalSpan.validateTextIs("95.00€");

        proceedToCheckout.submitButtonWithEnter();
        billingDetailsHeading.toBeVisible().waitToBe();
    }
}
```

Explanations
------------
```java
public class ExtendedButton extends Button {
    public void submitButtonWithEnter() {
        var action = new Actions(getWrappedDriver());
        action.moveToElement(getWrappedElement()).sendKeys(Keys.ENTER).perform();
    }

    public void javaScriptFocus() {
        getJavaScriptService().execute("window.focus();");
        getJavaScriptService().execute("arguments[0].focus();", this);
    }
}
```
The way of extending an existing component is to create a child component. Extend the component you need. In this case, two methods are added to the standard **Button** component. Next in your tests, use the **ExtendedButton** instead of regular **Button** to have access to these methods.
The same strategy can be used to create a completely new component that BELLATRIX does not provide. You need to extend the **WebComponent** as a base class.
```java
ExtendedButton proceedToCheckout = app().create().byClassContaining(ExtendedButton.class, "checkout-button button alt wc-forward");
```
Instead of the regular button, we create the **ExtendedButton**, this way we can use its new methods.
```java
proceedToCheckout.submitButtonWithEnter();
```
Use the new custom method provided by the **ExtendedButton** class.