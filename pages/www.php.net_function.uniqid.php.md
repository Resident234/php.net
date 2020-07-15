# uniqid



For the record, the underlying function to uniqid() appears to be roughly as follows:<br><br>$m=microtime(true);<br>sprintf("%8x%05x\n",floor($m),($m-floor($m))*1000000);<br><br>In other words, first 8 hex chars = Unixtime, last 5 hex chars = microseconds. This is why it has microsecond precision. Also, it provides a means by which to reverse-engineer the time when a uniqid was generated: <br><br>date("r",hexdec(substr(uniqid(),0,8)));<br><br>Increasingly as you go further down the string, the number becomes "more unique" over time, with the exception of digit 9, where numeral prevalence is 0..3&gt;4&gt;5..f, because of the difference between 10^6 and 16^5 (this is presumably true for the remaining digits as well but much less noticeable).  

#

The following class generates VALID RFC 4211 COMPLIANT Universally Unique IDentifiers (UUID) version 3, 4 and 5.<br><br>Version 3 and 5 UUIDs are named based. They require a namespace (another valid UUID) and a value (the name). Given the same namespace and name, the output is always the same.<br><br>Version 4 UUIDs are pseudo-random.<br><br>UUIDs generated below validates using OSSP UUID Tool, and output for named-based UUIDs are exactly the same. This is a pure PHP implementation.<br><br>

```
<?php
class UUID {
  public static function v3($namespace, $name) {
    if(!self::is_valid($namespace)) return false;

    // Get hexadecimal components of namespace
    $nhex = str_replace(array('-','{','}'), '', $namespace);

    // Binary Value
    $nstr = '';

    // Convert Namespace UUID to bits
    for($i = 0; $i &lt; strlen($nhex); $i+=2) {
      $nstr .= chr(hexdec($nhex[$i].$nhex[$i+1]));
    }

    // Calculate hash value
    $hash = md5($nstr . $name);

    return sprintf('%08s-%04s-%04x-%04x-%12s',

      // 32 bits for "time_low"
      substr($hash, 0, 8),

      // 16 bits for "time_mid"
      substr($hash, 8, 4),

      // 16 bits for "time_hi_and_version",
      // four most significant bits holds version number 3
      (hexdec(substr($hash, 12, 4)) &amp; 0x0fff) | 0x3000,

      // 16 bits, 8 bits for "clk_seq_hi_res",
      // 8 bits for "clk_seq_low",
      // two most significant bits holds zero and one for variant DCE1.1
      (hexdec(substr($hash, 16, 4)) &amp; 0x3fff) | 0x8000,

      // 48 bits for "node"
      substr($hash, 20, 12)
    );
  }

  public static function v4() {
    return sprintf('%04x%04x-%04x-%04x-%04x-%04x%04x%04x',

      // 32 bits for "time_low"
      mt_rand(0, 0xffff), mt_rand(0, 0xffff),

      // 16 bits for "time_mid"
      mt_rand(0, 0xffff),

      // 16 bits for "time_hi_and_version",
      // four most significant bits holds version number 4
      mt_rand(0, 0x0fff) | 0x4000,

      // 16 bits, 8 bits for "clk_seq_hi_res",
      // 8 bits for "clk_seq_low",
      // two most significant bits holds zero and one for variant DCE1.1
      mt_rand(0, 0x3fff) | 0x8000,

      // 48 bits for "node"
      mt_rand(0, 0xffff), mt_rand(0, 0xffff), mt_rand(0, 0xffff)
    );
  }

  public static function v5($namespace, $name) {
    if(!self::is_valid($namespace)) return false;

    // Get hexadecimal components of namespace
    $nhex = str_replace(array('-','{','}'), '', $namespace);

    // Binary Value
    $nstr = '';

    // Convert Namespace UUID to bits
    for($i = 0; $i &lt; strlen($nhex); $i+=2) {
      $nstr .= chr(hexdec($nhex[$i].$nhex[$i+1]));
    }

    // Calculate hash value
    $hash = sha1($nstr . $name);

    return sprintf('%08s-%04s-%04x-%04x-%12s',

      // 32 bits for "time_low"
      substr($hash, 0, 8),

      // 16 bits for "time_mid"
      substr($hash, 8, 4),

      // 16 bits for "time_hi_and_version",
      // four most significant bits holds version number 5
      (hexdec(substr($hash, 12, 4)) &amp; 0x0fff) | 0x5000,

      // 16 bits, 8 bits for "clk_seq_hi_res",
      // 8 bits for "clk_seq_low",
      // two most significant bits holds zero and one for variant DCE1.1
      (hexdec(substr($hash, 16, 4)) &amp; 0x3fff) | 0x8000,

      // 48 bits for "node"
      substr($hash, 20, 12)
    );
  }

  public static function is_valid($uuid) {
    return preg_match('/^\{?[0-9a-f]{8}\-?[0-9a-f]{4}\-?[0-9a-f]{4}\-?'.
                      '[0-9a-f]{4}\-?[0-9a-f]{12}\}?$/i', $uuid) === 1;
  }
}

// Usage
// Named-based UUID.

$v3uuid = UUID::v3('1546058f-5a25-4334-85ae-e68f2a44bbaf', 'SomeRandomString');
$v5uuid = UUID::v5('1546058f-5a25-4334-85ae-e68f2a44bbaf', 'SomeRandomString');

// Pseudo-random UUID

$v4uuid = UUID::v4();
?>
```
  

