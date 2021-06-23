# Test Environment

## Table of content :
* [Specify options when running tests](#specify-options-when-running-tests)
* [Configure phpunit with an XML file](#configure-phpunit-with-an-xml-file)
* [Autoload classes using composer](#autoload-classes-using-composer)

## Specify options when running tests

- See options :
> $phpunit
- Run a test :
> $phpunit tests/ExampleTest.php
- Run a test as a file without extension :
> $phpunit tests/ExampleTest
- Run all the tests present in the folder :
> $phpunit tests/
- Run a specific test method present in a test class :
> $phpunit tests/ --filter=testReturnsFullName
- Highlighted color depend on result (OK or FAILURE) option :
> $phpunit tests/ --filter=testReturnsFullName --color

## Configure phpunit with an XML file

* Documentation here : [link](https://phpunit.readthedocs.io/fr/latest/configuration.html)

We can specify options directly in a XML file, this allows for example not to write the options again.

Let's set the colors attribute to true :

*phpunit.xml*
```xml
<?xml version="1.0" encoding="UTF-8"?>
<phpunit colors="true">
</phpunit>
```

Now if we run the tests again without specifying the color option on the command line, the colours are enabled and we get highlighted output.

We can also specify what we want to test on every execution :

*phpunit.xml*
```xml
<?xml version="1.0" encoding="UTF-8"?>
<phpunit colors="true">
  <testsuites>
    <testsuite name="Test suite">
      <directory suffix=".php">tests</directory>
    </testsuite>
  </testsuites>
</phpunit>
```

In this case we run all the test classes in the tests folder that have the file name extension **.php**.

## Autoload classes using composer

Use multiple **require** can be redundant, to solve this problem composer offers several ways to **autoload classes**, so we don't have to worry about loading class files manually.

1. Add the autoload in your composer package list :
*composer.json*
```json
{
  "require": {
    "phpunit/phpunit": "^7.3"  
  },
  "autoload": {
    "psr-4": {
      "": "src/"
    }
  }
}
```

In this way, any classes we place in the src folder will be autoload.

2. Generate the composer autoload files :

> $ composer dump-autoload

3. Pass the option to run autoload before the tests.

> $ phpunit --bootstrap='vendor/autoload.php'
