---
layout: default
title:  "Locate Components"
excerpt: "Learn how to locate iOS components with BELLATRIX mobile module."
date:   2018-10-20 06:50:17 +0200
parent: ios-automation
permalink: /ios-automation/locate-components/
anchors:
  example: Example
  explanations: Explanations
  available-createby-methods: Available create().by Methods
  find-multiple-components: Find Multiple Components
  available-createallby-methods: Available create().allBy Methods
  find-nested-components: Find Nested Components
  available-create-methods-for-finding-nested-components: Available Nested create Methods
---
Example
-------
```java
@Test
public void componentFound_When_CreateById_And_ComponentIsOnScreen() {
    var button = app().create().byId(Button.class, "ComputeSumButton");

    button.validateIsVisible();

    System.out.println(button.getFindStrategy().getValue())

    System.out.println(button.getWrappedElement().getTagName());

    var answerLabel = app().create().byId(Button.class, "BELLATRIX");
    answerLabel.scrollToVisible();

    answerLabel.click();
}
```

Explanations
-------
```java

var button = app().create().byId(Button.class, "ComputeSumButton");
```
There are different ways to locate elements on the screen. To do it you use the element create service. You need to know that BELLATRIX has a built-in complex mechanism for waiting for elements, so you do not need to worry about this anymore. Keep in mind that when you use the **create().by** methods, the element is not searched on the screen. All elements use lazy loading. Which means that they are searched once you perform an action or assertion on them. By default on each new action, the element is searched again and be refreshed.
```java
System.out.println(button.getFindStrategy().getValue())
```
Because of the proxy element mechanism (we have a separate type of element instead of single **WebDriver** **WebElement** interface or Appium **IOSElement**) we have several benefits. Each control (element type – ComboBox, TextField and so on) contains only the actions you can do with it, and the methods are named properly. In vanilla **WebDriver** to type the text you call **sendKeys** method. Also, we have some additional properties in the proxy web control such as **getFindStrategy**. Now you can get the locator with which you element was found.
```java
System.out.println(button.getWrappedElement().getTagName());
```
You can access the **WebDriver** wrapped element through **getWrappedElement** and the current AppiumDriver instance through – **getWrappedDriver**.
```java
var answerLabel = app().create().byId(Button.class, "BELLATRIX");
answerLabel.scrollToVisible();
```
Sometimes, the elements you need to perform operations on are not in the visible part of the screen. In order Appium to be able to locate them, you need to scroll to them first. To do so for iOS, you need to use **scrollToVisible** method.

