---
layout: default
title:  "Dialog Service"
excerpt: "Learn how to use BELLATRIX dialog service."
date:   2018-06-22 06:50:17 +0200
parent: web-automation
permalink: /web-automation/dialog-service/
anchors:
  example: Example
  explanations: Explanations
---
Example
-------
```java
public class DialogServiceTests extends WebTest {
    @Test
    public void acceptDialogAlert() {
        app().navigate().to("http://demos.bellatrix.solutions/welcome/");

        var couponButton = app().create().byId(Button.class, "couponBtn");
        couponButton.click();

        app().dialogs().handle();
    }

    @Test
    public void happyBirthdayCouponDisplayed_When_ClickOnCouponButton() {
        app().navigate().to("http://demos.bellatrix.solutions/welcome/");

        var couponButton = app().create().byId(Button.class, "couponBtn");
        couponButton.click();

        app().dialogs().handle(a -> Assert.assertEquals(a.getText(), "Try the coupon- happybirthday"));
    }

    @Test
    @Ignore
    public void dismissDialogAlert() {
        app().navigate().to("http://demos.bellatrix.solutions/welcome/");

        var couponButton = app().create().byId(Button.class, "couponBtn");
        couponButton.click();
        
        app().dialogs().handle(DialogButton.CANCEL);
    }
}
```

Explanations
------------
BELLATRIX gives you some methods for handling dialogs.
```java
app().dialogs().handle();
```
You can click on the OK button and handle the alert.
```java
app().dialogs().handle(a -> Assert.assertEquals(a.getText(), "Try the coupon- happybirthday"));
```
You can pass an anonymous lambda function and do something with the alert.
```java
app().dialogs().handle(DialogButton.CANCEL);
```
You can tell the dialog service to click a different button.