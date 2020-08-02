# strip_tags



Hi. I made a function that removes the HTML tags along with their contents:<br><br>Function:<br>

```
<?php
function strip_tags_content($text, $tags = '', $invert = FALSE) {

  preg_match_all('/<(.+?)[\s]*\/?[\s]*>/si', trim($tags), $tags);
  $tags = array_unique($tags[1]);
    
  if(is_array($tags) AND count($tags) > 0) {
    if($invert == FALSE) {
      return preg_replace('@<(?!(?:'. implode('|', $tags) .')\b)(\w+)\b.*?>.*?</\1>@si', '', $text);
    }
    else {
      return preg_replace('@<('. implode('|', $tags) .')\b.*?>.*?</\1>@si', '', $text);
    }
  }
  elseif($invert == FALSE) {
    return preg_replace('@<(\w+)\b.*?>.*?</\1>@si', '', $text);
  }
  return $text;
}
?>
```
<br><br>Sample text:<br>$text = &apos;&lt;b&gt;sample&lt;/b&gt; text with &lt;div&gt;tags&lt;/div&gt;&apos;;<br><br>Result for strip_tags($text):<br>sample text with tags<br><br>Result for strip_tags_content($text):<br> text with <br><br>Result for strip_tags_content($text, &apos;&lt;b&gt;&apos;):<br>&lt;b&gt;sample&lt;/b&gt; text with <br><br>Result for strip_tags_content($text, &apos;&lt;b&gt;&apos;, TRUE);<br> text with &lt;div&gt;tags&lt;/div&gt;<br><br>I hope that someone is useful :)  

---

Since strip_tags does not remove attributes and thus creates a potential XSS security hole, here is a small function I wrote to allow only specific tags with specific attributes and strip all other tags and attributes.<br><br>If you only allow formatting tags such as b, i, and p, and styling attributes such as class, id and style, this will strip all javascript including event triggers in formatting tags.<br><br>Note that allowing anchor tags or href attributes opens another potential security hole that this solution won&apos;t protect against. You&apos;ll need more comprehensive protection if you plan to allow links in your text.<br><br>

```
<?php
function stripUnwantedTagsAndAttrs($html_str){
  $xml = new DOMDocument();
//Suppress warnings: proper error handling is beyond scope of example
  libxml_use_internal_errors(true);
//List the tags you want to allow here, NOTE you MUST allow html and body otherwise entire string will be cleared
  $allowed_tags = array("html", "body", "b", "br", "em", "hr", "i", "li", "ol", "p", "s", "span", "table", "tr", "td", "u", "ul");
//List the attributes you want to allow here
  $allowed_attrs = array ("class", "id", "style");
  if (!strlen($html_str)){return false;}
  if ($xml->loadHTML($html_str, LIBXML_HTML_NOIMPLIED | LIBXML_HTML_NODEFDTD)){
    foreach ($xml->getElementsByTagName("*") as $tag){
      if (!in_array($tag->tagName, $allowed_tags)){
        $tag->parentNode->removeChild($tag);
      }else{
        foreach ($tag->attributes as $attr){
          if (!in_array($attr->nodeName, $allowed_attrs)){
            $tag->removeAttribute($attr->nodeName);
          }
        }
      }
    }
  }
  return $xml->saveHTML();
}
?>
```
  

---

A word of caution. strip_tags() can actually be used for input validation as long as you remove ANY tag. As soon as you accept a single tag (2nd parameter), you are opening up a security hole such as this:<br><br>&lt;acceptedTag onLoad="javascript:malicious()" /&gt;<br><br>Plus: regexing away attributes or code block is really not the right solution. For effective input validation when using strip_tags() with even a single tag accepted, http://htmlpurifier.org/ is the way to go.  

---

a HTML code like this: <br><br>

```
<?php
$html = '
<div>
<p style="color:blue;">color is blue</p><p>size is <span style="font-size:200%;">huge</span></p>
<p>material is wood</p>
</div>
'; 
?>
```


with 

```
<?php $str = strip_tags($html); ?>
```

... the result is:

$str = 'color is bluesize is huge
material is wood'; 

notice: the words 'blue' and 'size' grow together :( 
and line-breaks are still in new string $str

if you need a space between the words (and without line-break) 
use my function: 

```
<?php $str = rip_tags($html); ?>
```

... the result is:

$str = 'color is blue size is huge material is wood'; 

the function: 



```
<?php
// -------------------------------------------------------------- 

function rip_tags($string) { 
    
    // ----- remove HTML TAGs ----- 
    $string = preg_replace ('/<[^>]*>/', ' ', $string); 
    
    // ----- remove control characters ----- 
    $string = str_replace("\r", '', $string);    // --- replace with empty space
    $string = str_replace("\n", ' ', $string);   // --- replace with space
    $string = str_replace("\t", ' ', $string);   // --- replace with space
    
    // ----- remove multiple spaces ----- 
    $string = trim(preg_replace('/ {2,}/', ' ', $string));
    
    return $string; 

}

// -------------------------------------------------------------- 
?>
```
<br><br>the KEY is the regex pattern: &apos;/&lt;[^&gt;]*&gt;/&apos;<br>instead of strip_tags() <br>... then remove control characters and multiple spaces<br>:)  

---

