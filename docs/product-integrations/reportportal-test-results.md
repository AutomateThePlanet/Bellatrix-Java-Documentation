---
layout: default
title:  "ReportPortal Test Results"
excerpt: "Learn to analyze BELLATRIX test results through ReportPortal."
date:   2022-08-12 06:50:17 +0200
parent: product-integrations
permalink: /product-integrations/reportportal-test-results/
anchors:
  what-is-reportportal: What Is ReportPortal?
  installation: Installation
  configuration: Configuration
---
What is ReportPortal?
-------
**[ReportPortal](http://reportportal.io/)** is a service, that provides increased capabilities to speed up results analysis and reporting through the use of built-in analytic features. ReportPortal is a great addition to the Continuous Integration and Continuous Testing process.

The tool gives you the capability to create custom dashboards. I have created a dashboard visualizing the data from our latest BELLATRIX test runs. The first one shows the passing rate of all tests. The next one displays overall statistics- how many tests were executed and various types of bugs after the investigation process. Bellow, you can find some trend charts between runs. Also, there are some nice widgets for making a comparison between the test runs duration. 

![Bellatrix](images/reportportal-configurable-dashboards.png)

There are widgets that display the latest runs flaky tests and the tests that most failed. It is a neat feature for stabilizing these tests.

![Bellatrix](images/reportportal-configurable-dashboards2.png)

In the Launches section, you can see the latest runs and filter them.

![Bellatrix](images/reportportal-all-launches.png)

When you open a failed tests it initially is set that it needs investigation after that you can mark it as Product Bug, Automation Bug or System issue like a problem in the test environment.

![Bellatrix](images/report-portal-investigation.png)

All test failure info is synced automatically and well displayed.

![Bellatrix](images/report-portal-errors-visualisation.png)

You can filter based on the bug type and check all of these tests.

![Bellatrix](images/reportportal-filters.png)


Installation
------------------
The easiest way to deploy ReportPortal it to use [Docker](https://docs.docker.com/). Docker allows to install ReportPortal on Linux, Mac or Windows. Make sure that you have allocated at least **2 CPUs** and **3GB RAM** for Docker operations.

1 Make sure the Docker ([Engine](https://docs.docker.com/engine/installation/), [Compose](https://docs.docker.com/compose/install/)) is installed.

2 Download the latest compose descriptor example from [here](https://github.com/reportportal/reportportal/blob/master/docker-compose.yml). You can make it by next command:

```
curl https://raw.githubusercontent.com/reportportal/reportportal/master/docker-compose.yml -o docker-compose.yml
```

3 Start the application using the following command:

```
docker-compose -p reportportal up -d --force-recreate
```

4 Open your browser with the IP address of the deployed environment at port 8080

```
http://127.0.0.1:8080
```

5 Use next login\pass for access:

**default\1q2w3e** or **superadmin\erebus**

[Official Documentation](http://reportportal.io/docs/Installation-steps)

Configuration
-------------
First, you need to install the ReportPortal Maven dependencies.
```json
<dependency>
   <groupId>com.epam.reportportal</groupId>
   <artifactId>agent-java-junit5</artifactId>
   <version>5.1.5</version>
</dependency>
<dependency>
    <groupId>com.epam.reportportal</groupId>
    <artifactId>logger-java-logback</artifactId>
    <version>5.1.1</version>
</dependency>
```
Also, you need to add their plugin:
```json
<build>
    <plugins>
        <plugin>
            <artifactId>maven-surefire-plugin</artifactId>
            <version>3.0.0-M5</version>
            <configuration>
                <properties>
                    <configurationParameters>
                        junit.jupiter.extensions.autodetection.enabled = true
                    </configurationParameters>
                </properties>
            </configuration>
        </plugin>
        <plugin>
            <artifactId>maven-surefire-plugin</artifactId>
            <version>3.0.0-M5</version>
            <configuration>
                <properties>
                    <configurationParameters>
                        junit.jupiter.extensions.autodetection.enabled = true
                    </configurationParameters>
                </properties>
            </configuration>
        </plugin>
    </plugins>
</build>
```
After that when you execute your tests through native **maven clean test** test runner the tests will be automatically synced with the portal. Next you need to a JSON configuration file to your project called **reportportal.properties**.
```json
rp.endpoint = http://localhost:8080
rp.uuid = 8ca3be4c-12321312-asdasd-122121
rp.launch = default_JUNIT_AGENT
rp.project = lambda-test-reports-examples
```
You need to mention the name of your project, and from the ReportPortal settings section, you need to copy the authentication guid for your user. In the launch settings, you can customize the name of the test runs and add some tags.

[Official Documentation](https://reportportal.io/installation)

If you run your tests from IntelliJ the results won't show up in the portal. Instead you need to run them from command line.