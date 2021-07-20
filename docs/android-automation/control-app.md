---
layout: default
title:  "Control App"
excerpt: "Learn how to control Android applications with BELLATRIX android module."
date:   2018-10-20 06:50:17 +0200
parent: android-automation
permalink: /android-automation/control-app/
anchors:
  overview: Overview
  explanations: Explanations
---
Overview
--------

This is how one BELLATRIX test class looks like.
```java
@ExecutionApp(appPath = "user.home/ApiDemos.apk",
        androidVersion = "7.1",
        deviceName = "android25-test",
        appPackage = "com.example.android.apis",
        appActivity = ".view.Controls1",
        lifecycle = Lifecycle.REUSE_IF_STARTED)
public class BellatrixAppBehaviourTests extends AndroidTest {
    @Test
    public void buttonClicked_When_CallClickMethod() {
        app().appService().startActivity("com.example.android.apis", ".view.Controls1");

        var button = app().create().byIdContaining(Button.class, "button");

        button.click();
    }

    @Test
    public void returnsSave_When_GetText() {
        var button = app().create().byIdContaining(Button.class, "button");

        Assert.assertEquals(button.getText(), "Save");
    }
}
```

Explanations
------------
```java
@ExecutionApp(appPath = "user.home/ApiDemos.apk"
        androidVersion = "7.1",
        deviceName = "android25-test",
        appPackage = "com.example.android.apis",
        appActivity = ".view.Controls1",
        lifecycle = Lifecycle.REUSE_IF_STARTED)
```
This is the annotation for automatic start/control of Android apps by BELLATRIX. If you have to do it manually properly, you will need thousands of lines of code.  
**appPath** – sets the path where your application APK is.  
**lifecycle** – uses the **Lifecycle** enum to control when the app is started and stopped. This can drastically increase or decrease the tests execution time, depending on your needs.  
However you need to be careful because in case of tests failures the app may need to be restarted.
**Available options:**

- **RESTART_EVERY_TIME**- for each test a separate WebDriver instance is created and the previous app instance is
- **RESTART_ON_FAIL**- the app is only restarted if the previous test failed. Alternatively, if the previous test's app was different.
- **REUSE_IF_STARTED**- the app is only restarted if the previous test's app was different. In all other cases, the app is reused if possible.

**Note**: However, use this option with caution since in some rare cases if you have not properly setup your tests you may need to restart the app if the test fails otherwise all other tests may fail too.

There are even more things you can do with this attribute, but we look into them in the next sections.

```java
public class BellatrixAppBehaviourTests extends AndroidTest
```
All Android BELLATRIX test classes should inherit from the AndroidTest base class. This way you can use all built-in BELLATRIX tools and functionalities.
```java
@ExecutionApp(appPath = "user.home/ApiDemos.apk"
        androidVersion = "7.1",
        deviceName = "android25-test",
        appPackage = "com.example.android.apis",
        appActivity = ".view.Controls1",
        lifecycle = Lifecycle.REUSE_IF_STARTED)
public class BellatrixAppBehaviourTests extends AndroidTest
```
If you place attribute over the class all tests inherit the behaviour.
```java
@Test
public void buttonClicked_When_CallClickMethod()
```
All TestNG/JUnit tests should be marked with the Test annotation.
```java
var button = app().create().byIdContaining(Button.class, "button");
```
Use the element creation service to create an instance of the button. There are much more details about this process in the next sections.
