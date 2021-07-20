---
layout: default
title:  "Validate Assertions"
excerpt: "Learn how to use BELLATRIX validate assertion methods."
date:   2018-10-22 06:50:17 +0200
parent: android-automation
permalink: /android-automation/validate-assertions/
anchors:
  example: Example
  explanations: Explanations
---
Example
-------
```java
@Test
public void commonAssertionsAndroidControls() {
    var button = app().create().byIdContaining(Button.class, "button");

    button.validateNotDisabled();

    var checkBox = app().create().byIdContaining(CheckBox.class, "check1");

    checkBox.check();

    checkBox.validateIsChecked();

    var comboBox = app().create().byIdContaining(ComboBox.class, "spinner1");

    comboBox.selectByText("Jupiter");

    comboBox.validateTextIs("Jupiter");

    var label = app().create().byText(Label.class, "textColorPrimary");

    label.validateIsVisible();

    var radioButton = app().create().byIdContaining(RadioButton.class, "radio2");

    radioButton.click();

    radioButton.validateIsChecked();
}
```

Explanations
------------
```java
// Assert.assertFalse(radioButton.isDisabled());
button.validateNotDisabled();
```
We can assert whether the control is disabled. The different BELLATRIX Android components classes contain lots of these properties which are a representation of the most important app component attributes. The biggest drawback of using vanilla assertions is that the messages displayed on failure are not meaningful at all. If the below assertion fails the following message is displayed:
```
Expected :false
Actual   :true
```
You can guess what happened, but you do not have information which component failed and on which page. If we use the **validate** methods, BELLATRIX waits some time for the condition to pass. After this period if it is not successful, a beautified meaningful exception message is displayed:
```
The Button (id containing button) shouldn't be disabled but was. The test failed on activity: .view.Controls1
```
```java
var checkBox = app().create().byIdContaining(CheckBox.class, "check1");
checkBox.validateIsChecked();
```
Here we assert that the checkbox is checked. On fail the following message is displayed: "*Message: Assert.IsTrue failed.*" Cannot learn much about what happened. Now if we use the ValidateIsChecked method and the assertion does not succeed the following error message is displayed: "***The control should be checked but was NOT.***"
```java
var comboBox = app().create().byIdContaining(ComboBox.class, "spinner1");
comboBox.validateTextIs("Jupiter");
```
Assert that the proper item is selected from the combobox items.
```java
var label = app().create().byText(Label.class, "textColorPrimary");
label.validateIsVisible();
```
See if the component is visible or not using the **isVisible** property.

Bellatrix provides you with a full BDD logging support for **validate** assertions and gives you a way to hook your logic in multiple places.