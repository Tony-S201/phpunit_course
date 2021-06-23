# Basics

## Table of content :

* [Structure of a unit test](#structure-of-a-unit-test)
* [Commands](#commands)
* [Introduction to assertions](#introduction-to-assertions) 
* [Test a function of an existing file of your project](#test-a-function-of-an-existing-file-of-your-project)
* [Test that incorrect results are not returned using multiple tests methods](#test-that-incorrect-results-are-not-returned-using-multiple-tests-methods)
* [Test a class](#test-a-class)

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

- **All assertions methods list from official documentation** : [link](https://phpunit.readthedocs.io/fr/latest/assertions.html)

An assertion basically verifies that some condition is true in a test.

For example, let's check that 2 + 2 = 4, we can use **->assertEquals()** :

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

## Test that incorrect results are not returned using multiple tests methods

We can check that incorrect result is not returned using **->assertNotEquals()** :

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
  
  public function testAddDoesntReturnTheIncorrectSum()
  {
    $this->assertNotEquals(5, add(2, 2));
  }
}
```

## Test a class

Instead of function, we can test everything we want in a class.

*User.php*
```php
<?php

class User
{
  public $first_name;
  public $surname;
  
  public function getFullName()
  {
    return trim("$this->first_name $this->surname");
  }
}
```

For example, we test that the getFullName() method return always the good result, and nothing if first_name and surname are empty.

*UserTest.php*
```php
<?php

class UserTest extends PHPUnit\Framework\TestCase
{
  public function testReturnsFullName()
  {
    require 'User.php';
    
    $user = new User;
    $user->first_name = "Teresa";
    $user->surname = "Green";
    
    $this->assertEquals('Teresa Green', $user->getFullName());
  }
  
  public function testFullNameIsEmptyByDefault()
  {
    $user = new User;
    
    $this->assertEquals('', $user->getFullName());
  }
}
```

