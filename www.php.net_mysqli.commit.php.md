# mysqli::commit




<div class="phpcode"><span class="html">
I never recomend to use the ? with only one value variant like: $var = expression ? $var&#xA0; : other_value or $var = expression ? null&#xA0; : other_value ,and php suport Exception catchin so,use it :)<br><br>here my opinion abut lorenzo&apos;s post:<br><br>&#xA0; <span class="default">&lt;?php<br> <br></span><span class="comment">//variants combined<br><br></span><span class="default">$mysqli</span><span class="keyword">-&gt;</span><span class="default">autocommit</span><span class="keyword">(</span><span class="default">FALSE</span><span class="keyword">);<br><br>try{<br><br>&#xA0; </span><span class="default">$mysqli</span><span class="keyword">-&gt;</span><span class="default">query</span><span class="keyword">(</span><span class="string">&quot;INSERT INTO myCity (id) VALUES (100)&quot;</span><span class="keyword">) or throw new </span><span class="default">Exception</span><span class="keyword">(</span><span class="string">&apos;error!&apos;</span><span class="keyword">);<br><br></span><span class="comment">// or we can use<br><br>&#xA0; </span><span class="keyword">if( !</span><span class="default">$mysqli</span><span class="keyword">-&gt;</span><span class="default">query</span><span class="keyword">(</span><span class="string">&quot;INSERT INTO myCity (id) VALUES (200)&quot;</span><span class="keyword">){ <br>&#xA0; &#xA0; throw new </span><span class="default">Exception</span><span class="keyword">(</span><span class="string">&apos;error!&apos;</span><span class="keyword">); <br>&#xA0; }<br><br>}catch( </span><span class="default">Exception $e </span><span class="keyword">){<br>&#xA0; </span><span class="default">$mysqli</span><span class="keyword">-&gt;</span><span class="default">rollback</span><span class="keyword">();<br>}<br></span><span class="default">$mysqli</span><span class="keyword">-&gt;</span><span class="default">commit</span><span class="keyword">();<br><br></span><span class="default">?&gt;</span>
</span>
</div>
  

#


<div class="phpcode"><span class="html">
Please note that calling mysqli::commit() will NOT automatically set mysqli::autocommit() back to &apos;true&apos;.<br><br>This means that any queries following mysqli::commit() will be rolled back when your script exits.</span>
</div>
  

#


<div class="phpcode"><span class="html">
This is an example to explain the powerful of the rollback and commit functions.<br>Let&apos;s suppose you want to be sure that all queries have to be executed without errors before writing data on the database.<br>Here&apos;s the code:<br><br><span class="default">&lt;?php<br>$all_query_ok</span><span class="keyword">=</span><span class="default">true</span><span class="keyword">; </span><span class="comment">// our control variable<br><br>//we make 4 inserts, the last one generates an error<br>//if at least one query returns an error we change our control variable<br></span><span class="default">$mysqli</span><span class="keyword">-&gt;</span><span class="default">query</span><span class="keyword">(</span><span class="string">&quot;INSERT INTO myCity (id) VALUES (100)&quot;</span><span class="keyword">) ? </span><span class="default">null </span><span class="keyword">: </span><span class="default">$all_query_ok</span><span class="keyword">=</span><span class="default">false</span><span class="keyword">;<br></span><span class="default">$mysqli</span><span class="keyword">-&gt;</span><span class="default">query</span><span class="keyword">(</span><span class="string">&quot;INSERT INTO myCity (id) VALUES (200)&quot;</span><span class="keyword">) ? </span><span class="default">null </span><span class="keyword">: </span><span class="default">$all_query_ok</span><span class="keyword">=</span><span class="default">false</span><span class="keyword">;<br></span><span class="default">$mysqli</span><span class="keyword">-&gt;</span><span class="default">query</span><span class="keyword">(</span><span class="string">&quot;INSERT INTO myCity (id) VALUES (300)&quot;</span><span class="keyword">) ? </span><span class="default">null </span><span class="keyword">: </span><span class="default">$all_query_ok</span><span class="keyword">=</span><span class="default">false</span><span class="keyword">;<br></span><span class="default">$mysqli</span><span class="keyword">-&gt;</span><span class="default">query</span><span class="keyword">(</span><span class="string">&quot;INSERT INTO myCity (id) VALUES (100)&quot;</span><span class="keyword">) ? </span><span class="default">null </span><span class="keyword">: </span><span class="default">$all_query_ok</span><span class="keyword">=</span><span class="default">false</span><span class="keyword">; </span><span class="comment">//duplicated PRIMARY KEY VALUE<br><br>//now let&apos;s test our control variable<br></span><span class="default">$all_query_ok </span><span class="keyword">? </span><span class="default">$mysqli</span><span class="keyword">-&gt;</span><span class="default">commit</span><span class="keyword">() : </span><span class="default">$mysqli</span><span class="keyword">-&gt;</span><span class="default">rollback</span><span class="keyword">();<br><br></span><span class="default">$mysqli</span><span class="keyword">-&gt;</span><span class="default">close</span><span class="keyword">();<br></span><span class="default">?&gt;<br></span><br>hope to be helpful!</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/mysqli.commit.php)

**[To root](/README.md)**