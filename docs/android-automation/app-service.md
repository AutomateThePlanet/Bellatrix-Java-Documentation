---
layout: default
title:  "App Service"
excerpt: "Learn how to use BELLATRIX android app service."
date:   2018-06-22 06:50:17 +0200
parent: android-automation
permalink: /android-automation/app-service/
anchors:
  example: Example
  explanations: Explanations
---
Example
-------
```java
public class AppServiceTests extends AndroidTest {
    @Test
    public void testBackgroundApp() {
        app().appService().backgroundApp(1);
    }

    @Test
    public void testResetApp() {
        app().appService().resetApp();
    }

    @Test
    public void installAppInstalledFalse_When_AppIsUninstalled() throws URISyntaxException {
        URL appUrl = getClass().getClassLoader().getResource("Demos/ApiDemos.apk");
        File appFile = Paths.get(appUrl.toURI()).toFile();
        String appPath = appFile.getAbsolutePath();

        app().appService().installApp(appPath);

        app().appService().removeApp("com.example.android.apis");

        Assert.assertFalse(app().appService().isAppInstalled("com.example.android.apis"));

        app().appService().installApp(appPath);
        Assert.assertTrue(app().appService().isAppInstalled("com.example.android.apis"));
    }
}
```

Explanations
------------
BELLATRIX gives you an interface to most common operations for controlling the Android app through the **appService** method. We already saw one of them **startActivity** for opening a particular initial activity.
```java
app().appService().backgroundApp(1);
```
Backgrounds the app for the specified number of seconds.
```java
app().appService().resetApp();
```
Resets the app.
```csharp
Assert.assertTrue(app().appService().isAppInstalled("com.example.android.apis"));
```
Checks whether the app with the specified app package is installed.
```csharp
app().appService().installApp(appPath);
```
Installs the APK file on the device.
```csharp
app().appService().removeApp("com.example.android.apis");
```
Uninstalls the app with the specified app package.