---
layout: default
title:  "Locate And Wait for Components"
excerpt: "Learn how to locate and wait for web components with BELLATRIX web module."
date:   2018-06-22 06:50:17 +0200
parent: web-automation
permalink: /web-automation/locate-and-wait-for-components/
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
public void blogPageOpened_When_PromotionsButtonClicked() {
    app().navigate().to("http://demos.bellatrix.solutions/");

    Anchor blogLink = app().create().byLinkText(Anchor.class, "Blog").toBeClickable().toBeVisible();

    blogLink.click();

    Anchor promotionsLink = app().create().byLinkText(Anchor.class, "Promotions").toExist();

    promotionsLink.click();
}
```

Explanations
------------
```java
Anchor blogLink = app().create().byLinkText(Anchor.class, "Blog").toBeClickable().toBeVisible();
```
Sometimes you need to perform an action against an component only when a specific condition is true. As mentioned in previous part of the guide, BELLATRIX by default always waits for components to exist.
However, sometimes this may not be enough. For example, you may want to click on a button once it is clickable.
It may be disabled at the beginning of the tests because some validation is not met. Your test fulfill the initial condition and if you use vanilla **WebDriver** the test most probably fails because **WebDriver** clicks too fast before your button is enabled by your code. So we created additional syntax sugar methods to help you deal with this. You can use component "**toBe**" methods after the **create().by** and **create().allBy** methods.
As you can see in the example below you can chain multiple of this methods.

**Note**: *Since BELLATRIX, components creation logic is lazy loading as mentioned before, BELLATRIX waits for the conditions to be True on the first action you perform with the component.*

**Note**: *Keep in mind that with this syntax these conditions are checked every time you perform an action with the component. Which can lead tо small execution delays.*

All Available toBe Methods
--------------------------
### toExist ###
```java
app().create().byLinkText(Anchor.class, "Blog").toExist();
```
Waits for the component to exist on the page. BELLATRIX always does it by default. But if use another ToBe methods you need to add it again since you have to override the default behaviour.
### toBeVisible ###
```java
app().create().byLinkText(Anchor.class, "Blog").toBeVisible();
```
Waits for the component to be visible.
### toBeClickable ###
```java
app().create().byLinkText(Anchor.class, "Blog").toBeClickable();
```
Waits for the component to be clickable (may be disabled at first).
### toBeDisabled ###
```java
app().create().byLinkText(Anchor.class, "Blog").toBeDisabled();
```
Waits for the component to be disabled (may be clickable at first).
### toHaveContent ###
```java
app().create().byLinkText(Anchor.class, "Blog").toBeDisabled();
```
Waits for the component to have some content in it. For example, some validation DIV or label.
### toNotBeVisible ###
```java
app().create().byLinkText(Anchor.class, "Blog").toNotBeVisible();
```
Waits for the component to be invisible.
### toNotExist ###
```java
app().create().byLinkText(Anchor.class, "Blog").toNotExist();
```
Waits for the component to disappear. Usually, we use in assertion methods.

Configuration
-------------
The default timeouts that BELLATRIX uses are placed inside the **testFrameworkSettings.\<env\>.json** file, mentioned in 'Folder and File Structure'. Inside it, is the **timeoutSettings** section. All values are in seconds.
```json
"timeoutSettings": {
  "elementWaitTimeout": "30",
  "pageLoadTimeout": "30",
  "scriptTimeout": "1",
  "waitForAjaxTimeout": "30",
  "sleepInterval": "1",
  "waitUntilReadyTimeout": "30",
  "waitForJavaScriptAnimationsTimeout": "30",
  "waitForAngularTimeout": "30",
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
