---
layout: default
title:  "Wait for Components"
excerpt: "Learn how to wait for web components with BELLATRIX web module."
date:   2018-06-22 06:50:17 +0200
parent: web-automation
permalink: /web-automation/wait-for-components/
anchors:
  example: Example
  explanations: Explanations
---
Example
-------
```java
@Test
public void blogPageOpened_When_PromotionsButtonClicked() {
    app().navigate().to("http://demos.bellatrix.solutions/");

    var blogLink = app().create().byLinkText(Anchor.class, "Blog");
    ((Anchor)blogLink.toBeClickable().toBeVisible()).click();

    blogLink.toBeClickable().toBeVisible().waitToBe();
}
```

Explanations
------------
```java
var blogLink = app().create().byLinkText(Anchor.class, "Blog");
((Anchor)blogLink.toBeClickable().toBeVisible()).click();
```
Besides the ToBe methods that you can use on element creation, you have a couple of other options if you need to wait for elements. For example, if you want to reuse your element in multiple tests or if you use it through page objects (more about that in later chapters), you may not want to wait for all conditions to be executed every time. Sometimes the mentioned conditions during creation may not be correct for some specific test case. E.g. button wait to be disabled, but in most cases, you need to wait for it to be enabled. To give you more options BELLATRIX has a special method called **waitToBe**. The big difference compared to ToBe methods is that it forces BELLATRIX to locate your element immediately and wait for the condition to be satisfied.
This is also valid syntax the conditions are performed once the **click** method is called. It is the same as placing **toBe** methods after **create().byLinkText**.
```java
blogLink.toBeClickable().toBeVisible().waitToBe();
```
Why we have two syntaxes for almost the same thing? Because sometimes you do not need to perform an action or assertion against the element. In the above example, statement waits for the button to be clickable and visible before the click. However, in some cases, you want some element to show up but not act on it. This means the above syntax does not help you since the element is not searched in the DOM at all because of the lazy loading.
Using the **waitToBe** method forces BELLATRIX to locate your element and wait for the condition without the need to do an action or assertion.