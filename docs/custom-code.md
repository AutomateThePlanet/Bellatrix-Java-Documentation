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
public class Calculator {
    public int add(int a, int b) {
        return a + b;
    }

    public int divide(int a, int b) {
        if (b == 0)
            throw new ArithmeticException("/ by zero");
        return a/b;
    }
}
```

```java
@Target({ ElementType.TYPE, ElementType.METHOD })
@Retention(RetentionPolicy.RUNTIME)
@Tag("nightlyRun")
public @interface NightlyRun {
}
```

```java
@Target(ElementType.METHOD)
@Retention(RetentionPolicy.RUNTIME)
@Tag("nightlyRun")
@Test
public @interface NightlyRunTest {
}
```


```java
@TestMethodOrder(value = MethodOrderer.Random.class)
public class CalculatorTests {
    private final Calculator _calculator = new Calculator();

    @BeforeAll
    public static void setUpClass() {
        System.out.println("This is @BeforeAll annotation");
    }

    @BeforeEach
    public void setUp() {
        System.out.println("This is @BeforeEach annotation");
    }

    @NightlyRunTest
    @Order(2)
    public void test_Addition() {
        System.out.println("This is test 1");
        var actualResult = _calculator.add(1, 1);

        Assertions.assertEquals(2, actualResult);
    }

    @Test
    @Order(1)
    public void testAdditionDifferentNumbers(){
        System.out.println("This is test 2");
        var actualResult = _calculator.add(2, 1);

        Assertions.assertEquals(3, actualResult);
    }

    @AfterEach
    public void tearDown() {
        System.out.println("This is @After annotation");
    }

    @AfterAll
    public static void tearDownClass() {
        System.out.println("This is @AfterAll annotation");
    }
}
```

```xml
<dependencies>
    <dependency>
        <groupId>org.junit.jupiter</groupId>
        <artifactId>junit-jupiter-api</artifactId>
        <version>RELEASE</version>
        <scope>test</scope>
    </dependency>
    <dependency>
        <groupId>org.junit.jupiter</groupId>
        <artifactId>junit-jupiter-engine</artifactId>
        <version>RELEASE</version>
        <scope>test</scope>
    </dependency>
    <dependency>
        <groupId>org.junit.jupiter</groupId>
        <artifactId>junit-jupiter-api</artifactId>
        <version>5.8.1</version>
        <scope>compile</scope>
    </dependency>
    <dependency>
        <groupId>org.junit.jupiter</groupId>
        <artifactId>junit-jupiter-api</artifactId>
        <version>5.8.1</version>
        <scope>compile</scope>
    </dependency>

    <dependency>
        <groupId>org.seleniumhq.selenium</groupId>
        <artifactId>selenium-java</artifactId>
        <version>4.0.0</version>
    </dependency>

    <dependency>
        <groupId>io.github.bonigarcia</groupId>
        <artifactId>webdrivermanager</artifactId>
        <version>4.3.1</version>
        <scope>test</scope>
    </dependency>
</dependencies>
```

```java
public class DateTimeAssert {
    public static void assertEquals(LocalDateTime expectedDate, LocalDateTime actualDate, DateTimeDeltaType deltaType, int count) throws Exception {
        if (((expectedDate == null) && (actualDate == null))) {
            return;
        }
        else if ((expectedDate == null)) {
            throw new NullPointerException("The expected date was null");
        }
        else if ((actualDate == null)) {
            throw new NullPointerException("The actual date was null");
        }

        Duration expectedDelta = DateTimeAssert.getTimeSpanDeltaByType(deltaType, count);

        double totalSecondsDifference = Math.abs((actualDate.until(expectedDate, ChronoUnit.SECONDS)));

        if ((totalSecondsDifference > expectedDelta.getSeconds())) {
            var exceptionMessage =String.format("Expected Date: {0}, Actual Date: {1} \nExpected Delta: {2}, Actual Delta in seconds- {3} (Delta Type: " +
                    "{4})", expectedDate, actualDate, expectedDelta, totalSecondsDifference, deltaType);
            throw new Exception(exceptionMessage);
        }
    }

    private static Duration getTimeSpanDeltaByType(DateTimeDeltaType type, int count) {
        Duration result;
        switch (type) {
            case DAYS:
                result = Duration.ofDays(count);
                break;
            case MINUTES:
                result = Duration.ofMinutes(count);
                break;
            default:
                throw new NotImplementedException("The delta type is not implemented.");
        }
        return result;
    }
}
```
```java
public enum DateTimeDeltaType {
    DAYS,
    MINUTES
}
```

```java
public class FirstSeleniumTests {
    private WebDriver driver;

    @BeforeAll
    public static void setUpClass() {
        WebDriverManager.chromedriver().setup();
    }

    @BeforeEach
    public void setUp() {
        driver = new ChromeDriver();
    }

    @Test
    public void properCheckboxSelected() throws Exception {
        driver.navigate().to("https://lambdatest.github.io/sample-todo-app/");

        LocalDate birthDay = LocalDate.of(1990, 10, 20);
        // us 10/20/1990
        DateTimeFormatter usDateFormat = DateTimeFormatter.ofPattern("dd-MM-yyyy");
        String dateToType = usDateFormat.format(birthDay);

        WebElement todoInput = driver.findElement(By.id("sampletodotext"));
        todoInput.sendKeys(dateToType);

        var addButton = driver.findElement(By.id("addbutton"));
        addButton.click();

        var todoCheckboxes = driver.findElements(By.xpath("//li[@ng-repeat]/input"));

        todoCheckboxes.get(2).click();

        var todoInfos = driver.findElements(By.xpath("//li[@ng-repeat]/span"));

        Assertions.assertEquals("20-10-1990", todoInfos.get(5).getText());

        String expectedUrl = "https://lambdatest.github.io/sample-todo-app/";
        Assertions.assertTrue(expectedUrl.equals(driver.getCurrentUrl()), "URL does not match");

        String notExpectedUrl = "https://www.lambdatest.com/";
        Assertions.assertFalse(notExpectedUrl.equals(driver.getCurrentUrl()), "URL match");

        var expectedItems = new String[] {"First Item", "Second Item", "Third Item", "Fourth Item", "Fifth Item", "20-10-1990"};
        var actualToDoInfos  = todoInfos.stream().map(e -> e.getText()).toArray();
        Assertions.assertArrayEquals(expectedItems, actualToDoInfos);

        Exception exception = Assertions.assertThrows(ArithmeticException.class, () -> new Calculator().divide(1, 0));

        Assertions.assertEquals("/ by zero", exception.getMessage());

        Assertions.assertTimeout(ofMinutes(2), () -> {
            // perform your tasks
        });

        Assertions.assertAll(
                () -> Assertions.assertTrue(expectedUrl.equals(driver.getCurrentUrl()), "URL does not match"),
                () -> Assertions.assertFalse(notExpectedUrl.equals(driver.getCurrentUrl()), "URL match"),
                () -> Assertions.assertArrayEquals(expectedItems, actualToDoInfos)
        );

        double actualDoubleValue= 2.999;
        double expectedDoubleValue = 3.000;

        Assertions.assertEquals(expectedDoubleValue, actualDoubleValue, 0.001);

        var currentTime = LocalDateTime.now();
        var currentTimeInPast = LocalDateTime.now().minusMinutes(3);

        DateTimeAssert.assertEquals(currentTime, currentTimeInPast, DateTimeDeltaType.MINUTES, 4);
    }

    @AfterEach
    public void tearDown() {
        if (driver != null) {
            driver.quit();
        }
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