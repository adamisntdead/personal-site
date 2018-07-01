---
title: The easy way to start automatically testing your website
date: '2018-07-01T03:00:38+01:00'
excerpt: >-
  Some testing tasks can be automated to handle these tasks more efficiently.
  This article will cover what sorts of tests can be automated, and how you can
  implement them using Node.js and Nightwatch.
comments: true
noauthor: false
math: false
share: true
categories: posts
---
> This post was first published on my [Medium Account](https://medium.freecodecamp.org/how-to-easily-start-automatically-testing-your-website-8629ea8df04a)


Manually running the same tests on Chrome, Safari, and Firefox multiple times can be time-consuming and tedious.

Some testing tasks can be automated to handle these tasks more efficiently. This article will cover what sorts of tests can be automated, and how you can implement them using Node.js and Nightwatch.

Nightwatch connects to a WebDriver (such as Selenium). It works over a REST API. For example, the initialization is:

![How Nightwatch connects to Selenium](https://cdn-images-1.medium.com/max/3180/1*VU8YPJRqwqwcxywKFP4KuA.png)*How Nightwatch connects to Selenium*

## Requirements

You should have Java installed. You can check this by running

```bash
$ java -version
```

in your terminal. If you don’t have it installed, go [install](https://www.java.com/en/download/help/download_options.xml) it.

You must have a fairly recent version of Safari, version 10 or later. To use Safari, you must also enable the developer menu and enable the **Allow Remote Automation** option.

Lastly, you must also have [Node.js](https://nodejs.org/en/).

## Setting Up
> Note: This assumes that the site already uses node.js. If it doesn’t, you would also have to initialize it with a `package.json` file.

### Installing

**[Nightwatch.js](http://nightwatchjs.org/)** is an easier way to write tests that running them using Selenium.

To install it, `cd` into your project, and then install the module from npm to your dev-dependencies:

```bash
$ npm install --save-dev nightwatch
```

Now you need to install Selenium. The easiest way to do that is with another npm module, `selenium-server`:

```bash
$ npm install --save-dev selenium-server
```

We want to test on Chrome, Safari, and Firefox, so we must also install their drivers. The driver is what is used by Selenium to control the browsers. Safari’s driver is built into MacOS, and the Chrome and Firefox drivers can be installed using npm:

```bash
$ npm install --save-dev chromedriver geckodriver
```

### Configuring

Nightwatch is configured using a configuration file. The configuration file is a normal JavaScript file, which I will call `nightwatch.conf.js`. This file can be put in a folder, such as `tests`.

Copy and paste this basic configuration into your `nightwatch.conf.js` file:

```javascript
// Get Selenium and the drivers
var seleniumServer = require('selenium-server');
var chromedriver = require('chromedriver');
var geckodriver = require('geckodriver');

var config = {
  src_folders: [
    // Folders with tests
    'tests/features'
  ],
  output_folder: 'reports', // Where to output the test reports
  selenium: {
    // Information for selenium, such as the location of the drivers ect.
    start_process: true,
    server_path: seleniumServer.path,
    port: 4444, // Standard selenium port
    cli_args: {
      'webdriver.chrome.driver': chromedriver.path,
      'webdriver.gecko.driver': geckodriver.path
    }
  },
  test_workers: {
    // This allows more then one browser to be opened and tested in at once
    enabled: true,
    workers: 'auto'
  },
  test_settings: {
    default: {
      screenshots: {
        enabled: false
      },
      globals: {
        // How long to wait (in milliseconds) before the test times out
        waitForConditionTimeout: 5000
      },
      desiredCapabilities: {
        // The default test
        browserName: 'chrome',
        javascriptEnabled: true,
        acceptSslCerts: true,
        nativeEvents: true
      }
    },
    // Here, we give each of the browsers we want to test in, and their driver configuration
    chrome: {
      desiredCapabilities: {
        browserName: 'chrome',
        javascriptEnabled: true,
        acceptSslCerts: true,
        nativeEvents: true
      }
    },
    firefox: {
      desiredCapabilities: {
        browserName: 'firefox',
        javascriptEnabled: true,
        acceptSslCerts: true,
        nativeEvents: true
      }
    },
    safari: {
      desiredCapabilities: {
        browserName: 'safari',
        javascriptEnabled: true,
        acceptSslCerts: true,
        nativeEvents: true
      }
    }
  }
};

module.exports = config;
```

Now you have your automated testing framework setup and you are able to start writing tests!

## What Can/Should You Test?

There are two types of testing which you can do with this setup:

1. Functional testing

1. Regression testing

Functional testing is the type of testing which tries to see if your website conforms with all of its requirements. Usually, this means implementing tests that make sure it has all of the features required.

For example, if the client wants a login form, you could write a test to check that when you’re logged in, your name is visible on the web page.

The other type of testing is Regression Testing. This is done to make sure the website you developed and tested before still works the same way after it has changed. This is especially useful when clients can edit parts of their website that could potentially cause functionality errors.

For example, you might write a test to make sure that a page has the correct meta tags and information.

A limitation to this testing framework is that it tests the website, not the code. For example, it cannot test that a DB query has been made, but it could test that there is a new row made from that query.

However, you could use other unit testing frameworks alongside (such as [Jest](https://facebook.github.io/jest/), [Mocha](https://mochajs.org/), and [Ava](https://github.com/avajs/ava)), to test your core logic and code.

## Writing Tests

Welcome to the fun part of testing. Ah just joking. **It’s all fun**.

Anyway, tests in Nightwatch are written using a normal JavaScript module. In the nightwatch.conf.js file, we specified to look in tests/features for the tests.

Nightwatch will take this folder and look for all JavaScript files in it, and then will try to run them as tests.

Let’s create a really simple test: we’ll test that the title of [www.bing.com](http://www.bing.com) is Bing. Why you would go to Bing in the first place is a bigger issue that cannot be addressed in this article.

Create a new file in `tests/features` called `bing.test.js`, containing the following code:

```javascript
module.exports = {
  'Title is Bing': function(browser) {
    // Browser is the browser that is being controlled
    browser
      .url('https://www.bing.com') // Navigate to the url
      .waitForElementVisible('body', 1000) // Wait until you can see the body element.
      .verify.title('Bing') // Verify that the title is 'Bing'
      .end() // This must be called to close the browser at the end
    }
}
```

Now the test is created!

Here, `Title is Bing` is the name of the test. This is shown when the tests are run. `.verify` is used to actually perform a test. `.waitForElementVisable` expects a CSS selector or an Xpath selector to specify what elements you are referring to.

You can have more then one test in the same file, and it’s good practice to keep related tests in the same file.

For more on creating tests, the Nightwatch documentation is really good. If you want to know how to do something, check [this](http://nightwatchjs.org/guide/#using-nightwatch) out first. Also, check out the “Above and Beyond” section of this guide.

## Running Tests

To run the tests that you now have, go to the `package.json` file. Add the following:

```json
“scripts”: {
  “nightwatch”: “nightwatch -c tests/nightwatch.conf.js -e chrome,firefox,safari”
}
```

This will run your tests with Chrome, Firefox, and Safari.

To run your test, in your terminal do the command:

```bash
$ npm run nightwatch
```

If you still have the same test as above, your output should look like this:

![Output of the test suite](https://cdn-images-1.medium.com/max/2160/1*osMTcdZ_LyV0LXFqem2d6A.png)*Output of the test suite*

If you get errors, check out the *Common Issues* section of this article, or Google the error.

And **There you go**, you have now set up and used automated browser testing! 🎉

## Above and Beyond

### Best Practices and More In Depth Guides

There are loads of good resources, but here are some of my favorites:

* UI Testing with Nightwatch.js — Page Objects | [http://matthewroach.me/ui-testing-with-nightwatch-js-page-objects/](http://matthewroach.me/ui-testing-with-nightwatch-js-page-objects/)

* Writing UI Tests For TodoMVC APP | [https://blog.cloudboost.io/e2e-testing-with-nightwatch-part-two-aaa25a4dc033](https://blog.cloudboost.io/e2e-testing-with-nightwatch-part-two-aaa25a4dc033)

* Understanding the Command Queue | [https://github.com/nightwatchjs/nightwatch/wiki/Understanding-the-Command-Queue](https://github.com/nightwatchjs/nightwatch/wiki/Understanding-the-Command-Queue)

* Page Object API | [https://github.com/nightwatchjs/nightwatch/wiki/Page-Object-API](https://github.com/nightwatchjs/nightwatch/wiki/Page-Object-API)

* Nightwatch API | [http://nightwatchjs.org/api](http://nightwatchjs.org/api)

* Nightwatch.js Gotchas | [https://ericheikes.com/nightwatch-js-gotchas/](https://ericheikes.com/nightwatch-js-gotchas/)

* Nightwatch how to assert multiple elements | [https://stackoverflow.com/questions/27116103/nightwatch-js-how-to-assert-multiple-elements?rq=1](https://stackoverflow.com/questions/27116103/nightwatch-js-how-to-assert-multiple-elements?rq=1)

### Testing With Edge Browser

You can test with Edge using the [Microsoft WebDriver](https://developer.microsoft.com/en-us/microsoft-edge/tools/webdriver/). It is compatible with Windows 10 onwards.

After you download the binary, go to your `nightwatch.conf.js` file and under `"webdriver.gecko.driver"` put

```json
"webdriver.edge.driver" : "location/of/binary/MicrosoftWebDriver.exe"
```

Then, go to `test_settings` and add another object under `safari`:

```ja
edge: {
    desiredCapabilities: {
        browserName: 'MicrosoftEdge',
        javascriptEnabled: true,
        acceptSslCerts: true,
        nativeEvents: true
    }
}
```

Lastly, go to your `package.json` and change the `nightwatch` script to:

```json
"nightwatch": "nightwatch -c tests/nightwatch.conf.js -e chrome,firefox,safari,edge"
```

### Using With CI

This is beyond the scope of this article, but you can check out:

* [https://github.com/dwyl/learn-nightwatch/issues/8](https://github.com/dwyl/learn-nightwatch/issues/8)

* Using Meteor, Travis CI and Nightwatch [https://github.com/zeroasterisk/meteor-travis-ci-nightwatch](https://github.com/zeroasterisk/meteor-travis-ci-nightwatch)

* Selenium integration with Jenkins [http://learn-automation.com/selenium-integration-with-jenkins/](http://learn-automation.com/selenium-integration-with-jenkins/)

## Common Issues

### Nightwatch Cannot Find Selenium

If nightwatch cannot find selenium, try running your configuration file with node. If your config file, `nightwatch.conf.js` is in the directory `tests`, run:

```bash
$ node tests/nightwatch.conf.js
```

### Safari Popups

Safari might warn you that it is being controlled. Just allow it. If the test failed or timed out, run it again.

### Safari Doesn’t Close

Just close it manually.

### To view this web content, you need to install the Java Runtime Environment

I did say you had to install Java, so go [install it](https://www.java.com/en/download/help/download_options.xml).

### ERROR — Port 4444 is busy, please choose a free port and specify it using -port option

This means that something is using the 4444 port. You can either stop that process or change that port.

To see which process is using the port on Mac, use the command

```bash
$ lsof -n -i4TCP:4444 | grep LISTEN
```

You can then kill that process.

If you don’t want to kill that process, go to the `nightwatch.conf.js` file and in `selenium`, change `port` to an unused port.
