# Constants




<div class="phpcode"><span class="html">
11/14/2016 - note updated by sobak
<br>-----
<br>
<br>CONSTANTS and PHP Class Definitions
<br>
<br>Using &quot;define(&apos;MY_VAR&apos;, &apos;default value&apos;)&quot; INSIDE a class definition does not work as expected. You have to use the PHP keyword &apos;const&apos; and initialize it with a scalar value -- boolean, int, float, string (or array in PHP 5.6+) -- right away.
<br>
<br><span class="default">&lt;?php
<br>
<br>define</span><span class="keyword">(</span><span class="string">&apos;MIN_VALUE&apos;</span><span class="keyword">, </span><span class="string">&apos;0.0&apos;</span><span class="keyword">);&#xA0;&#xA0; </span><span class="comment">// RIGHT - Works OUTSIDE of a class definition.
<br></span><span class="default">define</span><span class="keyword">(</span><span class="string">&apos;MAX_VALUE&apos;</span><span class="keyword">, </span><span class="string">&apos;1.0&apos;</span><span class="keyword">);&#xA0;&#xA0; </span><span class="comment">// RIGHT - Works OUTSIDE of a class definition.
<br>
<br>//const MIN_VALUE = 0.0;&#xA0; &#xA0; &#xA0; &#xA0;&#xA0; RIGHT - Works both INSIDE and OUTSIDE of a class definition.
<br>//const MAX_VALUE = 1.0;&#xA0; &#xA0; &#xA0; &#xA0;&#xA0; RIGHT - Works both INSIDE and OUTSIDE of a class definition.
<br>
<br></span><span class="keyword">class </span><span class="default">Constants
<br></span><span class="keyword">{
<br>&#xA0; </span><span class="comment">//define(&apos;MIN_VALUE&apos;, &apos;0.0&apos;);&#xA0; WRONG - Works OUTSIDE of a class definition.
<br>&#xA0; //define(&apos;MAX_VALUE&apos;, &apos;1.0&apos;);&#xA0; WRONG - Works OUTSIDE of a class definition.
<br>
<br>&#xA0; </span><span class="keyword">const </span><span class="default">MIN_VALUE </span><span class="keyword">= </span><span class="default">0.0</span><span class="keyword">;&#xA0; &#xA0; &#xA0; </span><span class="comment">// RIGHT - Works INSIDE of a class definition.
<br>&#xA0; </span><span class="keyword">const </span><span class="default">MAX_VALUE </span><span class="keyword">= </span><span class="default">1.0</span><span class="keyword">;&#xA0; &#xA0; &#xA0; </span><span class="comment">// RIGHT - Works INSIDE of a class definition.
<br>
<br>&#xA0; </span><span class="keyword">public static function </span><span class="default">getMinValue</span><span class="keyword">()
<br>&#xA0; {
<br>&#xA0; &#xA0; return </span><span class="default">self</span><span class="keyword">::</span><span class="default">MIN_VALUE</span><span class="keyword">;
<br>&#xA0; }
<br>
<br>&#xA0; public static function </span><span class="default">getMaxValue</span><span class="keyword">()
<br>&#xA0; {
<br>&#xA0; &#xA0; return </span><span class="default">self</span><span class="keyword">::</span><span class="default">MAX_VALUE</span><span class="keyword">;
<br>&#xA0; }
<br>}
<br>
<br></span><span class="default">?&gt;
<br></span>
<br>#Example 1:
<br>You can access these constants DIRECTLY like so:
<br> * type the class name exactly.
<br> * type two (2) colons.
<br> * type the const name exactly.
<br>
<br>#Example 2:
<br>Because our class definition provides two (2) static functions, you can also access them like so:
<br> * type the class name exactly.
<br> * type two (2) colons.
<br> * type the function name exactly (with the parentheses).
<br>
<br><span class="default">&lt;?php
<br>
<br></span><span class="comment">#Example 1:
<br></span><span class="default">$min </span><span class="keyword">= </span><span class="default">Constants</span><span class="keyword">::</span><span class="default">MIN_VALUE</span><span class="keyword">;
<br></span><span class="default">$max </span><span class="keyword">= </span><span class="default">Constants</span><span class="keyword">::</span><span class="default">MAX_VALUE</span><span class="keyword">;
<br>
<br></span><span class="comment">#Example 2:
<br></span><span class="default">$min </span><span class="keyword">= </span><span class="default">Constants</span><span class="keyword">::</span><span class="default">getMinValue</span><span class="keyword">();
<br></span><span class="default">$max </span><span class="keyword">= </span><span class="default">Constants</span><span class="keyword">::</span><span class="default">getMaxValue</span><span class="keyword">();
<br>
<br></span><span class="default">?&gt;
<br></span>
<br>Once class constants are declared AND initialized, they cannot be set to different values -- that is why there are no setMinValue() and setMaxValue() functions in the class definition -- which means they are READ-ONLY and STATIC (shared by all instances of the class).</span>
</div>
  

