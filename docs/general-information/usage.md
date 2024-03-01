---
layout: default
title:  "BELLATRIX Usage"
excerpt: "Learn how to integrate BELLATRIX in your projects."
date:   2024-03-01 06:50:17 +0200
parent: general-information
permalink: /general-information/usage/
anchors:
  overview: Overview
  simple-installation: Simple Installation
  release-strategy: Release Strategy
  integrating-bellatrix-as-a-submodule: Integrating BELLATRIX as a Submodule
  checking-out-a-specific-tag: Checking Out a Specific Tag
---
Overview
--------
Customize and extend our cross-platform .NET framework to perfectly fit your needs. Start on top of hundreds of best practice features and integrations.

Contains the full source code of BELLATRIX Test Automation Framework and Templates for faster usage

BELLATRIX is not a single thing it contains multiple framework libraries, extensions and tools. The tool is built to be cross-platform, however some of the features can be used under Windows since they are written for Visual Studio.

Simple Installation
------------------
1. Download the BELLATRIX projects as a zip file from the Code green button in the right corner.
2. Unzip it. Open BELLATRIX-Java in IntelliJ
3. Open one of the preconfigured modules. It works for both JUnit and TestNG without additional configuration.

4. Run the sample tests.
5. You can try to write a simple test yourself.
6. For an in-depth revision of all framework features you can check the [**official documentation**](http://docs.java.bellatrix.solutions/web-automation/)

Release Strategy
--------
**Versioning**

Our project uses [Semantic Versioning](https://semver.org/) (SemVer) for clear and predictable version management. Each release is tagged with a unique version number that reflects the nature of the changes made.

**Tags and Releases**
-   **Tags:** We create Git tags for each release to mark specific points in the project's history, making it easier for users to switch between versions.
-   **Release Notes:** Detailed notes accompany [each release](https://bellatrix.solutions/roadmap/release-3-9-0-0-lyra/), highlighting new features, bug fixes, and any breaking changes.


**Accessing Releases**

Users can access the specific releases directly from our GitHub repository's Releases page or BELLATRIX website's roadmap section. Each release includes a tag, a summary of changes, and assets (if applicable).

Integrating BELLATRIX as a Submodule in Your Project
--------
To leverage a specific version of BELLATRIX in your project, you can add it as a Git submodule. This allows you to keep your copy of BELLATRIX up to date or locked to a specific version, depending on your project's needs.
**Navigate to Your Project Directory:** Open a terminal and change to the directory where your project is located.
**Add BELLATRIX as a Submodule:** Use the following Git command to add BELLATRIX as a submodule to your project:
```
git submodule add https://github.com/BELLATRIX-Library/BELLATRIX.git path/to/submodule
``` 
Replace `path/to/submodule` with the relative path within your project where you'd like the BELLATRIX submodule to reside.
**Initialize and Clone the Submodule:** If you're adding the submodule for the first time, initialize your local configuration file and clone the BELLATRIX repository as follows:
```
git submodule update --init --recursive
```

Checking Out a Specific Tag (Release Version)
--------
After adding BELLATRIX as a submodule, you might want to use a specific release version rather than the latest commit on the main branch.
**Navigate to the Submodule Directory:** Change into the BELLATRIX submodule directory within your project:
```
cd path/to/submodule
```
**Fetch All Tags from the BELLATRIX Repository:** To ensure you have a list of all available tags, fetch them:
```
git fetch --tags
```
**Checkout the Desired Tag:** Check out the specific tag (release version) you want to use by replacing `<tagname>` with the desired version:
```   
git checkout tags/<tagname>` 
```
For example, if you want to check out version `v3.9.0.0`, you would use:
```
git checkout tags/v3.9.0.0
```
**Commit the Submodule Change:** Navigate back to your project's root directory, and commit the change to the submodule reference:
```
cd ../..
git add path/to/submodule
git commit -m "Update BELLATRIX submodule to v3.9.0.0"
```
This process ensures that your project uses a specific, fixed version of BELLATRIX, providing stability and consistency across environments or deployments.