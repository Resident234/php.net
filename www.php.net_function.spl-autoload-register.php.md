# spl_autoload_register




<div class="phpcode"><span class="html">
Good news for PHP 5.3 users with namespaced classes:<br><br>When you create a subfolder structure matching the namespaces of the containing classes, you will never even have to define an autoloader.<br><br><span class="default">&lt;?php<br>&#xA0; &#xA0; spl_autoload_extensions</span><span class="keyword">(</span><span class="string">&quot;.php&quot;</span><span class="keyword">); </span><span class="comment">// comma-separated list<br>&#xA0; &#xA0; </span><span class="default">spl_autoload_register</span><span class="keyword">();<br></span><span class="default">?&gt;<br></span><br>It is recommended to use only one extension for all classes. PHP (more exactly spl_autoload) does the rest for you and is even quicker than a semantically equal self-defined autoload function like this one:<br><br><span class="default">&lt;?php<br>&#xA0; &#xA0; </span><span class="keyword">function </span><span class="default">my_autoload </span><span class="keyword">(</span><span class="default">$pClassName</span><span class="keyword">) {<br>&#xA0; &#xA0; &#xA0; &#xA0; include(</span><span class="default">__DIR__ </span><span class="keyword">. </span><span class="string">&quot;/&quot; </span><span class="keyword">. </span><span class="default">$pClassName </span><span class="keyword">. </span><span class="string">&quot;.php&quot;</span><span class="keyword">);<br>&#xA0; &#xA0; }<br>&#xA0; &#xA0; </span><span class="default">spl_autoload_register</span><span class="keyword">(</span><span class="string">&quot;my_autoload&quot;</span><span class="keyword">);<br></span><span class="default">?&gt;<br></span><br>I compared them with the following setting: There are 10 folders, each having 10 subfolders, each having 10 subfolders, each containing 10 classes.<br><br>To load and instantiate these 1000 classes (parameterless no-action constructor), the user-definded autoload function approach took 50ms longer in average than the spl_autoload function in a series of 10 command-line calls for each approach.<br><br>I made this benchmark to ensure that I don&apos;t recommend something that could be called &quot;nice, but slow&quot; later.<br><br>Best regards,</span>
</div>
  

#


<div class="phpcode"><span class="html">
When switching from using __autoload() to using spl_autoload_register keep in mind that deserialization of the session can trigger class lookups.<br><br>This works as expected: <br><span class="default">&lt;?php<br>session_start</span><span class="keyword">();<br>function </span><span class="default">__autoload</span><span class="keyword">(</span><span class="default">$class</span><span class="keyword">) {<br>...<br>}<br></span><span class="default">?&gt;<br></span><br>This will result in &quot;__PHP_Incomplete_Class_Name&quot; errors when using classes deserialized from the session.<br><span class="default">&lt;?php<br>session_start</span><span class="keyword">();<br>function </span><span class="default">customAutoloader</span><span class="keyword">(</span><span class="default">$class</span><span class="keyword">) {<br>...<br>}<br></span><span class="default">spl_autoload_register</span><span class="keyword">(</span><span class="string">&quot;customAutoloader&quot;</span><span class="keyword">);<br></span><span class="default">?&gt;<br></span><br>So you need to make sure the spl_autoload_register is done BEFORE session_start() is called.<br><br>CORRECT:<br><span class="default">&lt;?php<br></span><span class="keyword">function </span><span class="default">customAutoloader</span><span class="keyword">(</span><span class="default">$class</span><span class="keyword">) {<br>...<br>}<br></span><span class="default">spl_autoload_register</span><span class="keyword">(</span><span class="string">&quot;customAutoloader&quot;</span><span class="keyword">);<br></span><span class="default">session_start</span><span class="keyword">();<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#


<div class="phpcode"><span class="html">
When using spl_autoload_register() with class methods, it might seem that it can use only public methods, though it can use private/protected methods as well, if registered from inside the class:<br><span class="default">&lt;?php<br><br>&#xA0; &#xA0; </span><span class="keyword">class </span><span class="default">ClassAutoloader </span><span class="keyword">{<br>&#xA0; &#xA0; &#xA0; &#xA0; public function </span><span class="default">__construct</span><span class="keyword">() {<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">spl_autoload_register</span><span class="keyword">(array(</span><span class="default">$this</span><span class="keyword">, </span><span class="string">&apos;loader&apos;</span><span class="keyword">));<br>&#xA0; &#xA0; &#xA0; &#xA0; }<br>&#xA0; &#xA0; &#xA0; &#xA0; private function </span><span class="default">loader</span><span class="keyword">(</span><span class="default">$className</span><span class="keyword">) {<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; echo </span><span class="string">&apos;Trying to load &apos;</span><span class="keyword">, </span><span class="default">$className</span><span class="keyword">, </span><span class="string">&apos; via &apos;</span><span class="keyword">, </span><span class="default">__METHOD__</span><span class="keyword">, </span><span class="string">&quot;()\n&quot;</span><span class="keyword">;<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; include </span><span class="default">$className </span><span class="keyword">. </span><span class="string">&apos;.php&apos;</span><span class="keyword">;<br>&#xA0; &#xA0; &#xA0; &#xA0; }<br>&#xA0; &#xA0; }<br><br>&#xA0; &#xA0; </span><span class="default">$autoloader </span><span class="keyword">= new </span><span class="default">ClassAutoloader</span><span class="keyword">();<br><br>&#xA0; &#xA0; </span><span class="default">$obj </span><span class="keyword">= new </span><span class="default">Class1</span><span class="keyword">();<br>&#xA0; &#xA0; </span><span class="default">$obj </span><span class="keyword">= new </span><span class="default">Class2</span><span class="keyword">();<br><br></span><span class="default">?&gt;<br></span><br>Output:<br>--------<br>Trying to load Class1 via ClassAutoloader::loader()<br>Class1::__construct()<br>Trying to load Class2 via ClassAutoloader::loader()<br>Class2::__construct()</span>
</div>
  

