# mb_strtolower



Note that mb_strtolower() is very SLOW, if you have a database connection, you may want to use it to convert your strings to lower case. Even latin1/9 (iso-8859-1/15) and other encodings are possible.<br><br>Have a look at my simple benchmark:<br><br>

```
<?php

$text = "L&#xF6;rem ip&#xDF;&#xFC;m d&#xF6;l&#xF6;r &#xDF;it &#xE4;met, c&#xF6;n&#xDF;ectet&#xFC;er &#xE4;dipi&#xDF;cing elit. Sed lig&#xFC;l&#xE4;. Pr&#xE4;e&#xDF;ent j&#xFC;&#xDF;t&#xF6; tell&#xFC;&#xDF;, gr&#xE4;vid&#xE4; e&#xFC;, temp&#xFC;&#xDF; &#xE4;, m&#xE4;tti&#xDF; n&#xF6;n, &#xF6;rci. N&#xE4;m q&#xFC;i&#xDF; l&#xF6;rem. N&#xE4;m &#xE4;liq&#xFC;et elit &#xDF;ed elit. Ph&#xE4;&#xDF;ell&#xFC;&#xDF; venen&#xE4;ti&#xDF; j&#xFC;&#xDF;t&#xF6; eget enim. D&#xF6;nec ni&#xDF;l. Pr&#xF6;in m&#xE4;tti&#xDF; venen&#xE4;ti&#xDF; j&#xFC;&#xDF;t&#xF6;. Sed &#xE4;liq&#xFC;&#xE4;m p&#xF6;rt&#xE4; &#xF6;rci. Cr&#xE4;&#xDF; elit ni&#xDF;l, c&#xF6;nv&#xE4;lli&#xDF; q&#xFC;i&#xDF;, tincid&#xFC;nt &#xE4;t, vehic&#xFC;l&#xE4; &#xE4;cc&#xFC;m&#xDF;&#xE4;n, &#xF6;di&#xF6;. Sed m&#xF6;le&#xDF;tie. Eti&#xE4;m m&#xF6;lli&#xDF; fe&#xFC;gi&#xE4;t elit. Ve&#xDF;tib&#xFC;l&#xFC;m &#xE4;nte ip&#xDF;&#xFC;m primi&#xDF; in f&#xE4;&#xFC;cib&#xFC;&#xDF; &#xF6;rci l&#xFC;ct&#xFC;&#xDF; et &#xFC;ltrice&#xDF; p&#xF6;&#xDF;&#xFC;ere c&#xFC;bili&#xE4; C&#xFC;r&#xE4;e; M&#xE4;ecen&#xE4;&#xDF; n&#xF6;n n&#xFC;ll&#xE4;.";

// mb_strtolower()
$timeMB = microtime(true);     
              
    for($i=0;$i<30000;$i++) 
        $lower = mb_strtolower("$text/no-cache-$i");

$timeMB = microtime(true) - $timeMB;

// MySQL lower()
$timeSQL = microtime(true);    

    mysql_query("set names latin1");               
    for($i=0;$i<30000;$i++) { 
        $r = mysql_fetch_row(mysql_query("select lower('$text/no-cache-$i')"));
        $lower = $r[0];
    }

$timeSQL = microtime(true) - $timeSQL;

echo "mb: ".sprintf("%.5f",$timeMB)." sek.<br />";
echo "sql: ".sprintf("%.5f",$timeSQL)." sek.<br />";

// Result on my notebook:
// mb: 11.50642 sek.
// sql: 5.44143 sek.

?>
```
  

#

