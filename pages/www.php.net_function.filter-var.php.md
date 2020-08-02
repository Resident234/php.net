# filter_var



Pay attention that the function will not validate "not latin" domains.<br><br>if (filter_var(&apos;&#x443;&#x43D;&#x438;&#x43A;&#x443;&#x43C;@&#x438;&#x437;.&#x440;&#x444;&apos;, FILTER_VALIDATE_EMAIL)) { <br>    echo &apos;VALID&apos;; <br>} else {<br>    echo &apos;NOT VALID&apos;;<br>}  

---

I found some addresses that FILTER_VALIDATE_EMAIL rejects, but RFC5321 permits:<br>

```
<?php
foreach (array(
        'localpart.ending.with.dot.@example.com',
        '(comment)localpart@example.com',
        '"this is v@lid!"@example.com', 
        '"much.more unusual"@example.com',
        'postbox@com',
        'admin@mailserver1',
        '"()<>[]:,;@\\"\\\\!#$%&amp;\'*+-/=?^_`{}| ~.a"@example.org',
        '" "@example.org',
    ) as $address) {
    echo "<p>$address is <b>".(filter_var($address, FILTER_VALIDATE_EMAIL) ? '' : 'not')." valid</b></p>";
}
?>
```
<br>Results:<br><br>localpart.ending.with.dot.@example.com is not valid<br>(comment)localpart@example.com is not valid<br>"this is v@lid!"@example.com is not valid<br>"much.more unusual"@example.com is not valid<br>postbox@com is not valid<br>admin@mailserver1 is not valid<br>"()&lt;&gt;[]:,;@\"\\!#$%&amp;&apos;*+-/=?^_`{}| ~.a"@example.org is not valid<br>" "@example.org is not valid<br><br>The documentation does not saying that FILTER_VALIDATE_EMAIL should pass the RFC5321, however you can meet with these examples (especially with the first one). So this is a note, not a bug report.  

---

And this is also a valid url <br><br>http://example.com/"&gt;&lt;script&gt;alert(document.cookie)&lt;/script&gt;  

---

note that FILTER_VALIDATE_BOOLEAN tries to be smart, recognizing words like Yes, No, Off, On, both string and native types of true and false, and is not case-sensitive when validating strings.<br><br>

```
<?php
$vals=array('on','On','ON','off','Off','OFF','yes','Yes','YES',
'no','No','NO',0,1,'0','1','true',
'True','TRUE','false','False','FALSE',true,false,'foo','bar');
foreach($vals as $val){
    echo var_export($val,true).': ';   var_dump(filter_var($val,FILTER_VALIDATE_BOOLEAN,FILTER_NULL_ON_FAILURE));
}
?>
```
<br><br>outputs:<br>&apos;on&apos;: bool(true)<br>&apos;On&apos;: bool(true)<br>&apos;ON&apos;: bool(true)<br>&apos;off&apos;: bool(false)<br>&apos;Off&apos;: bool(false)<br>&apos;OFF&apos;: bool(false)<br>&apos;yes&apos;: bool(true)<br>&apos;Yes&apos;: bool(true)<br>&apos;YES&apos;: bool(true)<br>&apos;no&apos;: bool(false)<br>&apos;No&apos;: bool(false)<br>&apos;NO&apos;: bool(false)<br>0: bool(false)<br>1: bool(true)<br>&apos;0&apos;: bool(false)<br>&apos;1&apos;: bool(true)<br>&apos;true&apos;: bool(true)<br>&apos;True&apos;: bool(true)<br>&apos;TRUE&apos;: bool(true)<br>&apos;false&apos;: bool(false)<br>&apos;False&apos;: bool(false)<br>&apos;FALSE&apos;: bool(false)<br>true: bool(true)<br>false: bool(false)<br>&apos;foo&apos;: NULL<br>&apos;bar&apos;: NULL  

---

FILTER_VALIDATE_URL allows:<br><br>filter_var(&apos;javascript://comment%0Aalert(1)&apos;, FILTER_VALIDATE_URL);<br><br>Where the %0A (URL encoded newline), in certain contexts, will split the comment from the JS code.<br><br>This can result in an XSS vulnerability.  

---

please note FILTER_VALIDATE_URL passes following url<br><br>http://example.ee/sdsf"f  

---

[Official documentation page](https://www.php.net/manual/en/function.filter-var.php)

**[To root](/README.md)**