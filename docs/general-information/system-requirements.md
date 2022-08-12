---
layout: default
title:  "System Requirements"
excerpt: "What do you need to install and run all BELLATRIX libraries and tools?"
date:   2022-08-12 06:50:17 +0200
parent: general-information
permalink: /general-information/system-requirements/
anchors:
  overview: Overview
  supported-code-editors: Supported Code Editors
  sdks-and-frameworks-prerequisites: SDKs and Frameworks Prerequisites
---
Overview
--------
Both TestNG and JUnit are supported.

Supported Code Editors
----------------------
The recommended code editor for writing BELLATRIX tests is IntelliJ

NOTE: After the support for .NET Framework 5.0 and higher, Microsoft officially not support .NET Core development in older versions of Visual Studio 2015, 2017 and so on.

### Other Supported Editors: ###
- Visual Studio Code
- IntelliJ
- NetBeans
- Eclipse

SDKs and Frameworks Prerequisites
-------------------------------- 
We recommend to install the latest stable version of JDK.

For BELLATRIX desktop modules you need to download [**WinAppDriver**](https://github.com/Microsoft/WinAppDriver/releases). You need to make sure it is started before running any BELLATRIX desktop tests.

For BELLATRIX mobile modules you need to download and install [**Appium**](http://appium.io/). You need to make sure it is started before running any BELLATRIX mobile tests.