# SoapServer::SoapServer



Here&apos;s my solution to make SOAP-headers based authentication.<br><br>1). First of all we define the decorator class for our service class:<br><br>

```
<?php

class SOAP_Service_Secure
{
    protected $class_name    = '';
    protected $authenticated = false;

    // -----

    public function __construct($class_name)
    {
        $this->class_name = $class_name;

    }

    public function AuthHeader($Header)
    {
        if($Header->username == 'foo' &amp;&amp; $Header->password == 'bar')
            $this->authenticated = true;

    }

    public function __call($method_name, $arguments)
    {
        if(!method_exists($this->class_name, $method_name))
            throw new Exception('method not found');

        $this->checkAuth();

        return call_user_func_array(array($this->class_name, $method_name), $arguments);

    }

    // -----

    protected function checkAuth()
    {
        if(!$this->authenticated)
            HTML_Output::error(403);

    }

}

?>
```


2). Then we pass an instance of it to the SoapServer object.



```
<?php

    $Service = new SOAP_Service_Secure('My_Service');

    $Server = new SoapServer('my-service.wsdl');

    $Server->setObject($Service);

    $Server->handle();

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

$Client = new SoapClient('http://example.com/my-service.wsdl', array(
    'exceptions' => true
));

// -----

$AuthHeader = new AuthHeader();

$AuthHeader->username = 'foo';
$AuthHeader->password = 'bar';

$Headers[] = new SoapHeader('http://example.com', 'AuthHeader', $AuthHeader);

$Client->__setSoapHeaders($Headers);

// -----

$Result = $Client->someMethod();

?>
```
<br><br>As you can see SoapServer uses our decorator class to answer client requests. If request contains "AuthHeader" SOAP-header then our wrapper checks the credentials and sets $authenticated parameter to TRUE (on success). Then SoapServer calls requested method on our wrapper, we are intercepting it with __call(), checking authentication and if everything is allright we are calling real method on the "My_Service" class.  

#

[Official documentation page](https://www.php.net/manual/en/soapserver.soapserver.php)

**[To root](/README.md)**