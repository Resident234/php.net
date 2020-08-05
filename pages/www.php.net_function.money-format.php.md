# money_format



For most of us in the US, we don&apos;t want to see a "USD" for our currency symbol, so &apos;%i&apos; doesn&apos;t cut it.  Here&apos;s what I used that worked to get what most  people expect to see for a number format.<br><br>$number = 123.4<br>setlocale(LC_MONETARY, &apos;en_US.UTF-8&apos;);<br>money_format(&apos;%.2n&apos;, $number);<br><br>output:<br>$123.40<br><br>That gives me a dollar sign at the beginning, and 2 digits at the end.  

---

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
    $regex  = '/%((?:[\^!\-]|\+|\(|\=.)*)([0-9]+)?'.
              '(?:#([0-9]+))?(?:\.([0-9]+))?([in%])/';
    if (setlocale(LC_MONETARY, 0) == 'C') {
        setlocale(LC_MONETARY, '');
    }
    $locale = localeconv();
    preg_match_all($regex, $format, $matches, PREG_SET_ORDER);
    foreach ($matches as $fmatch) {
        $value = floatval($number);
        $flags = array(
            'fillchar'  => preg_match('/\=(.)/', $fmatch[1], $match) ?
                           $match[1] : ' ',
            'nogroup'   => preg_match('/\^/', $fmatch[1]) > 0,
            'usesignal' => preg_match('/\+|\(/', $fmatch[1], $match) ?
                           $match[0] : '+',
            'nosimbol'  => preg_match('/\!/', $fmatch[1]) > 0,
            'isleft'    => preg_match('/\-/', $fmatch[1]) > 0
        );
        $width      = trim($fmatch[2]) ? (int)$fmatch[2] : 0;
        $left       = trim($fmatch[3]) ? (int)$fmatch[3] : 0;
        $right      = trim($fmatch[4]) ? (int)$fmatch[4] : $locale['int_frac_digits'];
        $conversion = $fmatch[5];

        $positive = true;
        if ($value < 0) {
            $positive = false;
            $value  *= -1;
        }
        $letter = $positive ? 'p' : 'n';

        $prefix = $suffix = $cprefix = $csuffix = $signal = '';

        $signal = $positive ? $locale['positive_sign'] : $locale['negative_sign'];
        switch (true) {
            case $locale["{$letter}_sign_posn"] == 1 &amp;&amp; $flags['usesignal'] == '+':
                $prefix = $signal;
                break;
            case $locale["{$letter}_sign_posn"] == 2 &amp;&amp; $flags['usesignal'] == '+':
                $suffix = $signal;
                break;
            case $locale["{$letter}_sign_posn"] == 3 &amp;&amp; $flags['usesignal'] == '+':
                $cprefix = $signal;
                break;
            case $locale["{$letter}_sign_posn"] == 4 &amp;&amp; $flags['usesignal'] == '+':
                $csuffix = $signal;
                break;
            case $flags['usesignal'] == '(':
            case $locale["{$letter}_sign_posn"] == 0:
                $prefix = '(';
                $suffix = ')';
                break;
        }
        if (!$flags['nosimbol']) {
            $currency = $cprefix .
                        ($conversion == 'i' ? $locale['int_curr_symbol'] : $locale['currency_symbol']) .
                        $csuffix;
        } else {
            $currency = '';
        }
        $space  = $locale["{$letter}_sep_by_space"] ? ' ' : '';

        $value = number_format($value, $right, $locale['mon_decimal_point'],
                 $flags['nogroup'] ? '' : $locale['mon_thousands_sep']);
        $value = @explode($locale['mon_decimal_point'], $value);

        $n = strlen($prefix) + strlen($currency) + strlen($value[0]);
        if ($left > 0 &amp;&amp; $left > $n) {
            $value[0] = str_repeat($flags['fillchar'], $left - $n) . $value[0];
        }
        $value = implode($locale['mon_decimal_point'], $value);
        if ($locale["{$letter}_cs_precedes"]) {
            $value = $prefix . $currency . $space . $value . $suffix;
        } else {
            $value = $prefix . $value . $space . $currency . $suffix;
        }
        if ($width > 0) {
            $value = str_pad($value, $width, $flags['fillchar'], $flags['isleft'] ?
                     STR_PAD_RIGHT : STR_PAD_LEFT);
        }

        $format = str_replace($fmatch[0], $value, $format);
    }
    return $format;
}

?>
```
  

---

In Rafael M. Salvioni function localeconv(); returns an invalid array in my Windows XP SP3 running PHP 5.4.13 so to prevent the Warning Message: implode(): Invalid arguments passed i just add the $locale manually. For other languages just fill the array with the correct settings.<br><br>

```
<?php

       $locale = array(
        'decimal_point'        => '.',
        'thousands_sep'        => '',
        'int_curr_symbol'    => 'EUR',
        'currency_symbol'    => '&#x20AC;',
        'mon_decimal_point'    => ',',
        'mon_thousands_sep'    => '.',
        'positive_sign'        => '',
        'negative_sign'     => '-',
        'int_frac_digits'    => 2,
        'frac_digits'        => 2,
        'p_cs_precedes'        => 0,
        'p_sep_by_space'    => 1,
        'p_sign_posn'        => 1,
        'n_sign_posn'        => 1,
        'grouping'            => array(),
        'mon_grouping'        => array(0 => 3, 1 => 3)
        
    );
?>
```
  

---

If money_format doesn&apos;t seem to be working properly, make sure you are defining a valid locale.  For example, on Debian, &apos;en_US&apos; is not a valid locale - you need &apos;en_US.UTF-8&apos; or &apos;en_US.ISO-8559-1&apos;.<br><br>This was frustrating me for a while.  Debian has a list of valid locales at /usr/share/i18n/SUPPORTED; find yours there if it&apos;s not working properly.  

---

[Official documentation page](https://www.php.net/manual/en/function.money-format.php)

**[To root](/README.md)**