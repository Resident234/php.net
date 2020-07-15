# America



Helpful tip for Brazilian coders in need of timezone information. Here follows the list of timezones used by each state (source: https://en.wikipedia.org/wiki/Time_in_Brazil):<br><br>$timezones = array(<br>&apos;AC&apos; =&gt; &apos;America/Rio_branco&apos;,   &apos;AL&apos; =&gt; &apos;America/Maceio&apos;,<br>&apos;AP&apos; =&gt; &apos;America/Belem&apos;,        &apos;AM&apos; =&gt; &apos;America/Manaus&apos;,<br>&apos;BA&apos; =&gt; &apos;America/Bahia&apos;,        &apos;CE&apos; =&gt; &apos;America/Fortaleza&apos;,<br>&apos;DF&apos; =&gt; &apos;America/Sao_Paulo&apos;,    &apos;ES&apos; =&gt; &apos;America/Sao_Paulo&apos;,<br>&apos;GO&apos; =&gt; &apos;America/Sao_Paulo&apos;,    &apos;MA&apos; =&gt; &apos;America/Fortaleza&apos;,<br>&apos;MT&apos; =&gt; &apos;America/Cuiaba&apos;,       &apos;MS&apos; =&gt; &apos;America/Campo_Grande&apos;,<br>&apos;MG&apos; =&gt; &apos;America/Sao_Paulo&apos;,    &apos;PR&apos; =&gt; &apos;America/Sao_Paulo&apos;,<br>&apos;PB&apos; =&gt; &apos;America/Fortaleza&apos;,    &apos;PA&apos; =&gt; &apos;America/Belem&apos;,<br>&apos;PE&apos; =&gt; &apos;America/Recife&apos;,       &apos;PI&apos; =&gt; &apos;America/Fortaleza&apos;,<br>&apos;RJ&apos; =&gt; &apos;America/Sao_Paulo&apos;,    &apos;RN&apos; =&gt; &apos;America/Fortaleza&apos;,<br>&apos;RS&apos; =&gt; &apos;America/Sao_Paulo&apos;,    &apos;RO&apos; =&gt; &apos;America/Porto_Velho&apos;,<br>&apos;RR&apos; =&gt; &apos;America/Boa_Vista&apos;,    &apos;SC&apos; =&gt; &apos;America/Sao_Paulo&apos;,<br>&apos;SE&apos; =&gt; &apos;America/Maceio&apos;,       &apos;SP&apos; =&gt; &apos;America/Sao_Paulo&apos;,<br>&apos;TO&apos; =&gt; &apos;America/Araguaia&apos;,     <br>);  

#

Note that "America" means "the continents of North and South America, including the Caribbean islands".  It does *not* mean "The United States Of America".<br><br>If you&apos;re looking for the Hawaii timezone, you&apos;ll find it in Pacific/Honolulu (not America/Honolulu).  

#

Mirai Densetsu is wrong by saying that "America/Sao_Paulo" timezone does not exist and should be changed to "America/Brasilia".<br>This list is based on "Time Zone Database", maintained by Internet Assigned Numbers Authority (IANA). The location part of the name, accordingly to the naming convention, uses the "most populous city in a region to represent the entire time zone, although other cities may be selected if they are more widely known or result in a less ambiguous name".<br>It does not strictly consider local political divisions, because they are subject to change more than names of large cities.<br><br>This suggestion does not proceed.  

#

For the United States:<br><br>Eastern ........... America/New_York<br>Central ........... America/Chicago<br>Mountain .......... America/Denver<br>Mountain no DST ... America/Phoenix<br>Pacific ........... America/Los_Angeles<br>Alaska ............ America/Anchorage<br>Hawaii ............ America/Adak<br>Hawaii no DST ..... Pacific/Honolulu<br><br>Source: http://stackoverflow.com/questions/4989209/list-of-us-time-zones-for

```
<??>
```
to-use  

#

[Official documentation page](https://www.php.net/manual/en/timezones.america.php)

**[To root](/README.md)**