#

Seriously, avoid using this function. Here&apos;s an example of why:<br><br>

```
<?php
for($i=0;$i&lt;20;$i++) {
    echo uniqid(), PHP_EOL;
}
?>
```


Output:

5819f3ad1c0ce
5819f3ad1c0ce
5819f3ad1c0ce
5819f3ad1c0ce
5819f3ad1c0ce
5819f3ad1c0ce
5819f3ad1c0ce
5819f3ad1c0ce
5819f3ad1c4b6
5819f3ad1c4b6
5819f3ad1c4b6
5819f3ad1c4b6
5819f3ad1c4b6
5819f3ad1c4b6
5819f3ad1c4b6
5819f3ad1c4b6
5819f3ad1c4b6
5819f3ad1c4b6
5819f3ad1c4b6
5819f3ad1c4b6

As you can see, using it w/ a DB can cause the creation of documents with repeated ID's. You should instead opt for:



```
<?php
function uniqidReal($lenght = 13) {
    // uniqid gives 13 chars, but you could adjust it to your needs.
    if (function_exists("random_bytes")) {
        $bytes = random_bytes(ceil($lenght / 2));
    } elseif (function_exists("openssl_random_pseudo_bytes")) {
        $bytes = openssl_random_pseudo_bytes(ceil($lenght / 2));
    } else {
        throw new Exception("no cryptographically secure random function available");
    }
    return substr(bin2hex($bytes), 0, $lenght);
}
?>
```


Test:


```
<?php
for($i=0;$i&lt;20;$i++) {
    echo uniqid(), "\t", uniqidReal(), PHP_EOL;
}
?>
```
<br><br>Output:<br><br>5819fa0b63be3   9f39aa0ecd89d<br>5819fa0b70ed3   2c0735cabfcce<br>5819fa0b712bb   15e45d1ca1e90<br>5819fa0b712bb   89593dc230eb3<br>5819fa0b712bb   449795704aeef<br>5819fa0b712bb   b046877b80ac9<br>5819fa0b712bb   0a6fa0ae3ec7b<br>5819fa0b712bb   ba2f3f4d6afe0<br>5819fa0b712bb   af03cfac83fd6<br>5819fa0b712bb   eb9c3c6d475c0<br>5819fa0b712bb   edfbbf59d5e1b<br>5819fa0b712bb   500dca18888d4<br>5819fa0b716a3   4f5a40ef715f1<br>5819fa0b716a3   154e42b616825<br>5819fa0b716a3   a879a22663c9b<br>5819fa0b716a3   ea7c044ddda8a<br>5819fa0b716a3   2c81a44dc674e<br>5819fa0b716a3   bb32f37304fd9<br>5819fa0b716a3   30cdf6c0317d7<br>5819fa0b716a3   d25f529d126ae  

