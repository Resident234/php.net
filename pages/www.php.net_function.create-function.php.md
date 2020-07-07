# create_function




<div class="phpcode"><span class="html">
Whilst it was correct 11 years ago, the statement of Dan D is not so correct any more&#x44E; Anonymous functions are now objects of a class Closure and are safely collected by garbage collector.</span>
</div>
  

#


<div class="phpcode"><span class="html">
Beware when using anonymous functions in PHP as you would in languages like Python, Ruby, Lisp or Javascript.&#xA0; As was stated previously, the allocated memory is never released; they are not objects in PHP -- they are just dynamically named global functions -- so they don&apos;t have scope and are not subject to garbage collection.<br><br>So, if you&apos;re developing anything remotely reusable (OO or otherwise), I would avoid them like the plague.&#xA0; They&apos;re slow, inefficient and there&apos;s no telling if your implementation will end up in a large loop.&#xA0; Mine ended up in an iteration over ~1 million records and quickly exhasted my 500MB-per-process limit.</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/function.create-function.php)

**[To root](/README.md)**