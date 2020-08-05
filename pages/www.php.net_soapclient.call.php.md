# SoapClient::__call



If you are using a WSDL, the library will strip out anything from your parameters that is not defined in WSDL, without giving you any notice about this.<br><br>So if your parameters are not fully matching the WSDL, it will simply send no parameters at all.<br>This can be a bit hard to debug if you don&apos;t have access to the target server.<br><br>__soapCall() expects parameters in an array called &apos;parameters&apos; as opposed to calling the function via it&apos;s WSDL name, where it accepts the parameters as a plain array.<br><br>I.e. if a function called sendOrder expects a parameter (array) called orderDetails, you can call it like this:<br><br>$orderDetails = array(/* your data */);<br>$soap-&gt;sendOrder(array(&apos;orderDetails&apos; =&gt; $orderDetails));<br><br>Which is equivalent to:<br><br>$client-&gt;__soapCall(&apos;sendOrder&apos;, array(&apos;parameters&apos; =&gt; array(&apos;orderDetails&apos; =&gt; $orderDetails)));<br><br>Note the additional &apos;parameters&apos; key used in __soapCall().  

---

[Official documentation page](https://www.php.net/manual/en/soapclient.call.php)

**[To root](/README.md)**