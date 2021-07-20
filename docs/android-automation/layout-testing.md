---
layout: default
title:  "Layout Testing"
excerpt: "Learn how to use the BELLATRIX layout testing library."
date:   2018-10-22 06:50:17 +0200
parent: android-automation
permalink: /android-automation/layout-testing/
anchors:
  example: Example
  explanations: Explanations
  bdd-logging: BDD Logging
  all-available-layout-validation-methods: All Available Methods
---
Example
-------
```java
@Test
public void testPageLayout() {
    var button = app().create().byIdContaining(Button.class, "button");
    var secondButton = app().create().byIdContaining(Button.class, "button_disabled");
    var checkBox = app().create().byIdContaining(CheckBox.class, "check1");
    var secondCheckBox = app().create().byIdContaining(CheckBox.class, "check2");
    var mainElement = app().create().byId(AndroidComponent.class, "android:id/content");

    button.above(checkBox).validate();

    button.above(checkBox).equal(105).validate();

    button.above(checkBox).greaterThan(100).validate();
    button.above(checkBox).greaterThanOrEqual(105).validate();
    button.above(checkBox).lessThan(110).validate();
    button.above(checkBox).lessThanOrEqual(105).validate();

    button.topInside(checkBox).greaterThan(100).validate();
    button.topInside(checkBox).greaterThanOrEqual(105).validate();
    button.topInside(checkBox).lessThan(106).validate();
    button.topInside(checkBox).lessThanOrEqual(105).validate();

    checkBox.right(button).validate();
    button.left(checkBox).validate();

    button.alignedHorizontallyAll(secondButton).validate();

    button.alignedHorizontallyTop(secondButton, secondButton).validate();
    button.alignedHorizontallyCentered(secondButton, secondButton).validate();
    button.alignedHorizontallyBottom(secondButton, secondButton).validate();

    secondCheckBox.alignedVerticallyAll(checkBox).validate();

    secondCheckBox.alignedVerticallyLeft(checkBox).validate();
    secondCheckBox.alignedVerticallyCentered(checkBox).validate();
    secondCheckBox.alignedVerticallyRight(checkBox).validate();

    button.inside(mainElement).validate();

    button.height().lessThan(100);
    button.width().greaterThan(50);
    button.width().lessThan(80);
}
```

Explanations
------------
Layout testing is a module from BELLATRIX that allows you to test the responsiveness of your app.
```java
button.above(checkBox).validate();
```
Depending on what you want to check, BELLATRIX gives lots of options. You can test PX perfect or just that some element is below another. Check that the button is above the CheckBox.
```java
button.above(checkBox).equal(105).validate();
```
Assert with the exact distance between them.
All layout assertion methods throw AssertionError if the check is not successful with beautified troubleshooting message:
```
Button (id containing button) should be above of CheckBox (id containing check1) = 105 px
```
```java
button.above(checkBox).greaterThan(100).validate();
button.above(checkBox).greaterThanOrEqual(105).validate();
button.above(checkBox).lessThan(110).validate();
button.above(checkBox).lessThanOrEqual(105).validate();
```
For each available method you have variations of it such as, >, >=, <, <=.
```java
checkBox.right(button).validate();
button.left(checkBox).validate();
```
You can assert the position of components again each other in all directions – above, below, right, left, top, top inside, bottom inside, left inside, right inside. Assert that the sort dropdown is positioned near the top right of the button.
```java
button.alignedHorizontallyAll(secondButton).validate();
button.alignedHorizontallyTop(secondButton, secondButton).validate();
button.alignedHorizontallyCentered(secondButton, secondButton).validate();
button.alignedHorizontallyBottom(secondButton, secondButton).validate();
```
You can tests whether different Android components are aligned correctly. You can pass as many components as you like.
```java
secondCheckBox.alignedVerticallyAll(checkBox).validate();
secondCheckBox.alignedVerticallyLeft(checkBox).validate();
secondCheckBox.alignedVerticallyCentered(checkBox).validate();
secondCheckBox.alignedVerticallyRight(checkBox).validate();
```
Assert that the components are aligned vertically only from the left side.
```csharp
button.AssertInsideOf(mainElement);
```
You can check that some component is inside in another. Assert that the button is present in the main view component.
```csharp
button.AssertHeightLessThan(100);
button.AssertWidthBetween(50, 80);
```
Verify the height and width of components.

BDD Logging
-----------
All layout assertion methods have full BDD logging support.

All Available Layout Validation Methods
--------------------------------------

### Validation Properties ###
- above
- below
- right
- left
- topInside
- bottomInside
- rightInside
- leftInside

### Comparison Methods ###
- equal
- greaterThan
- greaterThanOrEqual
- lessThan
- lessThanOrEqual

### Alignment Validation ###
- alignedHorizontallyAll
- alignedHorizontallyTop
- alignedHorizontallyCentered
- alignedHorizontallyBottom
- alignedVerticallyAll
- alignedVerticallyTop
- alignedVerticallyCentered
- alignedVerticallyBottom

### Build Validation ###
- validate