# class_alias



class_alias() gives you the ability to do conditional imports.<br><br>Whereas the following will not work:<br><br>

```
<?php
namespace Component;

if (version_compare(PHP_VERSION, '5.4.0', 'gte')) {
    use My\ArrayObject;
} else {
    use ArrayObject;
}

class Container extends ArrayObject
{
}
?>
```


the following, using class_alias, will:



```
<?php
namespace Component;

if (version_compare(PHP_VERSION, '5.4.0', 'lt')) {
    class_alias('My\ArrayObject', 'Component\ArrayObject');
} else {
    class_alias('ArrayObject', 'Component\ArrayObject');
}

class Container extends ArrayObject
{
}
?>
```
<br><br>The semantics are slightly different (I&apos;m now indicating that Container extends from an ArrayObject implementation in the same namespace), but the overall idea is the same: conditional imports.  

---

If you defined the class &apos;original&apos; in a namespace, you will have to specify the namespace(s), too:<br>

```
<?php
namespace ns1\ns2\ns3;

class A {}

class_alias('ns1\ns2\ns3\A', 'B');
/* or if you want B to exist in ns1\ns2\ns3 */
class_alias('ns1\ns2\ns3\A', 'ns1\ns2\ns3\B');
?>
```
  

---

class_alias also works for interfaces!<br><br>

```
<?php
interface foo {}
class_alias('foo', 'bar');
echo interface_exists('bar') ? 'yes!' : 'no'; // prints yes!
?>
```
  

---

[Official documentation page](https://www.php.net/manual/en/function.class-alias.php)

**[To root](/README.md)**