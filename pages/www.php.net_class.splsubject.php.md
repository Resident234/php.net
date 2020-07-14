# The SplSubject interface





```
<?php 

// Example implementation of Observer design pattern:

class MyObserver1 implements SplObserver {
    public function update(SplSubject $subject) {
        echo __CLASS__ . &apos; - &apos; . $subject-&gt;getName();
    }
}

class MyObserver2 implements SplObserver {
    public function update(SplSubject $subject) {
        echo __CLASS__ . &apos; - &apos; . $subject-&gt;getName();
    }
}

class MySubject implements SplSubject {
    private $_observers;
    private $_name;

    public function __construct($name) {
        $this-&gt;_observers = new SplObjectStorage();
        $this-&gt;_name = $name;
    }

    public function attach(SplObserver $observer) {
        $this-&gt;_observers-&gt;attach($observer);
    }

    public function detach(SplObserver $observer) {
        $this-&gt;_observers-&gt;detach($observer);
    }

    public function notify() {
        foreach ($this-&gt;_observers as $observer) {
            $observer-&gt;update($this);
        }
    }

    public function getName() {
        return $this-&gt;_name;
    }
}

$observer1 = new MyObserver1();
$observer2 = new MyObserver2();

$subject = new MySubject("test");

$subject-&gt;attach($observer1);
$subject-&gt;attach($observer2);
$subject-&gt;notify();

/* 
will output:

MyObserver1 - test
MyObserver2 - test
*/

$subject-&gt;detach($observer2);
$subject-&gt;notify();

/* 
will output:

MyObserver1 - test
*/

?>
```
  

#

[Official documentation page](https://www.php.net/manual/en/class.splsubject.php)

**[To root](/README.md)**