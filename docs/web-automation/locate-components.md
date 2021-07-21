---
layout: default
title:  "Locate Components"
excerpt: "Learn how to locate web components with BELLATRIX web module."
date:   2018-06-22 06:50:17 +0200
parent: web-automation
permalink: /web-automation/locate-components/
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
@Test
public void promotionsPageOpened_When_PromotionsButtonClicked() {
    app().navigate().to("http://demos.bellatrix.solutions/");

    var promotionsLink = app().create().byLinkText(Anchor.class, "Promotions");

    promotionsLink.click();
    System.out.println(promotionsLink.getFindStrategy().getValue());

    System.out.println(promotionsLink.getWrappedElement().getTagName());
}
```

Explanations
-------
```java
var promotionsLink = app().create().byLinkText(Anchor.class, "Promotions");
```
There are different ways to locate components on the page. To do it you use the component create service.
You need to know that BELLATRIX has a built-in complex mechanism for waiting for components, so you do not need to worry about this anymore. Keep in mind that when you use the **create** methods, the component is not searched on the page. All components use lazy loading. Which means that they are searched once you perform an action or assertion on them. By default on each new action, the component is searched again and is refreshed.
```java
System.out.println(promotionsLink.getFindStrategy().getValue());
```
Because of the proxy component mechanism (we have a separate type of component instead of single **WebDriver** **WebElement** interface) we have several benefits. Each control (component type – **ComboBox**, **TextField** and so on) contains only the actions you can do with it, and the methods are named properly.
In vanilla **WebDriver** to type the text you call **sendKeys** method.
Also, we have some additional properties in the proxy web control such as – **getFindStrategy**. Now you can get the locator with which you component was found.
```java
System.out.println(promotionsLink.getWrappedElement().getTagName());
```
You can access the WebDriver wrapped element through **getWrappedElement** and the current **WebDriver** instance through **getWrappedDriver**.

Available create().by Methods
------------------------
BELLATRIX extends the vanilla WebDriver selectors and give you additional ones.
### create().byIdEndingWith ###
```java
app().create().byIdEndingWith(Anchor.class, "myIdSuffix");
```
Searches the component by ID ending with the locator.
### create().byTag ###
```java
app().create().byTag(Anchor.class, "a");
```
Searches the component by its tag.
### create().byId ###
```java
app().create().byId(Button.class, "myId");
```
Searches the component by its ID.
### create().byIdContaining ###
```java
app().create().byIdContaining(Button.class, "myIdMiddle");
```
Searches the component by ID containing the specified text.
### create().byValueContaining ###
```java
app().create().byValueContaining(Button.class, "pay");
```
Searches the component by value attribute containing the specified text.
### create().byXpath ###
```java
app().create().byXpath(Button.class, "//*[@title='Add to cart']");
```
Searches the component by XPath locator.
### create().byLinkText ###
```java
app().create().byLinkText(Anchor.class, "blog");
```
Searches the component by its link (href).
### create().byLinkTextContaining ###
```java
app().create().byLinkTextContaining(Anchor.class, "account");
```
Searches the component by its link (href) if it contains specified value.
### create().byClass ###
```java
app().create().byClass(Anchor.class, "product selected");
```
Searches the component by its CSS classes.
### create().byClassContaining ###
```java
app().create().byClassContaining(Anchor.class, "selected");
```
Searches the component by its CSS classes containing the specified values.
### create().byInnerTextContaining ###
```java
app().create().byInnerTextContaining(Div.class, "Showing all");
```
Searches the component by its inner text content, including all child HTML components.
### create().byNameEndingWith ###
```java
app().create().byNameEndingWith(SearchField.class, "a");
```
Searches the component by its name containing the specified text.
### create().byAttributesContaining ###
```java
app().create().byAttributeContaining(Anchor.class, "data-product_id", "31");
```
Searches the component by some of its attribute containing the specified value.

Find Multiple Components
----------------------
Sometimes we need to find more than one component. For example, in this test we want to locate all Add to Cart buttons.
To do it you can use the component create service **allBy** method.

```java
@Test
public void checkAllAddToCartButtons() {
    app().navigate().to("http://demos.bellatrix.solutions/");
    
    var blogLink = app().create().allByXPath(Anchor.class, "//*[@title='Add to cart']");
}
```

Available create().allBy Methods
------------------------
### create().allByIdEndingWith ###
```java
app().create().allByIdEndingWith(Anchor.class, "myIdSuffix");
```
Searches the components by ID ending with the locator.
### create().allByTag ###
```java
app().create().allByTag(Anchor.class, "a");
```
Searches the components by their tag.
### create().allById ###
```java
app().create().allById(Button.class, "myId");
```
Searches the components by their ID.
### create().allByIdContaining ###
```java
app().create().allByIdContaining(Button.class, "myIdMiddle");
```
Searches the components by ID containing the specified text.
### create().allByValueContaining ###
```java
app().create().allByValueContaining(Button.class, "pay");
```
Searches the components by value attribute containing the specified text.
### create().allByXpath ###
```java
app().create().allByXpath(Button.class, "//*[@title='Add to cart']");
```
Searches the components by XPath locator.
### create().allByLinkText ###
```java
app().create().allByLinkText(Anchor.class, "blog");
```
Searches the components by their link (href).
### create().allByLinkTextContaining ###
```java
app().create().allByLinkTextContaining(Anchor.class, "account");
```
Searches the components by their link (href) if it contains specified value.
### create().allByClass ###
```java
app().create().allByClass(Anchor.class, "product selected");
```
Searches the components by their CSS classes.
### create().allByClassContaining ###
```java
app().create().allByClassContaining(Anchor.class, "selected");
```
Searches the components by their CSS classes containing the specified values.
### create().allByInnerTextContaining ###
```java
app().create().allByInnerTextContaining(Div.class, "Showing all");
```
Searches the components by their inner text content, including all child HTML components.
### create().allByNameEndingWith ###
```java
app().create().allByNameEndingWith(SearchField.class, "a");
```
Searches the components by their names containing the specified text.
### create().allByAttributesContaining ###
```java
app().create().allByAttributeContaining(Anchor.class, "data-product_id", "31");
```
Searches the components by some of their attributes containing the specified value.

Find Nested Components
----------------------
Sometimes it is easier to locate one component and then find the next one that you need, inside it. For example in this test we want to locate the Sale! button inside the product's description. To do it you can use the component's create methods.

```java
@Test
public void openSalesPage_When_LocatedSaleButtonInsideProductImage() {
    app().navigate().to("http://demos.bellatrix.solutions/");

    var productsColumn = app().create().byClass(Option.class, "products columns-4");

    var saleButton = productsColumn.createByXpath(Anchor.class, "div[class*='woocommerce-LoopProduct-link woocommerce-loop-product__link']").createByInnerTextContaining(Button.class, "Sale!");

    saleButton.click();
}
```
The first products row is located. Then search inside it for the first product image, inside it search for the Sale! Span element.

**Note**: *It is entirely legal to create a Button instead of Span. BELLATRIX library does not care about the real type of the HTML elements. The proxy types are convenience wrappers so to say. Meaning they give you a better interface of predefined properties and methods to make your tests more readable.*

Available create Methods for Finding Nested Components
----------------------------------------------------
### createByIdEndingWith ###
```java
component.createByIdEndingWith(Anchor.class, "myIdSuffix");
```
Searches the component by ID ending with the locator.
### createByTag ###
```java
component.createByTag(Anchor.class, "a");
```
Searches the component by its tag.
### createById ###
```java
component.createById(Button.class, "myId");
```
Searches the component by its ID.
### createByIdContaining ###
```java
component.createByIdContaining(Button.class, "myIdMiddle");
```
Searches the component by ID containing the specified text.
### createByValueContaining ###
```java
component.createByValueContaining(Button.class, "pay");
```
Searches the component by ID containing the specified text.
### createByXpath ###
```java
component.createByXpath(Button.class, "//*[@title='Add to cart']");
```
Searches the component by XPath locator.
### createByLinkText ###
```java
component.createByLinkText(Anchor.class, "blog");
```
Searches the component by its link (href).
### createByLinkTextContaining ###
```java
component.createByLinkTextContaining(Anchor.class, "blog");
```
Searches the component by its link (href) if it contains specified value.
### createByCss ###
```java
component.createByCss(Anchor.class, "a[href*='falcon-9']");
```
Searches the component by its CSS selector.
### createByClass ###
```java
component.createByClass(Anchor.class, "product selected");
```
Searches the component by its CSS classes.
### createByClassContaining ###
```java
component.createByClassContaining(Anchor.class, "selected");
```
Searches the component by its CSS classes.
### createByInnerTextContaining ###
```java
component.createByInnerTextContaining(Div.class, "Showing all");
```
Searches the component by its inner text content, including all child HTML components.
### createByNameEndingWith ###
```java
component.createByNameEndingWith(SearchField.class, "a");
```
Searches the component by its name containing the specified text.
### createByAttributesContaining ###
```java
component.createByAttributeContaining(Anchor.class, "data-product_id", "31");
```
Searches the component by some of its attribute containing the specified value.

Available createAll Methods for Finding Nested Components
----------------------------------------------------
### createAllByIdEndingWith ###
```java
component.createAllByIdEndingWith(Anchor.class, "myIdSuffix");
```
Searches the components by ID ending with the locator.
### createAllByTag ###
```java
component.createAllByTag(Anchor.class, "a");
```
Searches the components by their tag.
### createAllById ###
```java
component.createAllById(Button.class, "myId");
```
Searches the components by their ID.
### createAllByIdContaining ###
```java
component.createAllByIdContaining(Button.class, "myIdMiddle");
```
Searches the components by ID containing the specified text.
### createAllByValueContaining ###
```java
component.createAllByValueContaining(Button.class, "pay");
```
Searches the components by ID containing the specified text.
### createAllByXpath ###
```java
component.createAllByXpath(Button.class, "//*[@title='Add to cart']");
```
Searches the components by XPath locator.
### createAllByLinkText ###
```java
component.createAllByLinkText(Anchor.class, "blog");
```
Searches the components by their link (href).
### createAllByLinkTextContaining ###
```java
component.createAllByLinkTextContaining(Anchor.class, "blog");
```
Searches the components by their link (href) if it contains specified value.
### createAllByCss ###
```java
component.createAllByCss(Anchor.class, "a[href*='falcon-9']");
```
Searches the components by their CSS selector.
### createAllByClass ###
```java
component.createAllByClass(Anchor.class, "product selected");
```
Searches the components by their CSS classes.
### createAllByClassContaining ###
```java
component.createAllByClassContaining(Anchor.class, "selected");
```
Searches the components by their CSS classes.
### createAllByInnerTextContaining ###
```java
component.createAllByInnerTextContaining(Div.class, "Showing all");
```
Searches the components by their inner text content, including all child HTML components.
### createAllByNameEndingWith ###
```java
component.createAllByNameEndingWith(SearchField.class, "a");
```
Searches the components by their name containing the specified text.
### createAllByAttributesContaining ###
```java
component.createAllByAttributeContaining(Anchor.class, "data-product_id", "31");
```
Searches the components by some of their attribute containing the specified value.
