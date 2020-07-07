# mb_strtolower





Note that mb_strtolower() is very SLOW, if you have a database connection, you may want to use it to convert your strings to lower case. Even latin1/9 (iso-8859-1/15) and other encodings are possible.

Have a look at my simple benchmark:



```
<?php

$text = &quot;L&#xF6;rem ip&#xDF;&#xFC;m d&#xF6;l&#xF6;r &#xDF;it &#xE4;met, c&#xF6;n&#xDF;ectet&#xFC;er &#xE4;dipi&#xDF;cing elit. Sed lig&#xFC;l&#xE4;. Pr&#xE4;e&#xDF;ent j&#xFC;&#xDF;t&#xF6; tell&#xFC;&#xDF;, gr&#xE4;vid&#xE4; e&#xFC;, temp&#xFC;&#xDF; &#xE4;, m&#xE4;tti&#xDF; n&#xF6;n, &#xF6;rci. N&#xE4;m q&#xFC;i&#xDF; l&#xF6;rem. N&#xE4;m &#xE4;liq&#xFC;et elit &#xDF;ed elit. Ph&#xE4;&#xDF;ell&#xFC;&#xDF; venen&#xE4;ti&#xDF; j&#xFC;&#xDF;t&#xF6; eget enim. D&#xF6;nec ni&#xDF;l. Pr&#xF6;in m&#xE4;tti&#xDF; venen&#xE4;ti&#xDF; j&#xFC;&#xDF;t&#xF6;. Sed &#xE4;liq&#xFC;&#xE4;m p&#xF6;rt&#xE4; &#xF6;rci. Cr&#xE4;&#xDF; elit ni&#xDF;l, c&#xF6;nv&#xE4;lli&#xDF; q&#xFC;i&#xDF;, tincid&#xFC;nt &#xE4;t, vehic&#xFC;l&#xE4; &#xE4;cc&#xFC;m&#xDF;&#xE4;n, &#xF6;di&#xF6;. Sed m&#xF6;le&#xDF;tie. Eti&#xE4;m m&#xF6;lli&#xDF; fe&#xFC;gi&#xE4;t elit. Ve&#xDF;tib&#xFC;l&#xFC;m &#xE4;nte ip&#xDF;&#xFC;m primi&#xDF; in f&#xE4;&#xFC;cib&#xFC;&#xDF; &#xF6;rci l&#xFC;ct&#xFC;&#xDF; et &#xFC;ltrice&#xDF; p&#xF6;&#xDF;&#xFC;ere c&#xFC;bili&#xE4; C&#xFC;r&#xE4;e; M&#xE4;ecen&#xE4;&#xDF; n&#xF6;n n&#xFC;ll&#xE4;.&quot;;

// mb_strtolower()
$timeMB = microtime(true);&#xA0; &#xA0;&#xA0; 
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; 
&#xA0; &#xA0; for($i=0;$i&lt;30000;$i++) 
&#xA0; &#xA0; &#xA0; &#xA0; $lower = mb_strtolower(&quot;$text/no-cache-$i&quot;);

$timeMB = microtime(true) - $timeMB;

// MySQL lower()
$timeSQL = microtime(true);&#xA0; &#xA0; 

&#xA0; &#xA0; mysql_query(&quot;set names latin1&quot;);&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; 
&#xA0; &#xA0; for($i=0;$i&lt;30000;$i++) { 
&#xA0; &#xA0; &#xA0; &#xA0; $r = mysql_fetch_row(mysql_query(&quot;select lower(&apos;$text/no-cache-$i&apos;)&quot;));
&#xA0; &#xA0; &#xA0; &#xA0; $lower = $r[0];
&#xA0; &#xA0; }

$timeSQL = microtime(true) - $timeSQL;

echo &quot;mb: &quot;.sprintf(&quot;%.5f&quot;,$timeMB).&quot; sek.&lt;br /&gt;&quot;;
echo &quot;sql: &quot;.sprintf(&quot;%.5f&quot;,$timeSQL).&quot; sek.&lt;br /&gt;&quot;;

// Result on my notebook:
// mb: 11.50642 sek.
// sql: 5.44143 sek.

?>
```



  

#



Please, note that when using with UTF-8 mb_strtolower will only convert upper case characters to lower case which are marked with the Unicode property &quot;Upper case letter&quot; (&quot;Lu&quot;). However, there are also letters such as &quot;Letter numbers&quot; (Unicode property &quot;Nl&quot;) that also have lower case and upper case variants. These characters will not be converted be mb_strtolower!



