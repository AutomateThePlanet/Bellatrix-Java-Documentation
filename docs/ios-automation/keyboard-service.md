---
layout: default
title:  "KeyboardService"
excerpt: "Learn how to use BELLATRIX iOS KeyboardService."
date:   2018-11-22 06:50:17 +0200
parent: ios-automation
permalink: /ios-automation/keyboard-service/
anchors:
  example: Example
  explanations: Explanations
---
Example
-------
```java
public class KeyboardServiceTests extends IOSTest {
    @Test
    public void testHideKeyBoard() {
        var textField = app().create().byIdContaining(TextField.class, "IntegerA");
        textField.setText("");

        app().keyboard().hide();
    }
}
```

Explanations
------------
BELLATRIX gives you an interface for easier work with device's keyboard through the **keyboard** method.
```java
app().keyboard().hide();
```
Hides the keyboard.