#


<div class="phpcode"><span class="html">
If your autoload function is a class method, you can call spl_autoload_register with an array specifying the class and the method to run.<br><br>* You can use a static method :<br><span class="default">&lt;?php<br><br></span><span class="keyword">class </span><span class="default">MyClass </span><span class="keyword">{<br>&#xA0; public static function </span><span class="default">autoload</span><span class="keyword">(</span><span class="default">$className</span><span class="keyword">) {<br>&#xA0; &#xA0; </span><span class="comment">// ...<br>&#xA0; </span><span class="keyword">}<br>}<br><br></span><span class="default">spl_autoload_register</span><span class="keyword">(array(</span><span class="string">&apos;MyClass&apos;</span><span class="keyword">, </span><span class="string">&apos;autoload&apos;</span><span class="keyword">));<br></span><span class="default">?&gt;<br></span><br>* Or you can use an instance :<br><span class="default">&lt;?php<br></span><span class="keyword">class </span><span class="default">MyClass </span><span class="keyword">{<br>&#xA0; public function </span><span class="default">autoload</span><span class="keyword">(</span><span class="default">$className</span><span class="keyword">) {<br>&#xA0; &#xA0; </span><span class="comment">// ...<br>&#xA0; </span><span class="keyword">}<br>}<br><br></span><span class="default">$instance </span><span class="keyword">= new </span><span class="default">MyClass</span><span class="keyword">();<br></span><span class="default">spl_autoload_register</span><span class="keyword">(array(</span><span class="default">$instance</span><span class="keyword">, </span><span class="string">&apos;autoload&apos;</span><span class="keyword">));<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#


<div class="phpcode"><span class="html">
Think twice about throwing an exception from a registered autoloader.<br><br>If you have multiple autoloaders registered, and one (or more) throws an exception before a later autoloader loads the class, stacked exceptions are thrown (and must be caught) even though the class was loaded successfully.</span>
</div>
  

#


<div class="phpcode"><span class="html">
What I said here previously is only true on Windows. The built-in default autoloader that is registered when you call spl_autoload_register() without any arguments simply adds the qualified class name plus the registered file extension (.php) to each of the include paths and tries to include that file.<br><br>Example (on Windows):<br><br>include paths:<br>- &quot;.&quot;<br>- &quot;d:/projects/phplib&quot;<br><br>qualified class name to load:<br>network\http\rest\Resource<br><br>Here&apos;s what happens:<br><br>PHP tries to load<br>&apos;.\\network\\http\\rest\\Resource.php&apos;<br>-&gt; file not found<br><br>PHP tries to load<br>&apos;d:/projects/phplib\\network\\http\\rest\\Resource.php&apos;<br>-&gt; file found and included<br><br>Note the slashes and backslashes in the file path. On Windows this works perfectly, but on a Linux machine, the backslashes won&apos;t work and additionally the file names are case-sensitive.<br><br>That&apos;s why on Linux the quick-and-easy way would be to convert these qualified class names to slashes and to lowercase and pass them to the built-in autoloader like so:<br><br><span class="default">&lt;?php<br>spl_autoload_register</span><span class="keyword">(<br>&#xA0; function (</span><span class="default">$pClassName</span><span class="keyword">) {<br>&#xA0; &#xA0; </span><span class="default">spl_autoload</span><span class="keyword">(</span><span class="default">strtolower</span><span class="keyword">(</span><span class="default">str_replace</span><span class="keyword">(</span><span class="string">&quot;\\&quot;</span><span class="keyword">, </span><span class="string">&quot;/&quot;</span><span class="keyword">, </span><span class="default">$pClassName</span><span class="keyword">)));<br>&#xA0; }<br>);<br></span><span class="default">?&gt;<br></span><br>But this means, you have to save all your classes with lowercase file names. Otherwise, if you omit the strtolower call, you have to use the class names exactly as specified by the file name, which can be annoying for class names that are defined with non-straightforward case like e. g. XMLHttpRequest.<br><br>I prefer the lowercase approach, because it is easier to use and the file name conversion can be done automatically on deploying.</span>
</div>
  

#


<div class="phpcode"><span class="html">
Be careful using this function on case sensitive file systems.<br><br><span class="default">&lt;?php<br>spl_autoload_extensions</span><span class="keyword">(</span><span class="string">&apos;.php&apos;</span><span class="keyword">);<br></span><span class="default">spl_autoload_register</span><span class="keyword">();<br></span><span class="default">?&gt;<br></span><br>I develop on OS X and everything was working fine. But when releasing to my linux server, none of my class files were loading. I had to lowercase all my filenames, because calling a class &quot;DatabaseObject&quot; would try including &quot;databaseobject.php&quot;, instead of &quot;DatabaseObject.php&quot;<br><br>I think i&apos;ll go back to using the slower __autoload() function, just so i can keep my class files readable</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/function.spl-autoload-register.php)

**[To root](/README.md)**