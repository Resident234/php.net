# mysqli_stmt::get_result



I went through a lot of trouble on a server where mysqlnd wasn&apos;t available, and had a lot of headaches.<br><br>If you don&apos;t have mysqlnd installed/loaded whatever, you will get an undefined reference when trying to call "mysqli_stmt_get_result()".<br><br>I wrote my own mysqli_stmt_get_result() and a mysqli_result_fetch_array() to go with it.<br><br>

```
<?php
class iimysqli_result
{
    public $stmt, $nCols;
}    

function iimysqli_stmt_get_result($stmt)
{
    /**    EXPLANATION:
     * We are creating a fake "result" structure to enable us to have
     * source-level equivalent syntax to a query executed via
     * mysqli_query().
     *
     *    $stmt = mysqli_prepare($conn, "");
     *    mysqli_bind_param($stmt, "types", ...);
     *
     *    $param1 = 0;
     *    $param2 = 'foo';
     *    $param3 = 'bar';
     *    mysqli_execute($stmt);
     *    $result _mysqli_stmt_get_result($stmt);
     *        [ $arr = _mysqli_result_fetch_array($result);
     *            || $assoc = _mysqli_result_fetch_assoc($result); ]
     *    mysqli_stmt_close($stmt);
     *    mysqli_close($conn);
     *
     * At the source level, there is no difference between this and mysqlnd.
     **/
    $metadata = mysqli_stmt_result_metadata($stmt);
    $ret = new iimysqli_result;
    if (!$ret) return NULL;

    $ret->nCols = mysqli_num_fields($metadata);
    $ret->stmt = $stmt;

    mysqli_free_result($metadata);
    return $ret;
}

function iimysqli_result_fetch_array(&amp;$result)
{
    $ret = array();
    $code = "return mysqli_stmt_bind_result(\$result->stmt ";

    for ($i=0; $i&lt;$result->nCols; $i++)
    {
        $ret[$i] = NULL;
        $code .= ", \$ret['" .$i ."']";
    };

    $code .= ");";
    if (!eval($code)) { return NULL; };

    // This should advance the "$stmt" cursor.
    if (!mysqli_stmt_fetch($result->stmt)) { return NULL; };

    // Return the array we built.
    return $ret;
}
?>
```
<br><br>Hope this helps someone.  

#

Please OH PLEASE.<br>I have been trying to get a result set from this function, and I had 0 luck completely, for nearly 3 hours!<br><br>If you ARE using mysqli_stmt_get_results() to get a result set, in conjuction with mysqli_stmt_store_results in order to retrieve the number of rows returned, you are going to have some major trouble!<br><br>PHP Documentation states that to retrieve the number of rows returned by a prepared select sql statement, one should call the following statements respectively:<br><br>mysqli_stmt_execute($stmt);<br>mysqli_stmt_store_result($stmt);<br>$num_rows = mysqli_stmt_num_rows($stmt);<br><br>THIS IS A MAJOR DEATH TRAP, IF YOU ARE USING mysqli_stmt_get_result() in conjunction!!!! Results of doing so vary depending which statements you call first, but in the end, you will NOT get the desired result.<br><br>In conclusion, please, PLEASE, NEVER use mysqli_stmt_store_result(), then mysqli_ AND mysqli_stmt_get_result() at the the same time. This is a MAJOR death trap.<br><br>SOLUTION:<br>If you are trying to get a result set, and you need the number of rows returned at the same time, use the following statements respectively instead:<br><br>$result_set = mysqli_stmt_get_results($stmt);<br>$num_rows = mysqli_num_rows($result_set);<br><br>Reflecting on my actions, this solution may seem fairly obvious. However, to someone new using PHP (like me) or someone who is not fully comfortable with prepared statements, it&apos;s very easy to get lost by using Google and learn on your own.<br><br>Summary:<br>NEVER use mysqli_stmt_store_result($stmt) &amp; mysqli_stmt_num_rows($stmt) in conjunction with mysqli_stmt_get_result($stmt). You will regret it!! I have been stuck on this for hours, and Google offered me no answer!  

#

Please note that this method requires the mysqlnd driver. Othervise you will get this error: Call to undefined method mysqli_stmt::get_result()  

#

[Official documentation page](https://www.php.net/manual/en/mysqli-stmt.get-result.php)

**[To root](/README.md)**