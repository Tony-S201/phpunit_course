# Test Environment

## Table of content :
* [Specify options when running tests](#specify-options-when-running-tests)
* [Configure phpunit with an XML file](#configure-phpunit-with-an-xml-file)

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
