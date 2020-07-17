# str_ireplace



Here&apos;s a different approach to search result keyword highlighting that will match all keyword sub strings in a case insensitive manner and preserve case in the returned text. This solution first grabs all matches within $haystack in a case insensitive manner, and the secondly loops through each of those matched sub strings and applies a case sensitive replace in $haystack. This way each unique (in terms of case) instance of $needle is operated on individually allowing a case sensitive replace to be done in order to preserve the original case of each unique instance of $needle.<br><br>

```
<?php
function highlightStr($haystack, $needle, $highlightColorValue) {
     // return $haystack if there is no highlight color or strings given, nothing to do.
    if (strlen($highlightColorValue) < 1 || strlen($haystack) < 1 || strlen($needle) < 1) {
        return $haystack;
    }
    preg_match_all("/$needle+/i", $haystack, $matches);
    if (is_array($matches[0]) &amp;&amp; count($matches[0]) >= 1) {
        foreach ($matches[0] as $match) {
            $haystack = str_replace($match, '<span style="background-color:'.$highlightColorValue.';">'.$match.'</span>', $haystack);
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
    $query = preg_replace("/['\"]([^'\"]*)['\"]/i", "'<FONT COLOR='#FF6600'>$1</FONT>'", $query, -1);

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
                                    "<FONT COLOR='#FF6600'><B>*</B></FONT>",
                                    "<FONT COLOR='#00AA00'><B>SELECT</B> </FONT>",
                                    "<FONT COLOR='#00AA00'><B>UPDATE</B> </FONT>",
                                    "<FONT COLOR='#00AA00'><B>DELETE</B> </FONT>",
                                    "<FONT COLOR='#00AA00'><B>INSERT</B> </FONT>",
                                    "<FONT COLOR='#00AA00'><B>INTO</B></FONT>",
                                    "<FONT COLOR='#00AA00'><B>VALUES</B></FONT>",
                                    "<FONT COLOR='#00AA00'><B>FROM</B></FONT>",
                                    "<FONT COLOR='#00CC00'><B>LEFT</B></FONT>",
                                    "<FONT COLOR='#00CC00'><B>JOIN</B></FONT>",
                                    "<FONT COLOR='#00AA00'><B>WHERE</B></FONT>",
                                    "<FONT COLOR='#AA0000'><B>LIMIT</B></FONT>",
                                    "<FONT COLOR='#00AA00'><B>ORDER BY</B></FONT>",
                                    "<FONT COLOR='#0000AA'><B>AND</B></FONT>",
                                    "<FONT COLOR='#0000AA'><B>OR</B> </FONT>",
                                    "<FONT COLOR='#0000AA'><B>DESC</B></FONT>",
                                    "<FONT COLOR='#0000AA'><B>ASC</B></FONT>",
                                    "<FONT COLOR='#00DD00'><B>ON</B> </FONT>"
                                  ),
                            $query
                          );

    echo "<FONT COLOR='#0000FF'><B>SQL[".$SQL_INT."]:</B> ".$query."<FONT COLOR='#FF0000'>;</FONT></FONT><BR>\n";

    $SQL_INT++;

} //SQL_DEBUG
?>
```
  

#

[Official documentation page](https://www.php.net/manual/en/function.str-ireplace.php)

**[To root](/README.md)**