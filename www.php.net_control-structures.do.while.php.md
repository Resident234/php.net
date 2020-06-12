# do-while




<div class="phpcode"><span class="html">
There is one major difference you should be aware of when using the do--while loop vs. using a simple while loop:&#xA0; And that is when the check condition is made.&#xA0; <br><br>In a do--while loop, the test condition evaluation is at the end of the loop.&#xA0; This means that the code inside of the loop will iterate once through before the condition is ever evaluated.&#xA0; This is ideal for tasks that need to execute once before a test is made to continue, such as test that is dependant upon the results of the loop.&#xA0; <br><br>Conversely, a plain while loop evaluates the test condition at the begining of the loop before any execution in the loop block is ever made. If for some reason your test condition evaluates to false at the very start of the loop, none of the code inside your loop will be executed.</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/control-structures.do.while.php)

**[To root](/README.md)**