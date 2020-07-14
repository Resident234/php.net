# imap_search



The date format for e.g. SINCE is, according to rfc3501:<br><br>date            = date-text / DQUOTE date-text DQUOTE<br><br>date-day        = 1*2DIGIT<br>                    ; Day of month<br><br>date-day-fixed  = (SP DIGIT) / 2DIGIT<br>                    ; Fixed-format version of date-day<br><br>date-month      = "Jan" / "Feb" / "Mar" / "Apr" / "May" / "Jun" /<br>                  "Jul" / "Aug" / "Sep" / "Oct" / "Nov" / "Dec"<br><br>date-text       = date-day "-" date-month "-" date-year<br><br>So a valid date is e.g. "22-Jul-2012" with or without the double quotes.  

#

Hi, <br>be aware, that imap_search() does NOT (as you may exspect) return an empty array, if nothing was found! <br>As the manual says, it returns FALSE.<br><br>Do not test the result like "count($array)" as I did. <br>This gives you 1 for an empty result. Took me an hour to found out why :-(  RTFM  

#

To set your own CHARSET, which is useful if you are dealing with Chinese Japanese and Korean queries.<br><br>

```
<?php imap_search($inbox,&apos;BODY "&apos;.$keyword.&apos;"&apos;, SE_FREE, "UTF-8"); ?>
```
  

#

[Official documentation page](https://www.php.net/manual/en/function.imap-search.php)

**[To root](/README.md)**