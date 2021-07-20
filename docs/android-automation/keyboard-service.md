---
layout: default
title:  "Keyboard Service"
excerpt: "Learn how to use BELLATRIX android keyboard service."
date:   2018-06-22 06:50:17 +0200
parent: android-automation
permalink: /android-automation/keyboard-service/
anchors:
  example: Example
  explanations: Explanations
---
Example
-------
```java
public class KeyboardServiceTests extends AndroidTest {
    @Test
    public void testHideKeyBoard() {
        var textField = app().create().byIdContaining(TextField.class, "left_text_edit");
        textField.setText("");

        app().keyboard().hide();
    }

    @Test
    public void pressKeyCodeTest() {
        app().keyboard().pressKey(AndroidKey.HOME);
    }

    @Test
    public void longPressKeyCodeTest() {
        app().keyboard().longPressKey(AndroidKey.HOME);
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
```java
app().keyboard().pressKey(AndroidKey.HOME);
```
Press the Home button.
```java
app().keyboard().longPressKey(AndroidKey.HOME);
```
Long press the Home button.