# php_strip_whitespace



With this function You can compress Your PHP source code.<br><br>

```
<?php

function compress_php_src($src) {
    // Whitespaces left and right from this signs can be ignored
    static $IW = array(
        T_CONCAT_EQUAL,             // .=
        T_DOUBLE_ARROW,             // =&gt;
        T_BOOLEAN_AND,              // &amp;&amp;
        T_BOOLEAN_OR,               // ||
        T_IS_EQUAL,                 // ==
        T_IS_NOT_EQUAL,             // != or &lt;&gt;
        T_IS_SMALLER_OR_EQUAL,      // &lt;=
        T_IS_GREATER_OR_EQUAL,      // &gt;=
        T_INC,                      // ++
        T_DEC,                      // --
        T_PLUS_EQUAL,               // +=
        T_MINUS_EQUAL,              // -=
        T_MUL_EQUAL,                // *=
        T_DIV_EQUAL,                // /=
        T_IS_IDENTICAL,             // ===
        T_IS_NOT_IDENTICAL,         // !==
        T_DOUBLE_COLON,             // ::
        T_PAAMAYIM_NEKUDOTAYIM,     // ::
        T_OBJECT_OPERATOR,          // -&gt;
        T_DOLLAR_OPEN_CURLY_BRACES, // ${
        T_AND_EQUAL,                // &amp;=
        T_MOD_EQUAL,                // %=
        T_XOR_EQUAL,                // ^=
        T_OR_EQUAL,                 // |=
        T_SL,                       // &lt;&lt;
        T_SR,                       // &gt;&gt;
        T_SL_EQUAL,                 // &lt;&lt;=
        T_SR_EQUAL,                 // &gt;&gt;=
    );
    if(is_file($src)) {
        if(!$src = file_get_contents($src)) {
            return false;
        }
    }
    $tokens = token_get_all($src);
    
    $new = "";
    $c = sizeof($tokens);
    $iw = false; // ignore whitespace
    $ih = false; // in HEREDOC
    $ls = "";    // last sign
    $ot = null;  // open tag
    for($i = 0; $i &lt; $c; $i++) {
        $token = $tokens[$i];
        if(is_array($token)) {
            list($tn, $ts) = $token; // tokens: number, string, line
            $tname = token_name($tn);
            if($tn == T_INLINE_HTML) {
                $new .= $ts;
                $iw = false;
            } else {
                if($tn == T_OPEN_TAG) {
                    if(strpos($ts, " ") || strpos($ts, "\n") || strpos($ts, "\t") || strpos($ts, "\r")) {
                        $ts = rtrim($ts);
                    }
                    $ts .= " ";
                    $new .= $ts;
                    $ot = T_OPEN_TAG;
                    $iw = true;
                } elseif($tn == T_OPEN_TAG_WITH_ECHO) {
                    $new .= $ts;
                    $ot = T_OPEN_TAG_WITH_ECHO;
                    $iw = true;
                } elseif($tn == T_CLOSE_TAG) {
                    if($ot == T_OPEN_TAG_WITH_ECHO) {
                        $new = rtrim($new, "; ");
                    } else {
                        $ts = " ".$ts;
                    }
                    $new .= $ts;
                    $ot = null;
                    $iw = false;
                } elseif(in_array($tn, $IW)) {
                    $new .= $ts;
                    $iw = true;
                } elseif($tn == T_CONSTANT_ENCAPSED_STRING
                       || $tn == T_ENCAPSED_AND_WHITESPACE)
                {
                    if($ts[0] == &apos;"&apos;) {
                        $ts = addcslashes($ts, "\n\t\r");
                    }
                    $new .= $ts;
                    $iw = true;
                } elseif($tn == T_WHITESPACE) {
                    $nt = @$tokens[$i+1];
                    if(!$iw &amp;&amp; (!is_string($nt) || $nt == &apos;

```
<?php<br><br>function compress_php_src($src) {<br>    // Whitespaces left and right from this signs can be ignored<br>    static $IW = array(<br>        T_CONCAT_EQUAL,             // .=<br>        T_DOUBLE_ARROW,             // =&gt;<br>        T_BOOLEAN_AND,              // &amp;&amp;<br>        T_BOOLEAN_OR,               // ||<br>        T_IS_EQUAL,                 // ==<br>        T_IS_NOT_EQUAL,             // != or &lt;&gt;<br>        T_IS_SMALLER_OR_EQUAL,      // &lt;=<br>        T_IS_GREATER_OR_EQUAL,      // &gt;=<br>        T_INC,                      // ++<br>        T_DEC,                      // --<br>        T_PLUS_EQUAL,               // +=<br>        T_MINUS_EQUAL,              // -=<br>        T_MUL_EQUAL,                // *=<br>        T_DIV_EQUAL,                // /=<br>        T_IS_IDENTICAL,             // ===<br>        T_IS_NOT_IDENTICAL,         // !==<br>        T_DOUBLE_COLON,             // ::<br>        T_PAAMAYIM_NEKUDOTAYIM,     // ::<br>        T_OBJECT_OPERATOR,          // -&gt;<br>        T_DOLLAR_OPEN_CURLY_BRACES, // ${<br>        T_AND_EQUAL,                // &amp;=<br>        T_MOD_EQUAL,                // %=<br>        T_XOR_EQUAL,                // ^=<br>        T_OR_EQUAL,                 // |=<br>        T_SL,                       // &lt;&lt;<br>        T_SR,                       // &gt;&gt;<br>        T_SL_EQUAL,                 // &lt;&lt;=<br>        T_SR_EQUAL,                 // &gt;&gt;=<br>    );<br>    if(is_file($src)) {<br>        if(!$src = file_get_contents($src)) {<br>            return false;<br>        }<br>    }<br>    $tokens = token_get_all($src);<br>    <br>    $new = "";<br>    $c = sizeof($tokens);<br>    $iw = false; // ignore whitespace<br>    $ih = false; // in HEREDOC<br>    $ls = "";    // last sign<br>    $ot = null;  // open tag<br>    for($i = 0; $i &lt; $c; $i++) {<br>        $token = $tokens[$i];<br>        if(is_array($token)) {<br>            list($tn, $ts) = $token; // tokens: number, string, line<br>            $tname = token_name($tn);<br>            if($tn == T_INLINE_HTML) {<br>                $new .= $ts;<br>                $iw = false;<br>            } else {<br>                if($tn == T_OPEN_TAG) {<br>                    if(strpos($ts, " ") || strpos($ts, "\n") || strpos($ts, "\t") || strpos($ts, "\r")) {<br>                        $ts = rtrim($ts);<br>                    }<br>                    $ts .= " ";<br>                    $new .= $ts;<br>                    $ot = T_OPEN_TAG;<br>                    $iw = true;<br>                } elseif($tn == T_OPEN_TAG_WITH_ECHO) {<br>                    $new .= $ts;<br>                    $ot = T_OPEN_TAG_WITH_ECHO;<br>                    $iw = true;<br>                } elseif($tn == T_CLOSE_TAG) {<br>                    if($ot == T_OPEN_TAG_WITH_ECHO) {<br>                        $new = rtrim($new, "; ");<br>                    } else {<br>                        $ts = " ".$ts;<br>                    }<br>                    $new .= $ts;<br>                    $ot = null;<br>                    $iw = false;<br>                } elseif(in_array($tn, $IW)) {<br>                    $new .= $ts;<br>                    $iw = true;<br>                } elseif($tn == T_CONSTANT_ENCAPSED_STRING<br>                       || $tn == T_ENCAPSED_AND_WHITESPACE)<br>                {<br>                    if($ts[0] == &apos;"&apos;) {<br>                        $ts = addcslashes($ts, "\n\t\r");<br>                    }<br>                    $new .= $ts;<br>                    $iw = true;<br>                } elseif($tn == T_WHITESPACE) {<br>                    $nt = @$tokens[$i+1];<br>                    if(!$iw &amp;&amp; (!is_string($nt) || $nt == &apos;$&apos;) &amp;&amp; !in_array($nt[0], $IW)) {<br>                        $new .= " ";<br>                    }<br>                    $iw = false;<br>                } elseif($tn == T_START_HEREDOC) {<br>                    $new .= "&lt;&lt;&lt;S\n";<br>                    $iw = false;<br>                    $ih = true; // in HEREDOC<br>                } elseif($tn == T_END_HEREDOC) {<br>                    $new .= "S;";<br>                    $iw = true;<br>                    $ih = false; // in HEREDOC<br>                    for($j = $i+1; $j &lt; $c; $j++) {<br>                        if(is_string($tokens[$j]) &amp;&amp; $tokens[$j] == ";") {<br>                            $i = $j;<br>                            break;<br>                        } else if($tokens[$j][0] == T_CLOSE_TAG) {<br>                            break;<br>                        }<br>                    }<br>                } elseif($tn == T_COMMENT || $tn == T_DOC_COMMENT) {<br>                    $iw = true;<br>                } else {<br>                    if(!$ih) {<br>                        $ts = strtolower($ts);<br>                    }<br>                    $new .= $ts;<br>                    $iw = false;<br>                }<br>            }<br>            $ls = "";<br>        } else {<br>            if(($token != ";" &amp;&amp; $token != ":") || $ls != $token) {<br>                $new .= $token;<br>                $ls = $token;<br>            }<br>            $iw = true;<br>        }<br>    }<br>    return $new;<br>}<br><br>?>
```
<br><br>For example:<br>

```
<?php<br><br>$src = &lt;&lt;&lt;EOT<br>

```
<?php<br>// some comment<br>for ( $i = 0; $i &lt; 99; $i ++ ) {<br>   echo "i=${ i }\n";<br>   /* ... */<br>}<br>/** ... */<br>function abc() {<br>   return   "abc";<br>};<br><br>abc();<br>?>
```
<br>&lt;h1&gt;&lt;?= "Some text " . str_repeat("_-x-_ ", 32);;; ?>
```
&lt;/h1&gt;<br>EOT;<br>var_dump(compress_php_src($src));<br>?>
```
<br><br>And the result is:<br>string(125) "

```
<?php for(=0;&lt;99;++){echo "i=\n";}function abc(){return "abc";};abc(); ?>
```
<br>&lt;h1&gt;&lt;?="Some text ".str_repeat("_-x-_ ",32)?>
```
apos;) &amp;&amp; !in_array($nt[0], $IW)) {
                        $new .= " ";
                    }
                    $iw = false;
                } elseif($tn == T_START_HEREDOC) {
                    $new .= "&lt;&lt;&lt;S\n";
                    $iw = false;
                    $ih = true; // in HEREDOC
                } elseif($tn == T_END_HEREDOC) {
                    $new .= "S;";
                    $iw = true;
                    $ih = false; // in HEREDOC
                    for($j = $i+1; $j &lt; $c; $j++) {
                        if(is_string($tokens[$j]) &amp;&amp; $tokens[$j] == ";") {
                            $i = $j;
                            break;
                        } else if($tokens[$j][0] == T_CLOSE_TAG) {
                            break;
                        }
                    }
                } elseif($tn == T_COMMENT || $tn == T_DOC_COMMENT) {
                    $iw = true;
                } else {
                    if(!$ih) {
                        $ts = strtolower($ts);
                    }
                    $new .= $ts;
                    $iw = false;
                }
            }
            $ls = "";
        } else {
            if(($token != ";" &amp;&amp; $token != ":") || $ls != $token) {
                $new .= $token;
                $ls = $token;
            }
            $iw = true;
        }
    }
    return $new;
}

?>
```


For example:


```
<?php

$src = &lt;&lt;&lt;EOT


```
<?php
// some comment
for ( $i = 0; $i &lt; 99; $i ++ ) {
   echo "i=${ i }\n";
   /* ... */
}
/** ... */
function abc() {
   return   "abc";
};

abc();
?>
```

&lt;h1&gt;&lt;?= "Some text " . str_repeat("_-x-_ ", 32);;; ?>
```
&lt;/h1&gt;
EOT;
var_dump(compress_php_src($src));
?>
```


And the result is:
string(125) "

```
<?php for(=0;&lt;99;++){echo "i=\n";}function abc(){return "abc";};abc(); ?>
```

&lt;h1&gt;&lt;?="Some text ".str_repeat("_-x-_ ",32)?>
```
&lt;/h1&gt;"  

#

[Official documentation page](https://www.php.net/manual/en/function.php-strip-whitespace.php)

**[To root](/README.md)**