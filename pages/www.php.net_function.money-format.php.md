# money_format



For most of us in the US, we don&apos;t want to see a "USD" for our currency symbol, so &apos;%i&apos; doesn&apos;t cut it.  Here&apos;s what I used that worked to get what most  people expect to see for a number format.<br><br>$number = 123.4<br>setlocale(LC_MONETARY, &apos;en_US.UTF-8&apos;);<br>money_format(&apos;%.2n&apos;, $number);<br><br>output:<br>$123.40<br><br>That gives me a dollar sign at the beginning, and 2 digits at the end.  

#

This is a some function posted before, however various bugs were corrected.<br><br>Thank you to Stuart Roe by reporting the bug on printing signals.<br><br>

```
<?php
/*
That it is an implementation of the function money_format for the
platforms that do not it bear.  

The function accepts to same string of format accepts for the
original function of the PHP.  

(Sorry. my writing in English is very bad)  

The function is tested using PHP 5.1.4 in Windows XP
and Apache WebServer.
*/
function money_format($format, $number)
{
    $regex  = &apos;/%((?:[\^!\-]|\+|\(|\=.)*)([0-9]+)?&apos;.
              &apos;(?:#([0-9]+))?(?:\.([0-9]+))?([in%])/&apos;;
    if (setlocale(LC_MONETARY, 0) == &apos;C&apos;) {
        setlocale(LC_MONETARY, &apos;&apos;);
    }
    $locale = localeconv();
    preg_match_all($regex, $format, $matches, PREG_SET_ORDER);
    foreach ($matches as $fmatch) {
        $value = floatval($number);
        $flags = array(
            &apos;fillchar&apos;  =&gt; preg_match(&apos;/\=(.)/&apos;, $fmatch[1], $match) ?
                           $match[1] : &apos; &apos;,
            &apos;nogroup&apos;   =&gt; preg_match(&apos;/\^/&apos;, $fmatch[1]) &gt; 0,
            &apos;usesignal&apos; =&gt; preg_match(&apos;/\+|\(/&apos;, $fmatch[1], $match) ?
                           $match[0] : &apos;+&apos;,
            &apos;nosimbol&apos;  =&gt; preg_match(&apos;/\!/&apos;, $fmatch[1]) &gt; 0,
            &apos;isleft&apos;    =&gt; preg_match(&apos;/\-/&apos;, $fmatch[1]) &gt; 0
        );
        $width      = trim($fmatch[2]) ? (int)$fmatch[2] : 0;
        $left       = trim($fmatch[3]) ? (int)$fmatch[3] : 0;
        $right      = trim($fmatch[4]) ? (int)$fmatch[4] : $locale[&apos;int_frac_digits&apos;];
        $conversion = $fmatch[5];

        $positive = true;
        if ($value &lt; 0) {
            $positive = false;
            $value  *= -1;
        }
        $letter = $positive ? &apos;p&apos; : &apos;n&apos;;

        $prefix = $suffix = $cprefix = $csuffix = $signal = &apos;&apos;;

        $signal = $positive ? $locale[&apos;positive_sign&apos;] : $locale[&apos;negative_sign&apos;];
        switch (true) {
            case $locale["{$letter}_sign_posn"] == 1 &amp;&amp; $flags[&apos;usesignal&apos;] == &apos;+&apos;:
                $prefix = $signal;
                break;
            case $locale["{$letter}_sign_posn"] == 2 &amp;&amp; $flags[&apos;usesignal&apos;] == &apos;+&apos;:
                $suffix = $signal;
                break;
            case $locale["{$letter}_sign_posn"] == 3 &amp;&amp; $flags[&apos;usesignal&apos;] == &apos;+&apos;:
                $cprefix = $signal;
                break;
            case $locale["{$letter}_sign_posn"] == 4 &amp;&amp; $flags[&apos;usesignal&apos;] == &apos;+&apos;:
                $csuffix = $signal;
                break;
            case $flags[&apos;usesignal&apos;] == &apos;(&apos;:
            case $locale["{$letter}_sign_posn"] == 0:
                $prefix = &apos;(&apos;;
                $suffix = &apos;)&apos;;
                break;
        }
        if (!$flags[&apos;nosimbol&apos;]) {
            $currency = $cprefix .
                        ($conversion == &apos;i&apos; ? $locale[&apos;int_curr_symbol&apos;] : $locale[&apos;currency_symbol&apos;]) .
                        $csuffix;
        } else {
            $currency = &apos;&apos;;
        }
        $space  = $locale["{$letter}_sep_by_space"] ? &apos; &apos; : &apos;&apos;;

        $value = number_format($value, $right, $locale[&apos;mon_decimal_point&apos;],
                 $flags[&apos;nogroup&apos;] ? &apos;&apos; : $locale[&apos;mon_thousands_sep&apos;]);
        $value = @explode($locale[&apos;mon_decimal_point&apos;], $value);

        $n = strlen($prefix) + strlen($currency) + strlen($value[0]);
        if ($left &gt; 0 &amp;&amp; $left &gt; $n) {
            $value[0] = str_repeat($flags[&apos;fillchar&apos;], $left - $n) . $value[0];
        }
        $value = implode($locale[&apos;mon_decimal_point&apos;], $value);
        if ($locale["{$letter}_cs_precedes"]) {
            $value = $prefix . $currency . $space . $value . $suffix;
        } else {
            $value = $prefix . $value . $space . $currency . $suffix;
        }
        if ($width &gt; 0) {
            $value = str_pad($value, $width, $flags[&apos;fillchar&apos;], $flags[&apos;isleft&apos;] ?
                     STR_PAD_RIGHT : STR_PAD_LEFT);
        }

        $format = str_replace($fmatch[0], $value, $format);
    }
    return $format;
}

?>
```
  

