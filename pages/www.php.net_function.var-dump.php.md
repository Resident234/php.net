# var_dump



Keep in mind if you have xdebug installed it will limit the var_dump() output of array elements and object properties to 3 levels deep.<br><br>To change the default, edit your xdebug.ini file and add the folllowing line:<br>xdebug.var_display_max_depth=n<br><br>More information here:<br>http://www.xdebug.org/docs/display  

#

If you&apos;re like me and uses var_dump whenever you&apos;re debugging, you might find these two "wrapper" functions helpful.<br><br>This one automatically adds the PRE tags around the var_dump output so you get nice formatted arrays.<br><br>

```
<?php

function var_dump_pre($mixed = null) {
  echo &apos;&lt;pre&gt;&apos;;
  var_dump($mixed);
  echo &apos;&lt;/pre&gt;&apos;;
  return null;
}

?>
```


This one returns the value of var_dump instead of outputting it.



```
<?php

function var_dump_ret($mixed = null) {
  ob_start();
  var_dump($mixed);
  $content = ob_get_contents();
  ob_end_clean();
  return $content;
}

?>
```
<br><br>Fairly simple functions, but they&apos;re infinitely helpful (I use var_dump_pre() almost exclusively now).  

#

I post a new var_dump function with colors and collapse features. It can also adapt to terminal output if you execute it from there. No need to wrap it in a pre tag to get it to work in browsers. <br><br>

```
<?php
function dump_debug($input, $collapse=false) {
    $recursive = function($data, $level=0) use (&amp;$recursive, $collapse) {
        global $argv;

        $isTerminal = isset($argv);

        if (!$isTerminal &amp;&amp; $level == 0 &amp;&amp; !defined("DUMP_DEBUG_SCRIPT")) {
            define("DUMP_DEBUG_SCRIPT", true);

            echo &apos;&lt;script language="Javascript"&gt;function toggleDisplay(id) {&apos;;
            echo &apos;var state = document.getElementById("container"+id).style.display;&apos;;
            echo &apos;document.getElementById("container"+id).style.display = state == "inline" ? "none" : "inline";&apos;;
            echo &apos;document.getElementById("plus"+id).style.display = state == "inline" ? "inline" : "none";&apos;;
            echo &apos;}&lt;/script&gt;&apos;."\n";
        }

        $type = !is_string($data) &amp;&amp; is_callable($data) ? "Callable" : ucfirst(gettype($data));
        $type_data = null;
        $type_color = null;
        $type_length = null;

        switch ($type) {
            case "String": 
                $type_color = "green";
                $type_length = strlen($data);
                $type_data = "\"" . htmlentities($data) . "\""; break;

            case "Double": 
            case "Float": 
                $type = "Float";
                $type_color = "#0099c5";
                $type_length = strlen($data);
                $type_data = htmlentities($data); break;

            case "Integer": 
                $type_color = "red";
                $type_length = strlen($data);
                $type_data = htmlentities($data); break;

            case "Boolean": 
                $type_color = "#92008d";
                $type_length = strlen($data);
                $type_data = $data ? "TRUE" : "FALSE"; break;

            case "NULL": 
                $type_length = 0; break;

            case "Array": 
                $type_length = count($data);
        }

        if (in_array($type, array("Object", "Array"))) {
            $notEmpty = false;

            foreach($data as $key =&gt; $value) {
                if (!$notEmpty) {
                    $notEmpty = true;

                    if ($isTerminal) {
                        echo $type . ($type_length !== null ? "(" . $type_length . ")" : "")."\n";

                    } else {
                        $id = substr(md5(rand().":".$key.":".$level), 0, 8);

                        echo "&lt;a href=\"javascript:toggleDisplay(&apos;". $id ."&apos;);\" style=\"text-decoration:none\"&gt;";
                        echo "&lt;span style=&apos;color:#666666&apos;&gt;" . $type . ($type_length !== null ? "(" . $type_length . ")" : "") . "&lt;/span&gt;";
                        echo "&lt;/a&gt;";
                        echo "&lt;span id=\"plus". $id ."\" style=\"display: " . ($collapse ? "inline" : "none") . ";\"&gt;&amp;nbsp;&amp;#10549;&lt;/span&gt;";
                        echo "&lt;div id=\"container". $id ."\" style=\"display: " . ($collapse ? "" : "inline") . ";\"&gt;";
                        echo "&lt;br /&gt;";
                    }

                    for ($i=0; $i &lt;= $level; $i++) {
                        echo $isTerminal ? "|    " : "&lt;span style=&apos;color:black&apos;&gt;|&lt;/span&gt;&amp;nbsp;&amp;nbsp;&amp;nbsp;&amp;nbsp;&amp;nbsp;&amp;nbsp;&amp;nbsp;&amp;nbsp;";
                    }

                    echo $isTerminal ? "\n" : "&lt;br /&gt;";
                }

                for ($i=0; $i &lt;= $level; $i++) {
                    echo $isTerminal ? "|    " : "&lt;span style=&apos;color:black&apos;&gt;|&lt;/span&gt;&amp;nbsp;&amp;nbsp;&amp;nbsp;&amp;nbsp;&amp;nbsp;&amp;nbsp;&amp;nbsp;&amp;nbsp;";
                }

                echo $isTerminal ? "[" . $key . "] =&gt; " : "&lt;span style=&apos;color:black&apos;&gt;[" . $key . "]&amp;nbsp;=&gt;&amp;nbsp;&lt;/span&gt;";

                call_user_func($recursive, $value, $level+1);
            }

            if ($notEmpty) {
                for ($i=0; $i &lt;= $level; $i++) {
                    echo $isTerminal ? "|    " : "&lt;span style=&apos;color:black&apos;&gt;|&lt;/span&gt;&amp;nbsp;&amp;nbsp;&amp;nbsp;&amp;nbsp;&amp;nbsp;&amp;nbsp;&amp;nbsp;&amp;nbsp;";
                }

                if (!$isTerminal) {
                    echo "&lt;/div&gt;";
                }

            } else {
                echo $isTerminal ? 
                        $type . ($type_length !== null ? "(" . $type_length . ")" : "") . "  " : 
                        "&lt;span style=&apos;color:#666666&apos;&gt;" . $type . ($type_length !== null ? "(" . $type_length . ")" : "") . "&lt;/span&gt;&amp;nbsp;&amp;nbsp;";
            }

        } else {
            echo $isTerminal ? 
                    $type . ($type_length !== null ? "(" . $type_length . ")" : "") . "  " : 
                    "&lt;span style=&apos;color:#666666&apos;&gt;" . $type . ($type_length !== null ? "(" . $type_length . ")" : "") . "&lt;/span&gt;&amp;nbsp;&amp;nbsp;";

            if ($type_data != null) {
                echo $isTerminal ? $type_data : "&lt;span style=&apos;color:" . $type_color . "&apos;&gt;" . $type_data . "&lt;/span&gt;";
            }
        }

        echo $isTerminal ? "\n" : "&lt;br /&gt;";
    };

    call_user_func($recursive, $input);
}
?>
```
  

#

[Official documentation page](https://www.php.net/manual/en/function.var-dump.php)

**[To root](/README.md)**