# mysqli::querymysqli_query




<div class="phpcode"><span class="html">
This may or may not be obvious to people but perhaps it will help someone.
<br>
<br>When running joins in SQL you may encounter a problem if you are trying to pull two columns with the same name. mysqli returns the last in the query when called by name. So to get what you need you can use an alias.
<br>
<br>Below I am trying to join a user id with a user role. in the first table (tbl_usr), role is a number and in the second is a&#xA0; text name (tbl_memrole is a lookup table). If I call them both as role I get the text as it is the last &quot;role&quot; in the query. If I use an alias then I get both as desired as shown below.
<br>
<br><span class="default">&lt;?php
<br>$sql </span><span class="keyword">= </span><span class="string">&quot;SELECT a.uid, a.role AS roleid, b.role, 
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; FROM tbl_usr a
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; INNER JOIN tbl_memrole b
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; ON a.role = b.id
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &quot;</span><span class="keyword">;
<br>
<br>&#xA0; &#xA0; if (</span><span class="default">$result </span><span class="keyword">= </span><span class="default">$mysqli</span><span class="keyword">-&gt;</span><span class="default">query</span><span class="keyword">(</span><span class="default">$sql</span><span class="keyword">)) {
<br>&#xA0; &#xA0; &#xA0; &#xA0; while(</span><span class="default">$obj </span><span class="keyword">= </span><span class="default">$result</span><span class="keyword">-&gt;</span><span class="default">fetch_object</span><span class="keyword">()){
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$line</span><span class="keyword">.=</span><span class="default">$obj</span><span class="keyword">-&gt;</span><span class="default">uid</span><span class="keyword">;
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$line</span><span class="keyword">.=</span><span class="default">$obj</span><span class="keyword">-&gt;</span><span class="default">role</span><span class="keyword">;
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$line</span><span class="keyword">.=</span><span class="default">$obj</span><span class="keyword">-&gt;</span><span class="default">roleid</span><span class="keyword">;
<br>&#xA0; &#xA0; &#xA0; &#xA0; }
<br>&#xA0; &#xA0; }
<br>&#xA0; &#xA0; </span><span class="default">$result</span><span class="keyword">-&gt;</span><span class="default">close</span><span class="keyword">();
<br>&#xA0; &#xA0; unset(</span><span class="default">$obj</span><span class="keyword">);
<br>&#xA0; &#xA0; unset(</span><span class="default">$sql</span><span class="keyword">);
<br>&#xA0; &#xA0; unset(</span><span class="default">$query</span><span class="keyword">);
<br>&#xA0; &#xA0; 
<br></span><span class="default">?&gt;
<br></span>In this situation I guess I could have just renamed the role column in the first table roleid and that would have taken care of it, but it was a learning experience.</span>
</div>
  

#


<div class="phpcode"><span class="html">
When calling multiple stored procedures, you can run into the following error: &quot;Commands out of sync; you can&apos;t run this command now&quot;.<br>This can happen even when using the close() function on the result object between calls. <br>To fix the problem, remember to call the next_result() function on the mysqli object after each stored procedure call. See example below:<br><br><span class="default">&lt;?php<br></span><span class="comment">// New Connection<br></span><span class="default">$db </span><span class="keyword">= new </span><span class="default">mysqli</span><span class="keyword">(</span><span class="string">&apos;localhost&apos;</span><span class="keyword">,</span><span class="string">&apos;user&apos;</span><span class="keyword">,</span><span class="string">&apos;pass&apos;</span><span class="keyword">,</span><span class="string">&apos;database&apos;</span><span class="keyword">);<br><br></span><span class="comment">// Check for errors<br></span><span class="keyword">if(</span><span class="default">mysqli_connect_errno</span><span class="keyword">()){<br> echo </span><span class="default">mysqli_connect_error</span><span class="keyword">();<br>}<br><br></span><span class="comment">// 1st Query<br></span><span class="default">$result </span><span class="keyword">= </span><span class="default">$db</span><span class="keyword">-&gt;</span><span class="default">query</span><span class="keyword">(</span><span class="string">&quot;call getUsers()&quot;</span><span class="keyword">);<br>if(</span><span class="default">$result</span><span class="keyword">){<br>&#xA0; &#xA0;&#xA0; </span><span class="comment">// Cycle through results<br>&#xA0; &#xA0; </span><span class="keyword">while (</span><span class="default">$row </span><span class="keyword">= </span><span class="default">$result</span><span class="keyword">-&gt;</span><span class="default">fetch_object</span><span class="keyword">()){<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$user_arr</span><span class="keyword">[] = </span><span class="default">$row</span><span class="keyword">;<br>&#xA0; &#xA0; }<br>&#xA0; &#xA0; </span><span class="comment">// Free result set<br>&#xA0; &#xA0; </span><span class="default">$result</span><span class="keyword">-&gt;</span><span class="default">close</span><span class="keyword">();<br>&#xA0; &#xA0; </span><span class="default">$db</span><span class="keyword">-&gt;</span><span class="default">next_result</span><span class="keyword">();<br>}<br><br></span><span class="comment">// 2nd Query<br></span><span class="default">$result </span><span class="keyword">= </span><span class="default">$db</span><span class="keyword">-&gt;</span><span class="default">query</span><span class="keyword">(</span><span class="string">&quot;call getGroups()&quot;</span><span class="keyword">);<br>if(</span><span class="default">$result</span><span class="keyword">){<br>&#xA0; &#xA0;&#xA0; </span><span class="comment">// Cycle through results<br>&#xA0; &#xA0; </span><span class="keyword">while (</span><span class="default">$row </span><span class="keyword">= </span><span class="default">$result</span><span class="keyword">-&gt;</span><span class="default">fetch_object</span><span class="keyword">()){<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$group_arr</span><span class="keyword">[] = </span><span class="default">$row</span><span class="keyword">;<br>&#xA0; &#xA0; }<br>&#xA0; &#xA0;&#xA0; </span><span class="comment">// Free result set<br>&#xA0; &#xA0;&#xA0; </span><span class="default">$result</span><span class="keyword">-&gt;</span><span class="default">close</span><span class="keyword">();<br>&#xA0; &#xA0;&#xA0; </span><span class="default">$db</span><span class="keyword">-&gt;</span><span class="default">next_result</span><span class="keyword">();<br>}<br>else echo(</span><span class="default">$db</span><span class="keyword">-&gt;</span><span class="default">error</span><span class="keyword">);<br><br></span><span class="comment">// Close connection<br></span><span class="default">$db</span><span class="keyword">-&gt;</span><span class="default">close</span><span class="keyword">();<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#


