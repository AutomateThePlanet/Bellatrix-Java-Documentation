---
layout: default
title:  "Touch Actions Service"
excerpt: "Learn how to use BELLATRIX iOS touch actions service."
date:   2018-11-22 06:50:17 +0200
parent: ios-automation
permalink: /ios-automation/touch-actions-service/
anchors:
  example: Example
  explanations: Explanations
---
Example
-------
```java
public class TouchActionsServiceTests extends IOSTest {
    @Test
    public void elementSwiped_When_CallSwipeByCoordinatesMethod() {
        var textField = app().create().byId(TextField.class, "IntegerA");
        Point point = textField.getLocation();
        Dimension size = textField.getSize();

        app().touch().swipe(
                point.getY() + 5,
                point.getY() + 5,
                point.getX() + size.getWidth() - 5,
                point.getY() + size.getHeight() - 5,
                200);

        app().touch().swipe(
                point.getX() + size.getWidth() - 5,
                point.getY() + 5,
                point.getX() + 5,
                point.getY() + size.getHeight() - 5,
                2000);
    }

    @Test
    public void componentTapped_When_CallTap() {
        var buttons = app().create().allByClass(Button.class, "XCUIElementTypeButton");

        app().touch().tap(buttons.get(0), 10).perform();
    }

    @Test
    public void elementSwiped_When_CallPressWaitMoveToAndReleaseByCoordinates() {
        var element = app().create().byName(IOSComponent.class, "AppElem");
        int end = element.Size.Width;
        int y = element.Location.Y;
        int moveTo = (9 / 100) * end;

        App.TouchActionsService.Press(moveTo, y, 0).Release().Perform();
    }

    @Test
    public void ElementSwiped_When_CallPressWaitMoveToAndReleaseByCoordinatesMultiAction()
    {
        var element = App.ElementCreateService.CreateByName<Element>("AppElem");
        int end = element.Size.Width;
        int y = element.Location.Y;
        int moveTo = (9 / 100) * end;

        var swipe = App.TouchActionsService.Press(moveTo, y, 0).Release();
        App.TouchActionsService.AddNewAction(swipe);
        App.TouchActionsService.PerformAllActions();
    }

    @Test
    public void TwoTouchActionExecutedInOneMultiAction_When_CallPerformAllActions()
    {
        var buttons = App.ElementCreateService.CreateAllByClass<Button>("XCUIElementTypeButton");

        var tapOne = App.TouchActionsService.Tap(buttons[0], 10);
        App.TouchActionsService.AddNewAction(tapOne);
        App.TouchActionsService.AddNewAction(tapOne);
        App.TouchActionsService.PerformAllActions();

        var tapTwo = App.TouchActionsService.Tap(buttons[0], 10);
        App.TouchActionsService.AddNewAction(tapTwo);
        App.TouchActionsService.AddNewAction(tapTwo);
        App.TouchActionsService.PerformAllActions();
    }
}
```

Explanations
------------
BELLATRIX gives you an interface for easier work with touch actions through TouchActionsService. Performing a series of touch actions can be one of the most complicated jobs in automating mobile apps. BELLATRIX touch APIs are simplified and made to be user-friendly as possible. Their usage can eliminate lots of code duplication and boilerplate code.
```csharp
App.TouchActionsService.Swipe(
    point.X + 5,
    point.Y + 5,
    point.X + size.Width - 5,
    point.Y + size.Height - 5,
    200);
```
Performs swipe by using coordinates.
```csharp
App.TouchActionsService.Tap(buttons[0], 10).Perform();
```
Tap 10 times using BELLATRIX UI element directly.
```csharp
App.TouchActionsService.Press(moveTo, y, 0).Release().Perform();
```
Performs a series of actions using elements coordinates.
```csharp
var swipe = App.TouchActionsService.Press(moveTo, y, 0).Release();
App.TouchActionsService.AddNewAction(swipe);
App.TouchActionsService.PerformAllActions();
```
Performs multiple actions.
```csharp
var tapOne = App.TouchActionsService.Tap(buttons[0], 10);
App.TouchActionsService.AddNewAction(tapOne);
App.TouchActionsService.AddNewAction(tapOne);
App.TouchActionsService.PerformAllActions();

var tapTwo = App.TouchActionsService.Tap(buttons[0], 10);
App.TouchActionsService.AddNewAction(tapTwo);
App.TouchActionsService.AddNewAction(tapTwo);
App.TouchActionsService.PerformAllActions();
```
Executes two multi actions.