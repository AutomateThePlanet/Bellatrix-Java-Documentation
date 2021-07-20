---
layout: default
title:  "FileSystemService"
excerpt: "Learn how to use BELLATRIX Android FileSystemService."
date:   2018-06-22 06:50:17 +0200
parent: android-automation
permalink: /android-automation/file-system-service/
anchors:
  example: Example
  explanations: Explanations
---
Example
-------
```java
public class FileSystemServiceTests extends AndroidTest {
    @Test
    public void fileSavedToDevice_When_CallPushFileFromBytes() {
        String data = "The eventual code is no more than the deposit of your understanding. ~E. W. Dijkstra";
        byte[] bytes = data.getBytes(StandardCharsets.UTF_8);
        app().fileSystem().pushFile("/data/local/tmp/remote.txt", bytes);

        byte[] returnDataBytes = app().fileSystem().pullFile("/data/local/tmp/remote.txt");
        String returnedData = new String(returnDataBytes);

        Assert.assertEquals(returnedData, data);
    }
    
    @Test
    public void fileSavedToDevice_When_CallPushFileFromFile() throws IOException {
        String data = "The eventual code is no more than the deposit of your understanding. ~E. W. Dijkstra";
        File file = new File(UUID.randomUUID().toString());
        FileUtils.writeStringToFile(file, data, StandardCharsets.UTF_8);
        app().fileSystem().pushFile("/data/local/tmp/remote.txt", file);

        byte[] returnDataBytes = app().fileSystem().pullFile("/data/local/tmp/remote.txt");
        String returnedData = new String(returnDataBytes);

        Assert.assertEquals(returnedData, data);
    }

    @Test
    public void allFilesReturned_When_CallPullFolder() {
        String data = "The eventual code is no more than the deposit of your understanding. ~E. W. Dijkstra";
        app().fileSystem().pushFile("/data/local/tmp/remote.txt", data.getBytes());

        byte[] returnDataBytes = app().fileSystem().pullFolder("/data/local/tmp/");

        Assert.assertTrue(returnDataBytes.length > 0);
    }
}
```

Explanations
------------
BELLATRIX gives you an interface for easier work with files using the **fileSystem** method.
```java
byte[] returnDataBytes = app().fileSystem().pullFile("/data/local/tmp/remote.txt");
```
Returns the content of the specified file as a byte array.
```java
byte[] bytes = data.getBytes(StandardCharsets.UTF_8);
app().fileSystem().pushFile("/data/local/tmp/remote.txt", bytes);
```
Creates a new file on the device from the specified byte array.
```java
File file = new File(UUID.randomUUID().toString());
FileUtils.writeStringToFile(file, data, StandardCharsets.UTF_8);
app().fileSystem().pushFile("/data/local/tmp/remote.txt", file);
```
Creates a new file on the device from the specified file.
```java
byte[] returnDataBytes = app().fileSystem().pullFolder("/data/local/tmp/");
```
Returns the content of the specified folder as a byte array.