#

In Rafael M. Salvioni function localeconv(); returns an invalid array in my Windows XP SP3 running PHP 5.4.13 so to prevent the Warning Message: implode(): Invalid arguments passed i just add the $locale manually. For other languages just fill the array with the correct settings.<br><br>&lt;?<br><br>       $locale = array(<br>        &apos;decimal_point&apos;        =&gt; &apos;.&apos;,<br>        &apos;thousands_sep&apos;        =&gt; &apos;&apos;,<br>        &apos;int_curr_symbol&apos;    =&gt; &apos;EUR&apos;,<br>        &apos;currency_symbol&apos;    =&gt; &apos;&#x20AC;&apos;,<br>        &apos;mon_decimal_point&apos;    =&gt; &apos;,&apos;,<br>        &apos;mon_thousands_sep&apos;    =&gt; &apos;.&apos;,<br>        &apos;positive_sign&apos;        =&gt; &apos;&apos;,<br>        &apos;negative_sign&apos;     =&gt; &apos;-&apos;,<br>        &apos;int_frac_digits&apos;    =&gt; 2,<br>        &apos;frac_digits&apos;        =&gt; 2,<br>        &apos;p_cs_precedes&apos;        =&gt; 0,<br>        &apos;p_sep_by_space&apos;    =&gt; 1,<br>        &apos;p_sign_posn&apos;        =&gt; 1,<br>        &apos;n_sign_posn&apos;        =&gt; 1,<br>        &apos;grouping&apos;            =&gt; array(),<br>        &apos;mon_grouping&apos;        =&gt; array(0 =&gt; 3, 1 =&gt; 3)<br>        <br>    );<br>?>
```
  

#

If money_format doesn&apos;t seem to be working properly, make sure you are defining a valid locale.  For example, on Debian, &apos;en_US&apos; is not a valid locale - you need &apos;en_US.UTF-8&apos; or &apos;en_US.ISO-8559-1&apos;.<br><br>This was frustrating me for a while.  Debian has a list of valid locales at /usr/share/i18n/SUPPORTED; find yours there if it&apos;s not working properly.  

#

[Official documentation page](https://www.php.net/manual/en/function.money-format.php)

**[To root](/README.md)**