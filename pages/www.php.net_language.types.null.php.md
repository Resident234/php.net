# NULL





Note: empty array is converted to null by non-strict equal &apos;==&apos; comparison. Use is_null() or &apos;===&apos; if there is possible of getting empty array.

$a = array();

$a == null&#xA0; &lt;== return true
$a === null &lt; == return false
is_null($a) &lt;== return false

  

#



NULL is supposed to indicate the absence of a value, rather than being thought of as a value itself. It&apos;s the empty slot, it&apos;s the missing information, it&apos;s the unanswered question. It&apos;s not a jumped-up zero or empty set.

This is why a variable containing a NULL is considered to be unset: it doesn&apos;t have a value. Setting a variable to NULL is telling it to forget its value without providing a replacement value to remember instead. The variable remains so that you can give it a proper value to remember later; this is especially important when the variable is an array element or object property.

It&apos;s a bit of semantic awkwardness to speak of a &quot;null value&quot;, but if a variable can exist without having a value, the language and implementation have to have something to represent that situation. Because someone will ask. If only to see if the slot has been filled.

  

#



Note: Non Strict Comparison &apos;==&apos; returns bool(true) for 

null == 0 &lt;-- returns true

Use Strict Comparison Instead

null === 0 &lt;-- returns false

  

#

[Official documentation page](https://www.php.net/manual/en/language.types.null.php)

**[To root](/README.md)**