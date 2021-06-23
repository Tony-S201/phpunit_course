# Test dependencies, fixtures and exceptions

## Table of content :

* [Unit test a queue class](#unit-test-a-queue-class)
* [Make one test method dependent on another](#make-one-test-method-dependent-on-another)

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