<div class="phpcode"><span class="html">
The cryptic &quot;Couldn&apos;t fetch mysqli&quot; error message can mean any number of things, including:<br><br>1. You&apos;re trying to use a database object that you&apos;ve already closed (as noted by ceo at l-i-e dot com). Reopen your database connection, or find the call to <span class="default">&lt;?php mysqli_close</span><span class="keyword">(</span><span class="default">$db</span><span class="keyword">); </span><span class="default">?&gt;</span> or <span class="default">&lt;?php $db</span><span class="keyword">-&gt;</span><span class="default">close</span><span class="keyword">(); </span><span class="default">?&gt;</span> and remove it.<br>2. Your MySQLi object has been serialized and unserialized for some reason. Define a wakeup function to re-create your database connection. <a href="http://php.net/__wakeup" rel="nofollow" target="_blank">http://php.net/__wakeup</a> <br>3. Something besides you closed your mysqli connection (in particular, see <a href="http://bugs.php.net/bug.php?id=33772" rel="nofollow" target="_blank">http://bugs.php.net/bug.php?id=33772</a>)<br>4. You mixed OOP and functional calls to the database object. (So, you have <span class="default">&lt;?php $db</span><span class="keyword">-&gt;</span><span class="default">query</span><span class="keyword">() </span><span class="default">?&gt;</span> in the same program as <span class="default">&lt;?php mysqli_query</span><span class="keyword">(</span><span class="default">$db</span><span class="keyword">) </span><span class="default">?&gt;</span>).</span>
</div>
  

#


<div class="phpcode"><span class="html">
Here is an example of a clean query into a html table<br><br>&lt;table&gt;<br>&#xA0;&#xA0; &lt;tr&gt;<br>&#xA0; &#xA0;&#xA0; &lt;th&gt;First Name&lt;/th&gt;<br>&#xA0; &#xA0;&#xA0; &lt;th&gt;Last Name&lt;/th&gt;<br>&#xA0; &#xA0;&#xA0; &lt;th&gt;City&lt;/th&gt;<br>&#xA0;&#xA0; &lt;/tr&gt;<br>&#xA0;&#xA0; <span class="default">&lt;?php </span><span class="keyword">while (</span><span class="default">$row </span><span class="keyword">= </span><span class="default">$myquery</span><span class="keyword">-&gt;</span><span class="default">fetch_assoc</span><span class="keyword">()) { </span><span class="default">?&gt;<br></span>&#xA0;&#xA0; &lt;tr&gt;<br>&#xA0; &#xA0;&#xA0; &lt;td&gt;<span class="default">&lt;?php </span><span class="keyword">echo </span><span class="default">$row</span><span class="keyword">[</span><span class="string">&quot;firstname&quot;</span><span class="keyword">]; </span><span class="default">?&gt;</span>&lt;/td&gt;<br>&#xA0; &#xA0;&#xA0; &lt;td&gt;<span class="default">&lt;?php </span><span class="keyword">echo </span><span class="default">$row</span><span class="keyword">[</span><span class="string">&quot;lastname&quot;</span><span class="keyword">]; </span><span class="default">?&gt;</span>&lt;/td&gt;<br>&#xA0; &#xA0;&#xA0; &lt;td&gt;<span class="default">&lt;?php </span><span class="keyword">echo </span><span class="default">$row</span><span class="keyword">[</span><span class="string">&quot;city&quot;</span><span class="keyword">];</span><span class="default">?&gt;</span>&lt;/td&gt;<br>&#xA0;&#xA0; &lt;/tr&gt;<br>&#xA0;&#xA0; <span class="default">&lt;?php </span><span class="keyword">} </span><span class="default">?&gt;<br></span> &lt;/table&gt;</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/mysqli.query.php)

**[To root](/README.md)**