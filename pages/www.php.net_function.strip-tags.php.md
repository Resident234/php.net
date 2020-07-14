# strip_tags



Hi. I made a function that removes the HTML tags along with their contents:<br><br>Function:<br>

```
<?php
function strip_tags_content($text, $tags = &apos;&apos;, $invert = FALSE) {

  preg_match_all(&apos;/&lt;(.+?)[\s]*\/?[\s]*&gt;/si&apos;, trim($tags), $tags);
  $tags = array_unique($tags[1]);
    
  if(is_array($tags) AND count($tags) &gt; 0) {
    if($invert == FALSE) {
      return preg_replace(&apos;@&lt;(?!(?:&apos;. implode(&apos;|&apos;, $tags) .&apos;)\b)(\w+)\b.*?>
```
.*?&lt;/\1&gt;@si&apos;, &apos;&apos;, $text);
    }
    else {
      return preg_replace(&apos;@&lt;(&apos;. implode(&apos;|&apos;, $tags) .&apos;)\b.*?>
```
.*?&lt;/\1&gt;@si&apos;, &apos;&apos;, $text);
    }
  }
  elseif($invert == FALSE) {
    return preg_replace(&apos;@&lt;(\w+)\b.*?>
```
.*?&lt;/\1&gt;@si&apos;, &apos;&apos;, $text);
  }
  return $text;
}
?>
```
<br><br>Sample text:<br>$text = &apos;&lt;b&gt;sample&lt;/b&gt; text with &lt;div&gt;tags&lt;/div&gt;&apos;;<br><br>Result for strip_tags($text):<br>sample text with tags<br><br>Result for strip_tags_content($text):<br> text with <br><br>Result for strip_tags_content($text, &apos;&lt;b&gt;&apos;):<br>&lt;b&gt;sample&lt;/b&gt; text with <br><br>Result for strip_tags_content($text, &apos;&lt;b&gt;&apos;, TRUE);<br> text with &lt;div&gt;tags&lt;/div&gt;<br><br>I hope that someone is useful :)  

#

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
  if ($xml-&gt;loadHTML($html_str, LIBXML_HTML_NOIMPLIED | LIBXML_HTML_NODEFDTD)){
    foreach ($xml-&gt;getElementsByTagName("*") as $tag){
      if (!in_array($tag-&gt;tagName, $allowed_tags)){
        $tag-&gt;parentNode-&gt;removeChild($tag);
      }else{
        foreach ($tag-&gt;attributes as $attr){
          if (!in_array($attr-&gt;nodeName, $allowed_attrs)){
            $tag-&gt;removeAttribute($attr-&gt;nodeName);
          }
        }
      }
    }
  }
  return $xml-&gt;saveHTML();
}
?>
```
  

#

A word of caution. strip_tags() can actually be used for input validation as long as you remove ANY tag. As soon as you accept a single tag (2nd parameter), you are opening up a security hole such as this:<br><br>&lt;acceptedTag onLoad="javascript:malicious()" /&gt;<br><br>Plus: regexing away attributes or code block is really not the right solution. For effective input validation when using strip_tags() with even a single tag accepted, http://htmlpurifier.org/ is the way to go.  

#

a HTML code like this: <br><br>

```
<?php
$html = &apos;
&lt;div&gt;
&lt;p style="color:blue;"&gt;color is blue&lt;/p&gt;&lt;p&gt;size is &lt;span style="font-size:200%;"&gt;huge&lt;/span&gt;&lt;/p&gt;
&lt;p&gt;material is wood&lt;/p&gt;
&lt;/div&gt;
&apos;; 
?>
```


with 

```
<?php $str = strip_tags($html); ?>
```

... the result is:

$str = &apos;color is bluesize is huge
material is wood&apos;; 

notice: the words &apos;blue&apos; and &apos;size&apos; grow together :( 
and line-breaks are still in new string $str

if you need a space between the words (and without line-break) 
use my function: 

```
<?php $str = rip_tags($html); ?>
```

... the result is:

$str = &apos;color is blue size is huge material is wood&apos;; 

the function: 



```
<?php
// -------------------------------------------------------------- 

function rip_tags($string) { 
    
    // ----- remove HTML TAGs ----- 
    $string = preg_replace (&apos;/&lt;[^&gt;]*&gt;/&apos;, &apos; &apos;, $string); 
    
    // ----- remove control characters ----- 
    $string = str_replace("\r", &apos;&apos;, $string);    // --- replace with empty space
    $string = str_replace("\n", &apos; &apos;, $string);   // --- replace with space
    $string = str_replace("\t", &apos; &apos;, $string);   // --- replace with space
    
    // ----- remove multiple spaces ----- 
    $string = trim(preg_replace(&apos;/ {2,}/&apos;, &apos; &apos;, $string));
    
    return $string; 

}

// -------------------------------------------------------------- 
?>
```
<br><br>the KEY is the regex pattern: &apos;/&lt;[^&gt;]*&gt;/&apos;<br>instead of strip_tags() <br>... then remove control characters and multiple spaces<br>:)  

#

