# parse_str



It bears mentioning that the parse_str builtin does NOT process a query string in the CGI standard way, when it comes to duplicate fields.  If multiple fields of the same name exist in a query string, every other web processing language would read them into an array, but PHP silently overwrites them:<br><br>

```
<?php
# silently fails to handle multiple values
parse_str('foo=1&amp;foo=2&amp;foo=3');

# the above produces:
$foo = array('foo' => '3');
?>
```


Instead, PHP uses a non-standards compliant practice of including brackets in fieldnames to achieve the same effect.



```
<?php
# bizarre ?>
```
specific behavior
parse_str('foo[]=1&amp;foo[]=2&amp;foo[]=3');

# the above produces:
$foo = array('foo' => array('1', '2', '3') );
?>
```


This can be confusing for anyone who's used to the CGI standard, so keep it in mind.  As an alternative, I use a "proper" querystring parser function:



```
<?php
function proper_parse_str($str) {
  # result array
  $arr = array();

  # split on outer delimiter
  $pairs = explode('&amp;', $str);

  # loop through each pair
  foreach ($pairs as $i) {
    # split into name and value
    list($name,$value) = explode('=', $i, 2);
    
    # if name already exists
    if( isset($arr[$name]) ) {
      # stick multiple values into an array
      if( is_array($arr[$name]) ) {
        $arr[$name][] = $value;
      }
      else {
        $arr[$name] = array($arr[$name], $value);
      }
    }
    # otherwise, simply stick it in a scalar
    else {
      $arr[$name] = $value;
    }
  }

  # return result array
  return $arr;
}

$query = proper_parse_str($_SERVER['QUERY_STRING']);
?>
```
  

#

if you need custom arg separator, you can use this function. it returns parsed  query as associative array.<br><br>

```
<?php

/**
 * Parses http query string into an array
 * 
 * @author Alxcube &lt;alxcube@gmail.com&gt;
 * 
 * @param string $queryString String to parse
 * @param string $argSeparator Query arguments separator
 * @param integer $decType Decoding type
 * @return array
 */
function http_parse_query($queryString, $argSeparator = '&amp;', $decType = PHP_QUERY_RFC1738) {
        $result             = array();
        $parts              = explode($argSeparator, $queryString);

        foreach ($parts as $part) {
                list($paramName, $paramValue)   = explode('=', $part, 2);

                switch ($decType) {
                        case PHP_QUERY_RFC3986:
                                $paramName      = rawurldecode($paramName);
                                $paramValue     = rawurldecode($paramValue);
                                break;

                        case PHP_QUERY_RFC1738:
                        default:
                                $paramName      = urldecode($paramName);
                                $paramValue     = urldecode($paramValue);
                                break;
                }
                

                if (preg_match_all('/\[([^\]]*)\]/m', $paramName, $matches)) {
                        $paramName      = substr($paramName, 0, strpos($paramName, '['));
                        $keys           = array_merge(array($paramName), $matches[1]);
                } else {
                        $keys           = array($paramName);
                }
                
                $target         = &amp;$result;
                
                foreach ($keys as $index) {
                        if ($index === '') {
                                if (isset($target)) {
                                        if (is_array($target)) {
                                                $intKeys        = array_filter(array_keys($target), 'is_int');
                                                $index  = count($intKeys) ? max($intKeys)+1 : 0;
                                        } else {
                                                $target = array($target);
                                                $index  = 1;
                                        }
                                } else {
                                        $target         = array();
                                        $index          = 0;
                                }
                        } elseif (isset($target[$index]) &amp;&amp; !is_array($target[$index])) {
                                $target[$index] = array($target[$index]);
                        }

                        $target         = &amp;$target[$index];
                }

                if (is_array($target)) {
                        $target[]   = $paramValue;
                } else {
                        $target     = $paramValue;
                }
        }

        return $result;
}

?>
```
  

#

That&apos;s not says in the description but max_input_vars directive affects this function. If there are more input variables on the string than specified by this directive, an E_WARNING is issued, and further input variables are truncated from the request.  

#

Vladimir: the function is OK in how it deals with &amp;amp;.<br>&amp;amp; must only be used when outputing URLs in HTML/XML data.<br>You should ask yourself why you have &amp;amp; in your URL when you give it to parse_str.  

#

[Official documentation page](https://www.php.net/manual/en/function.parse-str.php)

**[To root](/README.md)**