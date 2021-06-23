# Test Environment

## Table of content :
* [Specify options when running tests](#specify-options-when-running-tests)

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
