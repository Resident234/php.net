# mb_strtolower



Note that mb_strtolower() is very SLOW, if you have a database connection, you may want to use it to convert your strings to lower case. Even latin1/9 (iso-8859-1/15) and other encodings are possible.<br><br>Have a look at my simple benchmark:<br><br>

```
<?php

$text = "L&#xF6;rem ip&#xDF;&#xFC;m d&#xF6;l&#xF6;r &#xDF;it &#xE4;met, c&#xF6;n&#xDF;ectet&#xFC;er &#xE4;dipi&#xDF;cing elit. Sed lig&#xFC;l&#xE4;. Pr&#xE4;e&#xDF;ent j&#xFC;&#xDF;t&#xF6; tell&#xFC;&#xDF;, gr&#xE4;vid&#xE4; e&#xFC;, temp&#xFC;&#xDF; &#xE4;, m&#xE4;tti&#xDF; n&#xF6;n, &#xF6;rci. N&#xE4;m q&#xFC;i&#xDF; l&#xF6;rem. N&#xE4;m &#xE4;liq&#xFC;et elit &#xDF;ed elit. Ph&#xE4;&#xDF;ell&#xFC;&#xDF; venen&#xE4;ti&#xDF; j&#xFC;&#xDF;t&#xF6; eget enim. D&#xF6;nec ni&#xDF;l. Pr&#xF6;in m&#xE4;tti&#xDF; venen&#xE4;ti&#xDF; j&#xFC;&#xDF;t&#xF6;. Sed &#xE4;liq&#xFC;&#xE4;m p&#xF6;rt&#xE4; &#xF6;rci. Cr&#xE4;&#xDF; elit ni&#xDF;l, c&#xF6;nv&#xE4;lli&#xDF; q&#xFC;i&#xDF;, tincid&#xFC;nt &#xE4;t, vehic&#xFC;l&#xE4; &#xE4;cc&#xFC;m&#xDF;&#xE4;n, &#xF6;di&#xF6;. Sed m&#xF6;le&#xDF;tie. Eti&#xE4;m m&#xF6;lli&#xDF; fe&#xFC;gi&#xE4;t elit. Ve&#xDF;tib&#xFC;l&#xFC;m &#xE4;nte ip&#xDF;&#xFC;m primi&#xDF; in f&#xE4;&#xFC;cib&#xFC;&#xDF; &#xF6;rci l&#xFC;ct&#xFC;&#xDF; et &#xFC;ltrice&#xDF; p&#xF6;&#xDF;&#xFC;ere c&#xFC;bili&#xE4; C&#xFC;r&#xE4;e; M&#xE4;ecen&#xE4;&#xDF; n&#xF6;n n&#xFC;ll&#xE4;.";

// mb_strtolower()
$timeMB = microtime(true);     
              
    for($i=0;$i&lt;30000;$i++) 
        $lower = mb_strtolower("$text/no-cache-$i");

$timeMB = microtime(true) - $timeMB;

// MySQL lower()
$timeSQL = microtime(true);    

    mysql_query("set names latin1");               
    for($i=0;$i&lt;30000;$i++) { 
        $r = mysql_fetch_row(mysql_query("select lower(&apos;$text/no-cache-$i&apos;)"));
        $lower = $r[0];
    }

$timeSQL = microtime(true) - $timeSQL;

echo "mb: ".sprintf("%.5f",$timeMB)." sek.&lt;br /&gt;";
echo "sql: ".sprintf("%.5f",$timeSQL)." sek.&lt;br /&gt;";

// Result on my notebook:
// mb: 11.50642 sek.
// sql: 5.44143 sek.

?>
```
  

#

Please, note that when using with UTF-8 mb_strtolower will only convert upper case characters to lower case which are marked with the Unicode property "Upper case letter" ("Lu"). However, there are also letters such as "Letter numbers" (Unicode property "Nl") that also have lower case and upper case variants. These characters will not be converted be mb_strtolower!<br><br>Example:<br>The Roman letters &#x2160;, &#x2161;, &#x2162;, ..., &#x216F; (UTF-8 code points 8544 through 8559) also exist in their respective lower case variants &#x2170;, &#x2171;, &#x2172;, ..., &#x217F; (UTF-8 code points 8560 through 8575) and should, in my opinion, also be converted by mb_strtolower, but they are not!<br><br>Big internet-companies (like Google) do match both variants as semantically equal (since the representations only differ in case).<br><br>Since I was not finding any proper solution in the internet on how to map all UTF8-strings to their lowercase counterpart in PHP, I offer the following hard-coded extended mb_strtolower function for UTF-8 strings:<br><br>The function wraps the existing function mb_strtolower() and additionally replaces uppercase UTF8-characters for which there is a lowercase representation. Since there is no proper Unicode uppercase and lowercase character-table in the internet that I was able to find, I checked the first million UTF8-characters against the Google-search and -KeywordTool and identified the following 78 characters as uppercase-characters, not being replaced by mb_strtolower, but having a UTF8 lowercase counterpart.<br><br>

