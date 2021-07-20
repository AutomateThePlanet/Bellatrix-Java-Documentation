---
layout: default
title:  "Add Custom Appium Options"
excerpt: "Learn how to add custom Appium options."
date:   2018-10-20 06:50:17 +0200
parent: android-automation
permalink: /android-automation/add-custom-appium-options/
anchors:
  example: Example
  explanations: Explanations
---
Example
-------
```java
public class CustomAppiumDriverCapabilities extends AndroidTest {
    @Override
    public void configure() {
        super.configure();
        app().addDriverOptions("locale", "fr_CA");
        app().addDriverOptions("language", "fr");
        app().addDriverOptions("autoWebview", "true");
        app().addDriverOptions("noReset", "false");
    }

    @Test
    public void buttonClicked_When_CallClickMethod() {
        var button = app().create().byIdContaining(Button.class, "button");

        button.click();
    }

    @Test
    public void buttonClicked_When_CallClickMethodSecond() {
        var button = app().create().byIdContaining(Button.class, "button");

        button.click();
    }
}
```

Explanations
------------
```java
app().addDriverOptions("locale", "fr_CA");
app().addDriverOptions("language", "fr");
app().addDriverOptions("autoWebview", "true");
app().addDriverOptions("noReset", "false");
```
BELLATRIX hides the complexity of initialization of WebDriver/Appium and all related services. In some cases, you need to customize the set up of a Appium with using custom Appium options. Using the **app** methods you can add all of these with ease. Make sure to call them in the **configure** which is called before the execution of the tests placed in the test class. These options are used only for the tests in this particular class.