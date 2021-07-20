---
layout: default
title:  "Troubleshooting – Full Page Screenshots on Fail"
excerpt: "Learn how to generate full page screenshots on test's fail."
date:   2018-06-22 06:50:17 +0200
parent: web-automation
permalink: /web-automation/troubleshooting-full-page-screenshots-on-fail/
anchors:
  example: Example
  explanations: Explanations
  configuration: Configuration
---
Example
-------
```java
public class FullPageScreenshotsOnFailTests extends WebTest {
    @Test
    public void promotionsPageOpened_When_PromotionsButtonClicked() {
        app().navigate().to("http://demos.bellatrix.solutions/");
        var promotionsLink = app().create().byLinkText(Anchor.class, "Promotions");
        promotionsLink.click();
    }
}
```

Explanations
------------
The engine checks after each test, its result, if failed, makes the screenshots. We have a unique engine for the screenshots. We do not use vanilla **WebDriver**. If you use the **WebDriver** method, it makes a screenshot only of the visible part of the page. If you have to do it manually precisely, you need thousands of lines of code.

Configuration
-------------
If you open the **testFrameworkSettings.\<env\>.json** file, you find the screenshot properties under **webSettings** section that controls this behaviour.
```json
"webSettings": {
    "screenshotsOnFailEnabled": "true",
    "screenshotsSaveLocation": "user.home/BELLATRIX/Screenshots"
}
```
You can turn off the making of screenshots for all tests and specify where the screenshots to be saved. In the extensibility chapters read more about how you can create different screenshots engine or change the saving strategy.