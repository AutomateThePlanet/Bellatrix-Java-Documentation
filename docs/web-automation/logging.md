---
layout: default
title:  "Logging"
excerpt: "Learn how to use the BELLATRIX logging library."
date:   2018-06-23 06:50:17 +0200
parent: web-automation
permalink: /web-automation/logging/
anchors:
  example: Example
  explanations: Explanations
---
Example
-------
```java
public class LoggingTests extends WebTest {
    @Test
    public void addCustomMessagesToLog() {
        app().navigate().to("http://demos.bellatrix.solutions/");

        Select sortDropDown = app().create().byNameEndingWith(Select.class, "orderby");
        Anchor protonMReadMoreButton = app().create().byInnerTextContaining(Anchor.class, "Read more");
        Anchor addToCartFalcon9 = app().create().byAttributeContaining(Anchor.class, "data-product_id", "28").toBeClickable();
        Anchor viewCartButton = addToCartFalcon9.createByClassContaining(Anchor.class, "added_to_cart wc-forward").toBeClickable();

        sortDropDown.selectByText("Sort by price: low to high");
        protonMReadMoreButton.hover();

        Log.info("before adding Falcon 9 rocket to cart.");

        addToCartFalcon9.focus();
        addToCartFalcon9.click();
        viewCartButton.click();
    }
}
```

Explanations
------------
```java
Log.info("before adding Falcon 9 rocket to cart.");
```
Sometimes is useful to add information to the generated test log. To do it you can use the BELLATRIX built-in logger through the **Log** class's static methods.

Generated Log, as you can see the above custom message is added to the log.

```
Start Test
Class = LoggingTests Name = AddCustomMessagesToLog
selecting 'Sort by price: low to high' from Select (name ending with orderby)
hovering Anchor (text containing Read more)
before adding Falcon 9 rocket to cart.
focusing Anchor (data-product_id containing 28)
clicking Anchor (data-product_id containing 28)
clicking Anchor (class containing added_to_cart wc-forward)
```