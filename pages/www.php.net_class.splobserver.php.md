# The SplObserver interface





```
<?php<br><br>/**<br> * Subject,that who makes news<br> */<br>class Newspaper implements \SplSubject{<br>    private $name;<br>    private $observers = array();<br>    private $content;<br>    <br>    public function __construct($name) {<br>        $this-&gt;name = $name;<br>    }<br><br>    //add observer<br>    public function attach(\SplObserver $observer) {<br>        $this-&gt;observers[] = $observer;<br>    }<br>    <br>    //remove observer<br>    public function detach(\SplObserver $observer) {<br>        <br>        $key = array_search($observer,$this-&gt;observers, true);<br>        if($key){<br>            unset($this-&gt;observers[$key]);<br>        }<br>    }<br>    <br>    //set breakouts news<br>    public function breakOutNews($content) {<br>        $this-&gt;content = $content;<br>        $this-&gt;notify();<br>    }<br>    <br>    public function getContent() {<br>        return $this-&gt;content." ({$this-&gt;name})";<br>    }<br>    <br>    //notify observers(or some of them)<br>    public function notify() {<br>        foreach ($this-&gt;observers as $value) {<br>            $value-&gt;update($this);<br>        }<br>    }<br>}<br><br>/**<br> * Observer,that who recieves news<br> */<br>class Reader implements SplObserver{<br>    private $name;<br>    <br>    public function __construct($name) {<br>        $this-&gt;name = $name;<br>    }<br>    <br>    public function update(\SplSubject $subject) {<br>        echo $this-&gt;name.&apos; is reading breakout news &lt;b&gt;&apos;.$subject-&gt;getContent().&apos;&lt;/b&gt;&lt;br&gt;&apos;;<br>    }<br>}<br><br>$newspaper = new Newspaper(&apos;Newyork Times&apos;);<br><br>$allen = new Reader(&apos;Allen&apos;);<br>$jim = new Reader(&apos;Jim&apos;);<br>$linda = new Reader(&apos;Linda&apos;);<br><br>//add reader<br>$newspaper-&gt;attach($allen);<br>$newspaper-&gt;attach($jim);<br>$newspaper-&gt;attach($linda);<br><br>//remove reader<br>$newspaper-&gt;detach($linda);<br><br>//set break outs<br>$newspaper-&gt;breakOutNews(&apos;USA break down!&apos;);<br><br>//=====output======<br>//Allen is reading breakout news USA break down! (Newyork Times)<br>//Jim is reading breakout news USA break down! (Newyork Times)  

#

Beware, you have written :<br><br>        if($key){<br>            unset($this-&gt;observers[$key]);<br>        }<br><br>When this should be :<br><br>        if(false !== $key){<br>            unset($this-&gt;observers[$key]);<br>        }<br><br>If the observer you want to delete is the first in your array, you will never delete it because the key would equal 0 and 0 == false as you know.  

#

[Official documentation page](https://www.php.net/manual/en/class.splobserver.php)

**[To root](/README.md)**