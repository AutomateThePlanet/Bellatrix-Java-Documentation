---
layout: default
title:  "Page Objects"
excerpt: "Learn how to use the BELLATRIX page objects."
date:   2018-06-22 06:50:17 +0200
parent: android-automation
permalink: /android-automation/page-objects/
anchors:
  introduction: Introduction
  non-page-object-test-example: Non-page-object Test Example
  how-to-create-bellatrix-page-object: How to Create BELLATRIX Page Object
  page-object-example: Page Object Example
  page-object-example-explanations: Page Object Example Explanations
  page-object-test-example: Page Object Test Example
  page-object-test-example-explanations: Page Object Test Example Explanations
---

Introduction
------------
As you most probably noticed this is like the 4th time we use almost the same elements and logic inside our tests. Similar test writing approach leads to unreadable and hard to maintain tests. Because of that people use the so-called Page Object design pattern to reuse their elements and pages' logic. BELLATRIX comes with powerful built-in page objects which are much more readable and maintainable than regular vanilla **WebDriver** ones.

Non-page-object Test Example
----------------------------
```java
@Test
public void actionsWithoutPageObjects() {
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
}
```

How to Create BELLATRIX Page Object
-----------------------------------
On most pages, you need to define elements. Placing them in a single place makes the changing of the locators easy. It is a matter of choice whether to have action methods or not. If you use the same combination of same actions against a group of elements then it may be a good idea to wrap them in a page object action method. In our example, we can wrap the transfer of an item in such a method.
In the assertions file, we may place some predefined **validate** methods. For example, if you always check the same email or title of a screen, there is no need to hard-code the string in each test. Later if the title is changed, you can do it in a single place. The same is true about most of the things you can assert in your tests.

Page Object Example
-------------------
### Methods File ###
```java
public class MainAndroidPage extends AndroidPage {
    protected String activityName;
    protected String packageName;

    public MainAndroidPage() {
        activityName = ".view.Controls1";
        packageName = "com.example.android.apis"
    }

    public void transferItem(String itemToBeTransferred, String userName, String password) {
        map().permanentTransfer().check();
        map().items().selectByText(itemToBeTransferred);
        map().returnItemAfter().toNotExist().waitToBe();
        map().userName().setText(userName);
        map().password().setPassword(password);
        map().keepMeLogged().click();
        map().transfer().click();
    }
}
```
### Elements File ###
```java
public class MainAndroidPage extends PageMap<Map, Asserts> {
    public Button transfer() {
        return create().byIdContaining(Button.class, "button");
    }

    public CheckBox permanentTransfer() {
        return create().byIdContaining(CheckBox.class, "check1");
    }

    public ComboBox items() {
        return create().byIdContaining(ComboBox.class, "spinner1");
    }

    public Button returnItemAfter() {
        return create().byIdContaining(Button.class, "toggle1");
    }

    public Label results() {
        return create().byText(Label.class, "textColorPrimary");
    }

    public PasswordField password() {
        return create().byIdContaining(PasswordField.class, "edit2");
    }

    public TextField userName() {
        return create().byIdContaining(TextField.class, "edit");
    }

    public RadioButton keepMeLogged() {
        return create().byIdContaining(RadioButton.class, "radio2");
    }
}
```
### Assertions File ###
```java
public class MainAndroidPage extends PageAsserts<Map> {
    public void permanentTransferIsChecked() {
        map().permanentTransfer().validateIsChecked();
    }

    public void rightItemSelected(String itemName) {
        map().items().validateTextIs(itemName);
    }

    public void rightUserNameSet(String userName) {
        map().userName().validateTextIs(userName);
    }

    public void keepMeLoggedChecked() {
        map().keepMeLogged().validateIsChecked();
    }
}
```

Page Object Example Explanations
--------------------------------
```java
public class MainAndroidPage extends AndroidPage<Map, Asserts>
```
All BELLATRIX page objects are implemented as a package with three different classes which means that you have separate files for different parts of it – actions, elements and assertions. This makes the maintainability and readability of these classes much better. Also, you can easily locate what you need. You can always create BELLATRIX page objects yourself by extending the AndroidPage class.
```java
public void transferItem(String itemToBeTransferred, String userName, String password) {
    map().permanentTransfer().check();
    map().items().selectByText(itemToBeTransferred);
    map().returnItemAfter().toNotExist().waitToBe();
    map().userName().setText(userName);
    map().password().setPassword(password);
    map().keepMeLogged().click();
    map().transfer().click();
}
```
These elements are always used together when an item is transferred. There are many test cases where you need to transfer different items and so on. This way you reuse the code instead of copy-paste it. If there is a change in the way how the item is transferred, change the workflow only here. Even single line of code is changed in your tests.
```java
protected String activityName;
protected String packageName;

public MainAndroidPage() {
    activityName = ".view.Controls1";
    packageName = "com.example.android.apis"
}
```
We use these values later to navigate to the page's activity.
```java
public Button transfer() {
    return create().byIdContaining(Button.class, "button");
}
```
All elements are placed inside the file **Map** so that the declarations of your elements to be in a single place. It is convenient since if there is a change in some of the locators or elements types you can apply the fix only here.
```java
public void assertPermanentTransferIsChecked() {
    map().permanentTransfer().validateIsChecked();
}
```
In the Asserts file, we have a method called totalPrice. We can access it through the asserts method, just like we used the map method above. If there is a change what needs to be checked –> for example, not span but different element you can change it in a single place.

Page Object Test Example
------------------------
```java
@Test
public void actionsWithPageObjects() {
    var mainPage = app().create(MainAndroidPage.class);

    mainPage.goTo();

    mainPage.transferItem("Item2", "bellatrix", "topsecret");

    mainPage.asserts().keepMeLoggedChecked();
    mainPage.asserts().permanentTransferIsChecked();
    mainPage.asserts().rightItemSelected("Item2");
    mainPage.asserts().rightUserNameSet("bellatrix");
}
```

Page Object Test Example Explanations
-------------------------------------
```java
var mainPage = app().create(MainAndroidPage.class);
```
You can use the App Create method to get an instance of it.
```java
mainPage.goTo();
```
Opens the page's Android activity screen.
```csharp
mainPage.transferItem("Item2", "bellatrix", "topsecret");
```
After you have the instance, you can directly start using the action methods of the page. As you can see the test became much shorter and more readable. The additional code pays off in future when changes are made to the page, or you need to reuse some of the methods.