---
layout: default
title:  "Logging"
excerpt: "Learn how to use the BELLATRIX logging library."
date:   2018-10-23 06:50:17 +0200
parent: android-automation
permalink: /android-automation/logging/
anchors:
  example: Example
  explanations: Explanations
---
Example
-------
```java
public class LoggingTests extends AndroidTest {
    @Test
    public void buttonClicked_When_CallClickMethod() {
        Log.info("$$$ Before clicking the button $$$");
        var button = app().create().byIdContaining(Button.class, "button");

        button.click();
    }
}
```

Explanations
------------
```csharp
Log.info("$$$ Before clicking the button $$$");
```
Sometimes is useful to add information to the generated test log. To do it you can use the BELLATRIX built-in logger through the **Log** class's static methods.

Generated Log, as you can see the above custom message is added to the log.

```
Start Test
Class = LoggingTests Name = ButtonClicked_When_CallClickMethod
$$$ before clicking the button $$$
clicking Button (id = button)
```