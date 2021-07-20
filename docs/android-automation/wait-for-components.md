---
layout: default
title:  "Wait for Components"
excerpt: "Learn how to wait for Android elements with BELLATRIX android module."
date:   2018-10-20 06:50:17 +0200
parent: android-automation
permalink: /android-automation/wait-for-components/
anchors:
  example: Example
  explanations: Explanations
---
Example
-------
```java
@Test
public void buttonClicked_When_ClickMethodCalled() {
    var button = app().create().byIdContaining(Button.class, "button");
    ((Button)button.toBeClickable().toBeVisible()).click();

    button.toBeClickable().toBeVisible().waitToBe();
}
```

Explanations
------------
```java
var button = app().create().byIdContaining(Button.class, "button");
((Button)button.toBeClickable().toBeVisible()).click();
```
Besides the **toBe** methods that you can use on component creation, you have a couple of other options if you need to wait for components. For example, if you want to reuse your component in multiple tests or if you use it through page objects (more about that in later chapters), you may not want to wait for all conditions to be executed every time. Sometimes the mentioned conditions during creation may not be correct for some specific test case. E.g. button wait to be disabled, but in most cases, you need to wait for it to be enabled. To give you more options BELLATRIX has a special method called **waitToBe**. The big difference compared to **toBe** methods is that it forces BELLATRIX to locate your component immediately and wait for the condition to be satisfied.
```java
var button = app().create().byIdContaining(Button.class, "button");
button.toBeClickable().toBeVisible().waitToBe();
```
Why we have two syntaxes for almost the same thing? Because sometimes you do not need to perform an action or assertion against the component. In the above example, statement waits for the button to be clickable and visible before the click. However, in some cases, you want some component to show up but not act on it. This means the above syntax does not help you since the component is not searched in the DOM at all because of the lazy loading. Using the **waitToBe** method forces BELLATRIX to locate your component and wait for the condition without the need to do an action or assertion.