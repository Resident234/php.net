# filter_input_array



Note that although you can provide a default filter for the entire input array there is no way to provide a flag for that filter without building the entire definition array yourself.<br><br>So here is a small function that can alleviate this hassle!<br><br>

```
<?php
function filter_input_array_with_default_flags($type, $filter, $flags, $add_empty = true) {
    $loopThrough = array();
    switch ($type) {
        case INPUT_GET : $loopThrough = $_GET; break;
        case INPUT_POST : $loopThrough = $_POST; break;
        case INPUT_COOKIE : $loopThrough = $_COOKIE; break;
        case INPUT_SERVER : $loopThrough = $_SERVER; break;
        case INPUT_ENV : $loopThrough = $_ENV; break;
    }
   
    $args = array();
    foreach ($loopThrough as $key=>$value) {
        $args[$key] = array('filter'=>$filter, 'flags'=>$flags);
    }
    
    return filter_input_array($type, $args, $add_empty);
}
?>
```
  

---

[New Version]<br>This function is very useful for filtering complicated array structure.<br>Also, Some integer bitmasks and invalid UTF-8 sequence detection are available.<br><br>Code:<br>

```
<?php
/**
 * @param  integer $type    Constant like INPUT_XXX.
 * @param  array   $default Default structure of the specified super global var.
 *                          Following bitmasks are available:
 *  + FILTER_STRUCT_FORCE_ARRAY - Force 1 dimensional array.
 *  + FILTER_STRUCT_TRIM        - Trim by ASCII control chars.
 *  + FILTER_STRUCT_FULL_TRIM   - Trim by ASCII control chars,
 *                                full-width and no-break space.
 * @return array            The value of the filtered super global var.
 */
define('FILTER_STRUCT_FORCE_ARRAY', 1);
define('FILTER_STRUCT_TRIM', 2);
define('FILTER_STRUCT_FULL_TRIM', 4);
function filter_struct_utf8($type, array $default) {
    static $func = __FUNCTION__;
    static $trim = "[\\x0-\x20\x7f]";
    static $ftrim = "[\\x0-\x20\x7f\xc2\xa0\xe3\x80\x80]";
    static $recursive_static = false;
    if (!$recursive = $recursive_static) {
        $types = array(
            INPUT_GET => $_GET,
            INPUT_POST => $_POST,
            INPUT_COOKIE => $_COOKIE,
            INPUT_REQUEST => $_REQUEST,
        );
        if (!isset($types[(int)$type])) {
            throw new LogicException('unknown super global var type');
        }
        $var = $types[(int)$type];
        $recursive_static = true;
    } else {
        $var = $type;
    }
    $ret = array();
    foreach ($default as $key => $value) {
        if ($is_int = is_int($value)) {
            if (!($value | (
                FILTER_STRUCT_FORCE_ARRAY |
                FILTER_STRUCT_FULL_TRIM | 
                FILTER_STRUCT_TRIM
            ))) {
                $recursive_static = false;
                throw new LogicException('unknown bitmask');
            }
            if ($value &amp; FILTER_STRUCT_FORCE_ARRAY) {
                $tmp = array();
                if (isset($var[$key])) {
                    foreach ((array)$var[$key] as $k => $v) {
                        if (!preg_match('//u', $k)){
                            continue;
                        }
                        $value &amp;= FILTER_STRUCT_FULL_TRIM | FILTER_STRUCT_TRIM;
                        $tmp += array($k => $value ? $value : '');
                    }
                }
                $value = $tmp;
            }
        }
        if ($isset = isset($var[$key]) and is_array($value)) {
            $ret[$key] = $func($var[$key], $value);
        } elseif (!$isset || is_array($var[$key])) {
            $ret[$key] = null;
        } elseif ($is_int &amp;&amp; $value &amp; FILTER_STRUCT_FULL_TRIM) {
            $ret[$key] = preg_replace("/\A{$ftrim}++|{$ftrim}++\z/u", '', $var[$key]);
        } elseif ($is_int &amp;&amp; $value &amp; FILTER_STRUCT_TRIM) {
            $ret[$key] = preg_replace("/\A{$trim}++|{$trim}++\z/u", '', $var[$key]);
        } else {
            $ret[$key] = preg_replace('//u', '', $var[$key]);
        }
        if ($ret[$key] === null) {
            $ret[$key] = $is_int ? '' : $value;
        }
    }
    if (!$recursive) {
        $recursive_static = false;
    }
    return $ret;
}
?>
```
  

---

[Official documentation page](https://www.php.net/manual/en/function.filter-input-array.php)

**[To root](/README.md)**