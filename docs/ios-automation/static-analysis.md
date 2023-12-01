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
@BeforeAll
public static void setUpClass() {
    WebDriverManager.chromedriver().setup();
}

@BeforeEach
public void setUp() {
    driver = new ChromeDriver();
    webDriverWait = new WebDriverWait(driver, Duration.ofSeconds(WAIT_FOR_ELEMENT_TIMEOUT));
    actions = new Actions(driver);
}

// tests

@AfterEach
public void tearDown() {
    if (driver != null) {
        driver.quit();
    }
}
```


```java
@Test
public void verifyToDoListCreatedSuccessfully() {
    // Navigate to the web application
    driver.navigate().to("https://todomvc.com/");
    // Open the specific technology app
    openTechnologyApp("Backbone.js");
    // Add new to-do items
    addNewToDoItem("Clean the car");
    addNewToDoItem("Clean the house");
    addNewToDoItem("Buy Ketchup");
    // Mark an item as completed
    getItemCheckbox("Buy Ketchup").click();
    // Assert the number of items left
    assertLeftItems(2);
}
```

```java
private void assertLeftItems(int expectedCount){
    var resultSpan = waitAndFindElement(By.xpath("//footer/*/span | //footer/span"));
    if (expectedCount == 1){
        var expectedText = String.format("%d item left", expectedCount);
        validateInnerTextIs(resultSpan, expectedText);
    } else {
        var expectedText = String.format("%d items left", expectedCount);
        validateInnerTextIs(resultSpan, expectedText);
    }
}

private void validateInnerTextIs(WebElement resultElement, String expectedText){
    webDriverWait.until(ExpectedConditions.textToBePresentInElement(resultElement, expectedText));
}

private WebElement getItemCheckbox(String todoItem){
    var xpathLocator = String.format("//label[text()='%s']/preceding-sibling::input", todoItem);
    return waitAndFindElement(By.xpath(xpathLocator));
}

private void openTechnologyApp(String technologyName){
    var technologyLink = waitAndFindElement(By.linkText(technologyName));
    technologyLink.click();
}

private void addNewToDoItem(String todoItem){
    var todoInput = waitAndFindElement(By.xpath("//input[@placeholder='What needs to be done?']"));
    todoInput.sendKeys(todoItem);
    actions.click(todoInput).sendKeys(Keys.ENTER).perform();
}

private WebElement waitAndFindElement(By locator){
    return webDriverWait.until(ExpectedConditions.presenceOfElementLocated(locator));
}
```


```java
@ParameterizedTest
@NullAndEmptySource
void isBlank_ShouldReturnTrueForNullAndEmptyStrings(String input) {
    Assertions.assertTrue(input == null);
}
```

```java
@ParameterizedTest(name = "{index}. verify todo list created successfully when technology = {0}")
@ValueSource(strings = {
        "Backbone.js",
        "AngularJS",
        "React",
        "Vue.js",
        "CanJS",
        "Ember.js",
        "KnockoutJS",
        "Marionette.js",
        "Polymer",
        "Angular 2.0",
        "Dart",
        "Elm",
        "Closure",
        "Vanilla JS",
        "jQuery",
        "cujoJS",
        "Spine",
        "Dojo",
        "Mithril",
        "Kotlin + React",
        "Firebase + AngularJS",
        "Vanilla ES6"
})
@NullAndEmptySource
public void verifyToDoListCreatedSuccessfully_withParams(String technology){
    driver.navigate().to("https://todomvc.com/");
    openTechnologyApp(technology);
    addNewToDoItem("Clean the car");
    addNewToDoItem("Clean the house");
    addNewToDoItem("Buy Ketchup");
    getItemCheckbox("Buy Ketchup").click();

    assertLeftItems(2);
}
```



```java
@ParameterizedTest
@EnumSource(WebTechnology.class)
public void verifyToDoListCreatedSuccessfully_withEnum(WebTechnology technology){
    driver.navigate().to("https://todomvc.com/");
    openTechnologyApp(technology.getTechnologyName());
    addNewToDoItem("Clean the car");
    addNewToDoItem("Clean the house");
    addNewToDoItem("Buy Ketchup");
    getItemCheckbox("Buy Ketchup").click();

    assertLeftItems(2);
}

// Enum filter - data driven
@ParameterizedTest
@EnumSource(value = WebTechnology.class, names = {"BACKBONEJS", "ANGULARJS", "EMBERJS", "KNOCKOUTJS"})
public void verifyToDoListCreatedSuccessfully_withEnumFilter(WebTechnology technology){
    driver.navigate().to("https://todomvc.com/");
    openTechnologyApp(technology.getTechnologyName());
    addNewToDoItem("Clean the car");
    addNewToDoItem("Clean the house");
    addNewToDoItem("Buy Ketchup");
    getItemCheckbox("Buy Ketchup").click();

    assertLeftItems(2);
}

