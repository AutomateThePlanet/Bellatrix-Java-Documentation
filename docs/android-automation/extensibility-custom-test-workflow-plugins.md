---
layout: default
title:  "Extensibility – Plugins"
excerpt: "Learn how to plugin your logic in BELLATRIX test workflow using plugins."
date:   2018-10-23 06:50:17 +0200
parent: android-automation
permalink: /android-automation/extensibility-custom-test-workflow-plugins/
anchors:
  example: Example
  explanations: Explanations
  screenshot-and-video-generation-plugins: Screenshot and Video Generation Plugins
---
Introduction
------------
The test workflow plugins are way to execute your logic before each test. For example- taking screenshots or videos on test failures, creating custom logs and so on. In the example you can find a plugin that integrates a **ManualTestCase** attribute for the automated tests, containing the ID to the corresponding manual test case. The main purpose of the test workflow is to fail the test if the **ManualTestCase** attribute is not set.
 
Example
-------
```java
@Retention(RetentionPolicy.RUNTIME)
@Target(ElementType.METHOD)
public @interface ManualTestCase {
    int id() default 0;
}
```
```java
public class AssociatedPlugin extends Plugin {
    @Override
    public void preBeforeTest(TestResult result, Method memberInfo) {
        validateManualTestCaseAttribute(memberInfo);
    }

    @Override
    public void beforeTestFailed(Exception e) {
        e.printStackTrace();
    }

    private void validateManualTestCaseAttribute(Method memberInfo) {
        var manualTestCase = memberInfo.getDeclaredAnnotation(ManualTestCase.class);
        if (manualTestCase == null) {
            throw new NullPointerException("No manual test case is associated with the BELLATRIX test.");
        }
        if (manualTestCase.id() <= 0) {
            throw new IllegalArgumentException("The associated manual test case ID cannot be <= 0.");
        }
    }
}
```

Explanations
------------
```java
public class AssociatedPlugin extends Plugin
```
To create a custom test workflow plugin:

- Create a new class that extends the 'Plugin' base class.
- Then override some of the workflow's methods adding there your logic.
- Register the workflow plugin using the **addPlugin** method of the **BaseTest** class.

```java
@Override
public void preBeforeTest(TestResult result, Method memberInfo) {
    validateManualTestCaseAttribute(memberInfo);
}
```
You can override all mentioned test workflow method hooks in your custom handlers. The method uses reflection to find out if the ManualTestCase annotation is set to the run test. If the attribute is not set or is set more than once an exception is thrown. The logic executes before the actual test run, during the **preBeforeTest** phase.
```java
public class CustomTestCaseExtensionTests extends WebTest {
    @Override
    public void configure() {
        super.configure();
        addPlugin(AssociatedPlugin.class);
    }

    @Test
    @ManualTestCase(id = 1532)
    public void buttonClicked_When_CallClickMethod() {
        var button = app().create().byIdContaining(Button.class, "button");

        button.click();
    }
}
```
Once we created the test workflow plugin, we need to add it to the existing test workflow. It is done using the **addPlugin** method of the **BaseTest** class.

Screenshot and Video Generation Plugins
---------------------------------------
You can use event listeners to plug in the screenshots and video generation on fail.
To do a post-screenshot generation action, use the **SCREENSHOT_GENERATED** event listener of the **ScreenshotPlugin** class.
To do a post-video generation action, use the **VIDEO_GENERATED** event listener of the **VideoPlugin** class.
Bellow you can find a sample usage from BELLATRIX Allure plugin.
```java
public class AllureWorkflowPlugin extends Plugin {
    private String testContainerId;
    private String testResultId;
    private boolean hasStarted;

    public void addScreenshotListener() {
        ScreenshotPlugin.SCREENSHOT_GENERATED.addListener(this::screenshotGenerated);
    }

    public void addVideoListener() {
        VideoPlugin.VIDEO_GENERATED.addListener(this::videoGenerated);
    }

    public void screenshotGenerated(ScreenshotPluginEventArgs e) {
        var file = new File(e.getScreenshotPath());
        if (file.exists()) {
            Allure.addAttachment("image on fail", "image/png", file.toString());
        }
    }

    public void videoGenerated(VideoPluginEventArgs e) {
        var file = new File(e.getVideoPath());
        if (file.exists()) {
            Allure.addAttachment("video on fail", "video/mpg", file.toString());
        }
    }

    // rest of the code...
}
```
