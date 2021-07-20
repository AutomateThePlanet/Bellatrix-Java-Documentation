---
layout: default
title:  "Extensibility – Add New Component Wait Methods"
excerpt: "Learn how to extend BELLATRIX adding new component wait methods."
date:   2018-06-23 06:50:17 +0200
parent: web-automation
permalink: /web-automation/extensibility-add-new-component-wait-methods/
anchors:
  introduction: Introduction
  example: Example
  usage: Usage
---
Introduction
------------
Imagine that you want to wait for an element to have a specific style. First, you need to create a new 'WaitStrategy' class that inheriting the **WaitStrategy** class.

Example
-------
```java
public class ToHaveStyleWaitStrategy extends WaitStrategy {
    public final String elementStyle;

    public ToHaveStyleWaitStrategy(String elementStyle) {
        timeoutInterval = ConfigurationService.get(WebSettings.class).getTimeoutSettings().getElementToBeClickableTimeout();
        sleepInterval = ConfigurationService.get(WebSettings.class).getTimeoutSettings().getSleepInterval();
        this.elementStyle = elementStyle;
    }

    public static ToHaveStyleWaitStrategy of(String elementStyle) {
        return new ToHaveStyleWaitStrategy(elementStyle);
    }

    @Override
    public void waitUntil(SearchContext searchContext, By by) {
        waitUntil((x) -> elementHasStyle(searchContext, by));
    }

    private boolean elementHasStyle(SearchContext searchContext, By by) {
        var element = findElement(searchContext, by);
        try {
            return element != null && element.getAttribute("style").equals(elementStyle);
        } catch (StaleElementReferenceException | NoSuchElementException e) {
            return false;
        }
    }
}
```
Find the element and check the current value in the style attribute. The internal **waitUntil** will wait until the value changes in the specified time.

<!-- The next and final step is to create an extension method for all UI elements.

```csharp
public static class UntilElementsExtensions
{
    public static TElementType ToHasSpecificStyle<TElementType>(this TElementType element, string style, int? timeoutInterval = null, int? sleepInterval = null)
        where TElementType : Element
    {
        var until = new WaitToHasStyleStrategy(style, timeoutInterval, sleepInterval);
        element.ValidateState(until);
        return element;
    }
}
```
After UntilHasStyle is created, it is important to be passed on to the element’s EnsureState method. -->

Usage
------------
```java
public class NewElementWaitTests extends WebTest {
    @Test
    public void promotionsPageOpened_When_PromotionsButtonClicked() {
        app().navigate().to("http://demos.bellatrix.solutions/");

        var promotionsLink = app().create().byLinkText(Anchor.class, "promo");

        promotionsLink.ensureState(ToHaveStyleWaitStrategy.of("padding: 1.618em 1em"));
        promotionsLink.click();
    }
}
```
After that, you can use the new wait method as shown in the example:
```java
var promotionsLink = app().create().byLinkText(Anchor.class, "promo");
promotionsLink.ensureState(ToHaveStyleWaitStrategy.of("padding: 1.618em 1em"));
```