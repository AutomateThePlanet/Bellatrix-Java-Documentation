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
  playwright: Playwright
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

Playwright
------------
Playwright auto-dismisses dialogs but in case you want to handle them yourself or if Playwright is unable to dismiss them on its own, there are slightly different methods you need to use.
```java
@Test
public void acceptDialogAlert()
{
    app().navigate().to("https://developer.mozilla.org/en-US/play");

    var clearButton = app().create().byXpath(Button.class, "//button[@id='clear']");

    app().dialogs().runAdnWaitForDialog(clearButton::click, 1000);

    app().dialogs().accept();
}
```

You pass the logic that will trigger a dialog to this **runAndWaitForDialog** method and you give timeout in milliseconds. In case you don't specify the timeout, it will set the timeout for **actionTimeoutWhenHandlingDialogs** from the config file.
If you don't use this method, no listener would be added and thus you won't be able to do the custom logic for the **Dialog**.

Available methods in this **DialogService** are:
```java
Dialog dialog = app().dialogs().runAdnWaitForDialog(() -> clearButton.click(), 1000);
```
Adds an Supplier<Dialog> to the Page which will listen for dialogs and eventually get the **Dialog** as soon as it appears, then performs the action you have specified which will trigger the event **OnDialog** and finally returns the **Dialog** object.
```java
app().dialogs().accept();
```
Accepts the dialog.
```java
app().dialogs().accept("prompt text");
```
Types the prompt text in the dialog and then accepts it.
```java
app().dialogs().dismiss();
```
Dismisses the dialog.
```java
String message = app().dialogs().getMessage();
```
Gets the dialog's message.