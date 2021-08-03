---
layout: default
title:  "Web Components"
excerpt: "Learn how to use BELLATRIX web components."
date:   2018-06-22 06:50:17 +0200
parent: web-automation
permalink: /web-automation/web-components/
anchors:
  example: Example
  explanations: Explanations
  full-list-of-all-supported-web-components: List of All Web Components
---
Example
-------
```java
@Test
public void purchaseRocket() {
    app().navigate().to("http://demos.bellatrix.solutions/");

    Select sortDropDown = app().create().byNameEndingWith(Select.class, "orderby");
    sortDropDown.selectByText("Sort by price: low to high");

    Anchor protonMReadMoreButton =
    app().create().byInnerTextContaining(Anchor.class, "Read more");

    protonMReadMoreButton.hover();

    Anchor addToCartFalcon9 =
    app().create().byAttributeContaining(Anchor.class, "data-product_id", "28").toBeClickable();
    addToCartFalcon9.focus();
    addToCartFalcon9.click();

    Anchor viewCartButton =
    app().create().byClassContaining(Anchor.class, "added_to_cart wc-forward").toBeClickable();
    viewCartButton.click();

    TextField couponCodeTextField = app().create().byId(TextField.class, "coupon_code");

    couponCodeTextField.setText("happybirthday");

    Button applyCouponButton = app().create().byValueContaining(Button.class, "Apply coupon");
    applyCouponButton.click();

    Div messageAlert = app().create().byClassContaining(Div.class, "woocommerce-message");

    messageAlert.toBeVisible().waitToBe();

    messageAlert.validateTextIs("Coupon code applied successfully.");

    NumberInput quantityBox = app().create().byClassContaining(NumberInput.class, "input-text qty text");

    quantityBox.setNumber(0);
    quantityBox.setNumber(2);

    Button updateCart = app().create().byValueContaining(Button.class, "Update cart").toBeClickable();
    updateCart.click();

    Span totalSpan = app().create().byXPath(Span.class, "//*[@class='order-total']//span");

    totalSpan.validateTextIs("95.00€");

    Anchor proceedToCheckout =
    app().create().byClassContaining(Anchor.class, "checkout-button button alt wc-forward");
    proceedToCheckout.click();

    Heading billingDetailsHeading = app().create().byInnerTextContaining(Heading.class, "Billing details");

    billingDetailsHeading.toBeVisible().waitToBe();

    Anchor showLogin = app().create().byInnerTextContaining(Anchor.class, "Click here to login");

    showLogin.validateHrefIs("http://demos.bellatrix.solutions/checkout/#");
    showLogin.validateClassIs("showlogin");

    TextArea orderCommentsTextArea = app().create().byId(TextArea.class, "order_comments");

    orderCommentsTextArea.scrollToVisible();
    orderCommentsTextArea.setText("Please send the rocket to my door step!");

    TextField billingFirstName = app().create().byId(TextField.class, "billing_first_name");
    billingFirstName.setText("In");

    TextField billingLastName = app().create().byId(TextField.class, "billing_last_name");
    billingLastName.setText("Deepthought");

    TextField billingCompany = app().create().byId(TextField.class, "billing_company");
    billingCompany.setText("Automate The Planet Ltd.");

    Select billingCountry = app().create().byId(Select.class, "billing_country");
    billingCountry.selectByText("Bulgaria");

    TextField billingAddress1 = app().create().byId(TextField.class, "billing_address_1");

    Assert.assertEquals(billingAddress1.getPlaceholder(), "House number and street name");
    billingAddress1.setText("bul. Yerusalim 5");

    TextField billingAddress2 = app().create().byId(TextField.class, "billing_address_2");
    billingAddress2.setText("bul. Yerusalim 6");

    TextField billingCity = app().create().byId(TextField.class, "billing_city");
    billingCity.setText("Sofia");

    Select billingState =
    app().create().byId(Select.class, "billing_state").toBeVisible().toBeClickable();
    billingState.selectByText("Sofia-Grad");

    TextField billingZip = app().create().byId(TextField.class, "billing_postcode");
    billingZip.setText("1000");

    PhoneField billingPhone = app().create().byId(PhoneField.class, "billing_phone");

    billingPhone.setPhone("+00359894646464");

    EmailField billingEmail = app().create().byId(EmailField.class, "billing_email");

    billingEmail.setEmail("info@bellatrix.solutions");

    CheckBox createAccountCheckBox = app().create().byId(CheckBox.class, "createaccount");

    createAccountCheckBox.check();

    RadioButton checkPaymentsRadioButton =
    app().create().byAttributeContaining(RadioButton.class, "for", "payment_method_cheque");

    checkPaymentsRadioButton.click();
}
```

