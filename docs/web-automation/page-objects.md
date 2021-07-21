---
layout: default
title:  "Page Objects"
excerpt: "Learn how to use the BELLATRIX page objects."
date:   2018-06-22 06:50:17 +0200
parent: web-automation
permalink: /web-automation/page-objects/
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
As you most probably noticed this is like the 4th time we use almost the same elements and logic inside our tests. Similar test writing approach leads to unreadable and hard to maintain tests.
Because of that people use the so-called Page Object design pattern to reuse their elements and pages' logic. BELLATRIX comes with powerful built-in page objects which are much more readable and maintainable than regular vanilla WebDriver ones.

Non-page-object Test Example
----------------------------
```java
@Test
public void purchaseRocketWithoutPageObjects() {
    app().navigate().to("http://demos.bellatrix.solutions/");

    // Home page elements
    Select sortDropDown = app().create().byNameEndingWith(Select.class, "orderby");
    Anchor protonMReadMoreButton = app().create().byInnerTextContaining(Anchor.class, "Read more");
    Anchor addToCartFalcon9 = app().create().byAttributeContaining(Anchor.class, "data-product_id", "28").toBeClickable();
    Anchor viewCartButton = app().create().byClassContaining(Anchor.class, "added_to_cart wc-forward").toBeClickable();

    // Home Page actions
    sortDropDown.selectByText("Sort by price: low to high");
    protonMReadMoreButton.hover();
    addToCartFalcon9.focus();
    addToCartFalcon9.click();
    viewCartButton.click();

    // Cart page elements
    TextField couponCodeTextField = app().create().byId(TextField.class, "coupon_code");
    Button applyCouponButton = app().create().byValueContaining(Button.class, "Apply coupon");
    Div messageAlert = app().create().byClassContaining(Div.class, "woocommerce-message");
    NumberInput quantityBox = app().create().byClassContaining(NumberInput.class, "input-text qty text");
    Button updateCart = app().create().byClassContaining(Button.class, "Update cart").toBeClickable();
    Span totalSpan = app().create().byXPath(Span.class, "//*[@class='order-total']//span");
    Anchor proceedToCheckout = app().create().byClassContaining(Anchor.class, "checkout-button button alt wc-forward");

    // Cart page actions
    couponCodeTextField.setText("happybirthday");
    applyCouponButton.click();
    messageAlert.toBeVisible().waitToBe();
    messageAlert.validateTextIs("Coupon code applied successfully.");

    quantityBox.setNumber(0);
    quantityBox.setNumber(2);
    updateCart.click();
    totalSpan.validateTextIs("95.00€");
    proceedToCheckout.click();

    // Checkout page elements
    Heading billingDetailsHeading = app().create().byInnerTextContaining(Heading.class, "Billing details");
    Anchor showLogin = app().create().byInnerTextContaining(Anchor.class, "Click here to login");
    TextArea orderCommentsTextArea = app().create().byId(TextArea.class, "order_comments");
    TextField billingFirstName = app().create().byId(TextField.class, "billing_first_name");
    TextField billingLastName = app().create().byId(TextField.class, "billing_last_name");
    TextField billingCompany = app().create().byId(TextField.class, "billing_company");
    Select billingCountry = app().create().byId(Select.class, "billing_country");
    TextField billingAddress1 = app().create().byId(TextField.class, "billing_address_1");
    TextField billingAddress2 = app().create().byId(TextField.class, "billing_address_2");
    TextField billingCity = app().create().byId(TextField.class, "billing_city");
    Select billingState = app().create().byId(Select.class, "billing_state").toBeVisible().toBeClickable();
    TextField billingZip = app().create().byId(TextField.class, "billing_postcode");
    PhoneInput billingPhone = app().create().byId(PhoneInput.class, "billing_phone");
    EmailInput billingEmail = app().create().byId(EmailInput.class, "billing_email");
    CheckBox createAccountCheckBox = app().create().byId(CheckBox.class, "createaccount");
    RadioButton checkPaymentsRadioButton = app().create().byAttributeContaining(RadioButton.class, "for", "payment_method_cheque");

    // Checkout page actions
    billingDetailsHeading.toBeVisible().waitToBe();
    showLogin.validateHrefIs("http://demos.bellatrix.solutions/checkout/#");
    showLogin.validateClassIs("showlogin");
    orderCommentsTextArea.scrollToVisible();
    orderCommentsTextArea.setText("Please send the rocket to my door step!");
    billingFirstName.setText("In");
    billingLastName.setText("Deepthought");
    billingCompany.setText("Automate The Planet Ltd.");
    billingCountry.selectByText("Bulgaria");
    billingAddress1.validatePlaceholderIs("House number and street name");
    billingAddress1.setText("bul. Yerusalim 5");
    billingAddress2.setText("bul. Yerusalim 6");
    billingCity.setText("Sofia");
    billingState.selectByText("Sofia-Grad");
    billingZip.setText("1000");
    billingPhone.setPhone("+00359894646464");
    billingEmail.setEmail("info@bellatrix.solutions");
    createAccountCheckBox.check();
    checkPaymentsRadioButton.click();
}
```

