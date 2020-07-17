# OAuthProvider::checkOAuthRequest



This function checks if OAuth request is valid and signed correctly.<br><br>$provider-&gt;checkOAuthRequest(); <br><br>It does this by first calling timestampNonceHandler and expects result OAUHT_OK from it. If the result is different, an exception is thrown. It&apos;s up to you to write the code that checks timestamp/nonce combinations.<br><br>Secondly, it calls consumerHandler and expects your code in the consumerHandler function to set $provider-&gt;consumer_secret to the correct value (you should take it from your consumer storage location where it&apos;s saved with consumer key). If $provider-&gt;consumer_secret is not set, or is not set with the proper value an exception is thrown. Proper value means that it should be the same consumer key that was used to sign the request by the consumer before sending it to here (the provider). Again expected result from this function is OAUTH_OK or some OAUTH error code if you want to throw exception.<br><br>Third, it calls tokenHandler, but only WHEN you are requesting ACCESS token or requesting protected data with authorized ACCESS TOKEN. In order for the provider to call tokenHandler, before a call to the checkOAuthRequest function is made, the provider should call the method that says that this is not a request token endpoint (this is access token endpoint):<br><br>$provider-&gt;isRequestTokenEndpoint (false);<br>$provider-&gt;checkOAuthRequest();<br><br>Again here OAuthProvider is expecting your code in the tokenHandler to set $provider-&gt;token_secret to the correct value (you should take it from your token storage place) because during the signing process it uses CONSUMER SECRET (for request token) and CONSUMER SECRET AND TOKEN SECRET (for access token and fetch of protected data) to sign the request.<br><br>After these 3 handler functions are called and return good results (OAUTH_OK) and set the values of the required fields $provider-&gt;consumer_secret and $provider-&gt;token_secret, then the checkOAuthRequest function signs the request. If something goes wrong, it throws exception, otherwise there comes the place for your code to proceed and handle the request:<br><br>- you can create request token (if it&apos;s a first request for request token)<br>- you can create access token (if it&apos;s a request for access token)<br>- you can return protected data to the consumer (if it&apos;s a request to fetch protected data)<br><br>This is how the functions in my code look like, however please have in mind that I&apos;ve just implemented it and it&apos;s possible that I have something missed or forgotten, but generally I think the idea should be clear:<br><br>$this-&gt;dbModel is the object for working with database and save/retrieve token and consumer data<br><br>

```
<?php

public function timestampNonceHandler ( $provider )
{
    return $this->dbModel->checkTimestampNonce ( $provider->consumer_key,
                                                 $provider->token, 
                                                 $provider->timestamp,
                                                 $provider->nonce );
}

public function consumerHandler ( $provider )
{
    $consumer = $this->dbModel->getConsumerSecrets ($provider->consumer_key);
    
    if($consumer['consumer_key'] != $provider->consumer_key)
    {
        return OAUTH_CONSUMER_KEY_UNKNOWN;
    }
    
    if( (int)$consumer['disabled'] != 0 )
    {
        return OAUTH_CONSUMER_KEY_REFUSED;
    }
    
    $provider->consumer_id = $consumer['consumer_id']; # this is not required by OAuthProvider but I use it later in tokenHandler
    $provider->consumer_secret = $consumer['consumer_secret']; # this is REQUIRED

    return OAUTH_OK;
}

public function tokenHandler ( $provider )
{
    $token = $this->dbModel->getToken( $provider->token );

    if( time() > $token['expire'] )
    {
        return OAUTH_TOKEN_EXPIRED;
    }
    
    if($token['consumer_id'] != $provider->consumer_id)
    {
        return OAUTH_TOKEN_REJECTED;
    }

    if( (int)$token['authorized'] == 0 )
    {
        return OAUTH_TOKEN_REJECTED;
    }

    if($token['token_type'] != 'access')
    {
        if($token['verifier'] != $provider->verifier)
            return OAUTH_VERIFIER_INVALID;
    }

    $provider->token_id = $token['token_id']; # not required to be set by OAuthProvider
    $provider->token_secret = $token['token_secret']; # this is REQUIRED
    
    return OAUTH_OK;
}

?>
```
  

#

[Official documentation page](https://www.php.net/manual/en/oauthprovider.checkoauthrequest.php)

**[To root](/README.md)**