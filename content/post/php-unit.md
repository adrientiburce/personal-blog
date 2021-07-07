---
title: "Unit Testing : from PhpUnit to Coverage Report"
date: 2020-07-22
tags: ["php", "phpunit", "testing"]
draft: false
---

If you want to be confident each time you add a new functionality in your code, or when you need to change some old code of your application unit tests are a necessity. In this article, I will not try to convince the few (I hope) developers reticent with unit testing but rather showing you how to implement it without hustles.

-----------------

# 1. Installing our tools

*   [**PhpUnit**](https://phpunit.de/) : it will run all our tests

On a Symfony project simply run `composer require — dev symfony/phpunit-bridge`

*   [**Xdebug**](https://xdebug.org/) : it will run our tests and generate a coverage report on our code.

-----------------


# 2. Running our tests

## Configuration

Add a `phpunit.xml` file at the root of your project :

```xml
<phpunit colors="true">
    <testsuites>
        <testsuite name="Manager Test Suite">
            <directory>./Tests</directory>
        </testsuite>
    </testsuites>
</phpunit>
``` 

In order to get color highlight in our terminal we can configure our `phpunit.xml` with the`colors="true"`option.


## Get the results

*   Run **all our test** : `./bin/phpunit`
*   Running **only a directory** : `./bin/phpunit Tests/Manager`
*   Running **only one test** :`./bin/phpunit Tests/AmountTest.php --filter=testTotalAmount`


![result colored](https://cdn.hashnode.com/res/hashnode/image/upload/v1598105143730/2-SRlshbc.png)

Result of our tests

-----------------

# 3. Code Coverage

Now that we test our codebase and see which tests are failing, we can go further. Once all our tests are green we need to know what we should test next. Or which tests are too weak.

## Configuration

Add a `phpunit.coverage.xml` file at the root of your project :


```xml
<phpunit colors="true">
    <testsuites>
        <testsuite name="Manager test suites">
            <directory>./Tests/Manager</directory>
        </testsuite>
    </testsuites>
    <logging>
        <log type="coverage-html"
             target="./log/codeCoverage"
             lowUpperBound="40"
             highLowerBound="75"
             showUncoveredFiles="false"/>
    </logging>
    <filter>
<!--        files with at least one line executed appears in report-->
        <whitelist addUncoveredFilesFromWhitelist="false">
            <directory suffix=".php">./Manager</directory>
        </whitelist>
    </filter>
</phpunit>
``` 


You can now change phpunit options as you want :

- `<testsuites>` : which tests will be executed

- `<logging>` : configure code coverage info.

    *   _lowUpperBound_ which percentage code should be covered to be regarded as low
    *   _highUpperBound_ : minimum percentage for high coverage

<br />

- `<filter>` : specify which files will be included in the code coverage report

- `<whitelist>` : specify which files are being included using following options :

    *   `addUncoveredFilesFromWhitelist="false"` to only show files executed by the test in the report
    *   `processUncoveredFilesFromWhitelist="false"` all files are added to the report

## Report

Run this to see the generated coverage report : `/.bin/phpunit -c phpunit.coverage.xml`

Using the previous configuration, this command will run our tests again but thanks to **Xdebug** it will generate an html report.

<br />

![code report](https://cdn.hashnode.com/res/hashnode/image/upload/v1598105158184/lox1rHYDh.png)

The html coverage report can now be found at the `target` location configured in our `phpunit.coverage.xml` , in our case : `.log/codeCoverage`
