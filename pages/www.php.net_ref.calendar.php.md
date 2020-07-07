# Calendar Functions





I created this function a while ago and needed it again recently, so had to trawl through some old files to find it. Thought I&apos;d post it here in case anyone else finds it useful.



```
<?php

/*
 *&#xA0; &#xA0; Function to calculate which days are British bank holidays (England &amp; Wales) for a given year.
 *
 *&#xA0; &#xA0; Created by David Scourfield, 07 August 2006, and released into the public domain.
 *&#xA0; &#xA0; Anybody may use and/or modify this code.
 *
 *&#xA0; &#xA0; USAGE:
 *
 *&#xA0; &#xA0; array calculateBankHolidays(int $yr)
 *
 *&#xA0; &#xA0; ARGUMENTS
 *
 *&#xA0; &#xA0; $yr = 4 digit numeric representation of the year (eg 1997).
 *
 *&#xA0; &#xA0; RETURN VALUE
 *
 *&#xA0; &#xA0; Returns an array of strings where each string is a date of a bank holiday in the format &quot;yyyy-mm-dd&quot;.
 *
 *&#xA0; &#xA0; See example below
 *
 */

function calculateBankHolidays($yr) {

&#xA0; &#xA0; $bankHols = Array();

&#xA0; &#xA0; // New year&apos;s:
&#xA0; &#xA0; switch ( date(&quot;w&quot;, strtotime(&quot;$yr-01-01 12:00:00&quot;)) ) {
&#xA0; &#xA0; &#xA0; &#xA0; case 6:
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $bankHols[] = &quot;$yr-01-03&quot;;
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; break;
&#xA0; &#xA0; &#xA0; &#xA0; case 0:
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $bankHols[] = &quot;$yr-01-02&quot;;
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; break;
&#xA0; &#xA0; &#xA0; &#xA0; default:
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $bankHols[] = &quot;$yr-01-01&quot;;
&#xA0; &#xA0; }

&#xA0; &#xA0; // Good friday:
&#xA0; &#xA0; $bankHols[] = date(&quot;Y-m-d&quot;, strtotime( &quot;+&quot;.(easter_days($yr) - 2).&quot; days&quot;, strtotime(&quot;$yr-03-21 12:00:00&quot;) ));

&#xA0; &#xA0; // Easter Monday:
&#xA0; &#xA0; $bankHols[] = date(&quot;Y-m-d&quot;, strtotime( &quot;+&quot;.(easter_days($yr) + 1).&quot; days&quot;, strtotime(&quot;$yr-03-21 12:00:00&quot;) ));

&#xA0; &#xA0; // May Day:
&#xA0; &#xA0; if ($yr == 1995) {
&#xA0; &#xA0; &#xA0; &#xA0; $bankHols[] = &quot;1995-05-08&quot;; // VE day 50th anniversary year exception
&#xA0; &#xA0; } else {
&#xA0; &#xA0; &#xA0; &#xA0; switch (date(&quot;w&quot;, strtotime(&quot;$yr-05-01 12:00:00&quot;))) {
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; case 0:
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $bankHols[] = &quot;$yr-05-02&quot;;
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; break;
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; case 1:
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $bankHols[] = &quot;$yr-05-01&quot;;
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; break;
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; case 2:
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $bankHols[] = &quot;$yr-05-07&quot;;
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; break;
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; case 3:
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $bankHols[] = &quot;$yr-05-06&quot;;
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; break;
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; case 4:
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $bankHols[] = &quot;$yr-05-05&quot;;
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; break;
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; case 5:
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $bankHols[] = &quot;$yr-05-04&quot;;
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; break;
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; case 6:
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $bankHols[] = &quot;$yr-05-03&quot;;
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; break;
&#xA0; &#xA0; &#xA0; &#xA0; }
&#xA0; &#xA0; }

&#xA0; &#xA0; // Whitsun:
&#xA0; &#xA0; if ($yr == 2002) { // exception year
&#xA0; &#xA0; &#xA0; &#xA0; $bankHols[] = &quot;2002-06-03&quot;;
&#xA0; &#xA0; &#xA0; &#xA0; $bankHols[] = &quot;2002-06-04&quot;;
&#xA0; &#xA0; } else {
&#xA0; &#xA0; &#xA0; &#xA0; switch (date(&quot;w&quot;, strtotime(&quot;$yr-05-31 12:00:00&quot;))) {
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; case 0:
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $bankHols[] = &quot;$yr-05-25&quot;;
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; break;
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; case 1:
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $bankHols[] = &quot;$yr-05-31&quot;;
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; break;
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; case 2:
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $bankHols[] = &quot;$yr-05-30&quot;;
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; break;
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; case 3:
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $bankHols[] = &quot;$yr-05-29&quot;;
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; break;
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; case 4:
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $bankHols[] = &quot;$yr-05-28&quot;;
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; break;
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; case 5:
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $bankHols[] = &quot;$yr-05-27&quot;;
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; break;
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; case 6:
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $bankHols[] = &quot;$yr-05-26&quot;;
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; break;
&#xA0; &#xA0; &#xA0; &#xA0; }
&#xA0; &#xA0; }

&#xA0; &#xA0; // Summer Bank Holiday:
&#xA0; &#xA0; switch (date(&quot;w&quot;, strtotime(&quot;$yr-08-31 12:00:00&quot;))) {
&#xA0; &#xA0; &#xA0; &#xA0; case 0:
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $bankHols[] = &quot;$yr-08-25&quot;;
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; break;
&#xA0; &#xA0; &#xA0; &#xA0; case 1:
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $bankHols[] = &quot;$yr-08-31&quot;;
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; break;
&#xA0; &#xA0; &#xA0; &#xA0; case 2:
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $bankHols[] = &quot;$yr-08-30&quot;;
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; break;
&#xA0; &#xA0; &#xA0; &#xA0; case 3:
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $bankHols[] = &quot;$yr-08-29&quot;;
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; break;
&#xA0; &#xA0; &#xA0; &#xA0; case 4:
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $bankHols[] = &quot;$yr-08-28&quot;;
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; break;
&#xA0; &#xA0; &#xA0; &#xA0; case 5:
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $bankHols[] = &quot;$yr-08-27&quot;;
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; break;
&#xA0; &#xA0; &#xA0; &#xA0; case 6:
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $bankHols[] = &quot;$yr-08-26&quot;;
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; break;
&#xA0; &#xA0; }

&#xA0; &#xA0; // Christmas:
&#xA0; &#xA0; switch ( date(&quot;w&quot;, strtotime(&quot;$yr-12-25 12:00:00&quot;)) ) {
&#xA0; &#xA0; &#xA0; &#xA0; case 5:
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $bankHols[] = &quot;$yr-12-25&quot;;
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $bankHols[] = &quot;$yr-12-28&quot;;
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; break;
&#xA0; &#xA0; &#xA0; &#xA0; case 6:
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $bankHols[] = &quot;$yr-12-27&quot;;
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $bankHols[] = &quot;$yr-12-28&quot;;
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; break;
&#xA0; &#xA0; &#xA0; &#xA0; case 0:
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $bankHols[] = &quot;$yr-12-26&quot;;
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $bankHols[] = &quot;$yr-12-27&quot;;
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; break;
&#xA0; &#xA0; &#xA0; &#xA0; default:
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $bankHols[] = &quot;$yr-12-25&quot;;
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $bankHols[] = &quot;$yr-12-26&quot;;
&#xA0; &#xA0; }

&#xA0; &#xA0; // Millenium eve
&#xA0; &#xA0; if ($yr == 1999) {
&#xA0; &#xA0; &#xA0; &#xA0; $bankHols[] = &quot;1999-12-31&quot;;
&#xA0; &#xA0; }

&#xA0; &#xA0; return $bankHols;

}

/*
 *&#xA0; &#xA0; EXAMPLE:
 *
 */

header(&quot;Content-type: text/plain&quot;); 

$bankHolsThisYear = calculateBankHolidays(2007);

print_r($bankHolsThisYear);

?>
```


Will output this result:

Array
(
&#xA0; &#xA0; [0] =&gt; 2007-01-01
&#xA0; &#xA0; [1] =&gt; 2007-04-06
&#xA0; &#xA0; [2] =&gt; 2007-04-09
&#xA0; &#xA0; [3] =&gt; 2007-05-07
&#xA0; &#xA0; [4] =&gt; 2007-05-28
&#xA0; &#xA0; [5] =&gt; 2007-08-27
&#xA0; &#xA0; [6] =&gt; 2007-12-25
&#xA0; &#xA0; [7] =&gt; 2007-12-26
)

  

#

[Official documentation page](https://www.php.net/manual/en/ref.calendar.php)

**[To root](/README.md)**