#


<div class="phpcode"><span class="html">
class constant are by default public in nature but they cannot be assigned visibility factor and in turn gives syntax error<br><br><span class="default">&lt;?php<br><br></span><span class="keyword">class </span><span class="default">constants </span><span class="keyword">{<br><br>&#xA0; &#xA0; const </span><span class="default">MAX_VALUE </span><span class="keyword">= </span><span class="default">10</span><span class="keyword">;<br>&#xA0; &#xA0; &#xA0; &#xA0; public const </span><span class="default">MIN_VALUE </span><span class="keyword">=</span><span class="default">1</span><span class="keyword">;<br><br>}<br><br></span><span class="comment">// This will work<br></span><span class="keyword">echo </span><span class="default">constants</span><span class="keyword">::</span><span class="default">MAX_VALUE</span><span class="keyword">;<br><br></span><span class="comment">// This will return syntax error <br></span><span class="keyword">echo </span><span class="default">constants</span><span class="keyword">::</span><span class="default">MIN_VALUE</span><span class="keyword">; <br></span><span class="default">?&gt;</span>
</span>
</div>
  

#


<div class="phpcode"><span class="html">
Lets expand comment of &apos;storm&apos; about usage of undefined constants. His claim that &apos;An undefined constant evaluates as true...&apos; is wrong and right at same time. As said further in documentation &apos; If you use an undefined constant, PHP assumes that you mean the name of the constant itself, just as if you called it as a string...&apos;. So yeah, undefined global constant when accessed directly will be resolved as string equal to name of sought constant (as thought PHP supposes that programmer had forgot apostrophes and autofixes it) and non-zero non-empty string converts to True.<br><br>There are two ways to prevent this:<br>1. always use function constant(&apos;CONST_NAME&apos;) to get constant value (BTW it also works for class constants - constant(&apos;CLASS_NAME::CONST_NAME&apos;) );<br>2. use only class constants (that are defined inside of class using keyword const) because they are not converted to string when not found but throw exception instead (Fatal error: Undefined class constant).</span>
</div>
  

#


<div class="phpcode"><span class="html">
I find using the concatenation operator helps disambiguate value assignments with constants. For example, setting constants in a global configuration file:
<br>
<br><span class="default">&lt;?php
<br>define</span><span class="keyword">(</span><span class="string">&apos;LOCATOR&apos;</span><span class="keyword">,&#xA0;&#xA0; </span><span class="string">&quot;/locator&quot;</span><span class="keyword">);
<br></span><span class="default">define</span><span class="keyword">(</span><span class="string">&apos;CLASSES&apos;</span><span class="keyword">,&#xA0;&#xA0; </span><span class="default">LOCATOR</span><span class="keyword">.</span><span class="string">&quot;/code/classes&quot;</span><span class="keyword">);
<br></span><span class="default">define</span><span class="keyword">(</span><span class="string">&apos;FUNCTIONS&apos;</span><span class="keyword">, </span><span class="default">LOCATOR</span><span class="keyword">.</span><span class="string">&quot;/code/functions&quot;</span><span class="keyword">);
<br></span><span class="default">define</span><span class="keyword">(</span><span class="string">&apos;USERDIR&apos;</span><span class="keyword">,&#xA0;&#xA0; </span><span class="default">LOCATOR</span><span class="keyword">.</span><span class="string">&quot;/user&quot;</span><span class="keyword">);
<br></span><span class="default">?&gt;
<br></span>
<br>Later, I can use the same convention when invoking a constant&apos;s value for static constructs such as require() calls:
<br>
<br><span class="default">&lt;?php
<br></span><span class="keyword">require_once(</span><span class="default">FUNCTIONS</span><span class="keyword">.</span><span class="string">&quot;/database.fnc&quot;</span><span class="keyword">);
<br>require_once(</span><span class="default">FUNCTIONS</span><span class="keyword">.</span><span class="string">&quot;/randchar.fnc&quot;</span><span class="keyword">);
<br></span><span class="default">?&gt;
<br></span>
<br>as well as dynamic constructs, typical of value assignment to variables:
<br>
<br><span class="default">&lt;?php
<br>$userid&#xA0; </span><span class="keyword">= </span><span class="default">randchar</span><span class="keyword">(</span><span class="default">8</span><span class="keyword">,</span><span class="string">&apos;anc&apos;</span><span class="keyword">,</span><span class="string">&apos;u&apos;</span><span class="keyword">);
<br></span><span class="default">$usermap </span><span class="keyword">= </span><span class="default">USERDIR</span><span class="keyword">.</span><span class="string">&quot;/&quot;</span><span class="keyword">.</span><span class="default">$userid</span><span class="keyword">.</span><span class="string">&quot;.png&quot;</span><span class="keyword">;
<br></span><span class="default">?&gt;
<br></span>
<br>The above convention works for me, and helps produce self-documenting code.
<br>
<br>-- Erich</span>
</div>
  

#


<div class="phpcode"><span class="html">
PHP Modules also define constants.&#xA0; Make sure to avoid constant name collisions.&#xA0; There are two ways to do this that I can think of.<br>First: in your code make sure that the constant name is not already used.&#xA0; ex. <span class="default">&lt;?php </span><span class="keyword">if (! </span><span class="default">defined</span><span class="keyword">(</span><span class="string">&quot;CONSTANT_NAME&quot;</span><span class="keyword">)) { </span><span class="default">Define</span><span class="keyword">(</span><span class="string">&quot;CONSTANT_NAME&quot;</span><span class="keyword">,</span><span class="string">&quot;Some Value&quot;</span><span class="keyword">); } </span><span class="default">?&gt;</span>&#xA0; This can get messy when you start thinking about collision handling, and the implications of this.<br>Second: Use some off prepend to all your constant names without exception&#xA0; ex. <span class="default">&lt;?php Define</span><span class="keyword">(</span><span class="string">&quot;SITE_CONSTANT_NAME&quot;</span><span class="keyword">,</span><span class="string">&quot;Some Value&quot;</span><span class="keyword">); </span><span class="default">?&gt;<br></span><br>Perhaps the developers or documentation maintainers could recommend a good prepend and ask module writers to avoid that prepend in modules.</span>
</div>
  

#


<div class="phpcode"><span class="html">
Warning, constants used within the heredoc syntax (<a href="http://www.php.net/manual/en/language.types.string.php" rel="nofollow" target="_blank">http://www.php.net/manual/en/language.types.string.php</a>) are not interpreted!
<br>
<br>Editor&apos;s Note: This is true. PHP has no way of recognizing the constant from any other string of characters within the heredoc block.</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/language.constants.php)

**[To root](/README.md)**