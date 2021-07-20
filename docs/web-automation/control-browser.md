---
layout: default
title:  "Control Browser"
excerpt: "Learn how to control browsers with BELLATRIX web module."
date:   2021-04-01 16:56:55 +0200
parent: web-automation
permalink: /web-automation/control-browser/
anchors:
  overview: Overview
  explanations: Explanations
---
Overview
--------

This is how one BELLATRIX test class looks like.
```java
@ExecutionBrowser(browser = Browser.FIREFOX, lifecycle = Lifecycle.REUSE_IF_STARTED)
public class ProductPurchaseTests extends WebTest {
    @Test
    public void promotionsPageOpened_When_PromotionsButtonClicked() {
        app().navigate().to("http://demos.bellatrix.solutions/");

        var promotionsLink = app().create().byLinkText(Anchor.class, "Promotions");

        promotionsLink.click();
    }

    @Test
    public void blogPageOpened_When_PromotionsButtonClicked() {
        app().navigate().to("http://demos.bellatrix.solutions/");

        var blogLink = app().create().byLinkText(Anchor.class, "Blog");

        blogLink.click();
    }
}
```

Explanations
------------
```java
@ExecutionBrowser(browser = Browser.FIREFOX, lifecycle = Lifecycle.REUSE_IF_STARTED)
```
This is the attribute for automatic start/control of WebDriver browsers by BELLATRIX. If you have to do it manually properly, you will need thousands of lines of code. 
**Browser** controls which browser is used. Available options are:
- Chrome
- Firefox
- Edge
- Opera
- Safari
- Internet Explorer
- Chrome in headless mode
- Edge in headless mode
- Firefox in headless mode.

**Note**: *Headless mode = executed in the browser but the browser's UI is not rendered, in theory, should be faster. In practice the time gain is little.*

**Lifecycle** enum controls when the browser is started and stopped. This can drastically increase or decrease the tests execution time, depending on your needs. However you need to be careful because in case of tests failures the browser may need to be restarted.
Available options:
- **RESTART_EVERY_TIME** – for each test a separate WebDriver instance is created and the previous browser is closed. The new browser comes with new cookies and cache.
- **RESTART_ON_FAIL** – the browser is only restarted if the previous test failed. Alternatively, if the previous test's browser was different.
- **REUSE_IF_STARTED** – the browser is only restarted if the previous test's browser was different. In all other cases, the browser is reused if possible.

**Note**: *However, use this option with caution since in some rare cases if you have not properly setup your tests you may need to restart the browser if the test fails otherwise all other tests may fail too.*

```java
public class ProductPurchaseTests extends WebTest
```
All web BELLATRIX test classes should inherit from the WebTest base class. This way you can use all built-in BELLATRIX tools and functionalities.
```java
@ExecutionBrowser(browser = Browser.FIREFOX, lifecycle = Lifecycle.REUSE_IF_STARTED)
public class ProductPurchaseTests extends WebTest
```
If you place attribute over the class all tests inherit the Lifecycle. It is possible to place it over each test and this way it overrides the class Lifecycle only for this particular test.
```java
@Test
public void promotionsPageOpened_When_PromotionsButtonClicked()
```
All TestNG/JUnit tests should be marked with the **Test** annotation.
```java
app().navigate().to("http://demos.bellatrix.solutions/");
```
There is more about the app class in the next sections. However, it is the primary point where you access the BELLATRIX services. It comes from the **WebTest** class as a property. Here we use the BELLATRIX navigation service to navigate to the demo page.
```java
var promotionsLink = app().create().byLinkText(Anchor.class, "Promotions");
```
Use the element creation service to create an instance of the anchor. There are much more details about this process in the next sections.
```java
@Test
public void blogPageOpened_When_PromotionsButtonClicked() {
    app().navigate().to("http://demos.bellatrix.solutions/");

    var blogLink = app().create().byLinkText(Anchor.class, "Blog");

    blogLink.click();
}
```
As mentioned above you can override the browser Lifecycle for a particular test. The global Lifecycle for all tests in the class is to reuse an instance of Firefox browser. Only for this particular test, BELLATRIX opens Chrome and restarts it only on fail.