Please, note that when using with UTF-8 mb_strtolower will only convert upper case characters to lower case which are marked with the Unicode property "Upper case letter" ("Lu"). However, there are also letters such as "Letter numbers" (Unicode property "Nl") that also have lower case and upper case variants. These characters will not be converted be mb_strtolower!<br><br>Example:<br>The Roman letters &#x2160;, &#x2161;, &#x2162;, ..., &#x216F; (UTF-8 code points 8544 through 8559) also exist in their respective lower case variants &#x2170;, &#x2171;, &#x2172;, ..., &#x217F; (UTF-8 code points 8560 through 8575) and should, in my opinion, also be converted by mb_strtolower, but they are not!<br><br>Big internet-companies (like Google) do match both variants as semantically equal (since the representations only differ in case).<br><br>Since I was not finding any proper solution in the internet on how to map all UTF8-strings to their lowercase counterpart in PHP, I offer the following hard-coded extended mb_strtolower function for UTF-8 strings:<br><br>The function wraps the existing function mb_strtolower() and additionally replaces uppercase UTF8-characters for which there is a lowercase representation. Since there is no proper Unicode uppercase and lowercase character-table in the internet that I was able to find, I checked the first million UTF8-characters against the Google-search and -KeywordTool and identified the following 78 characters as uppercase-characters, not being replaced by mb_strtolower, but having a UTF8 lowercase counterpart.<br><br>

