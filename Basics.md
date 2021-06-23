# Basics

## Table of content :

* [Structure of a unit test](#structure-of-a-unit-test)
* [Commands](#commands)
* [Introduction to assertions](#introduction-to-assertions) 
* [Test a function of an existing file of your project](#test-a-fonction-of-an-existing-file-of-your-project)

## Structure of a unit test

> The file must be placed in the tests folder, and the name should contains Test -> ExampleTest.php

First, we need to extends the **\PHPUnit\Framework\TestCase** class.

The template should be like this :

```php
<?php

use PHPUnit\Framework\TestCase;

class ExampleTest extends TestCase
{
  public function test()
  {
  
  }
}
```

## Commands

- Run a test :
> $phpunit tests/ExampleTest.php

## Introduction to assertions

An assertion basically verifies that some condition is true in a test.

For example, let's check that 2 + 2 = 4 :

```php
<?php

use PHPUnit\Framework\TestCase;

class ExampleTest extends TestCase
{
  public function testAddingTwoPlusTwoResultsInFour()
  {
    $this->assertEquals(4, 2 + 2);
  }
}
```

## Test a function of an existing file of your project

So, we have this method in functions.php :

```php
<?php

function add($a, $b) {
  return $a + $b;
}
```

And we want to check if she works correctly with multiple assertions :

```php
<?php

use PHPUnit\Framework\TestCase;

class ExampleTest extends TestCase
{
  public function testAddReturnsTheCorrectSum()
  {
    require 'functions.php';
    
    $this->assertEquals(4, add(2 + 2));
    $this->assertEquals(8, add(4 + 4));
  }
}
```