// Enum filter exclude - data driven
@ParameterizedTest
@EnumSource(value = WebTechnology.class, names = {"BACKBONEJS", "ANGULARJS", "EMBERJS", "KNOCKOUTJS"}, mode = EnumSource.Mode.EXCLUDE)
public void verifyToDoListCreatedSuccessfully_withEnumFilterExclude(WebTechnology technology){
    driver.navigate().to("https://todomvc.com/");
    openTechnologyApp(technology.getTechnologyName());
    addNewToDoItem("Clean the car");
    addNewToDoItem("Clean the house");
    addNewToDoItem("Buy Ketchup");
    getItemCheckbox("Buy Ketchup").click();

    assertLeftItems(2);
}

// Enum filter exclude - data driven
@ParameterizedTest
@EnumSource(value = WebTechnology.class, names = {".+JS"}, mode = EnumSource.Mode.EXCLUDE)
public void verifyToDoListCreatedSuccessfully_withEnumFilterExcludeRegex(WebTechnology technology){
    driver.navigate().to("https://todomvc.com/");
    openTechnologyApp(technology.getTechnologyName());
    addNewToDoItem("Clean the car");
    addNewToDoItem("Clean the house");
    addNewToDoItem("Buy Ketchup");
    getItemCheckbox("Buy Ketchup").click();

    assertLeftItems(2);
}
```

```java
public enum WebTechnology {
    BACKBONEJS("Backbone.js"),
    ANGULARJS("AngularJS"),
    REACT("React"),
    VUEJS("Vue.js"),
    CANJS("CanJS"),
    EMBERJS("Ember.js"),
    KNOCKOUTJS("KnockoutJS"),
    MARIONETTEJS("Marionette.js"),
    POLYMER("Polymer"),
    ANGULAR2("Angular 2.0"),
    DART("Dart"),
    ELM("Elm"),
    CLOSURE("Closure"),
    VANILLAJS("Vanilla JS"),
    JQUERY("jQuery"),
    CUJOJS("cujoJS"),
    SPINE("Spine"),
    DOJO("Dojo"),
    MITHRIL("Mithril"),
    KOTLIN_REACT("Kotlin + React"),
    FIREBASE_ANGULARJS("Firebase + AngularJS"),
    VANILLA_ES6("Vanilla ES6");

    private String technologyName;


    WebTechnology(String technologyName) {
        this.technologyName = technologyName;
    }

    public String getTechnologyName() {
        return technologyName;
    }
}
```

```java
// CSV Source without file
@ParameterizedTest
@CsvSource(value = {
        "Backbone.js,Clean the car,Clean the house,Buy Ketchup,Buy Ketchup,2",
        "AngularJS,Clean the car,Clean the house,Clean the house,Clean the house,2",
        "React,Clean the car,Clean the house,Clean the car,Clean the car,2"},
        delimiter = ',')
public void verifyToDoListCreatedSuccessfully_withParamsCsvSourceWithoutFile(String technology, String item1, String item2, String item3, String itemToCheck, int expectedLeftItems){
    driver.navigate().to("https://todomvc.com/");
    openTechnologyApp(technology);
    addNewToDoItem(item1);
    addNewToDoItem(item2);
    addNewToDoItem(item3);
    getItemCheckbox(itemToCheck).click();

    assertLeftItems(expectedLeftItems);
}
```

```java
@ParameterizedTest
@CsvFileSource(resources = "/data.csv", numLinesToSkip = 1)
public void verifyToDoListCreatedSuccessfully_withParamsCsvSourceWithFile(String technology, String item1, String item2, String item3, String itemToCheck, int expectedLeftItems){
    driver.navigate().to("https://todomvc.com/");
    openTechnologyApp(technology);
    addNewToDoItem(item1);
    addNewToDoItem(item2);
    addNewToDoItem(item3);
    getItemCheckbox(itemToCheck).click();

    assertLeftItems(expectedLeftItems);
}
```

```java
@ParameterizedTest
@MethodSource("provideWebTechnologies")
public void verifyToDoListCreatedSuccessfully_withMethod(String technology){
    driver.navigate().to("https://todomvc.com/");
    openTechnologyApp(technology);
    addNewToDoItem("Clean the car");
    addNewToDoItem("Clean the house");
    addNewToDoItem("Buy Ketchup");
    getItemCheckbox("Buy Ketchup").click();

    assertLeftItems(2);
}