Explanations
------------
As mentioned before BELLATRIX exposes 30+ web components. All of them implement Proxy design pattern which means that they are not located immediately when they are created. Another benefit is that each of them includes only the actions that you should be able to do with the specific control and nothing more.
For example, you cannot type into a button. Moreover, this way all of the actions has meaningful names – **Type** not **SendKeys** as in vanilla **WebDriver**.
```java
Select sortDropDown = app().create().byNameEndingWith(Select.class, "orderby");
```
Create methods accept a generic parameter the type of the web control. Then only the methods for this specific control are accessible. Here we tell BELLATRIX to find your component by name attribute ending with 'orderby'.

```html
<select name="orderby" class="orderby">
   <option value="popularity" selected="selected">Sort by popularity</option>
   <option value="rating">Sort by average rating</option>
   <option value="date">Sort by newness</option>
   <option value="price">Sort by price: low to high</option>
   <option value="price-desc">Sort by price: high to low</option>
</select>
```
```java
sortDropDown.selectByText("Sort by price: low to high");
```
You can select from select inputs by text (**selectByText**) or index (**selectByIndex**). Also, you can get the selected option through **getSelected** method.
```html
<a href='http://demos.bellatrix.solutions/product/proton-m/'>Read more</a>
```
```java
Anchor protonMReadMoreButton = app().create().byInnerTextContaining(Anchor.class, "Read more");
```
Here BELLATRIX finds the first anchor component which has inner text containing the 'Read more' text.
```java
protonMReadMoreButton.hover();
```
You can Hover and Focus on most web components. Also, can invoke Click on anchors.
```html
<a href="/?add-to-cart=28" data-product_id="28">Add to cart</a>
```
```java
Anchor addToCartFalcon9 = app().create().byAttributeContaining(Anchor.class, "data-product_id". "28").toBeClickable();
addToCartFalcon9.focus();
addToCartFalcon9.click();
```
Locate components by custom attribute. BELLATRIX waits till the anchor is clickable before doing any actions.
```html
<a href="http://demos.bellatrix.solutions/cart/" class="added_to_cart wc-forward" title="View cart">View cart</a>
```
```java
Anchor viewCartButton =
app().create().byClassContaining(Anchor.class, "added_to_cart wc-forward").toBeClickable();
viewCartButton.click();
```
Find the anchor by class 'added_to_cart wc-forward' and wait for the component again to be clickable.
```java
TextField couponCodeTextField = app().create().byId(TextField.class, "coupon_code");
```
Find a regular text input component by id = 'coupon_code'.
```java
couponCodeTextField.setText("happybirthday");
```
Instead of using vanilla WebDriver SendKeys to set the text, use the SetText method.
```html
<input type="submit" class="button" name="apply_coupon" value="Apply coupon">
```
```java
Button applyCouponButton = app().create().byValueContaining(Button.class, "Apply coupon");
applyCouponButton.click();
```
Create a button control by value attribute containing the text 'Apply coupon'. Button can be any of the following web components – input button, input submit or button.
```html
<div class="woocommerce-message" role="alert">Coupon code applied successfully.</div>
```
```java
Div messageAlert = app().create().byClassContaining(Div.class, "woocommerce-message");
messageAlert.toBeVisible().waitToBe();
```
Wait for the message DIV to show up.
```java
// Assert.assertEquals(billingAddress1.getPlaceholder(), "House number and street name");
```
Sometimes you need to verify the content of some component. However, since the asynchronous nature of websites, the text or event may not happen immediately. This makes the simple **Assert** methods + vanilla **WebDriver** useless.
The commented code fails 1 from 5 times.
To handle these situations, BELLATRIX has hundreds of Ensure methods that wait for some condition to happen before asserting.
Bellow the statement waits for the specific text to appear and assert it.
```java
Div messageAlert = app().create().byClassContaining(Div.class, "woocommerce-message");
messageAlert.validateTextIs("Coupon code applied successfully.");
```
**Note**: *There are much more details about these methods in the next chapters.*
```html
<input type="number" id="quantity_5ad35e76b34a2" step="1" min="0" max="" value="1" size="4" pattern="[0-9]*" inputmode="numeric">
```
```java
NumberInput quantityBox = app().create().byClassContaining(NumberInput.class, "input-text qty text");
```
Find the number input component by class 'input-text qty text'.
```java
NumberInput quantityBox = app().create().byClassContaining(NumberInput.class, "input-text qty text");
quantityBox.setNumber(0);
quantityBox.setNumber(2);
```
For number input components, you can set the number and get most of the properties of these components.
```java
Heading billingDetailsHeading = app().create().byInnerTextContaining(Heading.class, "Billing details");
```
As mentioned before, BELLATRIX has special synchronization mechanism for locating components, so usually, there is no need to wait for specific components to appear on the page. However, there may be some rare cases when you need to do it. The statement finds the heading by its inner text containing the text 'Billing details'.
```java
Heading billingDetailsHeading = app().create().byInnerTextContaining(Heading.class, "Billing details");
billingDetailsHeading.toBeVisible().waitToBe();
```
Wait for the heading with the above text to be visible. This means that the correct page is loaded.
```java
// Assert.assertEquals(showLogin.getHref(), "http://demos.bellatrix.solutions/checkout/#");
showLogin.validateHrefIs("http://demos.bellatrix.solutions/checkout/#");
// Assert.assertEquals(showLogin.getHtmlClass(), "showlogin");
showLogin.validateClassIs("showlogin");
```
All web components have multiple properties for their most important attributes and ensure methods for their verification.
```java
TextArea orderCommentsTextArea = app().create().byId(TextArea.class, "order_comments");
orderCommentsTextArea.scrollToVisible();
```
Here we find the order comments text area and since it is below the visible area we scroll down so that it gets visible on the video recordings. Then the text is set.
```java
TextField billingAddress1 = app().create().byId(TextField.class, "billing_address_1");
Assert.assertEquals(billingAddress1.getPlaceholder(), "House number and street name");
```
Through the Placeholder, you can get the default text of the control.
```java
PhoneField billingPhone = app().create().byId(PhoneField.class, "billing_phone");
billingPhone.setPhone("+00359894646464");
```
Create the special text field control Phone it contains some additional properties unique for this web component.
```java
EmailField billingEmail = app().create().byId(EmailField.class, "billing_email");
billingEmail.setEmail("info@bellatrix.solutions");
```
Here we create the special text field control Email it contains some additional properties unique for this web component.
```java
CheckBox createAccountCheckBox = app().create().byId(CheckBox.class, "createaccount");
createAccountCheckBox.check();
```
You can check and uncheck checkboxes.
```java
RadioButton checkPaymentsRadioButton =
app().create().byAttributeContaining(RadioButton.class, "for", "payment_method_cheque");
checkPaymentsRadioButton.click();
```
BELLATRIX finds the first RadioButton with attribute 'for' containing the value 'payment_method_cheque'. The radio buttons compared to checkboxes cannot be unchecked/unselected.

