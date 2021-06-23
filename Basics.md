# Basics

## Table of content :

* [Structure of a unit test](#Structure-of-a-unit-test)
* [Commands](#Commands)
* [Introduction to assertions](#Introduction-to-assertions) 

## Structure of a unit test

> The file must be placed in the tests folder, and the name should contains Test -> ExampleTest.php

First, we need to extends the **\PHPUnit\Framework\TestCase** class.

The template should be like this :

```php
<?php

use PHPUnit\Framework\TestCase;

class ExampleTest extends TestCase
{

}
```

## Commands

- Run a test :
> $phpunit tests/ExampleTest.php

## Introduction to assertions

