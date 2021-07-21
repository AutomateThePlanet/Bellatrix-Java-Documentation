---
layout: default
title:  "Android Components"
excerpt: "Learn how to use BELLATRIX android components."
date:   2018-06-22 06:50:17 +0200
parent: android-automation
permalink: /android-automation/android-components/
anchors:
  example: Example
  explanations: Explanations
  full-list-of-all-supported-android-controls: List of All Android Components
---
Example
-------
```java
@Test
public void commonActionsWithAndroidControls() {
    var button = app().create().byIdContaining(Button.class, "button");

    button.click();

    var radioButton = app().create().byIdContaining(RadioButton.class, "radio2");

    Assert.assertFalse(radioButton.isDisabled());

    var checkBox = app().create().byIdContaining(CheckBox.class, "check1");

    checkBox.check();

    Assert.assertTrue(checkBox.isChecked());

    checkBox.uncheck();

    Assert.assertFalse(checkBox.isChecked());

    var comboBox = app().create().byIdContaining(ComboBox.class, "spinner1");

    comboBox.selectByText("Jupiter");

    Assert.assertEquals(comboBox.getText(), "Jupiter");

    var seekBar = app().create().byClass(SeekBar.class, "android.widget.SeekBar");

    seekBar.set(9);

    seekBar.toExist().waitToBe();

    var label = app().create().byText(Label.class, "textColorPrimary");

    Assert.assertTrue(label.isVisible());

    var password = app().create().byDescription(PasswordField.class, "passwordBox");

    var textField = app().create().byIdContaining(TextField.class, "edit");

    textField.setText("BELLATRIX");

    Assert.assertEquals(textField.getText(), "BELLATRIX");

    radioButton.click();

    Assert.assertTrue(radioButton.isChecked());
}
```

Explanations
------------
As mentioned before BELLATRIX exposes 18+ Android controls. All of them implement Proxy design pattern which means that they are not located immediately when they are created. Another benefit is that each of them includes only the actions that you should be able to do with the specific control and nothing more. For example, you cannot type into a button. Moreover, this way all of the actions has meaningful names – type not **sendKeys** as in vanilla **WebDriver**.
```java
var button = app().create().byIdContaining(Button.class, "button");
```
Create methods accept a generic parameter the type of the Android control. Then only the methods for this specific control are accessible. Here we tell BELLATRIX to find your element by ID containing the value 'button'.
```java
button.click();
```
Clicking the button. At this moment BELLATRIX locates the element.
```java
var radioButton = app().create().byIdContaining(RadioButton.class, "radio2");
```
Locating the radio button control using ID containing the value 'radio2'.
```java
Assert.assertFalse(radioButton.isDisabled());
```
Most Android controls have properties such as checking whether the radio button is enabled or not.
```java
var checkBox = app().create().byIdContaining(CheckBox.class, "check1");
checkBox.check();
```
Checking and unchecking the checkbox with id = 'check1'
```java
Assert.assertTrue(checkBox.isChecked());
```
Asserting whether the check was successful.
```java
var comboBox = app().create().byIdContaining(ComboBox.class, "spinner1");
comboBox.selectByText("Jupiter");
```
Select a value in combobox by text.
```java
Assert.assertEquals(comboBox.getText(), "Jupiter");
```
Get the current comboBox text through getText method.
```java
var seekBar = app().create().byClass(SeekBar.class, "android.widget.SeekBar");
seekBar.set(9);
```
Moves the seekbar.
```java
seekBar.toExist().waitToBe();
```
Wait for the element to exist.
```java
var label = app().create().byText(Label.class, "textColorPrimary");
Assert.assertTrue(label.isVisible());
```
See if the element is visible or not using the **isPresent** property.
```java
var password = app().create().byDescription(PasswordField.class, "passwordBox");
password.setPassword("topsecret");
```
Instead of using the non-meaningful method **sendKeys**, BELLATRIX gives you more readable tests through proper methods and properties names. In this case, we set the text in the password field using the **setPassword** method and **setText** for regular text fields.

Full List of All Supported Android Controls
---------------------------------------
### Android Component ###
- getFindStrategy
- getAttribute
- getLocatiom
- scrollToVisible
- getWrappedElement
- getParentWrappedElement
- create
- createAll
- isVisible
- getComponentName
- toExist
- waitToBe
- isVisible

**Note**: *All other components have access to the above methods and properties*

Element | Available properties
------------ | -------------
Button | click, getText, isDisabled
CheckBox | check, uncheck, getText, isDisabled, ssChecked
ComboBox | selectByText, getText, isDisabled
Grid | getAll
Image | same as Android Component
ImageButton | click, getText, isDisabled
Label | getText
NumberInput | setNumber, getNumber, isDisabled
PasswordField | getPassword, setPassword, isDisabled
Progress | isDisabled
RadioButton | click, isDisabled, isChecked, getText
RadioGroup | clickByText, clickByIndex, getChecked, getAll
SeekBar | set, isDisabled
Switch | turnOn, turnOff, getText, isDisabled, isOn
Tabs | getAll
TextField | setText, getText, isDisabled
ToggleButton | turnOn, turnOff, getText, isDisabled, isOn