"5.3.4    strip_tags() no longer strips self-closing XHTML tags unless the self-closing XHTML tag is also given in allowable_tags."<br><br>This is poorly worded.<br><br>The above seems to be saying that, since 5.3.4, if you don&apos;t specify "&lt;br/&gt;" in allowable_tags then "&lt;br/&gt;" will not be stripped... but that&apos;s not actually what they&apos;re trying to say.<br><br>What it means is, in versions prior to 5.3.4, it "strips self-closing XHTML tags unless the self-closing XHTML tag is also given in allowable_tags", and that since 5.3.4 this is no longer the case.<br><br>So what reads as "no longer strips self-closing tags (unless the self-closing XHTML tag is also given in allowable_tags)" is actually saying "no longer (strips self-closing tags unless the self-closing XHTML tag is also given in allowable_tags)".<br><br>i.e.<br><br>pre-5.3.4: strip_tags(&apos;Hello World&lt;br&gt;&lt;br/&gt;&apos;,&apos;&lt;br&gt;&apos;) =&gt; &apos;Hello World&lt;br&gt;&apos; // strips &lt;br/&gt; because it wasn&apos;t explicitly specified in allowable_tags<br><br>5.3.4 and later: strip_tags(&apos;Hello World&lt;br&gt;&lt;br/&gt;&apos;,&apos;&lt;br&gt;&apos;) =&gt; &apos;Hello World&lt;br&gt;&lt;br/&gt;&apos; // does not strip &lt;br/&gt; because PHP matches it with &lt;br&gt; in allowable_tags  

---

Note the different outputs from different versions of the same tag:<br><br>

```
<?php // striptags.php
$data = '<br>Each<br/>New<br />Line';
$new  = strip_tags($data, '<br>');
var_dump($new);  // OUTPUTS string(21) "<br>EachNew<br />Line"



```
<?phpphp // striptags.php
$data = '<br>Each<br/>New<br />Line';
$new  = strip_tags($data, '<br/>');
var_dump($new); // OUTPUTS string(16) "Each<br/>NewLine"



```
<?phpphp // striptags.php
$data = '<br>Each<br/>New<br />Line';
$new  = strip_tags($data, '<br />');
var_dump($new); // OUTPUTS string(11) "EachNewLine"
?>
```
  

---

Features:<br>* allowable tags (as in strip_tags),<br>* optional stripping attributes of the allowable tags,<br>* optional comment preserving,<br>* deleting broken and unclosed tags and comments,<br>* optional callback function call for every piece processed allowing for flexible replacements.<br><br>

```
<?php
function better_strip_tags( $str, $allowable_tags = '', $strip_attrs = false, $preserve_comments = false, callable $callback = null ) {
  $allowable_tags = array_map( 'strtolower', array_filter( // lowercase
      preg_split( '/(?:>|^)\\s*(?:<|$)/', $allowable_tags, -1, PREG_SPLIT_NO_EMPTY ), // get tag names
      function( $tag ) { return preg_match( '/^[a-z][a-z0-9_]*$/i', $tag ); } // filter broken
  ) );
  $comments_and_stuff = preg_split( '/(<!--.*?(?:-->|$))/', $str, -1, PREG_SPLIT_DELIM_CAPTURE );
  foreach ( $comments_and_stuff as $i => $comment_or_stuff ) {
    if ( $i % 2 ) { // html comment
      if ( !( $preserve_comments &amp;&amp; preg_match( '/<!--.*?-->/', $comment_or_stuff ) ) ) {
        $comments_and_stuff[$i] = '';
      }
    } else { // stuff between comments
      $tags_and_text = preg_split( "/(<(?:[^>\"']++|\"[^\"]*+(?:\"|$)|'[^']*+(?:'|$))*(?:>|$))/", $comment_or_stuff, -1, PREG_SPLIT_DELIM_CAPTURE );
      foreach ( $tags_and_text as $j => $tag_or_text ) {
        $is_broken = false;
        $is_allowable = true;
        $result = $tag_or_text;
        if ( $j % 2 ) { // tag
          if ( preg_match( "%^(</?)([a-z][a-z0-9_]*)\\b(?:[^>\"'/]++|/+?|\"[^\"]*\"|'[^']*'.*?(/?>)%i", $tag_or_text, $matches ) ) {
            $tag = strtolower( $matches[2] );
            if ( in_array( $tag, $allowable_tags ) ) {
              if ( $strip_attrs ) {
                $opening = $matches[1];
                $closing = ( $opening === '</' ) ? '>' : $closing;
                $result = $opening . $tag . $closing;
              }
            } else {
              $is_allowable = false;
              $result = '';
            }
          } else {
            $is_broken = true;
            $result = '';
          }
        } else { // text
          $tag = false;
        }
        if ( !$is_broken &amp;&amp; isset( $callback ) ) {
          // allow result modification
          call_user_func_array( $callback, array( &amp;$result, $tag_or_text, $tag, $is_allowable ) );
        }
        $tags_and_text[$j] = $result;
      }
      $comments_and_stuff[$i] = implode( '', $tags_and_text );
    }
  }
  $str = implode( '', $comments_and_stuff );
  return $str;
}
?>
```
<br><br>Callback arguments:<br>* &amp;$result: contains text to be placed insted of original piece (e.g. empty string for forbidden tags), it can be changed;<br>* $tag_or_text: original piece of text or a tag (see below);<br>* $tag: false for text between tags, lowercase tag name for tags;<br>* $is_allowable: boolean telling if a tag isn&apos;t allowed (to avoid double checking), always true for text between tags<br>Callback function isn&apos;t called for comments and broken tags.<br><br>Caution: the function doesn&apos;t fully validate tags (the more so HTML itself), it just force strips those obviously broken (in addition to stripping forbidden tags). If you want to get valid tags then use strip_attrs option, though it doesn&apos;t guarantee tags are balanced or used in the appropriate context. For complex logic consider using DOM parser.  

---

[Official documentation page](https://www.php.net/manual/en/function.strip-tags.php)

**[To root](/README.md)**