How to Create BELLATRIX Page Object
-----------------------------------
- On most pages, you need to define elements. Placing them in a single place makes the changing of the locators easy. It is a matter of choice whether to have action methods or not. If you use the same combination of same actions against a group of elements then it may be a good idea to wrap them in a page object action method. In our example, we can wrap the filling the billing info such a method. 
- In the assertions file, we may place some predefined **validate** methods. For example, if you always check the same email or title of a page, there is no need to hardcode the string in each test. Later if the title is changed, you can do it in a single place. The same is true about most of the things you can assert in your tests.

Page Object Example
-------------------
### Page Methods File ###
```java
public class CartPage extends WebPage<Map, Asserts> {
    @Override
    protected String getUrl() {
        return "http://demos.bellatrix.solutions/cart/";
    }

    @Override
    protected void waitForPageLoad() {
        map().couponCodeTextField().toExist().waitToBe();
    }

    public void applyCoupon(String coupon) {
        map().couponCodeTextField().setText(coupon);
        map().applyCouponButton().click();
        browser().waitForAjax();
    }

    public void increaseProductQuantity(int newQuantity) {
        map().quantityBox().setText(String.valueOf(newQuantity));
        map().updateCart().click();
        browser().waitForAjax();
    }

    public void clickProceedToCheckout() {
        map().proceedToCheckout().click();
        browser().waitUntilPageLoadsCompletely();
    }

    public String getTotal() {
        return map().totalSpan().getText();
    }

    public String getMessageNotification() {
        return map().messageAlert().getText();
    }
}
```
### Page Element Map File ###
```java
public class Map extends PageMap {
    public TextField couponCodeTextField() {
        return create().byId(TextField.class, "coupon_code");
    }

    public Button applyCouponButton() {
        return create().byValueContaining(Button.class, "Apply coupon");
    }

    public List<TextField> quantityBoxes() {
        return create().allByClassContaining(TextField.class, "input-text qty text");
    }

    public Button updateCart() {
        return create().byValueContaining(Button.class, "Update cart");
    }

    public Div messageAlert() {
        return create().allByClassContaining(Div.class, "woocommerce-message");
    }

    public Span totalSpan() {
        return create().byXPath(Span.class, "//*[@class='order-total']//span");
    }

    public Button proceedToCheckout() {
        return create().allByClassContaining(Button.class, "checkout-button button alt wc-forward");
    }
}
```
### Page Asserts File ###
```java
public class Asserts extends PageAsserts<Map> {
    public void couponAppliedSuccessfully() {
        map().messageAlert().validateTextIs("Coupon code applied successfully.");
    }

    public void totalPrice(String expectedPrice) {
        map().totalSpan().validateTextIs(expectedPrice + "€");
    }
}
```

