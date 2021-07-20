---
layout: default
title:  "Web Service"
excerpt: "Learn how to use BELLATRIX android web service."
date:   2018-10-22 06:50:17 +0200
parent: android-automation
permalink: /android-automation/web-service/
anchors:
  example: Example
  explanations: Explanations
---
Example
-------
```java
@ExecutionApp(isMobileWebTest = true)
public class TouchActionsServiceTests extends AndroidTest {
    @Test
    public void htmlSourceContainsShop_When_OpenWebPageWithChrome() {
        app().web().navigate().to("http://demos.bellatrix.solutions/");
        Assert.assertTrue(app().web().browser().getPageSource().contains("Shop"));
    }
}
```

Explanations
------------
```java
@ExecutionApp(isMobileWebTest = true)
public class TouchActionsServiceTests extends AndroidTest
```
To test web apps, you can start Chrome browser by setting the **ExecutionApp** annotation's **isMobileWebTest** property to *true*.
```java
app().web().navigate().to("http://demos.bellatrix.solutions/");
```
Opens Chrome browser and navigates to the mentioned page.
```java
Assert.assertTrue(app().web().browser().getPageSource().contains("Shop"));
```
Through the **web** method you can access almost all BELLATRIX Web APIs.