---
layout: default
title:  "Behaviour Driven Development (BDD) Logging"
excerpt: "Learn the BELLATRIX Behaviour Driven Development (BDD) Logging works and how to use it."
date:   2018-06-2s 06:50:17 +0200
parent: web-automation
permalink: /web-automation/bdd-logging/
anchors:
  example: Example
  explanations: Explanations
---
Example
-------
```java
@Test
public void purchaseRocketWithLogs() {
    app().navigate().to("http://demos.bellatrix.solutions/");

    Select sortDropDown = app().create().byNameEndingWith(Select.class, "orderby");
    Anchor protonMReadMoreButton = app().create().byInnerTextContaining(Anchor.class, "Read more");
    Anchor addToCartFalcon9 =
    app().create().byAttributeContaining(Anchor.class, "data-product_id", "28").toBeClickable();
    Anchor viewCartButton =
    app().create().byClassContaining(Anchor.class, "added_to_cart wc-forward").toBeClickable();

    sortDropDown.selectByText("Sort by price: low to high");
    protonMReadMoreButton.hover();
    addToCartFalcon9.focus();
    addToCartFalcon9.click();
    viewCartButton.click();

    TextField couponCodeTextField = app().create().byId(TextField.class, "coupon_code");
    Button applyCouponButton = app().create().byValueContaining(Button.class, "Apply coupon");
    Div messageAlert = app().create().byClassContaining(Div.class, "woocommerce-message");
    NumberInput quantityBox = app().create().byClassContaining(NumberInput.class, "input-text qty text");
    Button updateCart = app().create().byValueContaining(Button.class, "Update cart").toBeClickable();
    Span totalSpan = app().create().byXPath(Span.class, "//*[@class='order-total']//span");
    Anchor proceedToCheckout =
    app().create().byClassContaining(Anchor.class, "checkout-button button alt wc-forward");

    couponCodeTextField.setText("happybirthday");
    applyCouponButton.click();
    messageAlert.toBeVisible().waitToBe();
    messageAlert.validateTextIs("Coupon code applied successfully.");
    quantityBox.setNumber(0);
    quantityBox.setNumber(2);
    updateCart.click();
    totalSpan.validateTextIs("95.00€");
    proceedToCheckout.click();

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
    RadioButton checkPaymentsRadioButton =
    app().create().byAttributeContaining(RadioButton.class, "for", "payment_method_cheque");

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

Explanations
------------
There are cases when you need to show your colleagues or managers what tests do you have. Sometimes you may have manual test cases, but their maintenance and up-to-date state are questionable. Also, many times you need additional work to associate the tests with the test cases. Some frameworks give you a way to write human readable tests through the Gherkin language. The main idea is non-technical people to write these tests. However, we believe this approach is doomed. Or it is doable only for simple tests. This is why in BELLATRIX we built a feature that generates the test cases after the tests execution. After each action or assertion, a new entry is logged.

After the test is executed the following log is created:

```
Start Test
class = BDDLoggingTests name = purchaseRocketWithLogs
selecting 'Sort by price: low to high' from Select (name ending with orderby)
hovering Anchor (text containing Read more)
focusing Anchor (data-product_id containing 28)
clicking Anchor (data-product_id containing 28)
clicking Anchor (class containing added_to_cart wc-forward)
typing 'happybirthday' into TextField (id = coupon_code)
clicking Button (value containing Apply coupon)
validating Div (class containing woocommerce-message) inner text is 'Coupon code applied successfully.'
setting '0' into NumberInput (class = input-text qty text)
setting '2' into NumberInput (class = input-text qty text)
clicking Button (value containing Update cart)
validating Span (xpath = //*[@class='order-total']//span) inner text is '95.00€'
clicking Anchor (class = checkout-button button alt wc-forward)
validating Anchor (text containing Click here to login) href is 'http://demos.bellatrix.solutions/checkout/#'
validating Anchor (text containing Click here to login) class is 'showlogin'
scroll to visible TextArea (id = order_comments)
typing 'Please send the rocket to my door step!' into TextArea (id = order_comments)
typing 'In' into TextField (id = billing_first_name)
typing 'Deepthought' into TextField (id = billing_last_name)
typing 'Automate The Planet Ltd.' into TextField (id = billing_company)
selecting 'Bulgaria' from Select (id = billing_country)
validating TextField (id = billing_address_1) placeholder is 'House number and street name'
typing 'bul. Yerusalim 5' into TextField (id = billing_address_1)
typing 'bul. Yerusalim 6' into TextField (id = billing_address_2)
typing 'Sofia' into TextField (id = billing_city)
selecting 'Sofia-Grad' from Select (id = billing_state)
typing '1000' into TextField (id = billing_postcode)
typing '+00359894646464' into TextField (id = billing_phone)
typing 'info@bellatrix.solutions' into TextField (id = billing_email)
checking CheckBox (id = createaccount)
clicking RadioButton (for containing payment_method_cheque)
```

You can notice that since we use **validate** assertions not the regular one they also present in the log:
```
validating Span (xpath = //*[@class='order-total']//span) inner text is '95.00€'
```

There are two specifics about the generation of the logs. If page objects are used, which are discussed in next chapters. Two things change.
1. Instead of locators, the exact names of the element properties are printed.
2. Instead of page URL, the name of the page is displayed.