private static Stream<String> provideWebTechnologies() {
    return Stream.of("Backbone.js",
            "AngularJS",
            "React",
            "Vue.js",
            "CanJS",
            "Ember.js",
            "KnockoutJS",
            "Marionette.js",
            "Polymer",
            "Angular 2.0",
            "Dart",
            "Elm",
            "Closure",
            "Vanilla JS",
            "jQuery",
            "cujoJS",
            "Spine",
            "Dojo",
            "Mithril",
            "Kotlin + React",
            "Firebase + AngularJS",
            "Vanilla ES6");
}
```

```java
@ParameterizedTest
@MethodSource("provideWebTechnologiesMultipleParams")
public void verifyToDoListCreatedSuccessfully_withMethod(String technology, List<String> itemsToAdd, List<String> itemsToCheck, int expectedLeftItems){
    driver.navigate().to("https://todomvc.com/");
    openTechnologyApp(technology);
    itemsToAdd.stream().forEach(itemToAdd -> addNewToDoItem(itemToAdd));
    itemsToCheck.stream().forEach(itemToCheck -> getItemCheckbox(itemToCheck).click());

    assertLeftItems(expectedLeftItems);
}

private Stream<Arguments> provideWebTechnologiesMultipleParams() {
    return Stream.of(
            Arguments.of("AngularJS", List.of("Buy Ketchup", "Buy House", "Buy Paper", "Buy Milk", "Buy Batteries"), List.of("Buy Ketchup", "Buy House"), 3),
            Arguments.of("React", List.of("Buy Ketchup", "Buy House", "Buy Paper", "Buy Milk", "Buy Batteries"), List.of("Buy Paper", "Buy Milk", "Buy Batteries"), 2),
            Arguments.of("Vue.js", List.of("Buy Ketchup", "Buy House", "Buy Paper", "Buy Milk", "Buy Batteries"), List.of("Buy Paper", "Buy Milk", "Buy Batteries"), 2),
            Arguments.of("Angular 2.0", List.of("Buy Ketchup", "Buy House", "Buy Paper", "Buy Milk", "Buy Batteries"), List.of(), 5)
    );
}
```

```java
@ParameterizedTest
@ArgumentsSource(WebTechnologiesCustomArgumentsProvider.class)
public void verifyToDoListCreatedSuccessfully_withArgumentSource(String technology, List<String> itemsToAdd, List<String> itemsToCheck, int expectedLeftItems){
    driver.navigate().to("https://todomvc.com/");
    openTechnologyApp(technology);
    itemsToAdd.stream().forEach(itemToAdd -> addNewToDoItem(itemToAdd));
    itemsToCheck.stream().forEach(itemToCheck -> getItemCheckbox(itemToCheck).click());

    assertLeftItems(expectedLeftItems);
}

@ParameterizedTest
@ArgumentsSource(WebTechnologiesCustomArgumentsProvider.class)
public void verifyToDoListCreatedSuccessfully_withArgumentSourceWithSingleArgument(@AggregateWith(ToDoListAggregator.class) ToDoList toDoList){
    driver.navigate().to("https://todomvc.com/");
    openTechnologyApp(toDoList.getTechnology());
    toDoList.getItemsToAdd().stream().forEach(itemToAdd -> addNewToDoItem(itemToAdd));
    toDoList.getItemsToCheck().stream().forEach(itemToCheck -> getItemCheckbox(itemToCheck).click());

    assertLeftItems(toDoList.getExpectedItemsLeft());
}
```

```java
public class WebTechnologiesCustomArgumentsProvider implements ArgumentsProvider {
    @Override
    public Stream<? extends Arguments> provideArguments(ExtensionContext extensionContext) throws Exception {
        return Stream.of(
                Arguments.of("AngularJS", List.of("Buy Ketchup", "Buy House", "Buy Paper", "Buy Milk", "Buy Batteries"), List.of("Buy Ketchup", "Buy House"), 3),
                Arguments.of("React", List.of("Buy Ketchup", "Buy House", "Buy Paper", "Buy Milk", "Buy Batteries"), List.of("Buy Paper", "Buy Milk", "Buy Batteries"), 2),
                Arguments.of("Vue.js", List.of("Buy Ketchup", "Buy House", "Buy Paper", "Buy Milk", "Buy Batteries"), List.of("Buy Paper", "Buy Milk", "Buy Batteries"), 2),
                Arguments.of("Angular 2.0", List.of("Buy Ketchup", "Buy House", "Buy Paper", "Buy Milk", "Buy Batteries"), List.of(), 5)
        );
    }
}
```

```java
@ParameterizedTest
@CsvSource({"2021-11-21,2021",
        "2022-01-12,2022"})
public void verifyYear_whenCustomConverter(@ConvertWith(DashDateConverter.class) LocalDate date, int expected){
    Assertions.assertEquals(expected, date.getYear());
}
```

```java
public class DashDateConverter implements ArgumentConverter {
    @Override
    public Object convert(Object o, ParameterContext parameterContext) throws ArgumentConversionException {
        if (!(o instanceof String)) {
            throw new IllegalArgumentException("The argument should be a string: " + o);
        }
        try {
            String[] parts = ((String) o).split("-");
            int year = Integer.parseInt(parts[0]);
            int month = Integer.parseInt(parts[1]);
            int day = Integer.parseInt(parts[2]);

            return LocalDate.of(year, month, day);
        } catch (Exception e) {
            throw new IllegalArgumentException("Failed to convert", e);
        }
    }
}
```