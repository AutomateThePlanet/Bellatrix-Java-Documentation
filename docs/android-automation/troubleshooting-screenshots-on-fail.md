---
layout: default
title:  "Troubleshooting – Screenshots on Fail"
excerpt: "Learn how to generate screenshots on test's fail."
date:   2018-10-22 06:50:17 +0200
parent: android-automation
permalink: /android-automation/troubleshooting-screenshots-on-fail/
anchors:
  example: Example
  explanations: Explanations
  configuration: Configuration
---
Example
-------
```java
public class ScreenshotsOnFailTests extends AndroidTest {
    @Test
    public void buttonClicked_When_CallClickMethod() {
        var button = app().create().byIdContaining(Button.class, "button");

        button.click();
    }
}
```

Explanations
------------
If it is turned on the engine will capture a screenshot in case the test failed.

Configuration
-------------
If you open the **testFrameworkSettings.\<env\>.json** file, you find the screenshot properties under **androidSettings** section that controls this behaviour.
```json
"screenshotsSettings": {
    "screenshotsOnFailEnabled": "true",
    "screenshotsSaveLocation": "user.home/BELLATRIX/Screenshots"
}
```
You can turn off the making of screenshots for all tests and specify where the screenshots to be saved.
In the extensibility chapters read more about how you can create different screenshots engine or change the saving strategy.