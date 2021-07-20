---
layout: default
title:  "Touch Actions Service"
excerpt: "Learn how to use BELLATRIX android touch actions service."
date:   2018-06-22 06:50:17 +0200
parent: android-automation
permalink: /android-automation/touch-actions-service/
anchors:
  example: Example
  explanations: Explanations
---
Example
-------
```java
public class TouchActionsServiceTests extends AndroidTest {
    @Test
    public void elementSwiped_When_CallSwipeByCoordinatesMethod() {
        app().appService().startActivity("com.example.android.apis", ".graphics.FingerPaint");

        var textField = app().create().byIdContaining(TextField.class, "content");
        Point point = textField.getLocation();
        Dimension size = textField.getSize();

        app().touch().swipe(
                point.getX() + size.getWidth() - 5,
                point.getY() + 5,
                point.getX() + 5,
                point.getY() + size.getHeight() - 5,
                2000);
    }

    @Test
    public void elementTaped_When_CallTap() {
        app().appService().startActivity("com.example.android.apis", ".ApiDemos");

        var elements = app().create().allByClass(TextField.class, "android.widget.TextView");
        int initialCount = elements.size();

        app().touch().tap(elements.get(4), 10).perform();

        elements = app().create().allByClass(TextField.class, "android.widget.TextView");

        Assert.assertNotEquals(elements.size(), initialCount);
        Assert.assertEquals(elements.size(), 1);
    }

    @Test
    public void elementSwiped_When_CallPressWaitMoveToAndReleaseByCoordinates() {
        app().appService().startActivity("com.example.android.apis", ".ApiDemos");

        var elements = app().create().allByClass(TextField.class, "android.widget.TextView");
        var locationOne = elements.get(7).getLocation();
        var locationTwo = elements.get(1).getLocation();

        app().touch().press(locationOne.getX(), locationOne.getY())
                .moveTo(locationTwo.getX(), locationTwo.getY())
                .release()
                .perform();

        elements = app().create().allByClass(TextField.class, "android.widget.TextView");

        Assert.assertNotEquals(elements.get(1).getLocation().getY(), elements.get(7).getLocation().getY());
    }
}
```

Explanations
------------
BELLATRIX gives you an interface for easier work with touch actions through TouchActionsService. Performing a series of touch actions can be one of the most complicated jobs in automating mobile apps. BELLATRIX touch APIs are simplified and made to be user-friendly as possible. Their usage can eliminate lots of code duplication and boilerplate code.
```java
app().touch().swipe(
        point.getX() + size.getWidth() - 5,
        point.getY() + 5,
        point.getX() + 5,
        point.getY() + size.getHeight() - 5,
        2000);
```
Performs swipe by using coordinates.
```java
app().touch().tap(elements.get(4), 10).perform();
```
Tap 10 times using BELLATRIX UI element directly.
```java
app().touch().press(locationOne.getX(), locationOne.getY())
        .moveTo(locationTwo.getX(), locationTwo.getY())
        .release()
        .perform();
```
Performs a series of actions using elements coordinates.