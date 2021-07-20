---
layout: default
title:  "Folder and File Structure"
excerpt: "Learn what each BELLATRIX project templates includes."
date:   2018-10-20 06:50:17 +0200
parent: android-automation
permalink: /android-automation/folder-and-file-structure/
anchors:
  test-framework-settings: Test Framework Settings
---
Overview
--------
Find detailed information about what each empty project contains or should contain if you wish to create it manually.

Test Framework Settings
-----------------------
In the resources folder, there is **application.properties** file where you set the environment variable. It is used to determine which **testFrameworkSettings** file to use. They are located in the same folder and are named like **testFrameworkSettings.\<env\>.json**, replacing **\<env\>** with the value of the environment variable.