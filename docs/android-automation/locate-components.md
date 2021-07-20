---
layout: default
title:  "Locate Components"
excerpt: "Learn how to locate Android components with BELLATRIX android module."
date:   2018-10-20 06:50:17 +0200
parent: android-automation
permalink: /android-automation/locate-components/
anchors:
  example: Example
  explanations: Explanations
  available-createby-methods: Available create().by Methods
  find-multiple-components: Find Multiple Components
  available-createallby-methods: Available create().allBy Methods
  find-nested-components: Find Nested Components
  available-create-methods-for-finding-nested-components: Available Nested create Methods
  available-createall-methods-for-finding-nested-components: Available Nested createAll Methods
---
Example
-------
```java
@ExecutionApp(androidVersion = "7.1",
        deviceName = "android25-test",
        appPackage = "com.example.android.apis",
        appActivity = ".view.Controls1",
        lifecycle = Lifecycle.REUSE_IF_STARTED)
public class BellatrixAppBehaviourTests extends AndroidTest {
    @Test
    public void elementFound_When_CreateByIdContaining_And_ElementIsOnScreen() {
        var button = app().create().byIdContaining(Button.class, "button");

        button.toBeVisible().waitToBe();

        System.out.println(button.getFindStrategy().getValue());

        System.out.println(button.getWrappedElement().getTagName());

        var textField = app().create().byIdContaining(TextField.class, "edit");

        textField.toBeVisible().waitToBe();
    }
}
```

Explanations
-------
```java
var button = app().create().byIdContaining(Button.class, "button");
```
There are different ways to locate elements on the screen. To do it you use the element create service. You need to know that BELLATRIX has a built-in complex mechanism for waiting for elements, so you do not need to worry about this anymore. Keep in mind that when you use the **create().by** methods, the element is not searched on the screen. All elements use lazy loading. Which means that they are searched once you perform an action or assertion on them. By default on each new action, the element is searched again and be refreshed.
```java
System.out.println(button.getFindStrategy().getValue());
```
Because of the proxy element mechanism (we have a separate type of element instead of single **WebDriver** **WebElement** interface or Appium **AndroidElement**) we have several benefits. Each control (element type – ComboBox, TextField and so on) contains only the actions you can do with it, and the methods are named properly. In vanilla **WebDriver** to type the text you call **sendKeys** method. Also, we have some additional properties in the proxy web control such as **getFindStrategy**. Now you can get the locator with which you element was found.
```java
System.out.println(button.getWrappedElement().getTagName());
```
You can access the **WebDriver** wrapped element through **getWrappedElement** and the current AppiumDriver instance through – **getWrappedDriver**.
```java
var textField = app().create().byIdContaining(TextField.class, "edit");
```
Sometimes, the elements you need to perform operations on are not in the visible part of the screen. In order Appium to be able to locate them, you need to scroll to them first. To do so for Android, you need to use complex **AndroidUIAutomator** expressions. To save you lots of trouble and complex code, most of BELLATRIX locators contains the scroll logic built-in. The below element is initially not visible on the screen. BELLATRIX automatically scrolls down till the element is visible and then searches for it.

Available create().by Methods
------------------------
BELLATRIX extends the vanilla WebDriver selectors and give you additional ones.
### byId ###
```java
app().create().byId(Button.class, "myId");
```
Searches the element by its ID.
### byIdContaining ###
```java
app().create().byIdContaining(Button.class, "myIdMiddle");
```
Searches the element by ID containing the specified value.
### byDescription ###
```java
app().create().byDescription(Button.class, "myDescription");
```
Searches the element by its description.
### byDescriptionContaining ###
```java
app().create().byDescriptionContaining(Button.class, "description");
```
Searches the element by its description if it contains specified value.
### byText ###
```java
app().create().byText(Button.class, "text");
```
Searches the element by its text.
### byTextContaining ###
```java
app().create().byTextContaining(Button.class, "partOfText");
```
Searches the element by its text if it contains specified value.
### byClass ###
```java
app().create().byClass(Button.class, "myClass");
```
Searches the element by its class.
### byTag ###
```java
app().create().byTag(Button.class, "tag");
```
Searches the element by its tag.
### byName ###
```java
app().create().byName(Button.class, "name");
```
Searches the element by its name.
### byAndroidUIAutomator ###
```csharp
app().create().byAndroidUIAutomator(Button.class, "ui-automator-expression");
```
Searches the element by Android UIAutomator expression.
### byXPath ###
```csharp
app().create().byXPath(Button.class, "//*[@title='Add to cart']");
```
Searches the element by XPath locator.
 

