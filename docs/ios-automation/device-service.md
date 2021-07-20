---
layout: default
title:  "Device Service"
excerpt: "Learn how to use BELLATRIX iOS device service."
date:   2018-11-20 06:50:17 +0200
parent: ios-automation
permalink: /ios-automation/device-service/
anchors:
  example: Example
  explanations: Explanations
---
Example
-------
```java
public class DeviceServiceTests extends IOSTest {
    @Test
    public void orientationSetToLandscape_When_CallRotateWithLandscape() {
        app().device().rotate(ScreenOrientation.LANDSCAPE);

        Assert.assertEquals(app().device().getScreenOrientation(), ScreenOrientation.LANDSCAPE);

        app().device().rotate(ScreenOrientation.PORTRAIT);
    }

    @Test
    public void correctTimeReturned_When_CallDeviceTime() {
        Assert.assertEquals(app().device().getDeviceTime().toEpochSecond(ZoneOffset.UTC), LocalDateTime.now().toEpochSecond(ZoneOffset.UTC), 5);
    }

    @Test
    public void deviceIsLockedTrue_When_CallLock() {
        app().device().lock();

        Assert.assertTrue(app().device().isLocked());
    }

    @Test
    public void testShakeDevice() {
        app().device().shake();
    }
}
```

Explanations
------------
BELLATRIX gives you an interface to most common operations for controlling the device through the **device** method.
```java
app().device().rotate(ScreenOrientation.LANDSCAPE);
```
Rotates the device horizontally.
```java
Assert.assertEquals(app().device().getScreenOrientation(), ScreenOrientation.LANDSCAPE);
```
Gets the current device orientation.
```java
Assert.assertEquals(app().device().getDeviceTime().toEpochSecond(ZoneOffset.UTC), LocalDateTime.now().toEpochSecond(ZoneOffset.UTC), 5);
```
Asserts current device time.
```java
app().device().lock();
```
```java
app().device().shake();
```
Shakes the device.