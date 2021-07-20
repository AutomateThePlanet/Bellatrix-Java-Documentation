---
layout: default
title:  "Extensibility – Extend Common Services"
excerpt: "Learn how to extend BELLATRIX common services."
date:   2018-06-23 06:50:17 +0200
parent: web-automation
permalink: /web-automation/extensibility-extend-common-services/
anchors:
  example: Example
  explanations: Explanations
---
Example
-------
```java
public class ExtendExistingCommonServicesTests extends WebTest {
    @Override
    public ExtendedApp app() {
        return new ExtendedApp();
    }

    @Test
    public void purchaseRocket() {
        app().navigate().viaJavaScript("http://demos.bellatrix.solutions/");

        Select sortDropDown = app().create().byNameEndingWith(Select.class, "orderby");
        Anchor protonMReadMoreButton =
                app().create().byInnerTextContaining(Anchor.class, "Read more");
        Anchor addToCartFalcon9 =
                app().create().byAttributesContaining(Anchor.class, "data-product_id", "28").ToBeClickable();
        Anchor viewCartButton =
                app().create().byClassContaining(Anchor.class, "added_to_cart wc-forward").ToBeClickable();
        TextField couponCodeTextField = app().create().byId(TextField.class, "coupon_code");
        Button applyCouponButton = app().create().byValueContaining(Button.class, "Apply coupon");
        Number quantityBox = app().create().byClassContaining(Number.class, "input-text qty text");
        Div messageAlert = app().create().byClassContaining(Div.class, "woocommerce-message");
        Button updateCart = app().create().byValueContaining(Button.class, "Update cart").toBeClickable();
        Button proceedToCheckout =
                app().create().byClassContaining(ExtendedButton.class, "checkout-button button alt wc-forward");
        Heading billingDetailsHeading =
                app().create().byInnerTextContaining(Heading.class, "Billing details");
        Span totalSpan = app().create().byXPath(Span.class, "//*[@class='order-total']//span");

        sortDropDown.selectByText("Sort by price: low to high");
        protonMReadMoreButton.hover();
        addToCartFalcon9.focus();
        addToCartFalcon9.click();
        viewCartButton.click();
        couponCodeTextField.setText("happybirthday");
        applyCouponButton.click();

        messageAlert.toHaveContent().toBeVisible().waitToBe();
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
public class ExtendedNavigationService extends NavigationService {
    public void viaJavaScript(String url) {
        var javaScriptService = new JavaScriptService();

        if (!isUrlValid(url)) {
            throw new IllegalArgumentException(String.format("The specified URL – %s is not in a valid format!", url));
        }

        javaScriptService.execute(String.format("window.location.href = '%s';", url));
    }

    public boolean isUrlValid(String url) {
        try {
            URL obj = new URL(url);
            obj.toURI();
            return true;
        } catch (MalformedURLException | URISyntaxException e) {
            return false;
        }
    }
}
```
```java
public class ExtendedApp extends App {
    @Override
    public ExtendedNavigationService navigate() {
        return SingletonFactory.getInstance(ExtendedNavigationService.class);
    }
}
```
A way to extend the BELLATRIX common services is to create a class extending the service for the additional action and create a class, extending the **App**, pointing to the new extended service, then override the **app** method in the test.
```java
@Override
public ExtendedApp app() {
    return new ExtendedApp();
}
```
Then you can use the additional method you created since **navigate** will now return **ExtendedNavigationService** instance.
```java
app().navigate().viaJavaScript("http://demos.bellatrix.solutions/");
```
Use newly added navigation though JavaScript which is not part of the original implementation of the common service.