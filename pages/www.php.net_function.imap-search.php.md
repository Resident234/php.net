# imap_search





The date format for e.g. SINCE is, according to rfc3501:

date&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; = date-text / DQUOTE date-text DQUOTE

date-day&#xA0; &#xA0; &#xA0; &#xA0; = 1*2DIGIT
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; ; Day of month

date-day-fixed&#xA0; = (SP DIGIT) / 2DIGIT
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; ; Fixed-format version of date-day

date-month&#xA0; &#xA0; &#xA0; = &quot;Jan&quot; / &quot;Feb&quot; / &quot;Mar&quot; / &quot;Apr&quot; / &quot;May&quot; / &quot;Jun&quot; /
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &quot;Jul&quot; / &quot;Aug&quot; / &quot;Sep&quot; / &quot;Oct&quot; / &quot;Nov&quot; / &quot;Dec&quot;

date-text&#xA0; &#xA0; &#xA0;&#xA0; = date-day &quot;-&quot; date-month &quot;-&quot; date-year

So a valid date is e.g. &quot;22-Jul-2012&quot; with or without the double quotes.

  

#



Hi, 
be aware, that imap_search() does NOT (as you may exspect) return an empty array, if nothing was found! 
As the manual says, it returns FALSE.

Do not test the result like &quot;count($array)&quot; as I did. 
This gives you 1 for an empty result. Took me an hour to found out why :-(&#xA0; RTFM

  

#



To set your own CHARSET, which is useful if you are dealing with Chinese Japanese and Korean queries.





```
<?php imap_search($inbox,&apos;BODY &quot;&apos;.$keyword.&apos;&quot;&apos;, SE_FREE, &quot;UTF-8&quot;); ?>
```



  

#

[Official documentation page](https://www.php.net/manual/en/function.imap-search.php)

**[To root](/README.md)**