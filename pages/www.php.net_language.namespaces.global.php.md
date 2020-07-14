# Global space



Included files will default to the global namespace.<br>

```
<?php
//test.php
namespace test {
  include &apos;test1.inc&apos;;
  echo &apos;-&apos;,__NAMESPACE__,&apos;-&lt;br /&gt;&apos;;
}
?>
```




```
<?php
//test1.inc
  echo &apos;-&apos;,__NAMESPACE__,&apos;-&lt;br /&gt;&apos;;
?>
```
<br><br>Results of test.php:<br><br>--<br>-test-  

#

In namespaced context the Exception class needs to be prefixed with global prefix operator.<br><br>

```
<?php<br><br>namespace hey\ho\lets\go;<br><br>class MyClass<br>{<br>    public function failToCatch()<br>    {<br>        try {<br>            $thing = somethingThrowingAnException();<br>        } catch (Exception $ex) {<br>              // Not catched<br>        }<br>    }<br><br>    public function succeedToCatch()<br>    {<br>        try {<br>            $thing = somethingThrowingAnException();<br>        } catch (\Exception $ex) {<br>              // This is now catched<br>        }<br>    }<br><br>}  

#

[Official documentation page](https://www.php.net/manual/en/language.namespaces.global.php)

**[To root](/README.md)**