# SoapServer::SoapServer



Here&apos;s my solution to make SOAP-headers based authentication.<br><br>1). First of all we define the decorator class for our service class:<br><br>

```
<?php

class SOAP_Service_Secure
{
    protected $class_name    = &apos;&apos;;
    protected $authenticated = false;

    // -----

    public function __construct($class_name)
    {
        $this-&gt;class_name = $class_name;

    }

    public function AuthHeader($Header)
    {
        if($Header-&gt;username == &apos;foo&apos; &amp;&amp; $Header-&gt;password == &apos;bar&apos;)
            $this-&gt;authenticated = true;

    }

    public function __call($method_name, $arguments)
    {
        if(!method_exists($this-&gt;class_name, $method_name))
            throw new Exception(&apos;method not found&apos;);

        $this-&gt;checkAuth();

        return call_user_func_array(array($this-&gt;class_name, $method_name), $arguments);

    }

    // -----

    protected function checkAuth()
    {
        if(!$this-&gt;authenticated)
            HTML_Output::error(403);

    }

}

?>
```


2). Then we pass an instance of it to the SoapServer object.



```
<?php

    $Service = new SOAP_Service_Secure(&apos;My_Service&apos;);

    $Server = new SoapServer(&apos;my-service.wsdl&apos;);

    $Server-&gt;setObject($Service);

    $Server-&gt;handle();

?>
```


3). Implementing a client:



```
<?php

class AuthHeader
{
    public $username;
    public $password;    
    
}

// -----

$Client = new SoapClient(&apos;http://example.com/my-service.wsdl&apos;, array(
    &apos;exceptions&apos; =&gt; true
));

// -----

$AuthHeader = new AuthHeader();

$AuthHeader-&gt;username = &apos;foo&apos;;
$AuthHeader-&gt;password = &apos;bar&apos;;

$Headers[] = new SoapHeader(&apos;http://example.com&apos;, &apos;AuthHeader&apos;, $AuthHeader);

$Client-&gt;__setSoapHeaders($Headers);

// -----

$Result = $Client-&gt;someMethod();

?>
```
<br><br>As you can see SoapServer uses our decorator class to answer client requests. If request contains "AuthHeader" SOAP-header then our wrapper checks the credentials and sets $authenticated parameter to TRUE (on success). Then SoapServer calls requested method on our wrapper, we are intercepting it with __call(), checking authentication and if everything is allright we are calling real method on the "My_Service" class.  

#

[Official documentation page](https://www.php.net/manual/en/soapserver.soapserver.php)

**[To root](/README.md)**