---
layout: default
title:  "Extensibility – Add New Find Locators"
excerpt: "Learn how to extend BELLATRIX adding new custom find locators."
date:   2018-10-23 06:50:17 +0200
parent: web-automation
permalink: /web-automation/extensibility-add-new-find-locators/
anchors:
  introduction: Introduction
  example: Example
  usage: Usage
---
Introduction
------------
Imagine that you want to create a new locator for finding all components with ID starting with specific value. First, you need to create a new 'FindStrategy' class.

Example
-------
```java
public class IdStartingWithFindStrategy extends FindStrategy {
    public IdStartingWithFindStrategy(String value) {
        super(value);
    }

    @Override
    public By convert() {
        return By.cssSelector(String.format("[id^='%s']", getValue()));
    }

    @Override
    public String toString() {
        return String.format("id starting with %s", getValue());
    }
}
```
In the **convert** method, we use a standard WebDriver **By** locator, and in this case we implement our requirements through a little CSS.

<!-- To ease the usage of the locator, we need to create methods in ComponentCreateService and WebComponent classes.

```csharp
public static class ElementCreateExtensions
{
    public static ElementsList<TElement> CreateAllByIdStartingWith<TElement>(this Element element, string idEnding)
        where TElement : Element => new ElementsList<TElement>(new FindIdStartingWithStrategy(idEnding), element.WrappedElement);
}
```

```csharp
public static class ElementRepositoryExtensions
{
    public static TElement CreateByIdStartingWith<TElement>(this ElementCreateService repository, string idPrefix, bool shouldCache = false)
        where TElement : Element => repository.Create<TElement, FindIdStartingWithStrategy>(new FindIdStartingWithStrategy(idPrefix), shouldCache);

    public static ElementsList<TElement> CreateAllByIdStartingWith<TElement>(this ElementCreateService repository, string idPrefix)
        where TElement : Element => new ElementsList<TElement>(new FindIdStartingWithStrategy(idPrefix), null);
}
``` -->

Usage
------------
```java
public class NewFindLocatorsTests extends WebTest {
    @Test
    public void promotionsPageOpened_When_PromotionsButtonClicked() {
        app().navigate().to("http://demos.bellatrix.solutions/");

        var promotionsLink = app().create().by(Anchor.class, new IdStartingWithFindStrategy("promo"));

        promotionsLink.click();
    }
}
```
You can use the new **FindStrategy** in the default **create().by** and **create().allBy** methods like in this example:
```java
var promotionsLink = app().create().by(Anchor.class, new IdStartingWithFindStrategy("promo"));
```