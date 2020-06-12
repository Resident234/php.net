# $argv




<div class="phpcode"><span class="html">
Please note that, $argv and $argc need to be declared global, while trying to access within a class method. <br><br><span class="default">&lt;?php<br></span><span class="keyword">class </span><span class="default">A<br></span><span class="keyword">{<br>&#xA0; &#xA0; public static function </span><span class="default">b</span><span class="keyword">()<br>&#xA0; &#xA0; {<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">var_dump</span><span class="keyword">(</span><span class="default">$argv</span><span class="keyword">);<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">var_dump</span><span class="keyword">(isset(</span><span class="default">$argv</span><span class="keyword">));<br>&#xA0; &#xA0; }<br>}<br><br></span><span class="default">A</span><span class="keyword">::</span><span class="default">b</span><span class="keyword">();<br></span><span class="default">?&gt;<br></span><br>will output NULL bool(false)&#xA0; with a notice of &quot;Undefined variable ...&quot;<br><br>whereas global $argv fixes that.</span>
</div>
  

#


<div class="phpcode"><span class="html">
To use $_GET so you dont need to support both if it could be used from command line and from web browser.<br><br>foreach ($argv as $arg) {<br>&#xA0; &#xA0; $e=explode(&quot;=&quot;,$arg);<br>&#xA0; &#xA0; if(count($e)==2)<br>&#xA0; &#xA0; &#xA0; &#xA0; $_GET[$e[0]]=$e[1];<br>&#xA0; &#xA0; else&#xA0; &#xA0; <br>&#xA0; &#xA0; &#xA0; &#xA0; $_GET[$e[0]]=0;<br>}</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/reserved.variables.argv.php)

**[To root](/README.md)**