# SoapClient::__getTypes





```
<?php<br>// to see formated types<br><br>$soap = new SoapClient(&apos;http://domain.com/ws.php?WSDL&apos;);<br><br>echo &apos;&lt;pre&gt;&apos;;<br>echo &apos;&lt;h2&gt;Types:&lt;/h2&gt;&apos;;<br>$types = $soap-&gt;__getTypes();<br>foreach ($types as $type) {<br>    $type = preg_replace(<br>        array(&apos;/(\w+) ([a-zA-Z0-9]+)/&apos;, &apos;/\n /&apos;),<br>        array(&apos;&lt;font color="green"&gt;${1}&lt;/font&gt; &lt;font color="blue"&gt;${2}&lt;/font&gt;&apos;, "\n\t"),<br>        $type<br>    );<br>    echo $type;<br>    echo "\n\n";<br>}<br>echo &apos;&lt;/pre&gt;&apos;;  

#

[Official documentation page](https://www.php.net/manual/en/soapclient.gettypes.php)

**[To root](/README.md)**