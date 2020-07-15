# str_ireplace



Here&apos;s a different approach to search result keyword highlighting that will match all keyword sub strings in a case insensitive manner and preserve case in the returned text. This solution first grabs all matches within $haystack in a case insensitive manner, and the secondly loops through each of those matched sub strings and applies a case sensitive replace in $haystack. This way each unique (in terms of case) instance of $needle is operated on individually allowing a case sensitive replace to be done in order to preserve the original case of each unique instance of $needle.<br><br>

```
<?php
function highlightStr($haystack, $needle, $highlightColorValue) {
     // return $haystack if there is no highlight color or strings given, nothing to do.
    if (strlen($highlightColorValue) &lt; 1 || strlen($haystack) &lt; 1 || strlen($needle) &lt; 1) {
        return $haystack;
    }
    preg_match_all("/$needle+/i", $haystack, $matches);
    if (is_array($matches[0]) &amp;&amp; count($matches[0]) &gt;= 1) {
        foreach ($matches[0] as $match) {
            $haystack = str_replace($match, '&lt;span style="background-color:'.$highlightColorValue.';"&gt;'.$match.'&lt;/span&gt;', $haystack);
        }
    }
    return $haystack;
}
?>
```
  

#

here&apos;s a neat little function I whipped up to do HTML color coding of SQL strings. <br><br>

```
<?php
/**
 * Output the HTML debugging string in color coded glory for a sql query
 * This is very nice for being able to see many SQL queries
 * @access     public
 * @return     void. prints HTML color coded string of the input $query.
 * @param     string $query The SQL query to be executed.
 * @author     Daevid Vincent [daevid@LockdownNetworks.com]
 *  @version     1.0
 * @date        04/05/05
 * @todo     highlight SQL functions.
 */
function SQL_DEBUG( $query )
{
    if( $query == '' ) return 0;

    global $SQL_INT;
    if( !isset($SQL_INT) ) $SQL_INT = 0;

    //[dv] this has to come first or you will have goofy results later.
    $query = preg_replace("/['\"]([^'\"]*)['\"]/i", "'&lt;FONT COLOR='#FF6600'&gt;$1&lt;/FONT&gt;'", $query, -1);

    $query = str_ireplace(
                            array (
                                    '*',
                                    'SELECT ',
                                    'UPDATE ',
                                    'DELETE ',
                                    'INSERT ',
                                    'INTO',
                                    'VALUES',
                                    'FROM',
                                    'LEFT',
                                    'JOIN',
                                    'WHERE',
                                    'LIMIT',
                                    'ORDER BY',
                                    'AND',
                                    'OR ', //[dv] note the space. otherwise you match to 'COLOR' ;-)
                                    'DESC',
                                    'ASC',
                                    'ON '
                                  ),
                            array (
                                    "&lt;FONT COLOR='#FF6600'&gt;&lt;B&gt;*&lt;/B&gt;&lt;/FONT&gt;",
                                    "&lt;FONT COLOR='#00AA00'&gt;&lt;B&gt;SELECT&lt;/B&gt; &lt;/FONT&gt;",
                                    "&lt;FONT COLOR='#00AA00'&gt;&lt;B&gt;UPDATE&lt;/B&gt; &lt;/FONT&gt;",
                                    "&lt;FONT COLOR='#00AA00'&gt;&lt;B&gt;DELETE&lt;/B&gt; &lt;/FONT&gt;",
                                    "&lt;FONT COLOR='#00AA00'&gt;&lt;B&gt;INSERT&lt;/B&gt; &lt;/FONT&gt;",
                                    "&lt;FONT COLOR='#00AA00'&gt;&lt;B&gt;INTO&lt;/B&gt;&lt;/FONT&gt;",
                                    "&lt;FONT COLOR='#00AA00'&gt;&lt;B&gt;VALUES&lt;/B&gt;&lt;/FONT&gt;",
                                    "&lt;FONT COLOR='#00AA00'&gt;&lt;B&gt;FROM&lt;/B&gt;&lt;/FONT&gt;",
                                    "&lt;FONT COLOR='#00CC00'&gt;&lt;B&gt;LEFT&lt;/B&gt;&lt;/FONT&gt;",
                                    "&lt;FONT COLOR='#00CC00'&gt;&lt;B&gt;JOIN&lt;/B&gt;&lt;/FONT&gt;",
                                    "&lt;FONT COLOR='#00AA00'&gt;&lt;B&gt;WHERE&lt;/B&gt;&lt;/FONT&gt;",
                                    "&lt;FONT COLOR='#AA0000'&gt;&lt;B&gt;LIMIT&lt;/B&gt;&lt;/FONT&gt;",
                                    "&lt;FONT COLOR='#00AA00'&gt;&lt;B&gt;ORDER BY&lt;/B&gt;&lt;/FONT&gt;",
                                    "&lt;FONT COLOR='#0000AA'&gt;&lt;B&gt;AND&lt;/B&gt;&lt;/FONT&gt;",
                                    "&lt;FONT COLOR='#0000AA'&gt;&lt;B&gt;OR&lt;/B&gt; &lt;/FONT&gt;",
                                    "&lt;FONT COLOR='#0000AA'&gt;&lt;B&gt;DESC&lt;/B&gt;&lt;/FONT&gt;",
                                    "&lt;FONT COLOR='#0000AA'&gt;&lt;B&gt;ASC&lt;/B&gt;&lt;/FONT&gt;",
                                    "&lt;FONT COLOR='#00DD00'&gt;&lt;B&gt;ON&lt;/B&gt; &lt;/FONT&gt;"
                                  ),
                            $query
                          );

    echo "&lt;FONT COLOR='#0000FF'&gt;&lt;B&gt;SQL[".$SQL_INT."]:&lt;/B&gt; ".$query."&lt;FONT COLOR='#FF0000'&gt;;&lt;/FONT&gt;&lt;/FONT&gt;&lt;BR&gt;\n";

    $SQL_INT++;

} //SQL_DEBUG
?>
```
  

#

[Official documentation page](https://www.php.net/manual/en/function.str-ireplace.php)

**[To root](/README.md)**