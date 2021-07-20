---
layout: default
title:  "Extensibility – Component Action Hooks"
excerpt: "Learn how to extend BELLATRIX Android component controls using component action hooks."
date:   2018-06-23 06:50:17 +0200
parent: android-automation
permalink: /android-automation/extensibility-component-action-hooks/
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
    }
}
```
```java
public class ElementActionHooksTests extends AndroidTest {
    @Override
    public void configure() {
        super.configure();
        LoggingButtonEvents.addEventListeners();
    }

    @Test
    public void buttonClicked_When_CallClickMethod() {
        var button = app().create().byIdContaining(Button.class, "button");

        button.click();
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

Each BELLATRIX Ensure method gives you a hook too. To implement them you can derive the **EnsureExtensionsEventHandlers** base class and override the event handler methods you need. For example for the method **EnsureIsChecked**, **EnsuredIsCheckedEvent** event is called after the check is done. -->