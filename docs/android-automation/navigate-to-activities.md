---
layout: default
title:  "Navigate to Activities"
excerpt: "Learn how to navigate to Android activities with BELLATRIX android module."
date:   2018-10-20 06:50:17 +0200
parent: android-automation
permalink: /android-automation/navigate-to-activities/
anchors:
  example: Example
  explanations: Explanations
---
Example
-------
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
}
```

Explanations
------------

```java
@ExecutionApp(appPath = "user.home/ApiDemos.apk",
        androidVersion = "7.1",
        deviceName = "android25-test",
        appPackage = "com.example.android.apis",
        appActivity = ".view.Controls1",
        lifecycle = Lifecycle.REUSE_IF_STARTED)
```
Depending on the types of tests you want to write there are a couple of ways to navigate to а specific activity.
If you use the Android attribute the first time the app is started it navigates to the specified activity.
```java
app().appService().startActivity("com.example.android.apis", ".view.Controls1");
```
You can always navigate in each separate tests, but if all of them open the same activity, you can use the above techniques for code reuse.