```
<?php

//the numbers in the in-line-comments display the characters&apos; Unicode code-points (CP).
function strtolower_utf8_extended( $utf8_string )
{
    $additional_replacements    = array
        ( "&#x1C5;"    =&gt; "&#x1C6;"        //   453 -&gt;   454
        , "&#x1C8;"    =&gt; "&#x1C9;"        //   456 -&gt;   457
        , "&#x1CB;"    =&gt; "&#x1CC;"        //   459 -&gt;   460
        , "&#x1F2;"    =&gt; "&#x1F3;"        //   498 -&gt;   499
        , "&#x3F7;"    =&gt; "&#x3F8;"        //  1015 -&gt;  1016
        , "&#x3F9;"    =&gt; "&#x3F2;"        //  1017 -&gt;  1010
        , "&#x3FA;"    =&gt; "&#x3FB;"        //  1018 -&gt;  1019
        , "&#x1F88;"    =&gt; "&#x1F80;"        //  8072 -&gt;  8064
        , "&#x1F89;"    =&gt; "&#x1F81;"        //  8073 -&gt;  8065
        , "&#x1F8A;"    =&gt; "&#x1F82;"        //  8074 -&gt;  8066
        , "&#x1F8B;"    =&gt; "&#x1F83;"        //  8075 -&gt;  8067
        , "&#x1F8C;"    =&gt; "&#x1F84;"        //  8076 -&gt;  8068
        , "&#x1F8D;"    =&gt; "&#x1F85;"        //  8077 -&gt;  8069
        , "&#x1F8E;"    =&gt; "&#x1F86;"        //  8078 -&gt;  8070
        , "&#x1F8F;"    =&gt; "&#x1F87;"        //  8079 -&gt;  8071
        , "&#x1F98;"    =&gt; "&#x1F90;"        //  8088 -&gt;  8080
        , "&#x1F99;"    =&gt; "&#x1F91;"        //  8089 -&gt;  8081
        , "&#x1F9A;"    =&gt; "&#x1F92;"        //  8090 -&gt;  8082
        , "&#x1F9B;"    =&gt; "&#x1F93;"        //  8091 -&gt;  8083
        , "&#x1F9C;"    =&gt; "&#x1F94;"        //  8092 -&gt;  8084
        , "&#x1F9D;"    =&gt; "&#x1F95;"        //  8093 -&gt;  8085
        , "&#x1F9E;"    =&gt; "&#x1F96;"        //  8094 -&gt;  8086
        , "&#x1F9F;"    =&gt; "&#x1F97;"        //  8095 -&gt;  8087
        , "&#x1FA8;"    =&gt; "&#x1FA0;"        //  8104 -&gt;  8096
        , "&#x1FA9;"    =&gt; "&#x1FA1;"        //  8105 -&gt;  8097
        , "&#x1FAA;"    =&gt; "&#x1FA2;"        //  8106 -&gt;  8098
        , "&#x1FAB;"    =&gt; "&#x1FA3;"        //  8107 -&gt;  8099
        , "&#x1FAC;"    =&gt; "&#x1FA4;"        //  8108 -&gt;  8100
        , "&#x1FAD;"    =&gt; "&#x1FA5;"        //  8109 -&gt;  8101
        , "&#x1FAE;"    =&gt; "&#x1FA6;"        //  8110 -&gt;  8102
        , "&#x1FAF;"    =&gt; "&#x1FA7;"        //  8111 -&gt;  8103
        , "&#x1FBC;"    =&gt; "&#x1FB3;"        //  8124 -&gt;  8115
        , "&#x1FCC;"    =&gt; "&#x1FC3;"        //  8140 -&gt;  8131
        , "&#x1FFC;"    =&gt; "&#x1FF3;"        //  8188 -&gt;  8179
        , "&#x2160;"    =&gt; "&#x2170;"        //  8544 -&gt;  8560
        , "&#x2161;"    =&gt; "&#x2171;"        //  8545 -&gt;  8561
        , "&#x2162;"    =&gt; "&#x2172;"        //  8546 -&gt;  8562
        , "&#x2163;"    =&gt; "&#x2173;"        //  8547 -&gt;  8563
        , "&#x2164;"    =&gt; "&#x2174;"        //  8548 -&gt;  8564
        , "&#x2165;"    =&gt; "&#x2175;"        //  8549 -&gt;  8565
        , "&#x2166;"    =&gt; "&#x2176;"        //  8550 -&gt;  8566
        , "&#x2167;"    =&gt; "&#x2177;"        //  8551 -&gt;  8567
        , "&#x2168;"    =&gt; "&#x2178;"        //  8552 -&gt;  8568
        , "&#x2169;"    =&gt; "&#x2179;"        //  8553 -&gt;  8569
        , "&#x216A;"    =&gt; "&#x217A;"        //  8554 -&gt;  8570
        , "&#x216B;"    =&gt; "&#x217B;"        //  8555 -&gt;  8571
        , "&#x216C;"    =&gt; "&#x217C;"        //  8556 -&gt;  8572
        , "&#x216D;"    =&gt; "&#x217D;"        //  8557 -&gt;  8573
        , "&#x216E;"    =&gt; "&#x217E;"        //  8558 -&gt;  8574
        , "&#x216F;"    =&gt; "&#x217F;"        //  8559 -&gt;  8575
        , "&#x24B6;"    =&gt; "&#x24D0;"        //  9398 -&gt;  9424
        , "&#x24B7;"    =&gt; "&#x24D1;"        //  9399 -&gt;  9425
        , "&#x24B8;"    =&gt; "&#x24D2;"        //  9400 -&gt;  9426
        , "&#x24B9;"    =&gt; "&#x24D3;"        //  9401 -&gt;  9427
        , "&#x24BA;"    =&gt; "&#x24D4;"        //  9402 -&gt;  9428
        , "&#x24BB;"    =&gt; "&#x24D5;"        //  9403 -&gt;  9429
        , "&#x24BC;"    =&gt; "&#x24D6;"        //  9404 -&gt;  9430
        , "&#x24BD;"    =&gt; "&#x24D7;"        //  9405 -&gt;  9431
        , "&#x24BE;"    =&gt; "&#x24D8;"        //  9406 -&gt;  9432
        , "&#x24BF;"    =&gt; "&#x24D9;"        //  9407 -&gt;  9433
        , "&#x24C0;"    =&gt; "&#x24DA;"        //  9408 -&gt;  9434
        , "&#x24C1;"    =&gt; "&#x24DB;"        //  9409 -&gt;  9435
        , "&#x24C2;"    =&gt; "&#x24DC;"        //  9410 -&gt;  9436
        , "&#x24C3;"    =&gt; "&#x24DD;"        //  9411 -&gt;  9437
        , "&#x24C4;"    =&gt; "&#x24DE;"        //  9412 -&gt;  9438
        , "&#x24C5;"    =&gt; "&#x24DF;"        //  9413 -&gt;  9439
        , "&#x24C6;"    =&gt; "&#x24E0;"        //  9414 -&gt;  9440
        , "&#x24C7;"    =&gt; "&#x24E1;"        //  9415 -&gt;  9441
        , "&#x24C8;"    =&gt; "&#x24E2;"        //  9416 -&gt;  9442
        , "&#x24C9;"    =&gt; "&#x24E3;"        //  9417 -&gt;  9443
        , "&#x24CA;"    =&gt; "&#x24E4;"        //  9418 -&gt;  9444
        , "&#x24CB;"    =&gt; "&#x24E5;"        //  9419 -&gt;  9445
        , "&#x24CC;"    =&gt; "&#x24E6;"        //  9420 -&gt;  9446
        , "&#x24CD;"    =&gt; "&#x24E7;"        //  9421 -&gt;  9447
        , "&#x24CE;"    =&gt; "&#x24E8;"        //  9422 -&gt;  9448
        , "&#x24CF;"    =&gt; "&#x24E9;"        //  9423 -&gt;  9449
        , "&#x10426;"    =&gt; "&#x1044E;"        // 66598 -&gt; 66638
        , "&#x10427;"    =&gt; "&#x1044F;"        // 66599 -&gt; 66639
        );
    
    $utf8_string    = mb_strtolower( $utf8_string, "UTF-8");
    
    $utf8_string    = strtr( $utf8_string, $additional_replacements );
    
    return $utf8_string;
} //strtolower_utf8_extended()

?>
```
  

#

[Official documentation page](https://www.php.net/manual/en/function.mb-strtolower.php)

**[To root](/README.md)**