"5.3.4    strip_tags() no longer strips self-closing XHTML tags unless the self-closing XHTML tag is also given in allowable_tags."<br><br>This is poorly worded.<br><br>The above seems to be saying that, since 5.3.4, if you don&apos;t specify "&lt;br/&gt;" in allowable_tags then "&lt;br/&gt;" will not be stripped... but that&apos;s not actually what they&apos;re trying to say.<br><br>What it means is, in versions prior to 5.3.4, it "strips self-closing XHTML tags unless the self-closing XHTML tag is also given in allowable_tags", and that since 5.3.4 this is no longer the case.<br><br>So what reads as "no longer strips self-closing tags (unless the self-closing XHTML tag is also given in allowable_tags)" is actually saying "no longer (strips self-closing tags unless the self-closing XHTML tag is also given in allowable_tags)".<br><br>i.e.<br><br>pre-5.3.4: strip_tags(&apos;Hello World&lt;br&gt;&lt;br/&gt;&apos;,&apos;&lt;br&gt;&apos;) =&gt; &apos;Hello World&lt;br&gt;&apos; // strips &lt;br/&gt; because it wasn&apos;t explicitly specified in allowable_tags<br><br>5.3.4 and later: strip_tags(&apos;Hello World&lt;br&gt;&lt;br/&gt;&apos;,&apos;&lt;br&gt;&apos;) =&gt; &apos;Hello World&lt;br&gt;&lt;br/&gt;&apos; // does not strip &lt;br/&gt; because PHP matches it with &lt;br&gt; in allowable_tags  

#

Note the different outputs from different versions of the same tag:<br><br>

```
<?php // striptags.php
$data = &apos;&lt;br&gt;Each&lt;br/&gt;New&lt;br /&gt;Line&apos;;
$new  = strip_tags($data, &apos;&lt;br&gt;&apos;);
var_dump($new);  // OUTPUTS string(21) "&lt;br&gt;EachNew&lt;br /&gt;Line"

&lt;?php // striptags.php
$data = &apos;&lt;br&gt;Each&lt;br/&gt;New&lt;br /&gt;Line&apos;;
$new  = strip_tags($data, &apos;&lt;br/&gt;&apos;);
var_dump($new); // OUTPUTS string(16) "Each&lt;br/&gt;NewLine"

&lt;?php // striptags.php
$data = &apos;&lt;br&gt;Each&lt;br/&gt;New&lt;br /&gt;Line&apos;;
$new  = strip_tags($data, &apos;&lt;br /&gt;&apos;);
var_dump($new); // OUTPUTS string(11) "EachNewLine"
?>
```
  

#

Features:<br>* allowable tags (as in strip_tags),<br>* optional stripping attributes of the allowable tags,<br>* optional comment preserving,<br>* deleting broken and unclosed tags and comments,<br>* optional callback function call for every piece processed allowing for flexible replacements.<br><br>

```
<?php
function better_strip_tags( $str, $allowable_tags = &apos;&apos;, $strip_attrs = false, $preserve_comments = false, callable $callback = null ) {
  $allowable_tags = array_map( &apos;strtolower&apos;, array_filter( // lowercase
      preg_split( &apos;/(?:&gt;|^)\\s*(?:&lt;|$)/&apos;, $allowable_tags, -1, PREG_SPLIT_NO_EMPTY ), // get tag names
      function( $tag ) { return preg_match( &apos;/^[a-z][a-z0-9_]*$/i&apos;, $tag ); } // filter broken
  ) );
  $comments_and_stuff = preg_split( &apos;/(&lt;!--.*?(?:--&gt;|$))/&apos;, $str, -1, PREG_SPLIT_DELIM_CAPTURE );
  foreach ( $comments_and_stuff as $i =&gt; $comment_or_stuff ) {
    if ( $i % 2 ) { // html comment
      if ( !( $preserve_comments &amp;&amp; preg_match( &apos;/&lt;!--.*?--&gt;/&apos;, $comment_or_stuff ) ) ) {
        $comments_and_stuff[$i] = &apos;&apos;;
      }
    } else { // stuff between comments
      $tags_and_text = preg_split( "/(&lt;(?:[^&gt;\"&apos;]++|\"[^\"]*+(?:\"|$)|&apos;[^&apos;]*+(?:&apos;|$))*(?:&gt;|$))/", $comment_or_stuff, -1, PREG_SPLIT_DELIM_CAPTURE );
      foreach ( $tags_and_text as $j =&gt; $tag_or_text ) {
        $is_broken = false;
        $is_allowable = true;
        $result = $tag_or_text;
        if ( $j % 2 ) { // tag
          if ( preg_match( "%^(&lt;/?)([a-z][a-z0-9_]*)\\b(?:[^&gt;\"&apos;/]++|/+?|\"[^\"]*\"|&apos;[^&apos;]*&apos;)*?(/?>
```
)%i", $tag_or_text, $matches ) ) {
            $tag = strtolower( $matches[2] );
            if ( in_array( $tag, $allowable_tags ) ) {
              if ( $strip_attrs ) {
                $opening = $matches[1];
                $closing = ( $opening === &apos;&lt;/&apos; ) ? &apos;&gt;&apos; : $closing;
                $result = $opening . $tag . $closing;
              }
            } else {
              $is_allowable = false;
              $result = &apos;&apos;;
            }
          } else {
            $is_broken = true;
            $result = &apos;&apos;;
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
      $comments_and_stuff[$i] = implode( &apos;&apos;, $tags_and_text );
    }
  }
  $str = implode( &apos;&apos;, $comments_and_stuff );
  return $str;
}
?>
```
<br><br>Callback arguments:<br>* &amp;$result: contains text to be placed insted of original piece (e.g. empty string for forbidden tags), it can be changed;<br>* $tag_or_text: original piece of text or a tag (see below);<br>* $tag: false for text between tags, lowercase tag name for tags;<br>* $is_allowable: boolean telling if a tag isn&apos;t allowed (to avoid double checking), always true for text between tags<br>Callback function isn&apos;t called for comments and broken tags.<br><br>Caution: the function doesn&apos;t fully validate tags (the more so HTML itself), it just force strips those obviously broken (in addition to stripping forbidden tags). If you want to get valid tags then use strip_attrs option, though it doesn&apos;t guarantee tags are balanced or used in the appropriate context. For complex logic consider using DOM parser.  

#

[Official documentation page](https://www.php.net/manual/en/function.strip-tags.php)

**[To root](/README.md)**