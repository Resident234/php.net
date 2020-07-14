# List of Keywords



RegEx to find all the keywords:<br><br>\b(<br>(a(bstract|nd|rray|s))|<br>(c(a(llable|se|tch)|l(ass|one)|on(st|tinue)))|<br>(d(e(clare|fault)|ie|o))|<br>(e(cho|lse(if)?|mpty|nd(declare|for(each)?|if|switch|while)|val|x(it|tends)))|<br>(f(inal|or(each)?|unction))|<br>(g(lobal|oto))|<br>(i(f|mplements|n(clude(_once)?|st(anceof|eadof)|terface)|sset))|<br>(n(amespace|ew))|<br>(p(r(i(nt|vate)|otected)|ublic))|<br>(re(quire(_once)?|turn))|<br>(s(tatic|witch))|<br>(t(hrow|r(ait|y)))|<br>(u(nset|se))|<br>(__halt_compiler|break|list|(x)?or|var|while)<br>)\b  

#

Please note that reserved words are still not allowed to be used as namespace or as part of it:<br><br>

```
<?php
namespace MyNameSpace\List;

class Test
{
}
?>
```
<br><br>This will fail with a Parse error:  syntax error, unexpected &apos;List&apos; (T_LIST), expecting identifier (T_STRING)  

#

Here they are as arrays:<br><br>

```
<?php
$keywords = array(&apos;__halt_compiler&apos;, &apos;abstract&apos;, &apos;and&apos;, &apos;array&apos;, &apos;as&apos;, &apos;break&apos;, &apos;callable&apos;, &apos;case&apos;, &apos;catch&apos;, &apos;class&apos;, &apos;clone&apos;, &apos;const&apos;, &apos;continue&apos;, &apos;declare&apos;, &apos;default&apos;, &apos;die&apos;, &apos;do&apos;, &apos;echo&apos;, &apos;else&apos;, &apos;elseif&apos;, &apos;empty&apos;, &apos;enddeclare&apos;, &apos;endfor&apos;, &apos;endforeach&apos;, &apos;endif&apos;, &apos;endswitch&apos;, &apos;endwhile&apos;, &apos;eval&apos;, &apos;exit&apos;, &apos;extends&apos;, &apos;final&apos;, &apos;for&apos;, &apos;foreach&apos;, &apos;function&apos;, &apos;global&apos;, &apos;goto&apos;, &apos;if&apos;, &apos;implements&apos;, &apos;include&apos;, &apos;include_once&apos;, &apos;instanceof&apos;, &apos;insteadof&apos;, &apos;interface&apos;, &apos;isset&apos;, &apos;list&apos;, &apos;namespace&apos;, &apos;new&apos;, &apos;or&apos;, &apos;print&apos;, &apos;private&apos;, &apos;protected&apos;, &apos;public&apos;, &apos;require&apos;, &apos;require_once&apos;, &apos;return&apos;, &apos;static&apos;, &apos;switch&apos;, &apos;throw&apos;, &apos;trait&apos;, &apos;try&apos;, &apos;unset&apos;, &apos;use&apos;, &apos;var&apos;, &apos;while&apos;, &apos;xor&apos;);

$predefined_constants = array(&apos;__CLASS__&apos;, &apos;__DIR__&apos;, &apos;__FILE__&apos;, &apos;__FUNCTION__&apos;, &apos;__LINE__&apos;, &apos;__METHOD__&apos;, &apos;__NAMESPACE__&apos;, &apos;__TRAIT__&apos;);
?>
```
<br><br>Along with get_defined_functions() and get_defined_constants(), this can be useful for checking eval() statements.  

#

[Official documentation page](https://www.php.net/manual/en/reserved.keywords.php)

**[To root](/README.md)**