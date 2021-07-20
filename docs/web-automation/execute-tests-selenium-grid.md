---
layout: default
title:  "Execute Tests in a Grid"
excerpt: "Learn to use BELLATRIX to execute tests in a grid."
date:   2018-06-23 06:50:17 +0200
parent: web-automation
permalink: /web-automation/execute-tests-grid/
anchors:
  example: Example
  explanations: Explanations
  configuration: Configuration
---
Example
-------
```java
@ExecutionBrowser(browser = Browser.CHROME, browserVersion = 89, platform = Platform.WINDOWS, lifecycle = Lifecycle.REUSE_IF_STARTED)
public class SeleniumGridTests extends WebTest {
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
```java
@ExecutionBrowser(browser = Browser.CHROME, browserVersion = 89, platform = Platform.WINDOWS, lifecycle = Lifecycle.REUSE_IF_STARTED)
```
To use BELLATRIX tests with in a grid, you should use the ExecutionBrowser annotation to set the additional parameters for browser version and platform type. You can set other parameters for browser width and hight, lifecycle and etc. just like in every other test class.

Configuration
-------------
There's an option about execution type in the **testFrameworkSettings.\<env\>.json** file under the **webSettings** section.
```json
"webSettings": {
    "executionType": "grid"
}
```
You can set the grid URL and set some additional arguments under the **gridSettings** array, setting **providerName** to the match the name you set in **executionType**. To set additional arguments for providers such as SauceLabs, BrowserStack or CrossBrowserTesting, you can use the **arguments** array parameter.
```json
"gridSettings": [
    {
        "providerName" : "grid",
        "url" : "http://127.0.0.1:4444/wd/hub",
        "arguments" : [
            {
                "name" : "bellatrix_run"
            }
        ]
    }
]
```