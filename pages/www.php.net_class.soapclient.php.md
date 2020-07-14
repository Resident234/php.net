# The SoapClient class



When you need to connect to services requiring to send extra header use this method.<br><br>Here how we can to it with PHP and SoapClient<br><br>

```
<?php
class exampleChannelAdvisorAuth
{
    public $DeveloperKey;
    public $Password;

    public function __construct($key, $pass)
    {
        $this-&gt;DeveloperKey = $key;
        $this-&gt;Password = $pass;
    }
}

$devKey        = "";
$password    = "";
$accountId    = "";

// Create the SoapClient instance
$url         = "";
$client     = new SoapClient($url, array("trace" =&gt; 1, "exception" =&gt; 0));

// Create the header
$auth         = new ChannelAdvisorAuth($devKey, $password);
$header     = new SoapHeader("http://www.example.com/webservices/", "APICredentials", $auth, false);

// Call wsdl function
$result = $client-&gt;__soapCall("DeleteMarketplaceAd", array(
    "DeleteMarketplaceAd" =&gt; array(
        "accountID"        =&gt; $accountId,
        "marketplaceAdID"    =&gt; "9938745"        // The ads ID
    )
), NULL, $header);

// Echo the result
echo "&lt;pre&gt;".print_r($result, true)."&lt;/pre&gt;";
if($result-&gt;DeleteMarketplaceAdResult-&gt;Status == "Success")
{
    echo "Item deleted!";
}
?>
```
  

#

[Official documentation page](https://www.php.net/manual/en/class.soapclient.php)

**[To root](/README.md)**