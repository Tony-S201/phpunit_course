# Test dependencies, fixtures and exceptions

## Table of content :

* [Unit test a queue class](#unit-test-a-queue-class)
* [Make one test method dependent on another](#make-one-test-method-dependent-on-another)
* [Make the tests independents for more understanding by using a state](#make-the-tests-independents-for-more-understanding-by-using-a-state)
* [Share fixtures between tests](#share-fixtures-between-tests)
* [Expecting code to throw an exception](#expecting-code-to-throw-an-exception)

## Unit test a queue class

> Here we have a class called Queue, a simple implementation of a queue data structure.
> A queue is a simple data structure like an array where items can be added and removed from the queue.

1. There is a push method which will add an item to the queue.
2. We have a pop mehod, which will take an item off the head of the queue and return it.
3. Finally we have a getCount method, which will return the number of items in the queue.

Let's write some unit tets to test the functionnality of this class.

*src/Queue.php*
```php
class Queue
{
  protected $items = [];
  
  public function push($item)
  {
    $this->items[] = $item;
  }
  public function pop()
  {
    return array_pop($this->items);
  }
  public function getCount()
  {
    return count($this->items);
  }
}
```

1. Assert that queue have 0 items by default.
2. Test item added to the queue.
3. Test remove item from the queue.

*tests/QueueTest.php*

```php
use PHPUnit\Framework\TestCase;

class QueueTest extends TestCase
{
  public function testNewQueueIsEmpty()
  {
    $queue = new Queue;
    
    $this->assertEquals(0, $queue->getCount());
  }
  public function testAnItemAddedToTheQueue()
  {
    $queue = new Queue;
    
    $queue->push('green');
    
    $this->assertEquals(1, $queue->getCount());
  }
  public function testAnItemIsRemovedFromTheQueue()
  {
    $queue = new Queue;
    
    $queue->push('green');
    
    $item = $queue->pop();
    
    $this->assertEquals(0, $queue->getCount());
    
    $this->assertEquals('green', $item);
  }
}
```

## Make one test method dependent on another

We can pass the sames items into differents tests to make it dependent on anothers.

For example, here in the second test, if we need to receive the same Queue object, we can pass it by adding a docblock property **@depends** and **the object to argument**.

> Notice we need to return the object in first method to access it in the second.

```php
use PHPUnit\Framework\TestCase;

class QueueTest extends TestCase
{
  public function testNewQueueIsEmpty()
  {
    $queue = new Queue;
    
    $this->assertEquals(0, $queue->getCount());
    
    return $queue;
  }
  /**
  * @depends testNewQueueIsEmpty
  */
  public function testAnItemAddedToTheQueue(Queue $queue)
  {
    $queue->push('green');
    
    $this->assertEquals(1, $queue->getCount());
  }
}
```

## Make the tests independents for more understanding by using a state

Having dependencies like previously can make it difficult to understand where a test's data coming from.

Before we had these dependencies, we were repeating code in each test that set up the data to a know state we actually made any assertions, this know state is called the fixture of the test.
So instead of making tests dependent on each other, PHPUnit provides us with various methods to set up the know state or fixture of each test method.

* The first is a method called **setUp()** and she create an empty Queue object for every test. 
* The second is a method called **tearDown()** and she destroy the object at the end of all tests.

```php
use PHPUnit\Framework\TestCase;

class QueueTest extends TestCase
{
  protected $queue;
  
  protected function setUp(): void
  {
    $this->queue = new Queue;
  }
  
  protected function tearDown(): void
  {
    unset($this->queue);
  }

  public function testNewQueueIsEmpty()
  {
    $this->assertEquals(0, $this->queue->getCount());
  }
  
  public function testAnItemAddedToTheQueue()
  {
    $this->queue->push('green');
    
    $this->assertEquals(1, $this->queue->getCount());
  }
}
```

## Share fixtures between tests

The previously methods create a know state for each test, this is fine if you're doing something simple.

But there are times when the setup code is more complicated and you might not want to call it for every test method.

* So in addition to the setup and tear down methods, we also have a **setUpBeforeClass()** and a **tearDownAfterClass()** methods.

These methods are just executed once the set up before class method is run, before the first test of the class is run and they tear down after class method is run afther the last test of the class.

* In this case, you can **share fixtures between test methods** by using these methods.

Example :

```php
use PHPUnit\Framework\TestCase;

class QueueTest extends TestCase
{
  protected static $queue;
  
  protected function setUp(): void
  {
    static::$queue->clear();
  }
  public static function setUpBeforeClass(): void
  {
    static::$queue = new Queue;
  }
  public static function tearDownAfterClass(): void
  {
    static::$queue = null;
  }

  public function testNewQueueIsEmpty()
  {
    $this->assertEquals(0, static::$queue->getCount());
  }
  
  public function testAnItemAddedToTheQueue()
  {
    static::$queue->push('green');
    
    $this->assertEquals(1, static::$queue->getCount());
  }
}
```

## Expecting code to throw an exception

We can add a call to the exception and the expected exception message method to check for that message or exception.

Two new methods to do this :

* **expectException()**
* **expectExceptionMessage()**

For example :

1. We create an exception message when the queue have one item.

*src/Queue.php*
```php
/**
* Add an item to the end of the queue
* @params mixed
*
* @throws QueueException
*
**/
public function push($item)
{
  $this->items[] = $item;

  if($this->getCount() == 1) {
    throw new QueueException("Queue is full");
  }
}
```

2. Create the custom exception class

*src/QueueException.php*
```php
<?php

class QueueException extends Exception
{
}
```

3. Expect the exception and exception message in your test

*tests/QueueTest.php*
```php
public function testExceptionsQueueItems()
{
  $this->queue->push('green');
  
  $this->expectException(QueueException::class);
  $this->expectExceptionMessage("Queue is full");
}
```

If there are exception and exception message 'Queue is full', the tests are passed without failure.