```
<?php

//the numbers in the in-line-comments display the characters' Unicode code-points (CP).
function strtolower_utf8_extended( $utf8_string )
{
    $additional_replacements    = array
        ( "&#x1C5;"    => "&#x1C6;"        //   453 ->   454
        , "&#x1C8;"    => "&#x1C9;"        //   456 ->   457
        , "&#x1CB;"    => "&#x1CC;"        //   459 ->   460
        , "&#x1F2;"    => "&#x1F3;"        //   498 ->   499
        , "&#x3F7;"    => "&#x3F8;"        //  1015 ->  1016
        , "&#x3F9;"    => "&#x3F2;"        //  1017 ->  1010
        , "&#x3FA;"    => "&#x3FB;"        //  1018 ->  1019
        , "&#x1F88;"    => "&#x1F80;"        //  8072 ->  8064
        , "&#x1F89;"    => "&#x1F81;"        //  8073 ->  8065
        , "&#x1F8A;"    => "&#x1F82;"        //  8074 ->  8066
        , "&#x1F8B;"    => "&#x1F83;"        //  8075 ->  8067
        , "&#x1F8C;"    => "&#x1F84;"        //  8076 ->  8068
        , "&#x1F8D;"    => "&#x1F85;"        //  8077 ->  8069
        , "&#x1F8E;"    => "&#x1F86;"        //  8078 ->  8070
        , "&#x1F8F;"    => "&#x1F87;"        //  8079 ->  8071
        , "&#x1F98;"    => "&#x1F90;"        //  8088 ->  8080
        , "&#x1F99;"    => "&#x1F91;"        //  8089 ->  8081
        , "&#x1F9A;"    => "&#x1F92;"        //  8090 ->  8082
        , "&#x1F9B;"    => "&#x1F93;"        //  8091 ->  8083
        , "&#x1F9C;"    => "&#x1F94;"        //  8092 ->  8084
        , "&#x1F9D;"    => "&#x1F95;"        //  8093 ->  8085
        , "&#x1F9E;"    => "&#x1F96;"        //  8094 ->  8086
        , "&#x1F9F;"    => "&#x1F97;"        //  8095 ->  8087
        , "&#x1FA8;"    => "&#x1FA0;"        //  8104 ->  8096
        , "&#x1FA9;"    => "&#x1FA1;"        //  8105 ->  8097
        , "&#x1FAA;"    => "&#x1FA2;"        //  8106 ->  8098
        , "&#x1FAB;"    => "&#x1FA3;"        //  8107 ->  8099
        , "&#x1FAC;"    => "&#x1FA4;"        //  8108 ->  8100
        , "&#x1FAD;"    => "&#x1FA5;"        //  8109 ->  8101
        , "&#x1FAE;"    => "&#x1FA6;"        //  8110 ->  8102
        , "&#x1FAF;"    => "&#x1FA7;"        //  8111 ->  8103
        , "&#x1FBC;"    => "&#x1FB3;"        //  8124 ->  8115
        , "&#x1FCC;"    => "&#x1FC3;"        //  8140 ->  8131
        , "&#x1FFC;"    => "&#x1FF3;"        //  8188 ->  8179
        , "&#x2160;"    => "&#x2170;"        //  8544 ->  8560
        , "&#x2161;"    => "&#x2171;"        //  8545 ->  8561
        , "&#x2162;"    => "&#x2172;"        //  8546 ->  8562
        , "&#x2163;"    => "&#x2173;"        //  8547 ->  8563
        , "&#x2164;"    => "&#x2174;"        //  8548 ->  8564
        , "&#x2165;"    => "&#x2175;"        //  8549 ->  8565
        , "&#x2166;"    => "&#x2176;"        //  8550 ->  8566
        , "&#x2167;"    => "&#x2177;"        //  8551 ->  8567
        , "&#x2168;"    => "&#x2178;"        //  8552 ->  8568
        , "&#x2169;"    => "&#x2179;"        //  8553 ->  8569
        , "&#x216A;"    => "&#x217A;"        //  8554 ->  8570
        , "&#x216B;"    => "&#x217B;"        //  8555 ->  8571
        , "&#x216C;"    => "&#x217C;"        //  8556 ->  8572
        , "&#x216D;"    => "&#x217D;"        //  8557 ->  8573
        , "&#x216E;"    => "&#x217E;"        //  8558 ->  8574
        , "&#x216F;"    => "&#x217F;"        //  8559 ->  8575
        , "&#x24B6;"    => "&#x24D0;"        //  9398 ->  9424
        , "&#x24B7;"    => "&#x24D1;"        //  9399 ->  9425
        , "&#x24B8;"    => "&#x24D2;"        //  9400 ->  9426
        , "&#x24B9;"    => "&#x24D3;"        //  9401 ->  9427
        , "&#x24BA;"    => "&#x24D4;"        //  9402 ->  9428
        , "&#x24BB;"    => "&#x24D5;"        //  9403 ->  9429
        , "&#x24BC;"    => "&#x24D6;"        //  9404 ->  9430
        , "&#x24BD;"    => "&#x24D7;"        //  9405 ->  9431
        , "&#x24BE;"    => "&#x24D8;"        //  9406 ->  9432
        , "&#x24BF;"    => "&#x24D9;"        //  9407 ->  9433
        , "&#x24C0;"    => "&#x24DA;"        //  9408 ->  9434
        , "&#x24C1;"    => "&#x24DB;"        //  9409 ->  9435
        , "&#x24C2;"    => "&#x24DC;"        //  9410 ->  9436
        , "&#x24C3;"    => "&#x24DD;"        //  9411 ->  9437
        , "&#x24C4;"    => "&#x24DE;"        //  9412 ->  9438
        , "&#x24C5;"    => "&#x24DF;"        //  9413 ->  9439
        , "&#x24C6;"    => "&#x24E0;"        //  9414 ->  9440
        , "&#x24C7;"    => "&#x24E1;"        //  9415 ->  9441
        , "&#x24C8;"    => "&#x24E2;"        //  9416 ->  9442
        , "&#x24C9;"    => "&#x24E3;"        //  9417 ->  9443
        , "&#x24CA;"    => "&#x24E4;"        //  9418 ->  9444
        , "&#x24CB;"    => "&#x24E5;"        //  9419 ->  9445
        , "&#x24CC;"    => "&#x24E6;"        //  9420 ->  9446
        , "&#x24CD;"    => "&#x24E7;"        //  9421 ->  9447
        , "&#x24CE;"    => "&#x24E8;"        //  9422 ->  9448
        , "&#x24CF;"    => "&#x24E9;"        //  9423 ->  9449
        , "&#x10426;"    => "&#x1044E;"        // 66598 -> 66638
        , "&#x10427;"    => "&#x1044F;"        // 66599 -> 66639
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