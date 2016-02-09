## Introduction

This tutorial is designed to help you get familiar with what Selenium WebDriver is, and how you can use it to automate manual interactions with a web page.

By the end of this tutorial, you can expect to know...

- How Selenium can help you perform manual tasks on web applications with ease
- How to write a Library that maps out your web pages
- How to Write a Selenium test

I will be guiding you through the process of...

1. Setting up your computer with a JDK
2. Installing / Configuring an IDE (Integrated Development Environment)
3. Writing a library
4. Writing your first tests

First, let's talk about Selenium.

**What is Selenium?**
An excerpt from Selenium's [home page](http://seleniumhq.org),
> Selenium automates browsers. that's it.

Selenium enables you to emulate user interaction with a web page.

**What is WebDriver?**
WebDriver is now an official [W3C Specification](http://www.w3.org/TR/webdriver/), and in-short, it is a method of interacting with a web browser.  Previously, with Selenium RC, Selenium would operate with the browser by injecting JavaScript to interact with elements.  With the adoption of WebDriver, browser manufacturers like Google, Mozilla, and Microsoft release their browser with the ability to be controlled by a hook, that Selenium can tap into.  This hook enables Selenium to interact with the webbrowser in the same way that humans do.

## Lets get started...

### Setting up your computer with a JDK

We need a few things first to get up and running using Selenium..

1. A JRE/JDK installed
2. A Java IDE

*If you already have a JRE/JDK and have done Java programming before, you may continue to [Configuring your IDE](#configuring-your-ide)*

**\#1**: **[Download and Install a JDK](http://www.oracle.com/technetwork/java/javase/downloads/index.html)**

### Configuring your IDE

There are several IDE's out there such as Eclipse, NetBeans etc.  In this tutorial, we will be using IntelliJ IDEA Community Edition to get you started because it is quick to get up and running with this tutorial.

> [&raquo; Download IntelliJ IDEA Community Edition &laquo;](https://www.jetbrains.com/idea/#community_edition)

On the IntelliJ IDEA Community Edition webpage, scroll down the page until you see Download links.

Once you've downloaded the installer, follow the steps through.

**Upon completing installation**, you will see a Splash Screen.

![splash](http://i.imgur.com/lI6HCMJ.png)

---

## Setting up your Project...
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

6. With the Maven pom still open, we are ready to bring in the [Conductor framework](http://conductor.ddavison.io).  The Conductor Framework is a wrapper around Selenium WebDriver that is extremely fast to get up and running with Selenium WebDriver, as well as very easy to learn and easy to use.  **In the pom XML file that is open,**  We are going to type the following code under `<version>...</version>`.
```markup
<project...>
  ...
  <version>1.0-SNAPSHOT</version>
  
  <!-- copy the source below -->

  <dependencies>
      <dependency>
          <groupId>io.ddavison</groupId>
          <artifactId>conductor</artifactId>
          <version>2.1.1</version>
      </dependency>
  </dependencies>
</project>
```
*After typing this in, IntelliJ should display a dialog box that says "Maven projects need to be imported" ***Click `"Import Changes"`**  This is because Maven detected that we added a dependency to our project.
![maven-dependency](http://i.imgur.com/Qwe1Tkd.png)

You are now ready to start writing your test! &raquo;

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

First, let's prepare our project with the Conductor framework.

### Preparation

In order for Selenium to work, you need WebDriver executables. The Conductor framework makes this simple, and allows you to simply put them into the project path.

We are going to start out with the ChromeDriver executable to power Google Chrome.  **[See the Conductor documentation](https://github.com/ddavison/conductor/wiki/WebDriver-Executables) to install the ChromeDriver binary.**

After you follow the instructions for your respective platform, you will see the executable within the `selenium-tutorial/` project folder.

Now that we have our project all set up, and our WebDriver executable in place, we are ready to take a look at what we are going to be testing today.

### The Test Cases

We will be testing some very basic things on SeleniumHQ.org.  The official Selenium project website.

We will have two test cases that we will be using Selenium to automate.

> **Test Case #1**: When I navigate to seleniumhq.org, a "Download Selenium" link appears on the page.

![case1](http://i.imgur.com/xJ4XMsC.png)

> **Test Case #2**: On the SeleniumHQ Home page, I expect to see a *Projects*, *Download*, *Documentation*, *Support*, and *About* tab.

![case2](http://i.imgur.com/3BgIoOo.png)

Now, before we start writing our tests, let's flesh out our page libraries.

---

## Writing a library

In order for Selenium to know what to interact with, a good practice that the Conductor framework fits well with, is to map the elements you need in your application inside of a Java class.

Since we are automating SeleniumHQ's home page, our libraries will consist of objects that can be found on SeleniumHQ.

*(If you haven't yet, please review the [Test Cases](#the-test-cases) so you know what we are testing)*

A library can be defined simply as: `"A Java Class that maps objects on the page, so the tests can use them."`

*Why a library?*  Imagine we have a hundred test cases.  What happens when SeleniumHQ changes the elements around? For example, then change a `<div>` element to a `<span>` element, or even change an `id=` attribute.  Now we will have to find and replace a hundred selectors to get the tests working again.  A library keeps things organized, and maintainable in one place.  Once you update the library, all tests will be updated as well.

* Before we actually write the library, however, we need a package to put it under.  Let's right click `src/main/java`, and click `"New Package"`.

![new package](http://i.imgur.com/p5vYVWN.png)

* Name the package the same name as we did in the pom XML file (i.e. the GroupId(`com.mysite`) that we filled into maven earlier, appended by the ArtifactId(`selenium`))  **Fill in `com.mysite.selenium` for the package name** and click OK

* Right click the new package, and click `New->Java Class` and name this class `HomePage`
  * This class is called `HomePage` because it is going to map out the objects we need, from SeleniumHQ
  
![new javaclass](http://i.imgur.com/HhmQ2es.png)  

* **Let's map out the objects we need**. If you recall our test cases, we are going to be needing to interact with all of the following elements:
  - "Projects" tab - *because #2 says we need to validate the tab is there*
  - "Download" tab - *because #2 says we need to validate the tab is there*
  - "Documentation" tab - *because #2 says we need to validate the tab is there*
  - "Support" tab - *because #2 says we need to validate the tab is there*
  - "About" tab - *because #2 says we need to validate the tab is there*
  - "Download Selenium" link - *because #1 says we need to validate that the download link is there*
  

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

The values of these fields are CSS Selectors, combined with other Selenium selector strategies like `xpath` and `linkText`.  The Conductor framework is designed to utilize CSS most because of its simplicity, and readability, but of course the Conductor framework will still work if you prefer other selector strategies.  We got these selectors (like `li#menu_about a[href$='about/']`) by using Google Chrome's Developer Tools to find out how to uniquely select the element we need.  We can Right click an element, and click "Inspect Element" and we can then see the HTML.  Based on the HTML, we can create CSS selectors.

![inspect element](http://i.imgur.com/ItfQpvq.png)

*[See more on writing CSS selectors for selenium](http://ddavison.io/css/2014/02/18/effective-css-selectors.html)*.

**Now that our library is prepared, let's write our tests...**

---

## Writing the tests

*We will be using jUnit in this tutorial, which comes packaged with IntelliJ.*

1. Right click the `src/test/java` and click `New->Package`
2. Type in `"com.mysite.selenium"` as we did in [the library](#writing-a-library)

3. Right click the newly created package, and click `New->Java Class` and name this class `SeleniumHQTest`

4. Inside the newly created class, let's add one test method for each test case

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

## Conclusion / Next Steps
This is the end of the tutorial, and I do hope this helped you see how Selenium can work using Java.

It should be noted that by no means are you required to use the Conductor framework in your Selenium future, we had used the Conductor framework in this tutorial to show you a good effective test writing strategy.  I urge you to [browse through the source](https://github.com/ddavison/conductor/blob/master/src/main/java/io/ddavison/conductor/Locomotive.java#L45) to see exactly how Conductor wraps Selenium to make it easier.

It also should be noted, that you are by no means restricted to IntelliJ IDEA Community Edition either.

Selenium allows you to automate your web browser, in many ways you see fit.  Here is a list of things that you can and cannot do with Selenium

Selenium can...

- Fill in text boxes
- Check checkboxes, radio buttons
- Click buttons and links
- Navigate to and from `<iframe>`'s
- Navigate to and from new windows / tabs created from links
- Interact with most elements just like a human would

Selenium **cannot**...

- Interact with flash, PDF, or Java Applets
- Cannot interact with any embedded object put in by either `<object>`, or `<embed>`

To give you a taste of what Conductor has to offer with Selenium, you can take a look through [this page](https://github.com/ddavison/conductor#actions) to see what all you can do.

As an exercise using the framework we just set up, see what kind of things you can do on http://seleniumhq.org by creating new test methods in `SeleniumHQTest.java`.

---
*Author Information* / *Links*:
```
Name: Daniel Davison
GitHub: http://github.com/ddavison
Conductor Framework: http://conductor.ddavison.io
Twitter: @sircapsalot
Website: http://ddavison.io

  Daniel is an avid pursuer of test automation,
  and the value that it can bring for companies.
  Daniel is on StackOverflow, and regulars the [selenium]
  tag, as well as contributes to the Selenium project in various ways.
```
