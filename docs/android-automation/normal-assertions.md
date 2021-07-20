---
layout: default
title:  "Normal Assertions"
excerpt: "Learn how to use normal assertion methods in BELLATRIX tests."
date:   2018-10-22 06:50:17 +0200
parent: android-automation
permalink: /android-automation/normal-assertions/
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
    Assert.assertFalse(button.isDisabled());

    var checkBox = app().create().byIdContaining(CheckBox.class, "check1");

    checkBox.check();

    Assert.assertTrue(checkBox.isChecked());

    var comboBox = app().create().byIdContaining(ComboBox.class, "spinner1");

    comboBox.selectByText("Jupiter");

    Assert.assertEquals(comboBox.getText(), "Jupiter");

    var label = app().create().byText(Label.class, "textColorPrimary");

    Assert.assertTrue(label.isVisible());

    var radioButton = app().create().byIdContaining(RadioButton.class, "radio2");

    radioButton.click();

    Assert.assertTrue(radioButton.isChecked());

    SoftAssert multipleAssert = new SoftAssert();
    multipleAssert.assertTrue(radioButton.isChecked());
    multipleAssert.assertTrue(label.isVisible());
    multipleAssert.assertAll();
}
```

Explanations
------------
```java
Assert.assertFalse(button.isDisabled());
```
We can assert whether the control is disabled. The different BELLATRIX Android component classes contain lots of these properties which are a representation of the most important app component attributes. The biggest drawback of using vanilla assertions is that the messages displayed on failure are not meaningful at all. This is so because most unit testing frameworks are created for much simpler and shorter unit tests. In next chapter, there is information how BELLATRIX solves the problems with the introduction of **validate** methods. If the bellow assertion fails the following message is displayed:
```
Expected :false
Actual   :true
```
You can guess what happened, but you do not have information which element failed and on which screen.
```java
Assert.assertTrue(checkBox.isChecked());
```
Asserts that the checkbox is checked. On fail the following message is displayed:
```
Expected :true
Actual   :false
```
Cannot learn much about what happened.
```java
Assert.assertEquals(comboBox.getText(), "Jupiter");
```
Assert that the proper item is selected from the combobox items.
```java
Assert.assertTrue(label.isVisible());
```
See if the element is visible or not using the **isVisible** property.
```java
Assert.assertTrue(radioButton.isChecked());
```
Assert that the radio button is clicked.
```java
SoftAssert multipleAssert = new SoftAssert();
multipleAssert.assertTrue(radioButton.isChecked());
multipleAssert.assertTrue(label.isVisible());
multipleAssert.assertAll();
```
You can execute multiple assertions failing only once viewing all results.

One more thing you need to keep in mind is that normal assertion methods do not include BDD logging and any available hooks. BELLATRIX provides you with a full BDD logging support for **validate** assertions and gives you a way to hook your logic in multiple places.