#

Generating an MD5 from a unique ID is naive and reduces much of the value of unique IDs, as well as providing significant (attackable) stricture on the MD5 domain.  That&apos;s a deeply broken thing to do.  The correct approach is to use the unique ID on its own; it&apos;s already geared for non-collision.<br><br>IDs should never be obfuscated for security, so if you&apos;re worried about someone guessing your ID, fix the system, don&apos;t just make it harder to guess (because it&apos;s nowhere near as difficult to guess as you imagine: you can just brute force the 60,000 MD5s that are generatable from millisecond IDs over the course of a given minute, which the typical computer can do in less than 0.1s).<br><br>If you absolutely need to involve a hash somehow - maybe to placate a boss who thinks they understand security much better than they actually do - append it instead.<br><br>function BadIdeaID() { return uniqid() . &apos;_&apos; . md5(mt_rand()); }  

#

The php5-uuid functions could definitely use some documentation to clarify how they should be used, but here&apos;s what I&apos;ve gleaned by examining the OSSP source code (found here: http://ossp-uuid.sourcearchive.com/documentation/1.5.1-1ubuntu1/php_2uuid_8c-source.html).<br><br>The uuid_make() function takes two arguments when generating v1 or v4, but four arguments are required when generating v3 or v5. The first two arguments have been demonstrated below and are straightforward, so I&apos;ll skip to the as-yet non-described arguments.<br><br>The third argument to uuid_make() is: $namespace<br>  - this is a secondary resource created with uuid_create(); it is apparently used to generate an internal UUID, which is used as the namespace of the output UUID<br><br>The fourth argument to uuid_make() is: $url<br>  - this is the value that is to be hashed (MD5 for v3, SHA-1 for v5); it may be any string or even null<br><br>Here&apos;s a simple class illustrating the proper usage (note that if php5-uuid is not installed on your system, each function call will just return false):<br><br>

```
<?php
class UUID {
  /**
   * Generates version 1: MAC address
   */
  public static function v1() {
    if (!function_exists('uuid_create'))
      return false;

    uuid_create(&amp;$context);
    uuid_make($context, UUID_MAKE_V1);
    uuid_export($context, UUID_FMT_STR, &amp;$uuid);
    return trim($uuid);
  }

  /**
   * Generates version 3 UUID: MD5 hash of URL
   */
  public static function v3($i_url) {
    if (!function_exists('uuid_create'))
      return false;

    if (!strlen($i_url))
      $i_url = self::v1();

    uuid_create(&amp;$context);
    uuid_create(&amp;$namespace);

    uuid_make($context, UUID_MAKE_V3, $namespace, $i_url);
    uuid_export($context, UUID_FMT_STR, &amp;$uuid);
    return trim($uuid);
  }

  /**
   * Generates version 4 UUID: random
   */
  public static function v4() {
    if (!function_exists('uuid_create'))
      return false;

    uuid_create(&amp;$context);

    uuid_make($context, UUID_MAKE_V4);
    uuid_export($context, UUID_FMT_STR, &amp;$uuid);
    return trim($uuid);
  }

  /**
   * Generates version 5 UUID: SHA-1 hash of URL
   */
  public static function v5($i_url) {
    if (!function_exists('uuid_create'))
      return false;

    if (!strlen($i_url))
      $i_url = self::v1();

    uuid_create(&amp;$context);
    uuid_create(&amp;$namespace);

    uuid_make($context, UUID_MAKE_V5, $namespace, $i_url);
    uuid_export($context, UUID_FMT_STR, &amp;$uuid);
    return trim($uuid);
  }
}
?>
```


And here's a demonstration:



```
<?php
for ($i = 1; $i &lt;= 3; ++$i) {
  echo 'microtime = ' . microtime(true) . '&lt;br/&gt;';
  echo "V1 UUID: " . UUID::v1() . '&lt;br/&gt;';
  echo "V3 UUID of URL='abc': " . UUID::v3('abc') . '&lt;br/&gt;';
  echo "V4 UUID: " . UUID::v4() . '&lt;br/&gt;';
  echo "V5 UUID of URL=null: " . UUID::v5(null) . '&lt;br/&gt;';
  echo '&lt;hr/&gt;';
}
?>
```
<br><br>And the output:<br><br>microtime = 1306620716.0457<br>V1 UUID: 7fddae8e-8977-11e0-bc11-003048c3b1f2<br>V3 UUID of URL=&apos;abc&apos;: 522ec739-ca63-3ec5-b082-08ce08ad65e2<br>V4 UUID: b3851ec7-4871-4527-92b5-ef5616bae1e6<br>V5 UUID of URL=null: e129f27c-5103-5c5c-844b-cdf0a15e160d<br>-------------------<br>microtime = 1306620716.0465<br>V1 UUID: 7fddb83e-8977-11e0-9e6e-003048c3b1f2<br>V3 UUID of URL=&apos;abc&apos;: 522ec739-ca63-3ec5-b082-08ce08ad65e2<br>V4 UUID: 7e78fe0d-59b8-4637-af7f-e88d221a7d1e<br>V5 UUID of URL=null: e129f27c-5103-5c5c-844b-cdf0a15e160d<br>-------------------<br>microtime = 1306620716.0467<br>V1 UUID: 7fddbfb4-8977-11e0-a2bc-003048c3b1f2<br>V3 UUID of URL=&apos;abc&apos;: 522ec739-ca63-3ec5-b082-08ce08ad65e2<br>V4 UUID: 12a940c7-0f3f-46a1-bb5f-bdd602e10654<br>V5 UUID of URL=null: e129f27c-5103-5c5c-844b-cdf0a15e160d<br><br>As you can see, the calls to v3() always return the same UUID because the same URL parameter, "abc", is always supplied. The same goes for the v5() function which is always supplied a null URL.<br><br>The v4() UUIDs are always entirely different because they are (pseudo)random. And the v1() calls are very similar but just slightly different because it&apos;s based on the computer&apos;s MAC address and the current time.  

#

Prefix can be useful, for instance, if you generate identifiers simultaneously on several hosts that might happen to generate the identifier at the same microsecond.<br>So we can include the hostname / servername in the id.<br>

```
<?php
echo uniqid(php_uname('n'), true);
// Output: darkstar4dfa8c27aea106.40781203
?>
```
  

#

I use this function to generate microsoft-compatible GUID&apos;s.<br><br>

```
<?php
 public function create_guid($namespace = '') {     
    static $guid = '';
    $uid = uniqid("", true);
    $data = $namespace;
    $data .= $_SERVER['REQUEST_TIME'];
    $data .= $_SERVER['HTTP_USER_AGENT'];
    $data .= $_SERVER['LOCAL_ADDR'];
    $data .= $_SERVER['LOCAL_PORT'];
    $data .= $_SERVER['REMOTE_ADDR'];
    $data .= $_SERVER['REMOTE_PORT'];
    $hash = strtoupper(hash('ripemd128', $uid . $guid . md5($data)));
    $guid = '{' .   
            substr($hash,  0,  8) . 
            '-' .
            substr($hash,  8,  4) .
            '-' .
            substr($hash, 12,  4) .
            '-' .
            substr($hash, 16,  4) .
            '-' .
            substr($hash, 20, 12) .
            '}';
    return $guid;
  }
?>
```
  

#

[Official documentation page](https://www.php.net/manual/en/function.uniqid.php)

**[To root](/README.md)**