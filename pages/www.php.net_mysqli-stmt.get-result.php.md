# mysqli_stmt::get_result





I went through a lot of trouble on a server where mysqlnd wasn&apos;t available, and had a lot of headaches.

If you don&apos;t have mysqlnd installed/loaded whatever, you will get an undefined reference when trying to call &quot;mysqli_stmt_get_result()&quot;.

I wrote my own mysqli_stmt_get_result() and a mysqli_result_fetch_array() to go with it.



```
<?php
class iimysqli_result
{
&#xA0; &#xA0; public $stmt, $nCols;
}&#xA0; &#xA0; 

function iimysqli_stmt_get_result($stmt)
{
&#xA0; &#xA0; /**&#xA0; &#xA0; EXPLANATION:
&#xA0; &#xA0;&#xA0; * We are creating a fake &quot;result&quot; structure to enable us to have
&#xA0; &#xA0;&#xA0; * source-level equivalent syntax to a query executed via
&#xA0; &#xA0;&#xA0; * mysqli_query().
&#xA0; &#xA0;&#xA0; *
&#xA0; &#xA0;&#xA0; *&#xA0; &#xA0; $stmt = mysqli_prepare($conn, &quot;&quot;);
&#xA0; &#xA0;&#xA0; *&#xA0; &#xA0; mysqli_bind_param($stmt, &quot;types&quot;, ...);
&#xA0; &#xA0;&#xA0; *
&#xA0; &#xA0;&#xA0; *&#xA0; &#xA0; $param1 = 0;
&#xA0; &#xA0;&#xA0; *&#xA0; &#xA0; $param2 = &apos;foo&apos;;
&#xA0; &#xA0;&#xA0; *&#xA0; &#xA0; $param3 = &apos;bar&apos;;
&#xA0; &#xA0;&#xA0; *&#xA0; &#xA0; mysqli_execute($stmt);
&#xA0; &#xA0;&#xA0; *&#xA0; &#xA0; $result _mysqli_stmt_get_result($stmt);
&#xA0; &#xA0;&#xA0; *&#xA0; &#xA0; &#xA0; &#xA0; [ $arr = _mysqli_result_fetch_array($result);
&#xA0; &#xA0;&#xA0; *&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; || $assoc = _mysqli_result_fetch_assoc($result); ]
&#xA0; &#xA0;&#xA0; *&#xA0; &#xA0; mysqli_stmt_close($stmt);
&#xA0; &#xA0;&#xA0; *&#xA0; &#xA0; mysqli_close($conn);
&#xA0; &#xA0;&#xA0; *
&#xA0; &#xA0;&#xA0; * At the source level, there is no difference between this and mysqlnd.
&#xA0; &#xA0;&#xA0; **/
&#xA0; &#xA0; $metadata = mysqli_stmt_result_metadata($stmt);
&#xA0; &#xA0; $ret = new iimysqli_result;
&#xA0; &#xA0; if (!$ret) return NULL;

&#xA0; &#xA0; $ret-&gt;nCols = mysqli_num_fields($metadata);
&#xA0; &#xA0; $ret-&gt;stmt = $stmt;

&#xA0; &#xA0; mysqli_free_result($metadata);
&#xA0; &#xA0; return $ret;
}

function iimysqli_result_fetch_array(&amp;$result)
{
&#xA0; &#xA0; $ret = array();
&#xA0; &#xA0; $code = &quot;return mysqli_stmt_bind_result(\$result-&gt;stmt &quot;;

&#xA0; &#xA0; for ($i=0; $i&lt;$result-&gt;nCols; $i++)
&#xA0; &#xA0; {
&#xA0; &#xA0; &#xA0; &#xA0; $ret[$i] = NULL;
&#xA0; &#xA0; &#xA0; &#xA0; $code .= &quot;, \$ret[&apos;&quot; .$i .&quot;&apos;]&quot;;
&#xA0; &#xA0; };

&#xA0; &#xA0; $code .= &quot;);&quot;;
&#xA0; &#xA0; if (!eval($code)) { return NULL; };

&#xA0; &#xA0; // This should advance the &quot;$stmt&quot; cursor.
&#xA0; &#xA0; if (!mysqli_stmt_fetch($result-&gt;stmt)) { return NULL; };

&#xA0; &#xA0; // Return the array we built.
&#xA0; &#xA0; return $ret;
}
?>
```


Hope this helps someone.

  

#



Please OH PLEASE.
I have been trying to get a result set from this function, and I had 0 luck completely, for nearly 3 hours!

If you ARE using mysqli_stmt_get_results() to get a result set, in conjuction with mysqli_stmt_store_results in order to retrieve the number of rows returned, you are going to have some major trouble!

PHP Documentation states that to retrieve the number of rows returned by a prepared select sql statement, one should call the following statements respectively:

mysqli_stmt_execute($stmt);
mysqli_stmt_store_result($stmt);
$num_rows = mysqli_stmt_num_rows($stmt);

THIS IS A MAJOR DEATH TRAP, IF YOU ARE USING mysqli_stmt_get_result() in conjunction!!!! Results of doing so vary depending which statements you call first, but in the end, you will NOT get the desired result.

In conclusion, please, PLEASE, NEVER use mysqli_stmt_store_result(), then mysqli_ AND mysqli_stmt_get_result() at the the same time. This is a MAJOR death trap.

SOLUTION:
If you are trying to get a result set, and you need the number of rows returned at the same time, use the following statements respectively instead:

$result_set = mysqli_stmt_get_results($stmt);
$num_rows = mysqli_num_rows($result_set);

Reflecting on my actions, this solution may seem fairly obvious. However, to someone new using PHP (like me) or someone who is not fully comfortable with prepared statements, it&apos;s very easy to get lost by using Google and learn on your own.

Summary:
NEVER use mysqli_stmt_store_result($stmt) &amp; mysqli_stmt_num_rows($stmt) in conjunction with mysqli_stmt_get_result($stmt). You will regret it!! I have been stuck on this for hours, and Google offered me no answer!

  

#



Please note that this method requires the mysqlnd driver. Othervise you will get this error: Call to undefined method mysqli_stmt::get_result()

  

#

[Official documentation page](https://www.php.net/manual/en/mysqli-stmt.get-result.php)

**[To root](/README.md)**