Page Object Example Explanations
--------------------------------
```csharp
public class CartPage extends WebPage<Map, Asserts>
```
All BELLATRIX page objects are implemented as a package with three different classes which means that you have separate files for different parts of it – actions, elements and assertions. This makes the maintainability and readability of these classes much better. Also, you can easily locate what you need. You can always create BELLATRIX page objects yourself by extending the WebPage class.
```java
@Override
protected String getUrl() {
    return "http://demos.bellatrix.solutions/cart/";
}
```
Overriding the **getUrl** method that comes from the base page object you can later you the **open** method to go to the page.
```java
public void applyCoupon(String coupon) {
    map().couponCodeTextField().setText(coupon);
    map().applyCouponButton().click();
    browser().waitForAjax();
}
```
Elements are accessed through the **map** method. These elements are always used together when coupon is applied. There are many test cases where you need to apply different coupons and so on. This way you reuse the code instead of copy-paste it. If there is a change in the way how the coupon is applied, change the workflow only here. Even single line of code is changed in your tests.
```java
public void increaseProductQuantity(int productNumber, int newQuantity) {
    if (productNumber > map().quantityBoxes().size()) {
        throw new IllegalArgumentException("There are less added items in the cart. Please specify smaller product number.");
    }

    map().quantityBoxes().get(productNumber - 1).setText(String.valueOf(newQuantity));
    map().updateCart().click();
    browser().waitForAjax();
}
```
Another method that we can add here is the one for updating the quantity of a product. This is an excellent place to put validations in your code. Here we make sure that the specified number of products that we want to update exists.
**create().allBy** method returns a List<TComponent> in this case List<TextField>. We can use all the default List methods here – in our case, we use get() and size() methods.
```java
for (var quantityBox : map().quantityBoxes()){
    quantityBox.setText(String.valueOf(newQuantity));
}
```
Since List<E> implements Collection<E> interface, we can use it directly in foreach statements.
```java
public TextField couponCodeTextField() {
    return create().byId(TextField.class, "coupon_code");
}
```
All elements are placed inside the file **Map** so that the declarations of your elements to be in a single place. It is convenient since if there is a change in some of the locators or elements types you can apply the fix only here. All elements are implements as properties.
```java
public List<TextField> quantityBoxes() {
    return create().allByClassContaining(TextField.class, "input-text qty text");
}
```
If you want to find multiple elements, you can use List<TComponent>.
```java
public void totalPrice(String expectedPrice) {
    map().totalSpan().validateTextIs(expectedPrice + "€");
}
```
In the **Asserts** file, we have a method called **totalPrice**. We can access it through the **asserts** method, just like we used the **map** method above. With this validation, reuse the formatting of the currency. Also, since the method is called from the page it makes your tests a little bit more readable. If there is a change what needs to be checked --> for example, not span but different element you can change it in a single place.

Page Object Test Example
------------------------
```java
@Test
public void purchaseRocketWithPageObjects() {
    var homePage = app().goTo(MainPage.class);
    homePage.addRocketToShoppingCart("Falcon 9");

    var cartPage = app().create(CartPage.class);

    cartPage.applyCoupon("happybirthday");
    cartPage.increaseProductQuantity(1, 2);
    cartPage.asserts().totalPrice("95.00");
    cartPage.map().proceedToCheckout().click();

    var purchaseInfo = new PurchaseInfo();
    purchaseInfo.setFirstName("In");
    purchaseInfo.setLastName("Deepthought");
    purchaseInfo.setCompany("Automate The Planet Ltd.");
    purchaseInfo.setCountry("Bulgaria");
    purchaseInfo.setAddress1("bul. Yerusalim 5");
    purchaseInfo.setAddress2("bul. Yerusalim 6");
    purchaseInfo.setCity("Sofia");
    purchaseInfo.setZip("1000");
    purchaseInfo.setPhone("+00359894646464");
    purchaseInfo.setEmail("info@bellatrix.solutions");
    purchaseInfo.setShouldCreateAccount(true);

    var checkoutPage = app().create(CheckoutPage.class);
    checkoutPage.fillBillingInfo(purchaseInfo);
    checkoutPage.map().checkPaymentsRadioButton().click();
}
```

Page Object Test Example Explanations
-------------------------------------
```java
var homePage = app().goTo(MainPage.class);
```
You can use the **app().goTo** method to navigate to the page and get an instance of it.
```java
homePage.addRocketToShoppingCart("Falcon 9");
```
After you have the instance, you can directly start using the action methods of the page. As you can see the test became much shorter and more readable. The additional code pays off in future when changes are made to the page, or you need to reuse some of the methods.
```java
var cartPage = app().create(CartPage.class);
```
Navigate to the shopping cart page by clicking the view cart button, so we do not have to call the GoTo method. But we still need an instance. We can get only an instance of the page through the **app().create** method.
```java
cartPage.applyCoupon("happybirthday");
cartPage.increaseProductQuantity(1, 2);
cartPage.asserts().totalPrice("95.00");
cartPage.map().proceedToCheckout().click();
```
Removing all elements and some implementation details from the test made it much more clear and readable. This is one of the strategies to follow for long-term successful automated testing.
```csharp
var purchaseInfo = new PurchaseInfo();
purchaseInfo.setFirstName("In");
purchaseInfo.setLastName("Deepthought");
purchaseInfo.setCompany("Automate The Planet Ltd.");
purchaseInfo.setCountry("Bulgaria");
purchaseInfo.setAddress1("bul. Yerusalim 5");
purchaseInfo.setAddress2("bul. Yerusalim 6");
purchaseInfo.setCity("Sofia");
purchaseInfo.setZip("1000");
purchaseInfo.setPhone("+00359894646464");
purchaseInfo.setEmail("info@bellatrix.solutions");
purchaseInfo.setShouldCreateAccount(true);
```
You can move the creation of the data objects in a separate factory method or class.