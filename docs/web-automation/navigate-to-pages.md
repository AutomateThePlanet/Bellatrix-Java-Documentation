---
layout: default
title:  "Navigate to Pages"
excerpt: "Learn how to navigate to web pages with BELLATRIX web module."
date:   2018-06-22 06:50:17 +0200
parent: web-automation
permalink: /web-automation/navigate-to-pages/
anchors:
  example: Example
  explanations: Explanations
---
Example
-------
```java
public class ProductPurchaseTests extends WebTest {
    @Test
    public void promotionsPageOpened_When_PromotionsButtonClicked() {
        app().navigate().to("http://demos.bellatrix.solutions/");

        var promotionsLink = app().create().byLinkText(Anchor.class, "Promotions");

        promotionsLink.click();

        app().browser().waitUntilPageLoadsCompletely();
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
app().navigate().to("http://demos.bellatrix.solutions/");
```
You can always navigate in each separate tests, but if all of them go to the same page, you can use the above techniques for code reuse.
```java
app().browser().waitUntilPageLoadsCompletely();
```
Sometimes, some AJAX async calls are not caught natively by WebDriver. So you can use the BELLATRIX browser service's method. **waitUntilPageLoadsCompletely** which waits for these calls automatically to finish. Keep in mind that usually this is not necessary since BELLATRIX has a complex built-in mechanism for handling element waits.

Depending on the types of tests you want to write there are a couple of ways to navigate to specific pages.
In later chapters, there are more details about the different test workflow hooks. Find here two of them.
```java
@Override
public void beforeClass() {
    app().navigate().to("http://demos.bellatrix.solutions/");
}
```
If you reuse your browser and want to navigate once to a specific page. You can use the **beforeClass** method.
It executes once for all tests in the class.
```java
@Override
public void beforeMethod() {
    app().navigate().to("http://demos.bellatrix.solutions/");
}
```
If you need each test to navigate each time to the same page, you can use the **beforeMethod** method.
```java
app().navigate().waitForPartialUrl("/blog/");
```
Sometimes before proceeding with searching and making actions on the next page, we need to wait for something.
It is useful in some cases to wait for a partial URL instead hard-coding the whole URL since it can change depending on the environment. Keep in mind that usually this is not necessary since BELLATRIX has a complex built-in mechanism for handling element waits.
```java
app().navigate().toLocalPage("testPage.html");
```
Sometimes you may need to navigate to a local HTML file. Make sure to copy the file to the resources folder of the module with your tests files.