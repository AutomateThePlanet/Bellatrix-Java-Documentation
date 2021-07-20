---
layout: default
title:  "Troubleshooting- Video Recording"
excerpt: "Learn how to use BELLATRIX cross-platform video recording."
date:   2018-10-22 06:50:17 +0200
parent: android-automation
permalink: /android-automation/troubleshooting-video-recording/
anchors:
  example: Example
  explanations: Explanations
  configuration: Configuration
---
Example
-------
```java
public class VideoRecordingTests extends AndroidTest {
    @Test
    public void buttonClicked_When_CallClickMethod() {
        var button = app().create().byIdContaining(Button.class, "button");

        button.click();
    }
}
```

Explanations
------------
If it is turned on the engine will capture video for every test and keep them in case the test failed.

Configuration
-------------
If you open the **testFrameworkSettings.\<env\>.json** file, you find the screenshot properties under **androidSettings** section that controls this behaviour.
```json
"androidSettings": {
    "videosOnFailEnabled": "true",
    "videosSaveLocation": "user.home/BELLATRIX/Videos"
}
```
You can turn off the making of videos for all tests and specify where the videos to be saved. In the extensibility chapters read more about how you can create custom video recorder or change the saving strategy.