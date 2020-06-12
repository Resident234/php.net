# Using namespaces: Aliasing/Importing




<div class="phpcode"><span class="html">
The keyword &quot;use&quot; has been recycled for three distinct applications: <br>1- to import/alias classes, traits, constants, etc. in namespaces, <br>2- to insert traits in classes, <br>3- to inherit variables in closures. <br>This page is only about the first application: importing/aliasing. Traits can be inserted in classes, but this is different from importing a trait in a namespace, which cannot be done in a block scope, as pointed out in example 5. This can be confusing, especially since all searches for the keyword &quot;use&quot; are directed to the documentation here on importing/aliasing.</span>
</div>
  

#


<div class="phpcode"><span class="html">
The <span class="default">&lt;?php </span><span class="keyword">use </span><span class="default">?&gt;</span> statement does not load the class file. You have to do this with the <span class="default">&lt;?php </span><span class="keyword">require </span><span class="default">?&gt;</span> statement or by using an autoload function.</span>
</div>
  

#


<div class="phpcode"><span class="html">
Note that you can not alias global namespace:<br><br>use \ as test;<br><br>echo test\strlen(&apos;&apos;);<br><br>won&apos;t work.</span>
</div>
  

#


<div class="phpcode"><span class="html">
Here is a handy way of importing classes, functions and conts using a single use keyword:<br><br><span class="default">&lt;?php<br></span><span class="keyword">use </span><span class="default">Mizo</span><span class="keyword">\</span><span class="default">Web</span><span class="keyword">\ {<br>&#xA0;&#xA0; </span><span class="default">Php</span><span class="keyword">\</span><span class="default">WebSite</span><span class="keyword">,<br>&#xA0;&#xA0; </span><span class="default">Php</span><span class="keyword">\</span><span class="default">KeyWord</span><span class="keyword">,<br>&#xA0;&#xA0; </span><span class="default">Php</span><span class="keyword">\</span><span class="default">UnicodePrint</span><span class="keyword">,<br>&#xA0;&#xA0; </span><span class="default">JS</span><span class="keyword">\</span><span class="default">JavaScript</span><span class="keyword">, <br>&#xA0;&#xA0; function </span><span class="default">JS</span><span class="keyword">\</span><span class="default">printTotal</span><span class="keyword">, <br>&#xA0;&#xA0; function </span><span class="default">JS</span><span class="keyword">\</span><span class="default">printList</span><span class="keyword">, <br>&#xA0;&#xA0; const </span><span class="default">JS</span><span class="keyword">\</span><span class="default">BUAIKUM</span><span class="keyword">, <br>&#xA0;&#xA0; const </span><span class="default">JS</span><span class="keyword">\</span><span class="default">MAUTAM<br></span><span class="keyword">};<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#


<div class="phpcode"><span class="html">
I couldn&apos;t find answer to this question so I tested myself. <br>I think it&apos;s worth noting:<br><br><span class="default">&lt;?php<br></span><span class="keyword">use </span><span class="default">ExistingNamespace</span><span class="keyword">\</span><span class="default">NonExsistingClass</span><span class="keyword">;<br>use </span><span class="default">ExistingNamespace</span><span class="keyword">\</span><span class="default">NonExsistingClass </span><span class="keyword">as </span><span class="default">whatever</span><span class="keyword">;<br>use </span><span class="default">NonExistingNamespace</span><span class="keyword">\</span><span class="default">NonExsistingClass</span><span class="keyword">;<br>use </span><span class="default">NonExistingNamespace</span><span class="keyword">\</span><span class="default">NonExsistingClass </span><span class="keyword">as </span><span class="default">whatever</span><span class="keyword">;<br></span><span class="default">?&gt;<br></span><br>None of above will actually cause errors unless you actually try to use class you tried to import. <br><br><span class="default">&lt;?php<br></span><span class="comment">// And this code will issue standard PHP error for non existing class.<br></span><span class="keyword">use </span><span class="default">ExistingNamespace</span><span class="keyword">\</span><span class="default">NonExsistingClass </span><span class="keyword">as </span><span class="default">whatever</span><span class="keyword">;<br></span><span class="default">$whatever </span><span class="keyword">= new </span><span class="default">whatever</span><span class="keyword">();<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#


<div class="phpcode"><span class="html">
Note the code `use ns1\c1` may refer to importing class `c1` from namespace `ns1` as well as importing whole namespace `ns1\c1` or even import both of them in one line. Example:<br><br><span class="default">&lt;?php<br></span><span class="keyword">namespace </span><span class="default">ns1</span><span class="keyword">;<br><br>class </span><span class="default">c1</span><span class="keyword">{}<br><br>namespace </span><span class="default">ns1</span><span class="keyword">\</span><span class="default">c1</span><span class="keyword">;<br><br>class </span><span class="default">c11</span><span class="keyword">{}<br><br>namespace </span><span class="default">main</span><span class="keyword">;<br><br>use </span><span class="default">ns1</span><span class="keyword">\</span><span class="default">c1</span><span class="keyword">;<br><br></span><span class="default">$c1 </span><span class="keyword">= new </span><span class="default">c1</span><span class="keyword">();<br></span><span class="default">$c11 </span><span class="keyword">= new </span><span class="default">c1</span><span class="keyword">\</span><span class="default">c11</span><span class="keyword">();<br><br></span><span class="default">var_dump</span><span class="keyword">(</span><span class="default">$c1</span><span class="keyword">); </span><span class="comment">// object(ns1\c1)#1 (0) { }<br></span><span class="default">var_dump</span><span class="keyword">(</span><span class="default">$c11</span><span class="keyword">); </span><span class="comment">// object(ns1\c1\c11)#2 (0) { }</span>
</span>
</div>
  

#


<div class="phpcode"><span class="html">
If you are testing your code at the CLI, note that namespace aliases do not work!<br><br>(Before I go on, all the backslashes in this example are changed to percent signs because I cannot get sensible results to display in the posting preview otherwise. Please mentally translate all percent signs henceforth as backslashes.)<br><br>Suppose you have a class you want to test in myclass.php:<br><br><span class="default">&lt;?php<br></span><span class="keyword">namespace </span><span class="default">my</span><span class="keyword">%</span><span class="default">space</span><span class="keyword">;<br>class </span><span class="default">myclass </span><span class="keyword">{<br> </span><span class="comment">// ...<br></span><span class="keyword">}<br></span><span class="default">?&gt;<br></span><br>and you then go into the CLI to test it. You would like to think that this would work, as you type it line by line:<br><br>require &apos;myclass.php&apos;;<br>use my%space%myclass; // should set &apos;myclass&apos; as alias for &apos;my%space%myclass&apos;<br>$x = new myclass; // FATAL ERROR<br><br>I believe that this is because aliases are only resolved at compile time, whereas the CLI simply evaluates statements; so use statements are ineffective in the CLI.<br><br>If you put your test code into test.php:<br><span class="default">&lt;?php<br></span><span class="keyword">require </span><span class="string">&apos;myclass.php&apos;</span><span class="keyword">;<br>use </span><span class="default">my</span><span class="keyword">%</span><span class="default">space</span><span class="keyword">%</span><span class="default">myclass</span><span class="keyword">;<br></span><span class="default">$x </span><span class="keyword">= new </span><span class="default">myclass</span><span class="keyword">;<br></span><span class="comment">//...<br></span><span class="default">?&gt;<br></span>it will work fine.<br><br>I hope this reduces the number of prematurely bald people.</span>
</div>
  

#


<div class="phpcode"><span class="html">
Something that is not immediately obvious, particular with PHP 5.3, is that namespace resolutions within an import are not resolved recursively.&#xA0; i.e.: if you alias an import and then use that alias in another import then this latter import will not be fully resolved with the former import.<br><br>For example:<br>use \Controllers as C;<br>use C\First;<br>use C\Last;<br><br>Both the First and Last namespaces are NOT resolved as \Controllers\First or \Controllers\Last as one might intend.</span>
</div>
  

#


<div class="phpcode"><span class="html">
You are allowed to &quot;use&quot; the same resource multiple times as long as it is imported under a different alias at each invocation.<br><br>For example:<br><br><span class="default">&lt;?php<br></span><span class="keyword">use </span><span class="default">Lend</span><span class="keyword">;<br>use </span><span class="default">Lend</span><span class="keyword">\</span><span class="default">l1</span><span class="keyword">;<br>use </span><span class="default">Lend</span><span class="keyword">\</span><span class="default">l1 </span><span class="keyword">as </span><span class="default">l3</span><span class="keyword">;<br>use </span><span class="default">Lend</span><span class="keyword">\</span><span class="default">l2</span><span class="keyword">;<br>use </span><span class="default">Lend</span><span class="keyword">\</span><span class="default">l1</span><span class="keyword">\</span><span class="default">Keller</span><span class="keyword">;<br>use </span><span class="default">Lend</span><span class="keyword">\</span><span class="default">l1</span><span class="keyword">\</span><span class="default">Keller </span><span class="keyword">as </span><span class="default">Stellar</span><span class="keyword">;<br>use </span><span class="default">Lend</span><span class="keyword">\</span><span class="default">l1</span><span class="keyword">\</span><span class="default">Keller </span><span class="keyword">as </span><span class="default">Zellar</span><span class="keyword">;<br>use </span><span class="default">Lend</span><span class="keyword">\</span><span class="default">l2</span><span class="keyword">\</span><span class="default">Keller </span><span class="keyword">as </span><span class="default">Dellar</span><span class="keyword">;<br><br>...<br><br></span><span class="default">?&gt;<br></span><br>In the above example, &quot;Keller&quot;, &quot;Stellar&quot;, and &quot;Zellar&quot; are all references to &quot;\Lend\l1\Keller&quot;, as are &quot;Lend\l1\Keller&quot;, &quot;l1\Keller&quot;, and &quot;l3\Keller&quot;.</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/language.namespaces.importing.php)

**[To root](/)**