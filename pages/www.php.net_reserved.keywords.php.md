# List of Keywords




<div class="phpcode"><span class="html">
RegEx to find all the keywords:<br><br>\b(<br>(a(bstract|nd|rray|s))|<br>(c(a(llable|se|tch)|l(ass|one)|on(st|tinue)))|<br>(d(e(clare|fault)|ie|o))|<br>(e(cho|lse(if)?|mpty|nd(declare|for(each)?|if|switch|while)|val|x(it|tends)))|<br>(f(inal|or(each)?|unction))|<br>(g(lobal|oto))|<br>(i(f|mplements|n(clude(_once)?|st(anceof|eadof)|terface)|sset))|<br>(n(amespace|ew))|<br>(p(r(i(nt|vate)|otected)|ublic))|<br>(re(quire(_once)?|turn))|<br>(s(tatic|witch))|<br>(t(hrow|r(ait|y)))|<br>(u(nset|se))|<br>(__halt_compiler|break|list|(x)?or|var|while)<br>)\b</span>
</div>
  

#


<div class="phpcode"><span class="html">
Please note that reserved words are still not allowed to be used as namespace or as part of it:<br><br><span class="default">&lt;?php<br></span><span class="keyword">namespace </span><span class="default">MyNameSpace</span><span class="keyword">\List;<br><br>class </span><span class="default">Test<br></span><span class="keyword">{<br>}<br></span><span class="default">?&gt;<br></span><br>This will fail with a Parse error:&#xA0; syntax error, unexpected &apos;List&apos; (T_LIST), expecting identifier (T_STRING)</span>
</div>
  

#


<div class="phpcode"><span class="html">
Here they are as arrays:<br><br><span class="default">&lt;?php<br>$keywords </span><span class="keyword">= array(</span><span class="string">&apos;__halt_compiler&apos;</span><span class="keyword">, </span><span class="string">&apos;abstract&apos;</span><span class="keyword">, </span><span class="string">&apos;and&apos;</span><span class="keyword">, </span><span class="string">&apos;array&apos;</span><span class="keyword">, </span><span class="string">&apos;as&apos;</span><span class="keyword">, </span><span class="string">&apos;break&apos;</span><span class="keyword">, </span><span class="string">&apos;callable&apos;</span><span class="keyword">, </span><span class="string">&apos;case&apos;</span><span class="keyword">, </span><span class="string">&apos;catch&apos;</span><span class="keyword">, </span><span class="string">&apos;class&apos;</span><span class="keyword">, </span><span class="string">&apos;clone&apos;</span><span class="keyword">, </span><span class="string">&apos;const&apos;</span><span class="keyword">, </span><span class="string">&apos;continue&apos;</span><span class="keyword">, </span><span class="string">&apos;declare&apos;</span><span class="keyword">, </span><span class="string">&apos;default&apos;</span><span class="keyword">, </span><span class="string">&apos;die&apos;</span><span class="keyword">, </span><span class="string">&apos;do&apos;</span><span class="keyword">, </span><span class="string">&apos;echo&apos;</span><span class="keyword">, </span><span class="string">&apos;else&apos;</span><span class="keyword">, </span><span class="string">&apos;elseif&apos;</span><span class="keyword">, </span><span class="string">&apos;empty&apos;</span><span class="keyword">, </span><span class="string">&apos;enddeclare&apos;</span><span class="keyword">, </span><span class="string">&apos;endfor&apos;</span><span class="keyword">, </span><span class="string">&apos;endforeach&apos;</span><span class="keyword">, </span><span class="string">&apos;endif&apos;</span><span class="keyword">, </span><span class="string">&apos;endswitch&apos;</span><span class="keyword">, </span><span class="string">&apos;endwhile&apos;</span><span class="keyword">, </span><span class="string">&apos;eval&apos;</span><span class="keyword">, </span><span class="string">&apos;exit&apos;</span><span class="keyword">, </span><span class="string">&apos;extends&apos;</span><span class="keyword">, </span><span class="string">&apos;final&apos;</span><span class="keyword">, </span><span class="string">&apos;for&apos;</span><span class="keyword">, </span><span class="string">&apos;foreach&apos;</span><span class="keyword">, </span><span class="string">&apos;function&apos;</span><span class="keyword">, </span><span class="string">&apos;global&apos;</span><span class="keyword">, </span><span class="string">&apos;goto&apos;</span><span class="keyword">, </span><span class="string">&apos;if&apos;</span><span class="keyword">, </span><span class="string">&apos;implements&apos;</span><span class="keyword">, </span><span class="string">&apos;include&apos;</span><span class="keyword">, </span><span class="string">&apos;include_once&apos;</span><span class="keyword">, </span><span class="string">&apos;instanceof&apos;</span><span class="keyword">, </span><span class="string">&apos;insteadof&apos;</span><span class="keyword">, </span><span class="string">&apos;interface&apos;</span><span class="keyword">, </span><span class="string">&apos;isset&apos;</span><span class="keyword">, </span><span class="string">&apos;list&apos;</span><span class="keyword">, </span><span class="string">&apos;namespace&apos;</span><span class="keyword">, </span><span class="string">&apos;new&apos;</span><span class="keyword">, </span><span class="string">&apos;or&apos;</span><span class="keyword">, </span><span class="string">&apos;print&apos;</span><span class="keyword">, </span><span class="string">&apos;private&apos;</span><span class="keyword">, </span><span class="string">&apos;protected&apos;</span><span class="keyword">, </span><span class="string">&apos;public&apos;</span><span class="keyword">, </span><span class="string">&apos;require&apos;</span><span class="keyword">, </span><span class="string">&apos;require_once&apos;</span><span class="keyword">, </span><span class="string">&apos;return&apos;</span><span class="keyword">, </span><span class="string">&apos;static&apos;</span><span class="keyword">, </span><span class="string">&apos;switch&apos;</span><span class="keyword">, </span><span class="string">&apos;throw&apos;</span><span class="keyword">, </span><span class="string">&apos;trait&apos;</span><span class="keyword">, </span><span class="string">&apos;try&apos;</span><span class="keyword">, </span><span class="string">&apos;unset&apos;</span><span class="keyword">, </span><span class="string">&apos;use&apos;</span><span class="keyword">, </span><span class="string">&apos;var&apos;</span><span class="keyword">, </span><span class="string">&apos;while&apos;</span><span class="keyword">, </span><span class="string">&apos;xor&apos;</span><span class="keyword">);<br><br></span><span class="default">$predefined_constants </span><span class="keyword">= array(</span><span class="string">&apos;__CLASS__&apos;</span><span class="keyword">, </span><span class="string">&apos;__DIR__&apos;</span><span class="keyword">, </span><span class="string">&apos;__FILE__&apos;</span><span class="keyword">, </span><span class="string">&apos;__FUNCTION__&apos;</span><span class="keyword">, </span><span class="string">&apos;__LINE__&apos;</span><span class="keyword">, </span><span class="string">&apos;__METHOD__&apos;</span><span class="keyword">, </span><span class="string">&apos;__NAMESPACE__&apos;</span><span class="keyword">, </span><span class="string">&apos;__TRAIT__&apos;</span><span class="keyword">);<br></span><span class="default">?&gt;<br></span><br>Along with get_defined_functions() and get_defined_constants(), this can be useful for checking eval() statements.</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/reserved.keywords.php)

**[To root](/README.md)**