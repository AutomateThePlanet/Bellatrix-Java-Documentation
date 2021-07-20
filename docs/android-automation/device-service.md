---
layout: default
title:  "Device Service"
excerpt: "Learn how to use BELLATRIX android device service."
date:   2018-06-22 06:50:17 +0200
parent: android-automation
permalink: /android-automation/device-service/
anchors:
  example: Example
  explanations: Explanations
---
Example
-------
```java
public class DeviceServiceTests extends AndroidTest {
    @Test
    public void orientationSetToLandscape_When_CallRotateWithLandscape() {
        app().device().rotate(ScreenOrientation.LANDSCAPE);

        Assert.assertEquals(app().device().getScreenOrientation(), ScreenOrientation.LANDSCAPE);
    }

    @Test
    public void correctTimeReturned_When_CallDeviceTime() {
        Assert.assertEquals(app().device().getDeviceTime().toEpochSecond(ZoneOffset.UTC), LocalDateTime.now().toEpochSecond(ZoneOffset.UTC), 5);
    }

    @Test
    public void deviceIsLockedFalse_When_DeviceIsUnlocked() {
        app().device().unlock();

        Assert.assertTrue(app().device().isLocked());
    }

    @Test
    public void deviceIsLockedTrue_When_CallLock() {
        app().device().lock();

        Assert.assertTrue(app().device().isLocked());
    }

    @Test
    public void testTurnOnLocationService() {
        app().device().turnOnLocationService();
    }

    @Test
    public void testOpenNotifications() {
        app().device().openNotifications();
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
app().device().unlock();
```
Unlocks the device.
```java
Assert.assertTrue(app().device().isLocked());
```
Checks if the device is locked or not.
```java
app().device().lock();
```
Locks the device.
```java
app().device().turnOnLocationService();
```
Turns on the location service.
```java
app().device().openNotifications();
```
Opens notifications.