---
layout: default
title:  "Cookies Service"
excerpt: "Learn how to use BELLATRIX cookies service."
date:   2018-06-22 06:50:17 +0200
parent: web-automation
permalink: /web-automation/cookies-service/
anchors:
  example: Example
  explanations: Explanations
---
Example
-------
```java
public class CookiesServiceTests extends WebTest
{
    @Test
    public void getAllCookies() {
        app().navigate().to("http://demos.bellatrix.solutions/welcome/");

        app().cookies().addCookie("woocommerce_items_in_cart1", "3");
        app().cookies().addCookie("woocommerce_items_in_cart2", "3");
        app().cookies().addCookie("woocommerce_items_in_cart3", "3");
        
        var cookies = app().cookies().getAllCookies();
        
        Assert.assertEquals(cookies.size(), 3);
    }

    @Test
    public void getSpecificCookie() {
        app().navigate().to("http://demos.bellatrix.solutions/welcome/");

        app().cookies().addCookie("woocommerce_items_in_cart", "3");

        var itemsInCartCookie = app().cookies().getCookie("woocommerce_items_in_cart");

        Assert.assertEquals(itemsInCartCookie.getValue(), "3");
    }

    @Test
    public void deleteAllCookies() {
        app().navigate().to("http://demos.bellatrix.solutions/welcome/");

        var protonRocketAddToCartBtn = app().create().allByInnerTextContaining(Anchor.class, "Add to cart").stream().findFirst().orElse(null);
        protonRocketAddToCartBtn.click();

        app().cookies().deleteAllCookies();
    }

    @Test
    public void deleteSpecificCookie() {
        app().navigate().to("http://demos.bellatrix.solutions/welcome/");

        var protonRocketAddToCartBtn = app().create().allByInnerTextContaining(Anchor.class, "Add to cart").stream().findFirst().orElse(null);
        protonRocketAddToCartBtn.click();

        app().cookies().deleteCookie("woocommerce_items_in_cart");
    }

    @Test
    public void addNewCookie() {
        app().navigate().to("http://demos.bellatrix.solutions/welcome/");

        app().cookies().addCookie("woocommerce_items_in_cart", "3");
    }
}
```

Explanations
------------
BELLATRIX gives you an interface for easier work with cookies using the **cookies** method. You need to make sure that you have navigated to the desired web page.
```java
var cookies = app().cookies().getAllCookies();
```
Get all cookies.
```java
var itemsInCartCookie = app().cookies().getCookie("woocommerce_items_in_cart");
```
Get a specific cookie by name.
```java
app().cookies().deleteAllCookies();
```
Delete all cookies.
```java
app().cookies().deleteCookie("woocommerce_items_in_cart");
```
Delete a specific cookie by name.
```java
app().cookies().addCookie("woocommerce_items_in_cart", "3");
```
Add a new cookie.