Full List of All Supported Web Components
---------------------------------------
### WebComponent ###
- createBy
- createAllBy
- getAttribute
- setAttribute 
- scrollToVisible
- waitToBe
- isVisible
- getHtmlClass
- getComponentName
- getHtmlClass
- getCssValue
- getTitle
- getTabIndex
- getAccessKey
- getStyle
- getDir
- getLang
- hover
- focus

**Note**: *All other components have access to the above methods and properties*

Component | Available properties
------------ | -------------
Anchor | click, getHtml, getText, getHref, getTarget, getRel
Button | click, getValue, isDisabled, getText
CheckBox | check, uncheck, isChecked, isDisabled, getValue
Div | getHtml, getText
FileInput | upload, isRequired, isMultiple, getAccept
Frame | getName
Heading | getText
Image | getSrc, getAlt, getHeight, getWidth, getSrcSet, getSizes, getLongDesc
Label | getText, getHtml, getFor
Option | isSelected, isDisabled, getText, getValue
RadioButton | click, isChecked, isDisabled, getValue
Reset | click, isDisabled, getText, getValue
Select | getSelected, getAllOptions, selectByText, selectByIndex, isDisabled, isReadonly, isRequired
Span | getText, getHtml
TextArea | getText, setText, isDisabled, isAutoComplete, isSpellCheck, isReadonly, isRequired, getPlaceholder, getMaxLength, getMinLength, getRows, getCols, getWrap
TextField | getText, setText isDisabled, isAutoComplete, isReadonly, isRequired, getPlaceholder, getMaxLength, getMinLength


