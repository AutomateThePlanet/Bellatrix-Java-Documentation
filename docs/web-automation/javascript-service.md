---
layout: default
title:  "JavaScript Service"
excerpt: "Learn how to use BELLATRIX JavaScript service."
date:   2018-06-22 06:50:17 +0200
parent: web-automation
permalink: /web-automation/javascript-service/
anchors:
  example: Example
  explanations: Explanations
  playwright: Playwright
---
Example
-------
```java
public class JavaScriptServiceTests extends WebTest {
    @Test
    public void fillUpAllFields() {
        app().navigate().to("http://demos.bellatrix.solutions/my-account/");

        app().script().execute("document.getElementById('username').value = 'BELLATRIX';");

        app().create().byId(PasswordInput.class, "password").setPassword("Gorgeous");
        var button = app().create().byClassContaining(Button.class, "woocommerce-Button button");
        
        app().script().execute("arguments[0].click();", button);
    }

    @Test
    @Ignore
    public void getElementStyle() {
        app().navigate().to("http://demos.bellatrix.solutions/my-account/");

        var resultsCount = app().create().byClassContaining(WebComponent.class, "woocommerce-result-count");

        String fontSize = app().script().execute("return arguments[0].style.font-size", resultsCount.getWrappedElement());

        Assert.assertEquals(fontSize, "14px");
    }
}
```

Explanations
------------
BELLATRIX gives you an interface for easier execution of JavaScript code using the **script** method. You need to make sure that you have navigated to the desired web page.
```java
app().script().execute("document.getElementById('username').value = 'BELLATRIX';");
```
Execute a JavaScript code on the page. Here we find an element with id = 'firstName' and sets its value to 'BELLATRIX'.
```java
app().script().execute("arguments[0].click();", button);
```
It is possible to pass an element, and the script executes on it.
```java
String fontSize = app().script().execute("return arguments[0].style.font-size", resultsCount.getWrappedElement());
```
Get the results from a script. After that, get the value for a specific style and assert it.

Playwright
------------
Similar methods are available for the Playwright module, but when performing JavaScript against an element, it is recommended to use **Evaluate** method of **WebComponent** class.
```java
float fontSize = (float) webComponent.evaluate("el => el.style.font-size");
```
As you can see, you don't need to pass the element as an argument.
You can also specify the return type.

The same method is available in the **JavaScriptService**, but you still need to write the JS script in the style shown above as internally the service calls the **Evaluate** method of the component.
```java
app().script().execute("el => el.click", component);
```
