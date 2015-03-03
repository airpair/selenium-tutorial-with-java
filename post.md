## Introduction

**What is Selenium?**
An excerpt from Selenium's [home page](http://seleniumhq.org),
> Selenium automates browsers. that's it.

Selenium allows you to emulate user interaction with a web page.

### Prerequisites

We need a few things first to get up and running using Selenium..

1. A JRE/JDK installed
2. A Java IDE

*If you already have a JRE/JDK and have done Java programming before, continue to \#2*

**\#1**: **[Download and Install a JDK](http://www.oracle.com/technetwork/java/javase/downloads/index.html)**

**\#2**: **[Proceed to *Configuring your IDE* &raquo;](#configuring-your-ide)**

### Configuring your IDE

There are several IDE's out there such as Eclipse, NetBeans etc.  In this tutorial, we will be using IntelliJ IDEA Community Edition to get you started because it is quick to get up and running with this tutorial.

> [&raquo; Download IntelliJ IDEA Community Edition &laquo;](https://www.jetbrains.com/idea/#community_edition)

On the IntelliJ IDEA Community Edition webpage, scroll down the page until you see Download links.

Once you've downloaded the installer, follow the steps through.

**Upon completing installation**, you will see a Splash Screen.

![splash](http://i.imgur.com/lI6HCMJ.png)

---

## Let's get started...
Follow these steps:

1. **We will click "Create New Project"**
![new-screen](http://i.imgur.com/TAjb2CD.png)

2. **We will select `Maven`**
![new-maven](http://i.imgur.com/CvMvl2J.png)

    * If your "Project SDK" states "&lt;None&gt;" as in the picture, you'll need to click "New", then find the installation path of the JDK you have installed.

3. **Now let's fill out what's required of a new maven project**
![maven-step2](http://i.imgur.com/dCWkwTY.png)
    * In Maven, a "GroupId" is the base java package that you will be using. (This will make sense later in the tutorial)
    * The ArtifactId is simply the name of the project we are working on.  In our case, this is a Selenium project, so we will be using the name `"selenium"`
    * Leave the "Version" as-is

4. In the next window that pops up, you will be able to fill out the project location, and the project name.  We will name this project `selenium-tutorial`.
![project-name](http://i.imgur.com/5Fjg561.png)

5. Click finish.  *Upon startup, you may see a "Tip of the Day" window pop up.  Close this.*  **At this point,** you will see an XML file open.  This XML file is the Maven pom (or Project Object Model).  The pom is a file dedicated to managing dependencies, and other configurations that your project has, such as plugins, and test goals.

6. With the Maven pom still open, we are ready to bring in the [Conductor framework](http://conductor.ddavison.io).  The Conductor Framework is a wrapper around Selenium 2 that is extremely fast to get up and running with Selenium 2, as well as very easy to learn and easy to use.  **In the pom XML file that is open,**  We are going to type the following code under `<version>...</version>`.
```markup
<project...>
  ...
  <version>1.0-SNAPSHOT</version>
  
  <!-- copy the source below -->

  <dependencies>
      <dependency>
          <groupId>io.ddavison</groupId>
          <artifactId>conductor</artifactId>
          <version>1.1</version>
      </dependency>
  </dependencies>
</project>
```
*After typing this in, IntelliJ should display a dialog box that says "Maven projects need to be imported" ***Click `"Import Changes"`**  This is because Maven detected that we added a dependency to our project.
![maven-dependency](http://i.imgur.com/Htx7gUY.png)

[You are now ready to start writing your test! &raquo;](#writing-our-first-test)

---

## Writing our first test
In our project structure, you will see that we have a folder hierarchy that looks like this:

```
|selenium-tutorial/
|  .idea/
|  src/
|    main/
|        java/
|        resources
|    test/
|        java/
```

You are looking at the standard project hierarchy for Maven.

`src/main/java` is where we are going to put our libraries for our webapp, and
`src/test/java` is where we are going to put our actual automated tests.

**Today, we will be automating the SeleniumHQ [home page](http://seleniumhq.org).**

First, [let's prepare our project &raquo;](#preparation) with the Conductor framework.

### Preparation

In order for Conductor (and Selenium for that matter,) you need respective WebDriver executables to be installed inside of the project path to use.

[See the Conductor documentation](https://github.com/ddavison/conductor/wiki/WebDriver-Executables) to install the ChromeDriver binary.

After you follow the instructions for your respective platform, you will see the executable within the `selenium-tutorial/` project folder.

*Screenshot*
![chromedriver](http://i.imgur.com/8NfzTEk.png)

> [Continue to the test cases &raquo;](#the-test-cases)

### The Test Cases

The test cases that we will be testing on SeleniumHQ.org, follow:

> **Test Case #1**: When I navigate to seleniumhq.org, a "Download Selenium" link appears on the page.

![case1](http://i.imgur.com/xJ4XMsC.png)

> **Test Case #2**: On the SeleniumHQ Home page, I expect to see a *Projects*, *Download*, *Documentation*, *Support*, and *About* tab.

![case2](http://i.imgur.com/3BgIoOo.png)

> [Proceed to writing the library... &raquo;](#writing-a-library)

---

## Writing a library

In order for Selenium to know what to interact with, a good practice that the Conductor framework fits well with, is to map the elements you need in your application inside of a Java class.

Since we are automating SeleniumHQ's home page, our libraries will consist of objects that can be found on SeleniumHQ.

*(If you haven't yet, please review the [Test Cases](the_test_cases) so you know what we are testing)*

A library can be defined simply as: `"A Java Class that maps objects on the page, so the tests can use them."`

*Why a library?*  Imagine we have a hundred test cases.  What happens when SeleniumHQ changes the selectors around?  Now we will have to find and replace a hundred selectors.  A library keeps things organized, and maintainable in one place.

1. Before we actually write the library, however, we need a package to put it under.  Let's right click `src/main/java`, and click `"New Package"`.

2. Name the package the same name as we did in the pom XML file (i.e. the GroupId(`com.mysite`) that we filled into maven earlier, appended by the ArtifactId(`selenium`))  **Fill in `com.mysite.selenium` for the package name**

3. Right click the new package, and click `New->Java Class` and name this class `HomePage`
  * This class is called `HomePage` because it is going to map out the objects we need, from SeleniumHQ

4. **Let's map out the objects we need**. According to the [test cases](the_test_cases) we are going to be needing to interact with all of the following elements:
  - "Projects" tab
  - "Download" tab
  - "Documentation" tab
  - "Support" tab
  - "About" tab
  - "Download Selenium" link

Each of these elements is going to be mapped out in our library.

**HomePage.java**
```java
package com.mysite.selenium;

import org.openqa.selenium.By;

public class HomePage {
    // the tabs
    public static final String LOC_LNK_PROJECTSTAB = "li#menu_projects a[href$='projects/']";
    public static final String LOC_LNK_DOWNLOADTAB = "li#menu_download a[href$='download/']";
    public static final By LOC_LNK_DOCUMENTATIONTAB = By.xpath("//li[@id='menu_documentation']/a[contains(@href, 'docs/')]");
    public static final String LOC_LNK_SUPPORTTAB = "li#menu_support a[href$='support/']";
    public static final String LOC_LNK_ABOUTTAB = "li#menu_about a[href$='about/']";

    // download link
    public static final By LOC_LNK_DOWNLOADSELENIUM = By.linkText("Download Selenium");
}
```

**Let's disect this...**

We are adding the `public static final` modifiers to these fields because *one*, they will not and should not be changed during runtime.  *two*, because they need to be public, and statically accessed using `HomePage.LOC_ELEMENT` in your tests.

The name, `LOC_LNK_PROJECTSTAB` is an easy moniker to figure out what you are selecting.  The `LOC_` simply means that this is a locator (so we dont get confused if we add things later).  The `LNK_` part, is based on the application of [hungarian notation](http://wikipedia.org/wiki/Hungarian_notation).  Since the element we are going to find is a Link, it is `LNK_`.  Respectively, if we are going to interact with an Input, it will be `LOC_TXT_` since it's a text box.  `LOC_BTN_` if it's a button.  It helps keep your libraries organized.

The values of these fields are CSS Selectors, combined with other Selenium selector strategies like `xpath` and `linkText`.  The Conductor framework is designed to utilize CSS most because of its simplicity, and readability, but of course the Conductor framework will still work if you prefer other selector strategies.  *[See more on writing effective css selectors for selenium](http://ddavison.io/css/2014/02/18/effective-css-selectors.html)*.

Now that our library is prepared, let's **[Continue on, and write our test &raquo;](#writing-the-test)**

---

## Writing the test

*We will be using jUnit in this tutorial, which comes packaged with IntelliJ.*

1. Right click the `src/test/java` and click `New->Package`
2. Type in `"com.mysite.selenium"` as we did in [the library](#writing-a-library)

3. Right click the newly created package, and click `New->Java Class` and name this class `SeleniumHQTest`

4. Inside the newly created class, let's add one test method for each [test case](#the-test-cases)

```java
package com.mysite.selenium;

import org.junit.Test;

public class SeleniumHQTest {
    @Test
    public void testDownloadLinkExists() {

    }

    @Test
    public void testTabsExist() {

    }
}

```

Then let's configure our class to run against SeleniumHQ.org using the Chrome browser, and to extend `Locomotive`, the base test class from the Conductor framework.

```java
package com.mysite.selenium;

import io.ddavison.conductor.Browser;
import io.ddavison.conductor.Config;
import io.ddavison.conductor.Locomotive;
import org.junit.Test;

@Config(
        browser = Browser.CHROME,
        url     = "http://seleniumhq.org"
)
public class SeleniumHQTest extends Locomotive {
    @Test
    public void testDownloadLinkExists() {

    }

    @Test
    public void testTabsExist() {

    }
}

```

### Test Case #1 - Validate that the download link exists.
```java
    public void testDownloadLinkExists() {
        validatePresent(HomePage.LOC_LNK_DOWNLOADSELENIUM);
    }
```
Simple as that.  By the time that this method starts, we will already be at the URL specified within `@Config` so we are ready to test.

Here, we are just validating that the `LOC_LNK_DOWNLOADSELENIUM` (The "Download Selenium" link that was put into the `HomePage` library) exists.

> **Right click the method `testDownloadLinkExists` and click `"Run testDownloadLinkExists()"`**

### Test Case #2 - Validate that all tabs exist.
```java
    public void testTabsExist() {
        validatePresent(HomePage.LOC_LNK_PROJECTSTAB)
        .validatePresent(HomePage.LOC_LNK_DOWNLOADTAB)
        .validatePresent(HomePage.LOC_LNK_DOCUMENTATIONTAB)
        .validatePresent(HomePage.LOC_LNK_SUPPORTTAB)
        .validatePresent(HomePage.LOC_LNK_ABOUTTAB)
        ;
    }
```

One of the beautiful things about the Conductor framework which makes it so easy to read and use, is the [fluent interface](http://wikipedia.org/wiki/Fluent_interface).  when written like this, you are able to move lines around, comment them out, as well as read them like plain english.

> **Right click the method `testTabsExists` and click `"Run testTabsExists()"`**
<br><br>

---

## Conclusion
This is the end of the tutorial, and I do hope this helped you see how Selenium can work using Java.

It should be noted that by no means are you required to use the Conductor framework in your Selenium future, we had used the Conductor framework in this tutorial to show you a good effective test writing strategy.

It also should be noted, that you are by no means restricted to IntelliJ IDEA Community Edition either.  This software is meant for educational, and open-source purposes.  You are perfectly able to setup the exact same testing infrastructure using any IDE, using any Test Framework.

Thanks so much!

---
*Author Information* / *Links*:
```
Name: Daniel Davison
GitHub: http://github.com/ddavison
Conductor Framework: http://conductor.ddavison.io
Twitter: @sircapsalot

  Daniel is an avid pursuer of test automation,
  and the value that it can bring for companies.
  Daniel is on StackOverflow, and regulars the [selenium]
  tag and in the top 10 worldwide of upvotes, and top 2 of
  number of answers.
```