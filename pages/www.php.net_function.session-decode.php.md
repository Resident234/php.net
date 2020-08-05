# session_decode



I noticed that the posted solutions for manually decoding sessions are not perfect, so I&apos;ve contributed a more robust solution.<br><br>The preg_match solution can never work. It&apos;s not so hard to find a case that might break unserialization.<br>In the case of jason-joeymail is breaks on:<br><br>

```
<?php
$_SESSION["test"] = ";oops|";
?>
```


Below you can find my solution. It doesn't use a regular expression but rather the reversibility of the serialize operation and the 'feature' that serialize ignores all further input when it thinks it's done. It's by no means a beautiful or particularly fast solution but it is a more robust solution.
I've added a deserializer for "php" and "php_binary". It should be trivial to add one for "wddx".



```
<?php
class Session {
    public static function unserialize($session_data) {
        $method = ini_get("session.serialize_handler");
        switch ($method) {
            case "php":
                return self::unserialize_php($session_data);
                break;
            case "php_binary":
                return self::unserialize_phpbinary($session_data);
                break;
            default:
                throw new Exception("Unsupported session.serialize_handler: " . $method . ". Supported: php, php_binary");
        }
    }

    private static function unserialize_php($session_data) {
        $return_data = array();
        $offset = 0;
        while ($offset < strlen($session_data)) {
            if (!strstr(substr($session_data, $offset), "|")) {
                throw new Exception("invalid data, remaining: " . substr($session_data, $offset));
            }
            $pos = strpos($session_data, "|", $offset);
            $num = $pos - $offset;
            $varname = substr($session_data, $offset, $num);
            $offset += $num + 1;
            $data = unserialize(substr($session_data, $offset));
            $return_data[$varname] = $data;
            $offset += strlen(serialize($data));
        }
        return $return_data;
    }

    private static function unserialize_phpbinary($session_data) {
        $return_data = array();
        $offset = 0;
        while ($offset < strlen($session_data)) {
            $num = ord($session_data[$offset]);
            $offset += 1;
            $varname = substr($session_data, $offset, $num);
            $offset += $num;
            $data = unserialize(substr($session_data, $offset));
            $return_data[$varname] = $data;
            $offset += strlen(serialize($data));
        }
        return $return_data;
    }
}
?>
```


Usage:



```
<?php
Session::unserialize(session_encode());
?>
```
  

---

I found this to be the simplest solution:<br><br>

```
<?php
// if session is not started
session_start();

// store our current session
$my_sess = $_SESSION;

// decode $data (the encoded session data, either from a file or database). Remember, decoded data is put directly into $_SESSION
session_decode($data);
$data = $_SESSION;

print_r($data);

// restore our own session
$_SESSION = $my_sess;

?>
```
  

---

[Official documentation page](https://www.php.net/manual/en/function.session-decode.php)

**[To root](/README.md)**