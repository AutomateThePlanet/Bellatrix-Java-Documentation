---
layout: default
title:  "Custom Code"
excerpt: "Custom Code"
date:   2018-02-20 06:50:17 +0200
permalink: /customcode/
---
Bellatrix Test Automation Framework 
---------------------------------------------------------

Java Test
```java
@ExecutionBrowser(browser = Browser.CHROME, lifecycle = Lifecycle.REUSE_IF_STARTED)
public class ProductPurchaseTests extends WebTest {
    @Test
    public void sandbox() {
        app().navigate().to("https://www.w3schools.com/tags/tryit.asp?filename=tryhtml5_input_type_checkbox");
        var frame = app().create().byId(Frame.class,"iframeResult");
        app().browser().switchToFrame(frame);
        var checkbox1 = app().create().byId(CheckBox.class,"vehicle1");
        var checkbox2 = app().create().byId(CheckBox.class,"vehicle2");
        var checkbox3 = app().create().byId(CheckBox.class,"vehicle3");
        checkbox3.check();
        checkbox1.validateIsUnchecked();
        checkbox2.validateIsUnchecked();
        checkbox3.validateIsChecked();
    }

    @Test
    public void completePurchaseSuccessfully_first() {
        app().navigate().to("http://demos.bellatrix.solutions/");
        var addToCartFalcon9 = app().create().byCss(Anchor.class,"[data-product_id*='28']");
        var blogLink = app().create().by(Anchor.class, TextContains.by("Blog"));
        addToCartFalcon9.click();
//        blogLink.layout().assertAboveOf(addToCartFalcon9);
        new MainPage().asserts().productBoxLink("", "");
    }

    @Test
    public void completePurchaseSuccessfully_second() {
        app().navigate().to("http://demos.bellatrix.solutions/");
        var addToCartFalcon9 = app().create().byCss(Anchor.class,"[data-product_id*='28']");
        addToCartFalcon9.click();
    }

    @Test
    public void falcon9LinkAddsCorrectProduct() {
        var mainPage = app().goTo(MainPage.class);

        mainPage.asserts().productBoxLink("Falcon 9", "http://demos.bellatrix.solutions/product/falcon-9/");
    }

    @Test
    public void saturnVLinkAddsCorrectProduct() {
        var mainPage = app().goTo(MainPage.class);

        mainPage.asserts().productBoxLink("Saturn V", "http://demos.bellatrix.solutions/product/saturn-v/");
    }

    @Test
    public void purchaseFalcon9WithoutFacade() throws InterruptedException {
        var mainPage = app().goTo(MainPage.class);
        mainPage.addRocketToShoppingCart("Falcon 9");

        var cartPage = app().create(CartPage.class);
        cartPage.applyCoupon("happybirthday");
        cartPage.asserts().couponAppliedSuccessfully();
        cartPage.increaseProductQuantity(2);
        cartPage.asserts().totalPrice("114.00€");
        cartPage.clickProceedToCheckout();

        var purchaseInfo = new PurchaseInfo();
        purchaseInfo.setEmail("info@berlinspaceflowers.com");
        purchaseInfo.setFirstName("Anton");
        purchaseInfo.setLastName("Angelov");
        purchaseInfo.setCompany("Space Flowers");
        purchaseInfo.setCountry("Germany");
        purchaseInfo.setAddress1("1 Willi Brandt Avenue Tiergarten");
        purchaseInfo.setAddress2("Lьtzowplatz 17");
        purchaseInfo.setCity("Berlin");
        purchaseInfo.setZip("10115");
        purchaseInfo.setPhone("+00498888999281");

        var checkoutPage = app().create(CheckoutPage.class);
        checkoutPage.fillBillingInfo(purchaseInfo);
        checkoutPage.asserts().orderReceived();
    }

    @Test
    public void purchaseSaturnVWithoutFacade() throws InterruptedException {
        var mainPage = app().goTo(MainPage.class);
        mainPage.addRocketToShoppingCart("Saturn V");

        var cartPage = app().create(CartPage.class);
        cartPage.applyCoupon("happybirthday");
        cartPage.asserts().couponAppliedSuccessfully();
        cartPage.increaseProductQuantity(3);
        cartPage.asserts().totalPrice("355.00€");
        cartPage.clickProceedToCheckout();

        var purchaseInfo = new PurchaseInfo();
        purchaseInfo.setEmail("info@berlinspaceflowers.com");
        purchaseInfo.setFirstName("Anton");
        purchaseInfo.setLastName("Angelov");
        purchaseInfo.setCompany("Space Flowers");
        purchaseInfo.setCountry("Germany");
        purchaseInfo.setAddress1("1 Willi Brandt Avenue Tiergarten");
        purchaseInfo.setAddress2("Lьtzowplatz 17");
        purchaseInfo.setCity("Berlin");
        purchaseInfo.setZip("10115");
        purchaseInfo.setPhone("+00498888999281");

        var checkoutPage = app().create(CheckoutPage.class);
        checkoutPage.fillBillingInfo(purchaseInfo);
        checkoutPage.asserts().orderReceived();
    }
}
```

