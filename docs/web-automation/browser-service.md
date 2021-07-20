---
layout: default
title:  "Browser Service"
excerpt: "Learn how to use BELLATRIX browser service."
date:   2018-06-22 06:50:17 +0200
parent: web-automation
permalink: /web-automation/browser-service/
anchors:
  example: Example
  explanations: Explanations
---
Example
-------
```java
public class BrowserServiceTests extends WebTest {
    @Test
    public void getCurrentUrl() {
        app().navigate().to("http://demos.bellatrix.solutions/");

        System.out.println(app().browser().getUrl());
    }

    @Test
    public void controlBrowser() {
        pp().navigate().to("http://demos.bellatrix.solutions/");

        app().browser().maximize();
        
        app().browser().back();

        app().browser().forward();

        app().browser().refresh();
    }

    @Test
    public void getTabTitle() {
        app().navigate().to("http://demos.bellatrix.solutions/");

        Assert.assertEquals(app().browser().getTitle(), "BELLATRIX Java test automation framework",);
    }

    @Test
    public void printCurrentPageHtml() {
        app().navigate().to("http://demos.bellatrix.solutions/");

        System.out.println(app().browser().getPageSource());
    }

    @Test
    @Ignore
    public void switchToFrame() {
        app().navigate().to("http://demos.bellatrix.solutions/");

        var frame = app().create().byId(Frame.class, "myFrameId");
        app().browser().switchToFrame(frame);

        var myButton = frame.createById(Button.class, "purchaseBtnId");

        myButton.click();

        app().browser().switchToDefault();
    }
}
```

Explanations
------------
BELLATRIX gives you an interface to most common operations for controlling the started browser through the **browser** method.
```java
app().browser().waitUntilPageLoadsCompletely();
```
Sometimes, some AJAX async calls are not caught natively by **WebDriver**. So you can use the BELLATRIX browser service's method **waitUntilPageLoadsCompletely** which waits for these calls automatically to finish. Keep in mind that usually this is not necessary since BELLATRIX has a complex built-in mechanism for handling element waits.
```java
System.out.println(app().browser().getUrl());
```
Get the current tab URL.
```java
app().browser().maximize();
```
Maximizes the browser.
```java
app().browser().back();
```
Simulates clicking the browser's back button.
```java
app().browser().forward();
```
Simulates clicking the browser's forward button.
```java
app().browser().refresh();
```
Simulates clicking the browser's refresh button.
```java
Assert.assertEquals(app().browser().getTitle(), "BELLATRIX Java test automation framework");
```
Get the current tab title.
```java
System.out.println(app().browser().getPageSource());
```
Get the current page's HTML source.
```java
var frame = app().create().byId(Frame.class, "myFrameId");
app().browser().switchToFrame(frame);
var myButton = frame.createById(Button.class, "purchaseBtnId");
```
To work with elements inside a frame, you should switch to it first. Search for the button inside the frame element. Of course, once you switched to frame, you can create the element through a **create().by** method too.
```java
app().browser().switchToDefault();
```
To continue searching in the whole page, you need to switch to default again. It is the same process of how you work with **WebDriver**.