### HTML5 Components: ### 

Component | Available properties
------------ | -------------
ColorInput | getColor, setColor, isAutoComplete, isDisabled, isRequired, getList, getValue
DateInput | getDate, setDate, isAutoComplete, isDisabled, isReadonly, isRequired, getMin, getMax, getStep, getValue
DateTimeInput | getTime, setTime, isAutoComplete, isDisabled, isReadonly, isRequired, getMin, getMax, getStep, getValue
EmailField | getEmail, setEmail, isAutoComplete, isDisabled, isReadonly, isRequired, getMinLength, getMaxLength, getPlaceholder, getSizeAttribute, getValue
MonthInput | getMonth, setMonth, isDisabled, isAutoComplete, isReadonly, isRequired, getMax, getMin, getStep, getValue
NumberInput | getNumber, setNumber, isDisabled, isAutoComplete, isReadonly, isRequired, getPlaceholder, getMax, getMin, getStep, getValue
Output | getText, getHtml, getFor
PasswordField | getPassword, setPassword, isDisabled, isAutoComplete, isReadonly, isRequired, getPlaceholder, getMaxLenght, getMinLenght, getSize, getValue
PhoneField | getPhone, setPhone, isDisabled, isAutoComplete, isReadonly, isRequired, getPlaceholder, getMaxLenght, getMinLenght, getSize, getValue
Progress | getMax, getText, getValue
RangeInput | getRange, setRange, isDisabled, isAutoComplete, isRequired, getList, getMax, getMin, getStep, getValue
SearchField | getSearch, setSearch, IsDisabled, isAutoComplete, isReadonly, isRequired, getPlaceholder, getMaxLenght, getMinLenght, getSize, getValue
TimeInput | getTime, setTime, getHover, getFocus, isDisabled, isAutoComplete, isReadonly, getMax, getMin, getStep, getValue
UrlField | getUrl, setUrl, isDisabled, isAutoComplete, isReadonly, isRequired, getPlaceholder, getMaxLenght, getMinLenght, getSize, getValue
WeekInput | getWeek, setWeek, isDisabled, isAutoComplete, isReadonly, getMax, getMin, getStep, getValue