3.2
```csharp
using NUnit.Framework;

namespace Bellatrix.Web.NUnit.Tests
{
    [TestFixture]
    [Browser(BrowserType.Chrome, BrowserBehavior.ReuseIfStarted)]
    [VideoRecording(VideoRecordingMode.DoNotRecord)]
    [ScreenshotOnFail(true)]
    public class LoginTests : WebTest
    {
        public override void TestInit() => App.NavigationService.Navigate("http://demos.bellatrix.solutions/my-account/");

        [Test]
        public void CorrectTextDisplayed_When_ClickDownloadButton()
        {
            var downloadButton = App.ElementCreateService.CreateByInnerTextContaining<Anchor>("Download");

            downloadButton.Click();

            var heading1 = App.ElementCreateService.CreateById<Heading>("feature-title");

            heading1.EnsureInnerTextIs("Download");
        }
    }
}
```


3.8
```csharp
TextField userNameField = App.ElementCreateService.CreateById<TextField>("username");
```

3.9
```csharp
[Test]
public void CorrectTextDisplayed_When_ClickDownloadButton()
{
    TextField userNameField = App.ElementCreateService.CreateById<TextField>("username");
    Password passwordField = App.ElementCreateService.CreateById<Password>("password");
}

```

3.10

```csharp
userNameField.SetText("info@berlinspaceflowers.com");
You have a similar method for the password field called SetPassword.
passwordField.SetPassword("@purISQzt%%DYBnLCIhaoG6$");
```

3.11

```csharp
Button loginButton = App.ElementCreateService.CreateByXpath<Button>("//button[@name='login']");
```

3.12
```csharp
loginButton.Click();
```

3.14

```csharp
Div myAccountContentDiv = App.ElementCreateService.CreateByClass<Div>("woocommerce-MyAccount-content");
```

3.15

```csharp
myAccountContentDiv.EnsureInnerTextContains("Hello info1");
```

3.16
```csharp
Anchor logoutLink = App.ElementCreateService.CreateByInnerTextContaining<Anchor>("Log out");
```

3.17
```csharp
logoutLink.EnsureIsVisible();
```

```csharp
[TestFixture]
[Browser(BrowserType.Chrome, BrowserBehavior.ReuseIfStarted)]
[VideoRecording(VideoRecordingMode.DoNotRecord)]
[ScreenshotOnFail(true)]
public class BellatrixLoginTests : WebTest
{
    public override void TestInit() => App.NavigationService.Navigate("http://demos.bellatrix.solutions/my-account/");

    [Test]
    public void CorrectTextDisplayed_When_ClickDownloadButton()
    {
        TextField userNameField = App.ElementCreateService.CreateById<TextField>("username");
        Password passwordField = App.ElementCreateService.CreateById<Password>("password");
        Button loginButton = App.ElementCreateService.CreateByXpath<Button>("//button[@name='login']");

        userNameField.SetText("info@berlinspaceflowers.com");
        passwordField.SetPassword("@purISQzt%%DYBnLCIhaoG6$");
        loginButton.Click();

        Div myAccountContentDiv = App.ElementCreateService.CreateByClass<Div>("woocommerce-MyAccount-content");
        myAccountContentDiv.EnsureInnerTextContains("Hello info1");

        Anchor logoutLink = App.ElementCreateService.CreateByInnerTextContaining<Anchor>("Log out");

        logoutLink.EnsureIsVisible();
    }
}
```

```csharp
test
```