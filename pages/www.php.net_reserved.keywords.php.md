# List of Keywords



RegEx to find all the keywords:<br><br>\b(<br>(a(bstract|nd|rray|s))|<br>(c(a(llable|se|tch)|l(ass|one)|on(st|tinue)))|<br>(d(e(clare|fault)|ie|o))|<br>(e(cho|lse(if)?|mpty|nd(declare|for(each)?|if|switch|while)|val|x(it|tends)))|<br>(f(inal|or(each)?|unction))|<br>(g(lobal|oto))|<br>(i(f|mplements|n(clude(_once)?|st(anceof|eadof)|terface)|sset))|<br>(n(amespace|ew))|<br>(p(r(i(nt|vate)|otected)|ublic))|<br>(re(quire(_once)?|turn))|<br>(s(tatic|witch))|<br>(t(hrow|r(ait|y)))|<br>(u(nset|se))|<br>(__halt_compiler|break|list|(x)?or|var|while)<br>)\b  

---

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

---

Here they are as arrays:<br><br>

```
<?php
$keywords = array('__halt_compiler', 'abstract', 'and', 'array', 'as', 'break', 'callable', 'case', 'catch', 'class', 'clone', 'const', 'continue', 'declare', 'default', 'die', 'do', 'echo', 'else', 'elseif', 'empty', 'enddeclare', 'endfor', 'endforeach', 'endif', 'endswitch', 'endwhile', 'eval', 'exit', 'extends', 'final', 'for', 'foreach', 'function', 'global', 'goto', 'if', 'implements', 'include', 'include_once', 'instanceof', 'insteadof', 'interface', 'isset', 'list', 'namespace', 'new', 'or', 'print', 'private', 'protected', 'public', 'require', 'require_once', 'return', 'static', 'switch', 'throw', 'trait', 'try', 'unset', 'use', 'var', 'while', 'xor');

$predefined_constants = array('__CLASS__', '__DIR__', '__FILE__', '__FUNCTION__', '__LINE__', '__METHOD__', '__NAMESPACE__', '__TRAIT__');
?>
```
<br><br>Along with get_defined_functions() and get_defined_constants(), this can be useful for checking eval() statements.  

---

[Official documentation page](https://www.php.net/manual/en/reserved.keywords.php)

**[To root](/README.md)**