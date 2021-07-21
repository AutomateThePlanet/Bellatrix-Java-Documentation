---
layout: default
title:  "Extensibility – Component Action Hooks"
excerpt: "Learn how to extend BELLATRIX web component controls using component action hooks."
date:   2018-06-23 06:50:17 +0200
parent: web-automation
permalink: /web-automation/extensibility-component-action-hooks/
anchors:
  introduction: Introduction
  example: Example
  explanations: Explanations
---
Introduction
------------
Another way to extend BELLATRIX is to use the components' hooks. This is how the BDD logging and highlighting are implemented. For each method of the component, there are two hooks – one that is called before the action and one after. For example, the available hooks for the button are:
- **CLICKING** - an event executed before button click
- **CLICKED** - an event executed after the button is clicked
- **HOVERING** - an event executed before button hover
- **HOVERED** - an event executed after the button is hovered
- **FOCUSING** - an event executed before button focus
- **FOCUSED** - an event executed after the button is focused
- **SCROLLING_TO_VISIBLE** - an event executed before the button is scrolled to be visible
- **SCROLLED_TO_VISIBLE** - an event executed after the button is scrolled to be visible
- **SETTING_ATTRIBUTE** - an event executed before the button's attribute is set to a value
- **ATTRIBUTE_SET** - an event executed after the button's attribute is set to a value
- **CREATING_ELEMENT** - an event executed before the button component is created
- **CREATED_ELEMENT** - an event executed after the button component is created
- **CREATING_ELEMENTS** - an event executed before the button components are created
- **CREATED_ELEMENTS** - an event executed after the button components are created

You need to implement the event handlers for these events and add listeners them.
In the example, **Logger** is called for each button event printing to the console the coordinates of the button. You can call external logging provider, making screenshots before or after each action, the possibilities are limitless.

Example
-------
```java
public class LoggingButtonEvents {
    public static void addEventListeners() {
        Button.CLICKING.addListener(arg -> Logger.logInformation(
                "before clicking button; coordinates: x=%d y=%d",
                arg.getComponent().getWrappedElement().getLocation().getX(),
                arg.getComponent().getWrappedElement().getLocation().getY()
        ));

        Button.CLICKED.addListener(arg -> Logger.logInformation(
                "after button clicked; coordinates: x=%d y=%d",
                arg.getComponent().getWrappedElement().getLocation().getX(),
                arg.getComponent().getWrappedElement().getLocation().getY()
        ));

        Button.HOVERING.addListener(arg -> Logger.logInformation(
                "before hovering button; coordinates: x=%d y=%d",
                arg.getComponent().getWrappedElement().getLocation().getX(),
                arg.getComponent().getWrappedElement().getLocation().getY()
        ));

        Button.HOVERED.addListener(arg -> Logger.logInformation(
                "after button hovered; coordinates: x=%d y=%d",
                arg.getComponent().getWrappedElement().getLocation().getX(),
                arg.getComponent().getWrappedElement().getLocation().getY()
        ));

        Button.FOCUSING.addListener(arg -> Logger.logInformation(
                "before focusing button; coordinates: x=%d y=%d",
                arg.getComponent().getWrappedElement().getLocation().getX(),
                arg.getComponent().getWrappedElement().getLocation().getY()
        ));

        Button.FOCUSED.addListener(arg -> Logger.logInformation(
                "after button focused; coordinates: x=%d y=%d",
                arg.getComponent().getWrappedElement().getLocation().getX(),
                arg.getComponent().getWrappedElement().getLocation().getY()
        ));
    }
}
```
```java
public class ElementActionHooksTests extends WebTest {
    @Override
    public void configure() {
        super.configure();
        LoggingButtonEvents.addEventListeners();
    }

    @Test
    public void purchaseRocketWithGloballyOverriddenMethods() {
        app().navigate().to("http://demos.bellatrix.solutions/");

        Select sortDropDown = app().create().byNameEndingWith(Select.class, "orderby");
        Anchor protonMReadMoreButton =
                app().create().byInnerTextContaining(Anchor.class, "Read more");
        Anchor addToCartFalcon9 =
                app().create().byAttributeContaining(Anchor.class, "data-product_id", "28").toBeClickable();
        Anchor viewCartButton =
                app().create().byClassContaining(Anchor.class, "added_to_cart wc-forward").toBeClickable();
        TextField couponCodeTextField = app().create().byId(TextField.class, "coupon_code");
        Button applyCouponButton = app().create().byValueContaining(Button.class, "Apply coupon");
        NumberInput quantityBox = app().create().byClassContaining(NumberInput.class, "input-text qty text");
        Div messageAlert = app().create().byClassContaining(Div.class, "woocommerce-message");
        Button updateCart = app().create().byValueContaining(Button.class, "Update cart").toBeClickable();
        Anchor proceedToCheckout =
                app().create().byClassContaining(Anchor.class, "checkout-button button alt wc-forward");
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

        messageAlert.toBeVisible().waitToBe();
        messageAlert.validateTextIs("Coupon code applied successfully.");
        quantityBox.setNumber(0);
        quantityBox.setNumber(2);

        updateCart.click();

        totalSpan.validateTextIs("95.00€");

        proceedToCheckout.click();
        billingDetailsHeading.toBeVisible().waitToBe();
    }
}
```

Explanations
------------
```java
@Override
public void configure() {
    super.configure();
    LoggingButtonEvents.addEventListeners();
}
```
Once you have created the class, you need to tell BELLATRIX to use it. To do so call the **addEventListeners** method that you created in the **configure** phase.

<!-- ```csharp
 App.RemoveElementEventHandler<DebugLoggingButtonEventHandlers>();
```
If you need to remove it during the run you can use the method bellow.

Each BELLATRIX validate method gives you a hook too. To implement them you can derive the **EnsureExtensionsEventHandlers** base class and override the event handler methods you need. For example for the method **EnsureIsChecked**, **EnsuredIsCheckedEvent** event is called after the check is done. -->