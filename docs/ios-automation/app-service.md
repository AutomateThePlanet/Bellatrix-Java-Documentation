---
layout: default
title:  "App Service"
excerpt: "Learn how to use BELLATRIX iOS app service."
date:   2018-11-20 06:50:17 +0200
parent: ios-automation
permalink: /ios-automation/app-service/
anchors:
  example: Example
  explanations: Explanations
---
Example
-------
```java
public class AppServiceTests extends IOSTest {
    @Test
    public void testBackgroundApp() {
        app().appService().backgroundApp(1);
    }

    @Test
    public void testResetApp() {
        app().appService().resetApp();
    }

    @Test
    public void installAppInstalledTrue_When_AppIsInstalled() {
        Assert.assertTrue(app().appService().isAppInstalled("com.apple.mobilecal"));
    }

    @Test
    public void installAppInstalledFalse_When_AppIsUninstalled() throws URISyntaxException {
        URL appUrl = getClass().getClassLoader().getResource("Demos/TestApp.app.zip");
        File appFile = Paths.get(appUrl.toURI()).toFile();
        String appPath = appFile.getAbsolutePath();

        app().appService().installApp(appPath);

        app().appService().removeApp("com.apple.mobilecal");

        Assert.assertFalse(app().appService().isAppInstalled("com.apple.mobilecal"));

        app().appService().installApp(appPath);
        Assert.assertTrue(app().appService().isAppInstalled("com.apple.mobilecal"));
    }
}
```

Explanations
------------
BELLATRIX gives you an interface to most common operations for controlling the iOS app through the **appService** method.
```java
app().appService().backgroundApp(1);
```
Backgrounds the app for the specified number of seconds.
```java
app().appService().resetApp();
```
Resets the app.
```java
Assert.assertTrue(app().appService().isAppInstalled("com.apple.mobilecal"));
```
Checks whether the app with the specified bundleId is installed.
```java
app().appService().installApp(appPath);
```
Installs the app file on the device.
```java
app().appService().removeApp("com.apple.mobilecal");
```
Uninstalls the app with the specified bundleId. You can get your app's bundleId from XCode.