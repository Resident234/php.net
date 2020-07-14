# SoapVar::SoapVar



I spent hours trying to find how to send a request where an element is repeated. Here is how I managed to do it:<br>

```
<?php
$parm = array();
$parm[] = new SoapVar(&apos;123&apos;, XSD_STRING, null, null, &apos;customerNo&apos; );
$parm[] = new SoapVar(&apos;THIS&apos;, XSD_STRING, null, null, &apos;selection&apos; );
$parm[] = new SoapVar(&apos;THAT&apos;, XSD_STRING, null, null, &apos;selection&apos; );
$resp = $client-&gt;getStuff( new SoapVar($parm, SOAP_ENC_OBJECT) );
?>
```
<br><br>This will send something like:<br>&lt;getStuff&gt;<br>  &lt;customerNo&gt;123&lt;/customerNo&gt;<br>  &lt;selection&gt;THIS&lt;/selection&gt;<br>  &lt;selection&gt;THAT&lt;/selection&gt;<br>&lt;/getStuff&gt;<br><br>Hope this will save someone else&apos;s time.  

#

I worked for hours to figure out how to create following structure:<br><br>&lt;ns:Foo attr=&apos;bar&apos;&gt;<br>   &lt;ns:Baz attr=&apos;foobar&apos;&gt;barbaz&lt;/ns:Baz&gt;<br>&lt;/ns:Foo&gt;<br><br>It can be done with following array structure:<br><br>$arr = array(<br>  &apos;Foo&apos; =&gt; array(<br>     &apos;attr&apos;=&gt;&apos;bar&apos;,<br>     &apos;Baz&apos;=&gt;array( &apos;_&apos; =&gt; &apos;barbaz&apos;, &apos;attr&apos;=&gt;&apos;foobar&apos;)<br>  )<br>);  

#

[Official documentation page](https://www.php.net/manual/en/soapvar.soapvar.php)

**[To root](/README.md)**