---
layout: default
title:  "Locate And Wait for Components"
excerpt: "Learn how to locate and wait for Android components with BELLATRIX android module."
date:   2018-10-20 06:50:17 +0200
parent: android-automation
permalink: /android-automation/locate-and-wait-for-components/
anchors:
  example: Example
  explanations: Explanations
  all-available-tobe-methods: All Available toBe Methods
  configuration: Configuration
---
Example
-------
```java
@Test
public void buttonClicked_When_ClickMethodCalled() {
    Button button = app().create().byIdContaining(Button.class, "button").toBeClickable().toBeVisible();

    button.click();

    RadioButton radioButton = app().create().byIdContaining(RadioButton.class, "radio2").toExist();

    radioButton.click();
}
```

Explanations
------------
```java
Button button = app().create().byIdContaining(Button.class, "button").toBeClickable().toBeVisible();
```
Sometimes you need to perform an action against an component only when a specific condition is true. As mentioned in previous part of the guide, BELLATRIX by default always waits for components to exist. However, sometimes this may not be enough. For example, you may want to click on a button once it is clickable. It may be disabled at the beginning of the tests because some validation is not met. Your test fulfill the initial condition and if you use vanilla **WebDriver** the test most probably fails because **WebDriver** clicks too fast before your button is enabled by your code. So we created additional syntax sugar methods to help you deal with this. You can use element "**toBe**" methods after the **create().by** and **create().allBy** methods. As you can see in the example below you can chain multiple of this methods.

**Note**: *Since BELLATRIX's component creation logic is lazy loading as mentioned above, BELLATRIX waits for the conditions to be true on the first action you perform with the component.*

**Note**: *Keep in mind that with this syntax these conditions are checked every time you perform an action with the component. Which can lead tо small execution delays.*

All Available toBe Methods
--------------------------
### toExist ###
```java
app().create().byName(Button.class, "Purchase").toExist();
```
Waits for the component to exist on the page. BELLATRIX always does it by default. But if use another **toBe** methods you need to add it again since you have to override the default behaviour.
### toBeVisible ###
```java
app().create().byName(Button.class, "Purchase").toBeVisible();
```
Waits for the component to be visible.
### toBeClickable ###
```java
app().create().byName(Button.class, "Purchase").toBeClickable();
```
Waits for the component to be clickable (may be disabled at first).

Configuration
-------------
The default timeouts that BELLATRIX use are placed inside the **testFrameworkSettings.\<env\>.json** file in the resources folder. Inside it, there's a the **timeoutSettings** section. All values are in seconds.
```json
"timeoutSettings": {
  "implicitWaitTimeout": "5",
  "elementWaitTimeout": "30",
  "sleepInterval": "1",
  "waitForPartialUrl": "30",
  "validationsTimeout": "30",
  "elementToBeVisibleTimeout": "30",
  "elementToExistTimeout": "30",
  "elementToNotExistTimeout": "30",
  "elementToBeClickableTimeout": "30",
  "elementNotToBeVisibleTimeout": "30",
  "elementToHaveContentTimeout": "15"
}
```