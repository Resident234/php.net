# Calendar Functions



I created this function a while ago and needed it again recently, so had to trawl through some old files to find it. Thought I&apos;d post it here in case anyone else finds it useful.<br><br>

```
<?php

/*
 *    Function to calculate which days are British bank holidays (England &amp; Wales) for a given year.
 *
 *    Created by David Scourfield, 07 August 2006, and released into the public domain.
 *    Anybody may use and/or modify this code.
 *
 *    USAGE:
 *
 *    array calculateBankHolidays(int $yr)
 *
 *    ARGUMENTS
 *
 *    $yr = 4 digit numeric representation of the year (eg 1997).
 *
 *    RETURN VALUE
 *
 *    Returns an array of strings where each string is a date of a bank holiday in the format "yyyy-mm-dd".
 *
 *    See example below
 *
 */

function calculateBankHolidays($yr) {

    $bankHols = Array();

    // New year's:
    switch ( date("w", strtotime("$yr-01-01 12:00:00")) ) {
        case 6:
            $bankHols[] = "$yr-01-03";
            break;
        case 0:
            $bankHols[] = "$yr-01-02";
            break;
        default:
            $bankHols[] = "$yr-01-01";
    }

    // Good friday:
    $bankHols[] = date("Y-m-d", strtotime( "+".(easter_days($yr) - 2)." days", strtotime("$yr-03-21 12:00:00") ));

    // Easter Monday:
    $bankHols[] = date("Y-m-d", strtotime( "+".(easter_days($yr) + 1)." days", strtotime("$yr-03-21 12:00:00") ));

    // May Day:
    if ($yr == 1995) {
        $bankHols[] = "1995-05-08"; // VE day 50th anniversary year exception
    } else {
        switch (date("w", strtotime("$yr-05-01 12:00:00"))) {
            case 0:
                $bankHols[] = "$yr-05-02";
                break;
            case 1:
                $bankHols[] = "$yr-05-01";
                break;
            case 2:
                $bankHols[] = "$yr-05-07";
                break;
            case 3:
                $bankHols[] = "$yr-05-06";
                break;
            case 4:
                $bankHols[] = "$yr-05-05";
                break;
            case 5:
                $bankHols[] = "$yr-05-04";
                break;
            case 6:
                $bankHols[] = "$yr-05-03";
                break;
        }
    }

    // Whitsun:
    if ($yr == 2002) { // exception year
        $bankHols[] = "2002-06-03";
        $bankHols[] = "2002-06-04";
    } else {
        switch (date("w", strtotime("$yr-05-31 12:00:00"))) {
            case 0:
                $bankHols[] = "$yr-05-25";
                break;
            case 1:
                $bankHols[] = "$yr-05-31";
                break;
            case 2:
                $bankHols[] = "$yr-05-30";
                break;
            case 3:
                $bankHols[] = "$yr-05-29";
                break;
            case 4:
                $bankHols[] = "$yr-05-28";
                break;
            case 5:
                $bankHols[] = "$yr-05-27";
                break;
            case 6:
                $bankHols[] = "$yr-05-26";
                break;
        }
    }

    // Summer Bank Holiday:
    switch (date("w", strtotime("$yr-08-31 12:00:00"))) {
        case 0:
            $bankHols[] = "$yr-08-25";
            break;
        case 1:
            $bankHols[] = "$yr-08-31";
            break;
        case 2:
            $bankHols[] = "$yr-08-30";
            break;
        case 3:
            $bankHols[] = "$yr-08-29";
            break;
        case 4:
            $bankHols[] = "$yr-08-28";
            break;
        case 5:
            $bankHols[] = "$yr-08-27";
            break;
        case 6:
            $bankHols[] = "$yr-08-26";
            break;
    }

    // Christmas:
    switch ( date("w", strtotime("$yr-12-25 12:00:00")) ) {
        case 5:
            $bankHols[] = "$yr-12-25";
            $bankHols[] = "$yr-12-28";
            break;
        case 6:
            $bankHols[] = "$yr-12-27";
            $bankHols[] = "$yr-12-28";
            break;
        case 0:
            $bankHols[] = "$yr-12-26";
            $bankHols[] = "$yr-12-27";
            break;
        default:
            $bankHols[] = "$yr-12-25";
            $bankHols[] = "$yr-12-26";
    }

    // Millenium eve
    if ($yr == 1999) {
        $bankHols[] = "1999-12-31";
    }

    return $bankHols;

}

/*
 *    EXAMPLE:
 *
 */

header("Content-type: text/plain"); 

$bankHolsThisYear = calculateBankHolidays(2007);

print_r($bankHolsThisYear);

?>
```
<br><br>Will output this result:<br><br>Array<br>(<br>    [0] =&gt; 2007-01-01<br>    [1] =&gt; 2007-04-06<br>    [2] =&gt; 2007-04-09<br>    [3] =&gt; 2007-05-07<br>    [4] =&gt; 2007-05-28<br>    [5] =&gt; 2007-08-27<br>    [6] =&gt; 2007-12-25<br>    [7] =&gt; 2007-12-26<br>)  

---

[Official documentation page](https://www.php.net/manual/en/ref.calendar.php)

**[To root](/README.md)**