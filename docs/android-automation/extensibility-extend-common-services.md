---
layout: default
title:  "Extensability – Extend Common Services"
excerpt: "Learn how to extend BELLATRIX common services."
date:   2018-10-23 06:50:17 +0200
parent: android-automation
permalink: /android-automation/extensibility-extend-common-services/
anchors:
  example: Example
  explanations: Explanations
---
Example
-------
```java
public class ExtendExistingCommonServicesTests extends AndroidTest {
    @Override
    public ExtendedApp app() {
        return new ExtendedApp();
    }

    @Test
    public void buttonClicked_When_CallClickMethod() {
        app().appService().loginToApp("bellatrix", "topSecret");

        var button = app().create().byIdContaining(Button.class, "button");

        button.click();
    }
}
```

Explanations
------------
```java
public class ExtendedAppService extends AppService {
    public void loginToApp(String userName, String password) {
        var componentCreateService = new ComponentCreateService();
        var userNameField = componentCreateService.byIdContaining(TextField.class, "textBox");
        var passwordField = componentCreateService.byIdContaining(PasswordField.class, "passwordBox");
        var loginButton = componentCreateService.byIdContaining(Button.class, "loginButton");

        userNameField.setText(userName);
        passwordField.setPassword(password);
        loginButton.click();
    }
}
```
```java
public class ExtendedApp extends App {
    @Override
    public ExtendedAppService appService() {
        return SingletonFactory.getInstance(ExtendedAppService.class);
    }
}
```
A way to extend the BELLATRIX common services is to create a class extending the service for the additional action and create a class, extending the **App**, pointing to the new extended service, then override the **app** method in the test.
```java
@Override
public ExtendedApp app() {
    return new ExtendedApp();
}
```
Then you can use the additional method you created since **appService** will now return **ExtendedAppService** instance.
```java
app().appService().loginToApp("bellatrix", "topSecret");
```
Use newly added login method which is not part of the original implementation of the common service.