# str_ireplace




<div class="phpcode"><span class="html">
Here&apos;s a different approach to search result keyword highlighting that will match all keyword sub strings in a case insensitive manner and preserve case in the returned text. This solution first grabs all matches within $haystack in a case insensitive manner, and the secondly loops through each of those matched sub strings and applies a case sensitive replace in $haystack. This way each unique (in terms of case) instance of $needle is operated on individually allowing a case sensitive replace to be done in order to preserve the original case of each unique instance of $needle.<br><br><span class="default">&lt;?php<br></span><span class="keyword">function </span><span class="default">highlightStr</span><span class="keyword">(</span><span class="default">$haystack</span><span class="keyword">, </span><span class="default">$needle</span><span class="keyword">, </span><span class="default">$highlightColorValue</span><span class="keyword">) {<br>&#xA0; &#xA0;&#xA0; </span><span class="comment">// return $haystack if there is no highlight color or strings given, nothing to do.<br>&#xA0; &#xA0; </span><span class="keyword">if (</span><span class="default">strlen</span><span class="keyword">(</span><span class="default">$highlightColorValue</span><span class="keyword">) &lt; </span><span class="default">1 </span><span class="keyword">|| </span><span class="default">strlen</span><span class="keyword">(</span><span class="default">$haystack</span><span class="keyword">) &lt; </span><span class="default">1 </span><span class="keyword">|| </span><span class="default">strlen</span><span class="keyword">(</span><span class="default">$needle</span><span class="keyword">) &lt; </span><span class="default">1</span><span class="keyword">) {<br>&#xA0; &#xA0; &#xA0; &#xA0; return </span><span class="default">$haystack</span><span class="keyword">;<br>&#xA0; &#xA0; }<br>&#xA0; &#xA0; </span><span class="default">preg_match_all</span><span class="keyword">(</span><span class="string">&quot;/</span><span class="default">$needle</span><span class="string">+/i&quot;</span><span class="keyword">, </span><span class="default">$haystack</span><span class="keyword">, </span><span class="default">$matches</span><span class="keyword">);<br>&#xA0; &#xA0; if (</span><span class="default">is_array</span><span class="keyword">(</span><span class="default">$matches</span><span class="keyword">[</span><span class="default">0</span><span class="keyword">]) &amp;&amp; </span><span class="default">count</span><span class="keyword">(</span><span class="default">$matches</span><span class="keyword">[</span><span class="default">0</span><span class="keyword">]) &gt;= </span><span class="default">1</span><span class="keyword">) {<br>&#xA0; &#xA0; &#xA0; &#xA0; foreach (</span><span class="default">$matches</span><span class="keyword">[</span><span class="default">0</span><span class="keyword">] as </span><span class="default">$match</span><span class="keyword">) {<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$haystack </span><span class="keyword">= </span><span class="default">str_replace</span><span class="keyword">(</span><span class="default">$match</span><span class="keyword">, </span><span class="string">&apos;&lt;span style=&quot;background-color:&apos;</span><span class="keyword">.</span><span class="default">$highlightColorValue</span><span class="keyword">.</span><span class="string">&apos;;&quot;&gt;&apos;</span><span class="keyword">.</span><span class="default">$match</span><span class="keyword">.</span><span class="string">&apos;&lt;/span&gt;&apos;</span><span class="keyword">, </span><span class="default">$haystack</span><span class="keyword">);<br>&#xA0; &#xA0; &#xA0; &#xA0; }<br>&#xA0; &#xA0; }<br>&#xA0; &#xA0; return </span><span class="default">$haystack</span><span class="keyword">;<br>}<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#


<div class="phpcode"><span class="html">
here&apos;s a neat little function I whipped up to do HTML color coding of SQL strings. 
<br>
<br><span class="default">&lt;?php
<br></span><span class="comment">/**
<br> * Output the HTML debugging string in color coded glory for a sql query
<br> * This is very nice for being able to see many SQL queries
<br> * @access&#xA0; &#xA0;&#xA0; public
<br> * @return&#xA0; &#xA0;&#xA0; void. prints HTML color coded string of the input $query.
<br> * @param&#xA0; &#xA0;&#xA0; string $query The SQL query to be executed.
<br> * @author&#xA0; &#xA0;&#xA0; Daevid Vincent [daevid@LockdownNetworks.com]
<br> *&#xA0; @version&#xA0; &#xA0;&#xA0; 1.0
<br> * @date&#xA0; &#xA0; &#xA0; &#xA0; 04/05/05
<br> * @todo&#xA0; &#xA0;&#xA0; highlight SQL functions.
<br> */
<br></span><span class="keyword">function </span><span class="default">SQL_DEBUG</span><span class="keyword">( </span><span class="default">$query </span><span class="keyword">)
<br>{
<br>&#xA0; &#xA0; if( </span><span class="default">$query </span><span class="keyword">== </span><span class="string">&apos;&apos; </span><span class="keyword">) return </span><span class="default">0</span><span class="keyword">;
<br>
<br>&#xA0; &#xA0; global </span><span class="default">$SQL_INT</span><span class="keyword">;
<br>&#xA0; &#xA0; if( !isset(</span><span class="default">$SQL_INT</span><span class="keyword">) ) </span><span class="default">$SQL_INT </span><span class="keyword">= </span><span class="default">0</span><span class="keyword">;
<br>
<br>&#xA0; &#xA0; </span><span class="comment">//[dv] this has to come first or you will have goofy results later.
<br>&#xA0; &#xA0; </span><span class="default">$query </span><span class="keyword">= </span><span class="default">preg_replace</span><span class="keyword">(</span><span class="string">&quot;/[&apos;\&quot;]([^&apos;\&quot;]*)[&apos;\&quot;]/i&quot;</span><span class="keyword">, </span><span class="string">&quot;&apos;&lt;FONT COLOR=&apos;#FF6600&apos;&gt;$1&lt;/FONT&gt;&apos;&quot;</span><span class="keyword">, </span><span class="default">$query</span><span class="keyword">, -</span><span class="default">1</span><span class="keyword">);
<br>
<br>&#xA0; &#xA0; </span><span class="default">$query </span><span class="keyword">= </span><span class="default">str_ireplace</span><span class="keyword">(
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; array (
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="string">&apos;*&apos;</span><span class="keyword">,
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="string">&apos;SELECT &apos;</span><span class="keyword">,
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="string">&apos;UPDATE &apos;</span><span class="keyword">,
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="string">&apos;DELETE &apos;</span><span class="keyword">,
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="string">&apos;INSERT &apos;</span><span class="keyword">,
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="string">&apos;INTO&apos;</span><span class="keyword">,
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="string">&apos;VALUES&apos;</span><span class="keyword">,
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="string">&apos;FROM&apos;</span><span class="keyword">,
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="string">&apos;LEFT&apos;</span><span class="keyword">,
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="string">&apos;JOIN&apos;</span><span class="keyword">,
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="string">&apos;WHERE&apos;</span><span class="keyword">,
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="string">&apos;LIMIT&apos;</span><span class="keyword">,
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="string">&apos;ORDER BY&apos;</span><span class="keyword">,
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="string">&apos;AND&apos;</span><span class="keyword">,
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="string">&apos;OR &apos;</span><span class="keyword">, </span><span class="comment">//[dv] note the space. otherwise you match to &apos;COLOR&apos; ;-)
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="string">&apos;DESC&apos;</span><span class="keyword">,
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="string">&apos;ASC&apos;</span><span class="keyword">,
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="string">&apos;ON &apos;
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="keyword">),
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; array (
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="string">&quot;&lt;FONT COLOR=&apos;#FF6600&apos;&gt;&lt;B&gt;*&lt;/B&gt;&lt;/FONT&gt;&quot;</span><span class="keyword">,
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="string">&quot;&lt;FONT COLOR=&apos;#00AA00&apos;&gt;&lt;B&gt;SELECT&lt;/B&gt; &lt;/FONT&gt;&quot;</span><span class="keyword">,
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="string">&quot;&lt;FONT COLOR=&apos;#00AA00&apos;&gt;&lt;B&gt;UPDATE&lt;/B&gt; &lt;/FONT&gt;&quot;</span><span class="keyword">,
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="string">&quot;&lt;FONT COLOR=&apos;#00AA00&apos;&gt;&lt;B&gt;DELETE&lt;/B&gt; &lt;/FONT&gt;&quot;</span><span class="keyword">,
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="string">&quot;&lt;FONT COLOR=&apos;#00AA00&apos;&gt;&lt;B&gt;INSERT&lt;/B&gt; &lt;/FONT&gt;&quot;</span><span class="keyword">,
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="string">&quot;&lt;FONT COLOR=&apos;#00AA00&apos;&gt;&lt;B&gt;INTO&lt;/B&gt;&lt;/FONT&gt;&quot;</span><span class="keyword">,
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="string">&quot;&lt;FONT COLOR=&apos;#00AA00&apos;&gt;&lt;B&gt;VALUES&lt;/B&gt;&lt;/FONT&gt;&quot;</span><span class="keyword">,
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="string">&quot;&lt;FONT COLOR=&apos;#00AA00&apos;&gt;&lt;B&gt;FROM&lt;/B&gt;&lt;/FONT&gt;&quot;</span><span class="keyword">,
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="string">&quot;&lt;FONT COLOR=&apos;#00CC00&apos;&gt;&lt;B&gt;LEFT&lt;/B&gt;&lt;/FONT&gt;&quot;</span><span class="keyword">,
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="string">&quot;&lt;FONT COLOR=&apos;#00CC00&apos;&gt;&lt;B&gt;JOIN&lt;/B&gt;&lt;/FONT&gt;&quot;</span><span class="keyword">,
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="string">&quot;&lt;FONT COLOR=&apos;#00AA00&apos;&gt;&lt;B&gt;WHERE&lt;/B&gt;&lt;/FONT&gt;&quot;</span><span class="keyword">,
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="string">&quot;&lt;FONT COLOR=&apos;#AA0000&apos;&gt;&lt;B&gt;LIMIT&lt;/B&gt;&lt;/FONT&gt;&quot;</span><span class="keyword">,
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="string">&quot;&lt;FONT COLOR=&apos;#00AA00&apos;&gt;&lt;B&gt;ORDER BY&lt;/B&gt;&lt;/FONT&gt;&quot;</span><span class="keyword">,
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="string">&quot;&lt;FONT COLOR=&apos;#0000AA&apos;&gt;&lt;B&gt;AND&lt;/B&gt;&lt;/FONT&gt;&quot;</span><span class="keyword">,
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="string">&quot;&lt;FONT COLOR=&apos;#0000AA&apos;&gt;&lt;B&gt;OR&lt;/B&gt; &lt;/FONT&gt;&quot;</span><span class="keyword">,
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="string">&quot;&lt;FONT COLOR=&apos;#0000AA&apos;&gt;&lt;B&gt;DESC&lt;/B&gt;&lt;/FONT&gt;&quot;</span><span class="keyword">,
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="string">&quot;&lt;FONT COLOR=&apos;#0000AA&apos;&gt;&lt;B&gt;ASC&lt;/B&gt;&lt;/FONT&gt;&quot;</span><span class="keyword">,
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="string">&quot;&lt;FONT COLOR=&apos;#00DD00&apos;&gt;&lt;B&gt;ON&lt;/B&gt; &lt;/FONT&gt;&quot;
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="keyword">),
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$query
<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="keyword">);
<br>
<br>&#xA0; &#xA0; echo </span><span class="string">&quot;&lt;FONT COLOR=&apos;#0000FF&apos;&gt;&lt;B&gt;SQL[&quot;</span><span class="keyword">.</span><span class="default">$SQL_INT</span><span class="keyword">.</span><span class="string">&quot;]:&lt;/B&gt; &quot;</span><span class="keyword">.</span><span class="default">$query</span><span class="keyword">.</span><span class="string">&quot;&lt;FONT COLOR=&apos;#FF0000&apos;&gt;;&lt;/FONT&gt;&lt;/FONT&gt;&lt;BR&gt;\n&quot;</span><span class="keyword">;
<br>
<br>&#xA0; &#xA0; </span><span class="default">$SQL_INT</span><span class="keyword">++;
<br>
<br>} </span><span class="comment">//SQL_DEBUG
<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/function.str-ireplace.php)

**[⬆ to root](/)**