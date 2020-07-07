# Global space





Included files will default to the global namespace.



```
<?php

//test.php

namespace test {

&#xA0; include &apos;test1.inc&apos;;

&#xA0; echo &apos;-&apos;,__NAMESPACE__,&apos;-&lt;br /&gt;&apos;;

}

?>
```






```
<?php

//test1.inc

&#xA0; echo &apos;-&apos;,__NAMESPACE__,&apos;-&lt;br /&gt;&apos;;

?>
```




Results of test.php:



--

-test-

  

#



In namespaced context the Exception class needs to be prefixed with global prefix operator.



```
<?php

namespace hey\ho\lets\go;

class MyClass
{
&#xA0; &#xA0; public function failToCatch()
&#xA0; &#xA0; {
&#xA0; &#xA0; &#xA0; &#xA0; try {
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $thing = somethingThrowingAnException();
&#xA0; &#xA0; &#xA0; &#xA0; } catch (Exception $ex) {
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; // Not catched
&#xA0; &#xA0; &#xA0; &#xA0; }
&#xA0; &#xA0; }

&#xA0; &#xA0; public function succeedToCatch()
&#xA0; &#xA0; {
&#xA0; &#xA0; &#xA0; &#xA0; try {
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $thing = somethingThrowingAnException();
&#xA0; &#xA0; &#xA0; &#xA0; } catch (\Exception $ex) {
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; // This is now catched
&#xA0; &#xA0; &#xA0; &#xA0; }
&#xA0; &#xA0; }

}


  

#

[Official documentation page](https://www.php.net/manual/en/language.namespaces.global.php)

**[To root](/README.md)**