Example:

The Roman letters &#x2160;, &#x2161;, &#x2162;, ..., &#x216F; (UTF-8 code points 8544 through 8559) also exist in their respective lower case variants &#x2170;, &#x2171;, &#x2172;, ..., &#x217F; (UTF-8 code points 8560 through 8575) and should, in my opinion, also be converted by mb_strtolower, but they are not!



Big internet-companies (like Google) do match both variants as semantically equal (since the representations only differ in case).



Since I was not finding any proper solution in the internet on how to map all UTF8-strings to their lowercase counterpart in PHP, I offer the following hard-coded extended mb_strtolower function for UTF-8 strings:



The function wraps the existing function mb_strtolower() and additionally replaces uppercase UTF8-characters for which there is a lowercase representation. Since there is no proper Unicode uppercase and lowercase character-table in the internet that I was able to find, I checked the first million UTF8-characters against the Google-search and -KeywordTool and identified the following 78 characters as uppercase-characters, not being replaced by mb_strtolower, but having a UTF8 lowercase counterpart.





```
<?php



//the numbers in the in-line-comments display the characters&apos; Unicode code-points (CP).

function strtolower_utf8_extended( $utf8_string )

{

&#xA0; &#xA0; $additional_replacements&#xA0; &#xA0; = array

&#xA0; &#xA0; &#xA0; &#xA0; ( &quot;&#x1C5;&quot;&#xA0; &#xA0; =&gt; &quot;&#x1C6;&quot;&#xA0; &#xA0; &#xA0; &#xA0; //&#xA0;&#xA0; 453 -&gt;&#xA0;&#xA0; 454

&#xA0; &#xA0; &#xA0; &#xA0; , &quot;&#x1C8;&quot;&#xA0; &#xA0; =&gt; &quot;&#x1C9;&quot;&#xA0; &#xA0; &#xA0; &#xA0; //&#xA0;&#xA0; 456 -&gt;&#xA0;&#xA0; 457

&#xA0; &#xA0; &#xA0; &#xA0; , &quot;&#x1CB;&quot;&#xA0; &#xA0; =&gt; &quot;&#x1CC;&quot;&#xA0; &#xA0; &#xA0; &#xA0; //&#xA0;&#xA0; 459 -&gt;&#xA0;&#xA0; 460

&#xA0; &#xA0; &#xA0; &#xA0; , &quot;&#x1F2;&quot;&#xA0; &#xA0; =&gt; &quot;&#x1F3;&quot;&#xA0; &#xA0; &#xA0; &#xA0; //&#xA0;&#xA0; 498 -&gt;&#xA0;&#xA0; 499

&#xA0; &#xA0; &#xA0; &#xA0; , &quot;&#x3F7;&quot;&#xA0; &#xA0; =&gt; &quot;&#x3F8;&quot;&#xA0; &#xA0; &#xA0; &#xA0; //&#xA0; 1015 -&gt;&#xA0; 1016

&#xA0; &#xA0; &#xA0; &#xA0; , &quot;&#x3F9;&quot;&#xA0; &#xA0; =&gt; &quot;&#x3F2;&quot;&#xA0; &#xA0; &#xA0; &#xA0; //&#xA0; 1017 -&gt;&#xA0; 1010

&#xA0; &#xA0; &#xA0; &#xA0; , &quot;&#x3FA;&quot;&#xA0; &#xA0; =&gt; &quot;&#x3FB;&quot;&#xA0; &#xA0; &#xA0; &#xA0; //&#xA0; 1018 -&gt;&#xA0; 1019

&#xA0; &#xA0; &#xA0; &#xA0; , &quot;&#x1F88;&quot;&#xA0; &#xA0; =&gt; &quot;&#x1F80;&quot;&#xA0; &#xA0; &#xA0; &#xA0; //&#xA0; 8072 -&gt;&#xA0; 8064

&#xA0; &#xA0; &#xA0; &#xA0; , &quot;&#x1F89;&quot;&#xA0; &#xA0; =&gt; &quot;&#x1F81;&quot;&#xA0; &#xA0; &#xA0; &#xA0; //&#xA0; 8073 -&gt;&#xA0; 8065

&#xA0; &#xA0; &#xA0; &#xA0; , &quot;&#x1F8A;&quot;&#xA0; &#xA0; =&gt; &quot;&#x1F82;&quot;&#xA0; &#xA0; &#xA0; &#xA0; //&#xA0; 8074 -&gt;&#xA0; 8066

&#xA0; &#xA0; &#xA0; &#xA0; , &quot;&#x1F8B;&quot;&#xA0; &#xA0; =&gt; &quot;&#x1F83;&quot;&#xA0; &#xA0; &#xA0; &#xA0; //&#xA0; 8075 -&gt;&#xA0; 8067

&#xA0; &#xA0; &#xA0; &#xA0; , &quot;&#x1F8C;&quot;&#xA0; &#xA0; =&gt; &quot;&#x1F84;&quot;&#xA0; &#xA0; &#xA0; &#xA0; //&#xA0; 8076 -&gt;&#xA0; 8068

&#xA0; &#xA0; &#xA0; &#xA0; , &quot;&#x1F8D;&quot;&#xA0; &#xA0; =&gt; &quot;&#x1F85;&quot;&#xA0; &#xA0; &#xA0; &#xA0; //&#xA0; 8077 -&gt;&#xA0; 8069

&#xA0; &#xA0; &#xA0; &#xA0; , &quot;&#x1F8E;&quot;&#xA0; &#xA0; =&gt; &quot;&#x1F86;&quot;&#xA0; &#xA0; &#xA0; &#xA0; //&#xA0; 8078 -&gt;&#xA0; 8070

&#xA0; &#xA0; &#xA0; &#xA0; , &quot;&#x1F8F;&quot;&#xA0; &#xA0; =&gt; &quot;&#x1F87;&quot;&#xA0; &#xA0; &#xA0; &#xA0; //&#xA0; 8079 -&gt;&#xA0; 8071

&#xA0; &#xA0; &#xA0; &#xA0; , &quot;&#x1F98;&quot;&#xA0; &#xA0; =&gt; &quot;&#x1F90;&quot;&#xA0; &#xA0; &#xA0; &#xA0; //&#xA0; 8088 -&gt;&#xA0; 8080

&#xA0; &#xA0; &#xA0; &#xA0; , &quot;&#x1F99;&quot;&#xA0; &#xA0; =&gt; &quot;&#x1F91;&quot;&#xA0; &#xA0; &#xA0; &#xA0; //&#xA0; 8089 -&gt;&#xA0; 8081

&#xA0; &#xA0; &#xA0; &#xA0; , &quot;&#x1F9A;&quot;&#xA0; &#xA0; =&gt; &quot;&#x1F92;&quot;&#xA0; &#xA0; &#xA0; &#xA0; //&#xA0; 8090 -&gt;&#xA0; 8082

&#xA0; &#xA0; &#xA0; &#xA0; , &quot;&#x1F9B;&quot;&#xA0; &#xA0; =&gt; &quot;&#x1F93;&quot;&#xA0; &#xA0; &#xA0; &#xA0; //&#xA0; 8091 -&gt;&#xA0; 8083

&#xA0; &#xA0; &#xA0; &#xA0; , &quot;&#x1F9C;&quot;&#xA0; &#xA0; =&gt; &quot;&#x1F94;&quot;&#xA0; &#xA0; &#xA0; &#xA0; //&#xA0; 8092 -&gt;&#xA0; 8084

&#xA0; &#xA0; &#xA0; &#xA0; , &quot;&#x1F9D;&quot;&#xA0; &#xA0; =&gt; &quot;&#x1F95;&quot;&#xA0; &#xA0; &#xA0; &#xA0; //&#xA0; 8093 -&gt;&#xA0; 8085

&#xA0; &#xA0; &#xA0; &#xA0; , &quot;&#x1F9E;&quot;&#xA0; &#xA0; =&gt; &quot;&#x1F96;&quot;&#xA0; &#xA0; &#xA0; &#xA0; //&#xA0; 8094 -&gt;&#xA0; 8086

&#xA0; &#xA0; &#xA0; &#xA0; , &quot;&#x1F9F;&quot;&#xA0; &#xA0; =&gt; &quot;&#x1F97;&quot;&#xA0; &#xA0; &#xA0; &#xA0; //&#xA0; 8095 -&gt;&#xA0; 8087

&#xA0; &#xA0; &#xA0; &#xA0; , &quot;&#x1FA8;&quot;&#xA0; &#xA0; =&gt; &quot;&#x1FA0;&quot;&#xA0; &#xA0; &#xA0; &#xA0; //&#xA0; 8104 -&gt;&#xA0; 8096

&#xA0; &#xA0; &#xA0; &#xA0; , &quot;&#x1FA9;&quot;&#xA0; &#xA0; =&gt; &quot;&#x1FA1;&quot;&#xA0; &#xA0; &#xA0; &#xA0; //&#xA0; 8105 -&gt;&#xA0; 8097

&#xA0; &#xA0; &#xA0; &#xA0; , &quot;&#x1FAA;&quot;&#xA0; &#xA0; =&gt; &quot;&#x1FA2;&quot;&#xA0; &#xA0; &#xA0; &#xA0; //&#xA0; 8106 -&gt;&#xA0; 8098

&#xA0; &#xA0; &#xA0; &#xA0; , &quot;&#x1FAB;&quot;&#xA0; &#xA0; =&gt; &quot;&#x1FA3;&quot;&#xA0; &#xA0; &#xA0; &#xA0; //&#xA0; 8107 -&gt;&#xA0; 8099

&#xA0; &#xA0; &#xA0; &#xA0; , &quot;&#x1FAC;&quot;&#xA0; &#xA0; =&gt; &quot;&#x1FA4;&quot;&#xA0; &#xA0; &#xA0; &#xA0; //&#xA0; 8108 -&gt;&#xA0; 8100

&#xA0; &#xA0; &#xA0; &#xA0; , &quot;&#x1FAD;&quot;&#xA0; &#xA0; =&gt; &quot;&#x1FA5;&quot;&#xA0; &#xA0; &#xA0; &#xA0; //&#xA0; 8109 -&gt;&#xA0; 8101

&#xA0; &#xA0; &#xA0; &#xA0; , &quot;&#x1FAE;&quot;&#xA0; &#xA0; =&gt; &quot;&#x1FA6;&quot;&#xA0; &#xA0; &#xA0; &#xA0; //&#xA0; 8110 -&gt;&#xA0; 8102

&#xA0; &#xA0; &#xA0; &#xA0; , &quot;&#x1FAF;&quot;&#xA0; &#xA0; =&gt; &quot;&#x1FA7;&quot;&#xA0; &#xA0; &#xA0; &#xA0; //&#xA0; 8111 -&gt;&#xA0; 8103

&#xA0; &#xA0; &#xA0; &#xA0; , &quot;&#x1FBC;&quot;&#xA0; &#xA0; =&gt; &quot;&#x1FB3;&quot;&#xA0; &#xA0; &#xA0; &#xA0; //&#xA0; 8124 -&gt;&#xA0; 8115

&#xA0; &#xA0; &#xA0; &#xA0; , &quot;&#x1FCC;&quot;&#xA0; &#xA0; =&gt; &quot;&#x1FC3;&quot;&#xA0; &#xA0; &#xA0; &#xA0; //&#xA0; 8140 -&gt;&#xA0; 8131

&#xA0; &#xA0; &#xA0; &#xA0; , &quot;&#x1FFC;&quot;&#xA0; &#xA0; =&gt; &quot;&#x1FF3;&quot;&#xA0; &#xA0; &#xA0; &#xA0; //&#xA0; 8188 -&gt;&#xA0; 8179

&#xA0; &#xA0; &#xA0; &#xA0; , &quot;&#x2160;&quot;&#xA0; &#xA0; =&gt; &quot;&#x2170;&quot;&#xA0; &#xA0; &#xA0; &#xA0; //&#xA0; 8544 -&gt;&#xA0; 8560

&#xA0; &#xA0; &#xA0; &#xA0; , &quot;&#x2161;&quot;&#xA0; &#xA0; =&gt; &quot;&#x2171;&quot;&#xA0; &#xA0; &#xA0; &#xA0; //&#xA0; 8545 -&gt;&#xA0; 8561

&#xA0; &#xA0; &#xA0; &#xA0; , &quot;&#x2162;&quot;&#xA0; &#xA0; =&gt; &quot;&#x2172;&quot;&#xA0; &#xA0; &#xA0; &#xA0; //&#xA0; 8546 -&gt;&#xA0; 8562

&#xA0; &#xA0; &#xA0; &#xA0; , &quot;&#x2163;&quot;&#xA0; &#xA0; =&gt; &quot;&#x2173;&quot;&#xA0; &#xA0; &#xA0; &#xA0; //&#xA0; 8547 -&gt;&#xA0; 8563

&#xA0; &#xA0; &#xA0; &#xA0; , &quot;&#x2164;&quot;&#xA0; &#xA0; =&gt; &quot;&#x2174;&quot;&#xA0; &#xA0; &#xA0; &#xA0; //&#xA0; 8548 -&gt;&#xA0; 8564

&#xA0; &#xA0; &#xA0; &#xA0; , &quot;&#x2165;&quot;&#xA0; &#xA0; =&gt; &quot;&#x2175;&quot;&#xA0; &#xA0; &#xA0; &#xA0; //&#xA0; 8549 -&gt;&#xA0; 8565

&#xA0; &#xA0; &#xA0; &#xA0; , &quot;&#x2166;&quot;&#xA0; &#xA0; =&gt; &quot;&#x2176;&quot;&#xA0; &#xA0; &#xA0; &#xA0; //&#xA0; 8550 -&gt;&#xA0; 8566

&#xA0; &#xA0; &#xA0; &#xA0; , &quot;&#x2167;&quot;&#xA0; &#xA0; =&gt; &quot;&#x2177;&quot;&#xA0; &#xA0; &#xA0; &#xA0; //&#xA0; 8551 -&gt;&#xA0; 8567

&#xA0; &#xA0; &#xA0; &#xA0; , &quot;&#x2168;&quot;&#xA0; &#xA0; =&gt; &quot;&#x2178;&quot;&#xA0; &#xA0; &#xA0; &#xA0; //&#xA0; 8552 -&gt;&#xA0; 8568

&#xA0; &#xA0; &#xA0; &#xA0; , &quot;&#x2169;&quot;&#xA0; &#xA0; =&gt; &quot;&#x2179;&quot;&#xA0; &#xA0; &#xA0; &#xA0; //&#xA0; 8553 -&gt;&#xA0; 8569

&#xA0; &#xA0; &#xA0; &#xA0; , &quot;&#x216A;&quot;&#xA0; &#xA0; =&gt; &quot;&#x217A;&quot;&#xA0; &#xA0; &#xA0; &#xA0; //&#xA0; 8554 -&gt;&#xA0; 8570

&#xA0; &#xA0; &#xA0; &#xA0; , &quot;&#x216B;&quot;&#xA0; &#xA0; =&gt; &quot;&#x217B;&quot;&#xA0; &#xA0; &#xA0; &#xA0; //&#xA0; 8555 -&gt;&#xA0; 8571

&#xA0; &#xA0; &#xA0; &#xA0; , &quot;&#x216C;&quot;&#xA0; &#xA0; =&gt; &quot;&#x217C;&quot;&#xA0; &#xA0; &#xA0; &#xA0; //&#xA0; 8556 -&gt;&#xA0; 8572

&#xA0; &#xA0; &#xA0; &#xA0; , &quot;&#x216D;&quot;&#xA0; &#xA0; =&gt; &quot;&#x217D;&quot;&#xA0; &#xA0; &#xA0; &#xA0; //&#xA0; 8557 -&gt;&#xA0; 8573

&#xA0; &#xA0; &#xA0; &#xA0; , &quot;&#x216E;&quot;&#xA0; &#xA0; =&gt; &quot;&#x217E;&quot;&#xA0; &#xA0; &#xA0; &#xA0; //&#xA0; 8558 -&gt;&#xA0; 8574

&#xA0; &#xA0; &#xA0; &#xA0; , &quot;&#x216F;&quot;&#xA0; &#xA0; =&gt; &quot;&#x217F;&quot;&#xA0; &#xA0; &#xA0; &#xA0; //&#xA0; 8559 -&gt;&#xA0; 8575

&#xA0; &#xA0; &#xA0; &#xA0; , &quot;&#x24B6;&quot;&#xA0; &#xA0; =&gt; &quot;&#x24D0;&quot;&#xA0; &#xA0; &#xA0; &#xA0; //&#xA0; 9398 -&gt;&#xA0; 9424

&#xA0; &#xA0; &#xA0; &#xA0; , &quot;&#x24B7;&quot;&#xA0; &#xA0; =&gt; &quot;&#x24D1;&quot;&#xA0; &#xA0; &#xA0; &#xA0; //&#xA0; 9399 -&gt;&#xA0; 9425

&#xA0; &#xA0; &#xA0; &#xA0; , &quot;&#x24B8;&quot;&#xA0; &#xA0; =&gt; &quot;&#x24D2;&quot;&#xA0; &#xA0; &#xA0; &#xA0; //&#xA0; 9400 -&gt;&#xA0; 9426

&#xA0; &#xA0; &#xA0; &#xA0; , &quot;&#x24B9;&quot;&#xA0; &#xA0; =&gt; &quot;&#x24D3;&quot;&#xA0; &#xA0; &#xA0; &#xA0; //&#xA0; 9401 -&gt;&#xA0; 9427

&#xA0; &#xA0; &#xA0; &#xA0; , &quot;&#x24BA;&quot;&#xA0; &#xA0; =&gt; &quot;&#x24D4;&quot;&#xA0; &#xA0; &#xA0; &#xA0; //&#xA0; 9402 -&gt;&#xA0; 9428

&#xA0; &#xA0; &#xA0; &#xA0; , &quot;&#x24BB;&quot;&#xA0; &#xA0; =&gt; &quot;&#x24D5;&quot;&#xA0; &#xA0; &#xA0; &#xA0; //&#xA0; 9403 -&gt;&#xA0; 9429

&#xA0; &#xA0; &#xA0; &#xA0; , &quot;&#x24BC;&quot;&#xA0; &#xA0; =&gt; &quot;&#x24D6;&quot;&#xA0; &#xA0; &#xA0; &#xA0; //&#xA0; 9404 -&gt;&#xA0; 9430

&#xA0; &#xA0; &#xA0; &#xA0; , &quot;&#x24BD;&quot;&#xA0; &#xA0; =&gt; &quot;&#x24D7;&quot;&#xA0; &#xA0; &#xA0; &#xA0; //&#xA0; 9405 -&gt;&#xA0; 9431

&#xA0; &#xA0; &#xA0; &#xA0; , &quot;&#x24BE;&quot;&#xA0; &#xA0; =&gt; &quot;&#x24D8;&quot;&#xA0; &#xA0; &#xA0; &#xA0; //&#xA0; 9406 -&gt;&#xA0; 9432

&#xA0; &#xA0; &#xA0; &#xA0; , &quot;&#x24BF;&quot;&#xA0; &#xA0; =&gt; &quot;&#x24D9;&quot;&#xA0; &#xA0; &#xA0; &#xA0; //&#xA0; 9407 -&gt;&#xA0; 9433

&#xA0; &#xA0; &#xA0; &#xA0; , &quot;&#x24C0;&quot;&#xA0; &#xA0; =&gt; &quot;&#x24DA;&quot;&#xA0; &#xA0; &#xA0; &#xA0; //&#xA0; 9408 -&gt;&#xA0; 9434

&#xA0; &#xA0; &#xA0; &#xA0; , &quot;&#x24C1;&quot;&#xA0; &#xA0; =&gt; &quot;&#x24DB;&quot;&#xA0; &#xA0; &#xA0; &#xA0; //&#xA0; 9409 -&gt;&#xA0; 9435

&#xA0; &#xA0; &#xA0; &#xA0; , &quot;&#x24C2;&quot;&#xA0; &#xA0; =&gt; &quot;&#x24DC;&quot;&#xA0; &#xA0; &#xA0; &#xA0; //&#xA0; 9410 -&gt;&#xA0; 9436

&#xA0; &#xA0; &#xA0; &#xA0; , &quot;&#x24C3;&quot;&#xA0; &#xA0; =&gt; &quot;&#x24DD;&quot;&#xA0; &#xA0; &#xA0; &#xA0; //&#xA0; 9411 -&gt;&#xA0; 9437

&#xA0; &#xA0; &#xA0; &#xA0; , &quot;&#x24C4;&quot;&#xA0; &#xA0; =&gt; &quot;&#x24DE;&quot;&#xA0; &#xA0; &#xA0; &#xA0; //&#xA0; 9412 -&gt;&#xA0; 9438

&#xA0; &#xA0; &#xA0; &#xA0; , &quot;&#x24C5;&quot;&#xA0; &#xA0; =&gt; &quot;&#x24DF;&quot;&#xA0; &#xA0; &#xA0; &#xA0; //&#xA0; 9413 -&gt;&#xA0; 9439

&#xA0; &#xA0; &#xA0; &#xA0; , &quot;&#x24C6;&quot;&#xA0; &#xA0; =&gt; &quot;&#x24E0;&quot;&#xA0; &#xA0; &#xA0; &#xA0; //&#xA0; 9414 -&gt;&#xA0; 9440

&#xA0; &#xA0; &#xA0; &#xA0; , &quot;&#x24C7;&quot;&#xA0; &#xA0; =&gt; &quot;&#x24E1;&quot;&#xA0; &#xA0; &#xA0; &#xA0; //&#xA0; 9415 -&gt;&#xA0; 9441

&#xA0; &#xA0; &#xA0; &#xA0; , &quot;&#x24C8;&quot;&#xA0; &#xA0; =&gt; &quot;&#x24E2;&quot;&#xA0; &#xA0; &#xA0; &#xA0; //&#xA0; 9416 -&gt;&#xA0; 9442

&#xA0; &#xA0; &#xA0; &#xA0; , &quot;&#x24C9;&quot;&#xA0; &#xA0; =&gt; &quot;&#x24E3;&quot;&#xA0; &#xA0; &#xA0; &#xA0; //&#xA0; 9417 -&gt;&#xA0; 9443

&#xA0; &#xA0; &#xA0; &#xA0; , &quot;&#x24CA;&quot;&#xA0; &#xA0; =&gt; &quot;&#x24E4;&quot;&#xA0; &#xA0; &#xA0; &#xA0; //&#xA0; 9418 -&gt;&#xA0; 9444

&#xA0; &#xA0; &#xA0; &#xA0; , &quot;&#x24CB;&quot;&#xA0; &#xA0; =&gt; &quot;&#x24E5;&quot;&#xA0; &#xA0; &#xA0; &#xA0; //&#xA0; 9419 -&gt;&#xA0; 9445

&#xA0; &#xA0; &#xA0; &#xA0; , &quot;&#x24CC;&quot;&#xA0; &#xA0; =&gt; &quot;&#x24E6;&quot;&#xA0; &#xA0; &#xA0; &#xA0; //&#xA0; 9420 -&gt;&#xA0; 9446

&#xA0; &#xA0; &#xA0; &#xA0; , &quot;&#x24CD;&quot;&#xA0; &#xA0; =&gt; &quot;&#x24E7;&quot;&#xA0; &#xA0; &#xA0; &#xA0; //&#xA0; 9421 -&gt;&#xA0; 9447

&#xA0; &#xA0; &#xA0; &#xA0; , &quot;&#x24CE;&quot;&#xA0; &#xA0; =&gt; &quot;&#x24E8;&quot;&#xA0; &#xA0; &#xA0; &#xA0; //&#xA0; 9422 -&gt;&#xA0; 9448

&#xA0; &#xA0; &#xA0; &#xA0; , &quot;&#x24CF;&quot;&#xA0; &#xA0; =&gt; &quot;&#x24E9;&quot;&#xA0; &#xA0; &#xA0; &#xA0; //&#xA0; 9423 -&gt;&#xA0; 9449

&#xA0; &#xA0; &#xA0; &#xA0; , &quot;&#x10426;&quot;&#xA0; &#xA0; =&gt; &quot;&#x1044E;&quot;&#xA0; &#xA0; &#xA0; &#xA0; // 66598 -&gt; 66638

&#xA0; &#xA0; &#xA0; &#xA0; , &quot;&#x10427;&quot;&#xA0; &#xA0; =&gt; &quot;&#x1044F;&quot;&#xA0; &#xA0; &#xA0; &#xA0; // 66599 -&gt; 66639

&#xA0; &#xA0; &#xA0; &#xA0; );

&#xA0; &#xA0; 

&#xA0; &#xA0; $utf8_string&#xA0; &#xA0; = mb_strtolower( $utf8_string, &quot;UTF-8&quot;);

&#xA0; &#xA0; 

&#xA0; &#xA0; $utf8_string&#xA0; &#xA0; = strtr( $utf8_string, $additional_replacements );

&#xA0; &#xA0; 

&#xA0; &#xA0; return $utf8_string;

} //strtolower_utf8_extended()



?>
```



  

#

[Official documentation page](https://www.php.net/manual/en/function.mb-strtolower.php)

**[To root](/README.md)**