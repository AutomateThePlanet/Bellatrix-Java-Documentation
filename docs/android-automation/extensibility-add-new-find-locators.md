---
layout: default
title:  "Extensibility – Add New Find Locators"
excerpt: "Learn how to extend BELLATRIX adding new custom find locators."
date:   2018-10-23 06:50:17 +0200
parent: android-automation
permalink: /android-automation/extensibility-add-new-find-locators/
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
    public MobileBy convert() {
        return MobileBy.ByAndroidUIAutomator(String.format("new UiSelector().resourceIdMatches(\"%s.*\");", getValue()));
    }

    @Override
    public String toString() {
        return String.format("id starting with %s", getValue());
    }
}
```
In the **convert** method, we use a standard WebDriver **MobileBy** locator, and in this case we implement our requirements through Android UI Automator.

<!-- ```csharp
public static class ElementRepositoryExtensions
{
    public static TElement CreateByIdStartingWith<TElement>(this ElementCreateService repo, string id)
        where TElement : Element<AndroidDriver<AndroidElement>, AndroidElement> => repo.Create<TElement, FindIdStartingWithStrategy, AndroidDriver<AndroidElement>, AndroidElement>(new FindIdStartingWithStrategy(id));

    public static ElementsList<TElement, FindIdStartingWithStrategy, AndroidDriver<AndroidElement>, AndroidElement> CreateAllByIdStartingWith<TElement>(this ElementCreateService repo, string id)
        where TElement : Element<AndroidDriver<AndroidElement>, AndroidElement> => new ElementsList<TElement, FindIdStartingWithStrategy, AndroidDriver<AndroidElement>, AndroidElement>(new FindIdStartingWithStrategy(id), null);
}
```

```csharp
public static class ElementCreateExtensions
{
    public static TElement CreateByIdStartingWith<TElement>(this Element<AndroidDriver<AndroidElement>, AndroidElement> element, string id)
        where TElement : Element<AndroidDriver<AndroidElement>, AndroidElement> => element.Create<TElement, FindIdStartingWithStrategy>(new FindIdStartingWithStrategy(id));

    public static ElementsList<TElement, FindIdStartingWithStrategy, AndroidDriver<AndroidElement>, AndroidElement> CreateAllByIdStartingWith<TElement>(this Element<AndroidDriver<AndroidElement>, AndroidElement> element, string id)
        where TElement : Element<AndroidDriver<AndroidElement>, AndroidElement> => new ElementsList<TElement, FindIdStartingWithStrategy, AndroidDriver<AndroidElement>, AndroidElement>(new FindIdStartingWithStrategy(id), element.WrappedElement);
}
``` -->

Usage
------------
```java
public class AddNewFindLocatorsTests extends AndroidTest {
    @Test
    public void buttonClicked_When_CallClickMethod() {
        var button = app().create().by(Button.class, new IdStartingWithFindStrategy("button"));

        button.click();
    }
}
```
You can use the new **FindStrategy** in the default **create().by** and **create().allBy** methods like in this example:
```java
var button = app().create().by(Button.class, new IdStartingWithFindStrategy("button"));
```