Available create.by() Methods
------------------------
BELLATRIX extends the vanilla WebDriver selectors and give you additional ones.
### byId ###
```java
app().create().byId(Button.class, "myId");
```
Searches the component by its ID.
### byName ###
```java
app().create().byName(Button.class, "ComputeSumButton");
```
Searches the component by its name.
### byValueContaining ###
```java
app().create().byValueContaining(Label.class, "SumLabel");
```
Searches the component by its value if it contains specified value.
### byIOSClassChain ###
```java
app().create().byIOSClassChain(Button.class, ".**/XCUIElementTypeCell[`name BEGINSWITH \"C\"`]/XCUIElementTypeButton[10]");
```
Searches the component by iOS Class Chain expressions.
### byIOSNsPredicate ###
```java
app().create().byIOSNsPredicate(RadioButton.class, "type == 'XCUIElementTypeSwitch' AND name == 'All-day'");
```
Searches the component by iOS NsPredicate expression.
### byClass ###
```java
app().create().byClass(TextField.class, "XCUIElementTypeTextField");
```
Searches the component by its class.
### byXPath ###
```java
app().create().byXPath(TextField.class, "//XCUIElementTypeButton[@name='ComputeSumButton']");
```
Searches the component by XPath locator.
### byAccesibilityIdContaining ###
```java
app().create().byAccesibilityIdContaining(TextField.class, "SomeAccessibilityID");
```
Searches the component by Accesibility ID if it contains specified value.
 

Find Multiple Components
----------------------
Sometimes we need to find more than one component. For example, in this test we want to locate all Add to Cart buttons.
To do it you can use the component create service **create().allBy** method.

```csharp
[TestMethod]
public void elementFound_When_CreateAllByName_And_ComponentIsOnScreen()
{
	var buttons = app().create().allByName<Button>("ComputeSumButton");

	buttons[0].validateIsVisible();
}
```

Available create().allBy Methods
------------------------
### allById ###
```java
app().create().allById(Button.class, "myId");
```
Searches the components by their ID.
### allByName ###
```java
app().create().allByName(Button.class, "ComputeSumButton");
```
Searches the components by their name.
### allByValueContaining ###
```java
app().create().allByValueContaining(Label.class, "SumLabel");
```
Searches the components by their value if it contains specified value.
### allByIOSClassChain ###
```java
app().create().allByIOSClassChain(Button.class, ".**/XCUIElementTypeCell[`name BEGINSWITH \"C\"`]/XCUIElementTypeButton[10]");
```
Searches the components by iOS Class Chain expressions.
### allByIOSNsPredicate ###
```java
app().create().allByIOSNsPredicate(RadioButton.class, "type == 'XCUIElementTypeSwitch' AND name == 'All-day'");
```
Searches the components by iOS NsPredicate expression.
### allByClass ###
```java
app().create().allByClass(TextField.class, "XCUIElementTypeTextField");
```
Searches the components by their class.
### allByXPath ###
```java
app().create().allByXPath(TextField.class, "//XCUIElementTypeButton[@name='ComputeSumButton']");
```
Searches the components by XPath locator.
### allByAccesibilityIdContaining ###
```java
app().create().allByAccesibilityIdContaining(TextField.class, "SomeAccessibilityID");
```
Searches the components by Accesibility ID if it contains specified value.

Find Nested Components
----------------------
Sometimes it is easier to locate one element and then find the next one that you need, inside it. For example in this test we want to locate the button inside the main view element. To do it you can use the element's **create** methods.

```java
public void elementFound_When_CreateById_And_ElementIsOnScreen_NestedElement() {
	var mainElement = app().create().byIOSNsPredicate(IOSComponent.class,
								"type == 'XCUIElementTypeApplication' AND name == 'TestApp'");
    var button = mainElement.createById(RadioButton.class, "ComputeSumButton");
    button.validateIsVisible();
}
```

**Note**: *it is entirely legal to create a **RadioButton** instead of **Button**. BELLATRIX library does not care about the real type of the iOS componenst. The proxy types are convenience wrappers so to say. Meaning they give you a better interface of predefined properties and methods to make your tests more readable.*

Available Create Methods for Finding Nested Components
----------------------------------------------------
### createById ###
```java
element.createById(Button.class, "myId");
```
Searches the component by its ID.
### createByName ###
```java
element.createByName(Button.class, "ComputeSumButton");
```
Searches the component by its name.
### createByValueContaining ###
```java
element.createByValueContaining(Label.class, "SumLabel");
```
Searches the component by its value if it contains specified value.
### createByIOSClassChain ###
```java
element.createByIOSClassChain(Button.class, ".**/XCUIElementTypeCell[`name BEGINSWITH \"C\"`]/XCUIElementTypeButton[10]");
```
Searches the component by iOS Class Chain expressions.
### createByIOSNsPredicate ###
```java
element.createByIOSNsPredicate(RadioButton.class, "type == 'XCUIElementTypeSwitch' AND name == 'All-day'");
```
Searches the component by iOS NsPredicate expression.
### createByClass ###
```java
element.createByClass(TextField.class, "XCUIElementTypeTextField");
```
Searches the component by its class.
### createByXPath ###
```java
element.createByXPath(TextField.class, "//XCUIElementTypeButton[@name='ComputeSumButton']");
```
Searches the component by XPath locator.
### createByAccesibilityIdContaining ###
```java
element.createByAccesibilityIdContaining(TextField.class, "SomeAccessibilityID");
```
Searches the component by Accesibility ID if it contains specified value.
### createAllById ###
```java
element.createAllById(Button.class, "myId");
```
Searches the components by their ID.
### createAllByName ###
```java
element.createAllByName(Button.class, "ComputeSumButton");
```
Searches the components by their name.
### createAllByValueContaining ###
```java
element.createAllByValueContaining(Label.class, "SumLabel");
```
Searches the components by their value if it contains specified value.
### createAllByIOSClassChain ###
```java
element.createAllByIOSClassChain(Button.class, ".**/XCUIElementTypeCell[`name BEGINSWITH \"C\"`]/XCUIElementTypeButton[10]");
```
Searches the components by iOS Class Chain expressions.
### createAllByIOSNsPredicate ###
```java
element.createAllByIOSNsPredicate(RadioButton.class, "type == 'XCUIElementTypeSwitch' AND name == 'All-day'");
```
Searches the components by iOS NsPredicate expression.
### createAllByClass ###
```java
element.createAllByClass(TextField.class, "XCUIElementTypeTextField");
```
Searches the components by their class.
### createAllByXPath ###
```java
element.createAllByXPath(TextField.class, "//XCUIElementTypeButton[@name='ComputeSumButton']");
```
Searches the components by XPath locator.
### createAllByAccesibilityIdContaining ###
```java
element.createAllByAccesibilityIdContaining(TextField.class, "SomeAccessibilityID");
```
Searches the components by Accesibility ID if it contains specified value.