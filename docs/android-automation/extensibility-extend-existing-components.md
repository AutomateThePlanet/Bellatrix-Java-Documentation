---
layout: default
title:  "Extensability – Extend Existing Components"
excerpt: "Learn how to extend BELLATRIX Android components."
date:   2018-10-23 06:50:17 +0200
parent: android-automation
permalink: /android-automation/extensibility-extend-existing-components/
anchors:
  example: Example
  explanations: Explanations
---
Example
-------
```java
public class ExtendExistingElementWithChildElementsTests extends AndroidTest {
    @Test
    public void buttonClicked_When_CallClickMethod() {
        var button = app().create().byIdContaining(ExtendedButton.class, "button");

        button.submitButtonWithScroll();
    }
}
```

Explanations
------------
```java
public class ExtendedButton extends Button {
    public void submitButtonWithScroll()
        {
        this.toExist().toBeClickable().waitToBe();
        scrollToVisible();
        click();
    }
}
```
The way of extending an existing component is to create a child component. Extend the component you need. In this case, a new method is added to the standard **Button** component. Next in your tests, use the **ExtendedButton** instead of regular **Button** to have access to this method. The same strategy can be used to create a completely new component that BELLATRIX does not provide. You need to extend the AndroidComponent as a base class.
```java
var button = app().create().byIdContaining(ExtendedButton.class, "button");
```
Instead of the regular button, we create the **ExtendedButton**, this way we can use its new method.
```java
button.submitButtonWithScroll();
```
Use the new custom method provided by the **ExtendedButton** class.