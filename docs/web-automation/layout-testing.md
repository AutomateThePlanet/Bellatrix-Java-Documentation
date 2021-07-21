---
layout: default
title:  "Layout Testing"
excerpt: "Learn how to use the BELLATRIX layout testing library."
date:   2018-06-22 06:50:17 +0200
parent: web-automation
permalink: /web-automation/layout-testing/
anchors:
  example: Example
  explanations: Explanations
  bdd-logging: BDD Logging
  all-available-layout-validation-methods: All Available Methods
---
Example
-------
```java
@ExecutionBrowser(browser = Browser.CHROME, width = 1280, height = 1024, lifecycle = Lifecycle.RESTART_EVERY_TIME)
public class LayoutTestingTests extends WebTest {
    @Test
    public void testPageLayout() {
        app().navigate().to("http://demos.bellatrix.solutions/");

        Select sortDropDown = app().create().byNameEndingWith(Select.class, "orderby");
        Anchor protonRocketAnchor =
        app().create().byAttributeContaining(Anchor.class, "href", "/proton-rocket/");
        Anchor protonMAnchor =
        app().create().byAttributeContaining(Anchor.class, "href", "/proton-m/");
        Anchor saturnVAnchor =
        app().create().byAttributeContaining(Anchor.class, "href", "/saturn-v/");
        Anchor falconHeavyAnchor =
        app().create().byAttributeContaining(Anchor.class, "href", "/falcon-heavy/");
        Anchor falcon9Anchor =
        app().create().byAttributeContaining(Anchor.class, "href", "/falcon-9/");
        Div saturnVRating = saturnVAnchor.createByClassContaining(Div.class, "star-rating");

        sortDropDown.above(protonRocketAnchor).validate();

        sortDropDown.above(protonRocketAnchor).equal(41).validate();

        sortDropDown.above(protonRocketAnchor).greaterThan(40).validate();
        sortDropDown.above(protonRocketAnchor).greaterThanOrEqual(41).validate();
        sortDropDown.above(protonRocketAnchor).lessThan(50).validate();
        sortDropDown.above(protonRocketAnchor).lessThanOrEqual(43).validate();

        saturnVAnchor.right(sortDropDown).validate();
        sortDropDown.left(saturnVAnchor).validate();

        protonRocketAnchor.alignedHorizontallyAll(protonMAnchor).validate();
        protonRocketAnchor.alignedHorizontallyTop(protonMAnchor, saturnVAnchor).validate();
        protonRocketAnchor.alignedHorizontallyCentered(protonMAnchor, saturnVAnchor).validate();
        protonRocketAnchor.alignedHorizontallyBottom(protonMAnchor, saturnVAnchor).validate();

        falcon9Anchor.alignedVerticallyAll(falconHeavyAnchor).validate();

        falcon9Anchor.alignedVerticallyLeft(falconHeavyAnchor).validate();
        falcon9Anchor.alignedVerticallyCentered(falconHeavyAnchor).validate();
        falcon9Anchor.alignedVerticallyRight(falconHeavyAnchor).validate();

        saturnVRating.inside(saturnVAnchor).validate();

        saturnVRating.height().lessThan(100).validate();
        saturnVRating.width().greaterThan(50).validate();
        saturnVRating.width().lessThan(70).validate();

        saturnVRating.inside(SpecialComponents.getViewport());
        saturnVRating.inside(SpecialComponents.getScreen());
    }
}
```

Explanations
------------
Layout testing is a module from BELLATRIX that allows you to test the responsiveness of your website.
```java
// Desktop Resolution
@ExecutionBrowser(browser = Browser.CHROME, width = 1280, height = 1024, lifecycle = Lifecycle.RESTART_EVERY_TIME)
```
```java
// Mobile Resolution
@ExecutionBrowser(browser = Browser.CHROME, width = 360, height = 640, lifecycle = Lifecycle.RESTART_EVERY_TIME)
```
```java
// Tablet Resolution
@ExecutionBrowser(browser = Browser.CHROME, width = 600, height = 1024, lifecycle = Lifecycle.RESTART_EVERY_TIME)
```
About 50 combinations of assertion methods are available to you to check the exact position of your web elements using the layout validation builder. Browser attribute gives you the option to resize your browser window so that you can test the rearrangement of the web elements on your pages.
```java
sortDropDown.above(protonRocketAnchor).validate();
```
Depending on what you want to check, BELLATRIX gives lots of options. You can test px perfect or just that some element is below another. Check that the popularity sort dropdown is above the proton rocket image.
```java
sortDropDown.above(protonRocketAnchor).equal(41).validate();
```
Assert with the exact distance between them.

All layout assertion methods throw AssertionError if the check is not successful with beautified troubleshooting message:
```
Select (name ending with orderby) should be above of Anchor (href containing /proton-rocket/) = 41 px
```

```java
sortDropDown.above(protonRocketAnchor).greaterThan(40).validate();
sortDropDown.above(protonRocketAnchor).greaterThanOrEqual(41).validate();
sortDropDown.above(protonRocketAnchor).lessThan(50).validate();
sortDropDown.above(protonRocketAnchor).lessThanOrEqual(43).validate();
```
For each available method the validation bulder also has comparisons such as >, >=, <, <=.
```csharp
saturnVAnchor.right(sortDropDown).validate();
sortDropDown.left(saturnVAnchor).validate();
```
You can assert the position of elements again each other in all directions – above, below, right, left, top, top inside, bottom inside, left inside, right inside. Assert that the sort dropdown is positioned near the top right of the Saturn B link.
```java
falcon9Anchor.alignedVerticallyAll(falconHeavyAnchor).validate();
```
You can tests whether different web elements are aligned correctly.
```java
falcon9Anchor.alignedVerticallyLeft(falconHeavyAnchor).validate();
falcon9Anchor.alignedVerticallyCentered(falconHeavyAnchor).validate();
falcon9Anchor.alignedVerticallyRight(falconHeavyAnchor).validate();
```
You can pass as many elements as you like.
```java
protonRocketAnchor.alignedHorizontallyAll(protonMAnchor).validate();
```
You can check horizontal alignment as well.
```java
protonRocketAnchor.alignedHorizontallyAll(protonMAnchor).validate();
protonRocketAnchor.alignedHorizontallyTop(protonMAnchor, saturnVAnchor).validate();
protonRocketAnchor.alignedHorizontallyCentered(protonMAnchor, saturnVAnchor).validate();
protonRocketAnchor.alignedHorizontallyBottom(protonMAnchor, saturnVAnchor).validate();
```
Assert that the elements are aligned vertically only from the left side.
```java
saturnVRating.inside(saturnVAnchor).validate();
```
You can check that some element is inside in another. Assert that the rating div is present in the Saturn V anchor.
```java
saturnVRating.height().lessThan(100).validate();
saturnVRating.width().greaterThan(50).validate();
saturnVRating.width().lessThan(70).validate();
```
Verify the height and width of elements.
```csharp
saturnVRating.inside(SpecialComponents.getViewport());
saturnVRating.inside(SpecialComponents.getScreen());
```
You can use for all layout assertions the special web elements – Viewport and Screen.
- **Screen** - represents the whole page area inside browser even that which is not visible.
- **Viewport** - it takes the browsers client window.
It is useful if you want to check some fixed element on the screen which sticks to viewport even when you scroll.

BDD Logging
-----------
All layout assertion methods have full BDD logging support.
<!-- Below you can find the generated BDD log. Of course if you use BELLATRIX page objects the log looks even better as mentioned in previous chapters. -->

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