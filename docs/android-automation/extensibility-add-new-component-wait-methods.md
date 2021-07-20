---
layout: default
title:  "Extensibility – Add New Component Wait Methods"
excerpt: "Learn how to extend BELLATRIX adding new component wait methods."
date:   2018-06-23 06:50:17 +0200
parent: android-automation
permalink: /android-automation/extensibility-add-new-component-wait-methods/
anchors:
  introduction: Introduction
  example: Example
  usage: Usage
---
Introduction
------------
Imagine that you want to wait for an element to have a specific content. First, you need to create a new 'WaitStrategy' class that inheriting the **WaitStrategy** class.

Example
-------
```java
public class ToHaveSpecificContentWaitStrategy extends WaitStrategy {
    public final String elementContent;

    public ToHaveSpecificContentWaitStrategy(String elementContent) {
        timeoutInterval = ConfigurationService.get(AndroidSettings.class).getTimeoutSettings().getElementToBeClickableTimeout();
        sleepInterval = ConfigurationService.get(AndroidSettings.class).getTimeoutSettings().getSleepInterval();
        this.elementContent = elementContent;
    }

    public static ToHaveSpecificContentWaitStrategy of(String elementContent) {
        return new ToHaveSpecificContentWaitStrategy(elementContent);
    }

    @Override
    public void waitUntil(SearchContext searchContext) {
        Function<WebDriver, Boolean> func = (w) -> componentHasContent(DriverService.getWrappedAndroidDriver(), findStrategy);
        waitUntil(func);
    }

    private <TFindStrategy extends FindStrategy> Boolean componentHasContent(AndroidDriver<MobileElement> searchContext, TFindStrategy findStrategy) {
        try {
            var element = findStrategy.findElement(searchContext);
            return element.getText() == elementContent;
        }
        catch (Exception e) {
            return false;
        }
    }
}
```
Find the element and check the current value in the style attribute. The internal **waitUntil** will wait until the value changes in the specified time.

Usage
------------
```java
public class NewElementWaitTests extends WebTest {
    @Test
    public void promotionsPageOpened_When_PromotionsButtonClicked() {
        app().navigate().to("http://demos.bellatrix.solutions/");

        var button = app().create().byIdContaining(Button.class, "button");
        
        button.ensureState(ToHaveSpecificContentWaitStrategy.of("button"));
        button.click();
    }
}
```
After that, you can use the new wait method as shown in the example:
```java
var button = app().create().byIdContaining(Button.class, "button");
button.ensureState(ToHaveSpecificContentWaitStrategy.of("button"));
```
