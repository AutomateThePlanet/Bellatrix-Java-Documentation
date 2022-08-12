---
layout: default
title:  "AWS Rekognition"
excerpt: "Learn how to validation text in PDFs and images via AWS Rekognition."
date:   2022-08-12 06:50:17 +0200
parent: product-integrations
permalink: /product-integrations/aws-rekognition/
anchors:
  what-is-aws-rekognition: What Is AWS Rekognition?
  usage: Usage
---
What is AWS Rekognition?
-------
**[AWS Rekognition](https://aws.amazon.com/rekognition/)** is a service that can be used to extract printed and handwritten text from images and documents with mixed languages and writing styles. In BELLATRIX, we use it to validate the layout of complex PDF documents. The service PAID after some usage each month. However, it is relatively cheaper than many other libraries and services or the cost of developing your own solution.
Usage
------------------
You can initialize directly the **Rekognition** service class. Using the **extractOCRTextFromLocalFile** method, you can get a list of all text snippets in your document. You can use the **validateText** method to check whether a particular text sequence appears in your document under specific order.
![Sample Invoice](images/sampleinvoice.png)
```java
@Test
public void makeTextExtractionFromPDF() {
    var rekognition = new Rekognition();
    var textSnippets = rekognition.extractOCRTextFromLocalFile("sampleinvoice.pdf");
    textSnippets.forEach(System.out::println);

    List<string> expectedTextSnippets = Arrays.asList(
        "69653 1st Point, 45 Acker Driv",
        "Subtotal",
        "$84.00",
        "Total",
        "$136.00",
    );

    rekognition.validateText("sampleinvoice.pdf", expectedTextSnippets);
}
```
You are not limited to PDFs only. You can use the same feature for extracting text from complex images. Similar to the one below.
![Graph Image](images/devPortalGraph1.png)
You can use the following code snippet to verify the info.
```java
@Test
public void ExtractTextFromGraph() {
    var rekognition = new Rekognition();
    var textSnippets = rekognition.extractOCRTextFromLocalFile("devPortalGraph1.PNG");
    textSnippets.forEach(System.out::println);

    List<string> expectedTextSnippets = Arrays.asList(
        "Apr 12 01:00",
        "Apr 13 00:00",
    );

    rekognition.validateText("devPortalGraph1.PNG", expectedTextSnippets);
}
```