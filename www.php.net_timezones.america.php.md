# America




<div class="phpcode"><span class="html">
Helpful tip for Brazilian coders in need of timezone information. Here follows the list of timezones used by each state (source: <a href="https://en.wikipedia.org/wiki/Time_in_Brazil" rel="nofollow" target="_blank">https://en.wikipedia.org/wiki/Time_in_Brazil</a>):<br><br>$timezones = array(<br>&apos;AC&apos; =&gt; &apos;America/Rio_branco&apos;,&#xA0;&#xA0; &apos;AL&apos; =&gt; &apos;America/Maceio&apos;,<br>&apos;AP&apos; =&gt; &apos;America/Belem&apos;,&#xA0; &#xA0; &#xA0; &#xA0; &apos;AM&apos; =&gt; &apos;America/Manaus&apos;,<br>&apos;BA&apos; =&gt; &apos;America/Bahia&apos;,&#xA0; &#xA0; &#xA0; &#xA0; &apos;CE&apos; =&gt; &apos;America/Fortaleza&apos;,<br>&apos;DF&apos; =&gt; &apos;America/Sao_Paulo&apos;,&#xA0; &#xA0; &apos;ES&apos; =&gt; &apos;America/Sao_Paulo&apos;,<br>&apos;GO&apos; =&gt; &apos;America/Sao_Paulo&apos;,&#xA0; &#xA0; &apos;MA&apos; =&gt; &apos;America/Fortaleza&apos;,<br>&apos;MT&apos; =&gt; &apos;America/Cuiaba&apos;,&#xA0; &#xA0; &#xA0;&#xA0; &apos;MS&apos; =&gt; &apos;America/Campo_Grande&apos;,<br>&apos;MG&apos; =&gt; &apos;America/Sao_Paulo&apos;,&#xA0; &#xA0; &apos;PR&apos; =&gt; &apos;America/Sao_Paulo&apos;,<br>&apos;PB&apos; =&gt; &apos;America/Fortaleza&apos;,&#xA0; &#xA0; &apos;PA&apos; =&gt; &apos;America/Belem&apos;,<br>&apos;PE&apos; =&gt; &apos;America/Recife&apos;,&#xA0; &#xA0; &#xA0;&#xA0; &apos;PI&apos; =&gt; &apos;America/Fortaleza&apos;,<br>&apos;RJ&apos; =&gt; &apos;America/Sao_Paulo&apos;,&#xA0; &#xA0; &apos;RN&apos; =&gt; &apos;America/Fortaleza&apos;,<br>&apos;RS&apos; =&gt; &apos;America/Sao_Paulo&apos;,&#xA0; &#xA0; &apos;RO&apos; =&gt; &apos;America/Porto_Velho&apos;,<br>&apos;RR&apos; =&gt; &apos;America/Boa_Vista&apos;,&#xA0; &#xA0; &apos;SC&apos; =&gt; &apos;America/Sao_Paulo&apos;,<br>&apos;SE&apos; =&gt; &apos;America/Maceio&apos;,&#xA0; &#xA0; &#xA0;&#xA0; &apos;SP&apos; =&gt; &apos;America/Sao_Paulo&apos;,<br>&apos;TO&apos; =&gt; &apos;America/Araguaia&apos;,&#xA0; &#xA0;&#xA0; <br>);</span>
</div>
  

#


<div class="phpcode"><span class="html">
Note that &quot;America&quot; means &quot;the continents of North and South America, including the Caribbean islands&quot;.&#xA0; It does *not* mean &quot;The United States Of America&quot;.<br><br>If you&apos;re looking for the Hawaii timezone, you&apos;ll find it in Pacific/Honolulu (not America/Honolulu).</span>
</div>
  

#


<div class="phpcode"><span class="html">
Mirai Densetsu is wrong by saying that &quot;America/Sao_Paulo&quot; timezone does not exist and should be changed to &quot;America/Brasilia&quot;.<br>This list is based on &quot;Time Zone Database&quot;, maintained by Internet Assigned Numbers Authority (IANA). The location part of the name, accordingly to the naming convention, uses the &quot;most populous city in a region to represent the entire time zone, although other cities may be selected if they are more widely known or result in a less ambiguous name&quot;.<br>It does not strictly consider local political divisions, because they are subject to change more than names of large cities.<br><br>This suggestion does not proceed.</span>
</div>
  

#


<div class="phpcode"><span class="html">
For the United States:<br><br>Eastern ........... America/New_York<br>Central ........... America/Chicago<br>Mountain .......... America/Denver<br>Mountain no DST ... America/Phoenix<br>Pacific ........... America/Los_Angeles<br>Alaska ............ America/Anchorage<br>Hawaii ............ America/Adak<br>Hawaii no DST ..... Pacific/Honolulu<br><br>Source: <a href="http://stackoverflow.com/questions/4989209/list-of-us-time-zones-for-php-to-use" rel="nofollow" target="_blank">http://stackoverflow.com/questions/4989209/list-of-us-time-zones-for-php-to-use</a></span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/timezones.america.php)

**[To root](/README.md)**