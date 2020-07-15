# Global space



Included files will default to the global namespace.<br>

```
<?php
//test.php
namespace test {
  include 'test1.inc';
  echo '-',__NAMESPACE__,'-<br />';
}
?>
```




```
<?php
//test1.inc
  echo '-',__NAMESPACE__,'-<br />';
?>
```
<br><br>Results of test.php:<br><br>--<br>-test-  

#

In namespaced context the Exception class needs to be prefixed with global prefix operator.<br><br>

```
<?php

namespace hey\ho\lets\go;

class MyClass
{
    public function failToCatch()
    {
        try {
            $thing = somethingThrowingAnException();
        } catch (Exception $ex) {
              // Not catched
        }
    }

    public function succeedToCatch()
    {
        try {
            $thing = somethingThrowingAnException();
        } catch (\Exception $ex) {
              // This is now catched
        }
    }

}?>
```
  

#

[Official documentation page](https://www.php.net/manual/en/language.namespaces.global.php)

**[To root](/README.md)**