---
layout: default
title:  "Static Analysis"
excerpt: "Learn how to use the BELLATRIX Static Analysis."
date:   2018-11-23 06:50:17 +0200
parent: ios-automation
permalink: /ios-automation/static-analysis/
anchors:
  introduction: Introduction
  benefits: Benefits
  bellatrix-coding-standards-modules: BELLATRIX Coding Standards Modules
---
Introduction
------------
Coding standards define a programming style. A coding standard does not usually concern itself with wrong or right in a more abstract sense. It is merely a set of rules and guidelines for the formatting of source code.

Benefits
--------
- Seamless Code Integration
- Team Member Integration
- Easy to Debug and Maintain
- Readability
- Minimizes Communication
- Minimizes Performance Pitfalls
- Saves Money Due to Less Man Hours

BELLATRIX Coding Standards Modules
----------------------------------

BELLATRIX comes with two modules for helping you apply coding standards in your tests.

### .editorconfig  ###

**EditorConfig** helps developers define and maintain consistent coding styles between different editors and IDEs. The **EditorConfig** project consists of a file format for defining coding styles and a collection of text editor plugins that enable editors to read the file format and adhere to defined styles. 
**EditorConfig** files are easily readable, and they work nicely with version control systems.
You can override the global Visual Studio settings through a **.editorconfig **file placed on solution level.
All projects come with a predefined set of this rules that we advise you to use. You can always change them to follow your company's global coding standards.

You can read more about it [here](https://automatetheplanet.com/coding-styles-editorconfig/).

### StyleCop ###

**StyleCop** is an open source static code analysis tool from Microsoft that checks C# code for conformance to **StyleCop**'s recommended coding styles moreover, a subset of Microsoft's .NET Framework Design Guidelines.
The StyleCopAnalyzers open source project is similar to **EditorConfig**. It integrates with all versions of Visual Studio. It contains set of style and consistency rules. The code is checked on a build. If some of the rules are violated warning messaged are displayed. This way you can quickly locate the problems and fix them.

You can find more detailed information [here](https://automatetheplanet.com/style-consistency-rules-stylecop/).

All projects come with predefined StyleCop rules:
- **stylecop.json**
- **StyleCopeRules.ruleset**

**Note**:* You can reuse both **.editorconfig** and **StyleCop** files. Place them in a folder inside your solution and change their paths inside your projects' MSBuild files. As with **.editorconfig**, you can change the predefined rules to fit your company's standards.*

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