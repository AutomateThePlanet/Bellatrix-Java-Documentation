---
layout: default
title:  "Execute Tests in a Grid"
excerpt: "Learn to use BELLATRIX to execute Android tests in a grid."
date:   2018-10-23 06:50:17 +0200
parent: android-automation
permalink: /android-automation/execute-tests-grid/
anchors:
  example: Example
  explanations: Explanations
  configuration: Configuration
---
Example
-------
```java
@ExecutionApp(appPath = "pngG38y26LZ5muB1p46P",
        androidVersion = "7.1",
        deviceName = "Android GoogleAPI Emulator",
        appPackage = "com.example.android.apis",
        appActivity = ".view.ControlsMaterialDark",
        lifecycle = Lifecycle.RESTART_EVERY_TIME)
public class GridTests extends AndroidTest {
    @Test
    public void buttonClicked_When_CallClickMethod() {
        var button = app().create().byIdContaining(Button.class, "button");

        button.click();
    }
}
```

Explanations
------------
```java
@ExecutionApp(appPath = "pngG38y26LZ5muB1p46P",
        androidVersion = "7.1",
        deviceName = "Android GoogleAPI Emulator",
        appPackage = "com.example.android.apis",
        appActivity = ".view.ControlsMaterialDark",
        lifecycle = Lifecycle.RESTART_EVERY_TIME)
```
To use BELLATRIX tests in a grid, you should use the ExecutionBrowser annotation to set additional parameters.

Configuration
-------------
There's an option about execution type in the **testFrameworkSettings.\<env\>.json** file under the **androidSettings** section.
```json
"androidSettings": {
    "executionType": "browserstack"
}
```
You can set the grid URL and set some additional arguments under the **gridSettings** array, setting **providerName** to the match the name you set in **executionType**. To set additional arguments for providers such as SauceLabs, BrowserStack or CrossBrowserTesting, you can use the **arguments** array parameter.
```json
"gridSettings": [
    {
        "providerName" : "browserstack",
        "url" : "http://hub-cloud.browserstack.com/wd/hub/",
        "arguments" : [
            {
                "user": "soioa1",
                "key":  "pnFG3Ky2yLZ5muB1p46P"
            }
        ]
    }
]
```