# ReflectionClass::newInstanceWithoutConstructor




<div class="phpcode"><span class="html">
It should be made clear that from an OOP theory perspective the use of this method is very bad practice in the same manner as goto, eval and singletons. If you find a need to use it in production code you&apos;re almost certainly doing something wrong somewhere. It may occasionally be useful for debugging, but even then hints at poor initial code.<br><br>The problem? It breaks encapsulation. An object can exist in the application but may not be able to fulfill its responsibility because it&apos;s missing dependencies. The use of this method makes it possible for an incomplete object to exist in the system; the object can exist in a state that its author never intended. This is bad because it will cause unexpected things to happen! A fundamental principle in OOP is that objects are in complete control of their state, the use of this method prevents that guarantee. <br><br>n.b. The annotation based &quot;dependency injection&quot; listed below is not a solution or valid use-case for this either because it breaks encapsulation (Among other things!) and the class being constructed needs to know of the container by providing annotations.</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/reflectionclass.newinstancewithoutconstructor.php)

**[To root](/README.md)**