Find Multiple Components
----------------------
Sometimes we need to find more than one element. For example, in this test we want to locate all Add to Cart buttons.
To do it you can use the element create service createAll method.

```java
@Test
public void elementFound_When_CreateAllByIdContaining_And_ElementIsOnScreen() {
    var buttons = app().create().allByIdContaining(Button.class, "button");

    buttons.get(0).toBeVisible().waitToBe();
}
```

Available create().allBy Methods
------------------------
### allById ###
```java
app().create().allById(Button.class, "myId");
```
Searches the elements by its ID.
### allByIdContaining ###
```java
app().create().allByIdContaining(Button.class, "myIdMiddle");
```
Searches the elements by ID containing the specified value.
### allByDescription ###
```java
app().create().allByDescription(Button.class, "myDescription");
```
Searches the elements by their description.
### allByDescriptionContaining ###
```java
app().create().allByDescriptionContaining(Button.class, "description");
```
Searches the elements by their description if it contains specified value.
### allByText ###
```java
app().create().allByText(Button.class, "text");
```
Searches the elements by their text.
### allByTextContaining ###
```java
app().create().allByTextContaining(Button.class, "partOfText");
```
Searches the elements by their text if it contains specified value.
### allByClass ###
```java
app().create().allByClass(Button.class, "myClass");
```
Searches the elements by their class.
### allByTag ###
```java
app().create().allByTag(Button.class, "tag");
```
Searches the elements by their tag.
### allByName ###
```java
app().create().allByName(Button.class, "name");
```
Searches the elements by their name.
### allByAndroidUIAutomator ###
```java
app().create().allByAndroidUIAutomator(Button.class, "ui-automator-expression");
```
Searches the elements by Android UIAutomator expression.
### allByXPath ###
```java
app().create().allByXPath(Button.class, "//*[@title='Add to cart']");
```
Searches the elements by XPath locator.

Find Nested Components
----------------------
Sometimes it is easier to locate one element and then find the next one that you need, inside it. For example in this test we want to locate the button inside the main view element. To do it you can use the element's Create methods.

```java
public void elementFound_When_CreateByIdContaining_And_ElementIsOnScreen_NestedElement() {
    var mainElement = app().create().byIdContaining(AndroidComponent.class, "decor_content_parent");
    var button = mainElement.createByXPath(Button.class, "//*[contains(@id,'containsbutton')]");
    button.toBeVisible().waitToBe();
}
```

**Note**: *it is entirely legal to create a **Button** instead of **ToggleButton**. BELLATRIX library does not care about the real type of the Android elements. The proxy types are convenience wrappers so to say. Meaning they give you a better interface of predefined properties and methods to make your tests more readable.*

Available create Methods for Finding Nested Components
----------------------------------------------------
### createByName ###
```java
element.createByName(Button.class, "name");
```
Searches the element by its name.
### createByTag ###
```java
element.createByTag(Button.class, "tag");
```
Searches the element by its tag.
### CreateByXPath ###
```java
element.createByXPath(Button.class, "//*[@title='Add to cart']");
```
Searches the element by XPath locator.

Available createAll Methods for Finding Nested Components
----------------------------------------------------
### createByName ###
```java
element.createAllByName(Button.class, "name");
```
Searches the elements by their name.
### createByTag ###
```java
element.createAllByTag(Button.class, "tag");
```
Searches the elements by their tag.
### CreateByXPath ###
```java
element.createAllByXPath(Button.class, "//*[@title='Add to cart']");
```
Searches the elements by XPath locator.