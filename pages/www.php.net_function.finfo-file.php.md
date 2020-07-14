# finfo_file



The way HOWTO get MIME-type of remote file.<br><br>

```
<?php
class MimeStreamWrapper
{
    const WRAPPER_NAME = &apos;mime&apos;;
    public $context;
    private static $isRegistered = false;
    private $callBackFunction;
    private $eof = false;
    private $fp;
    private $path;
    private $fileStat;
    private function getStat()
    {
        if ($fStat = fstat($this-&gt;fp)) {
            return $fStat;
        }

        $size = 100;
        if ($headers = get_headers($this-&gt;path, true)) {
            $head = array_change_key_case($headers, CASE_LOWER);
            $size = (int)$head[&apos;content-length&apos;];
        }
        $blocks = ceil($size / 512);
        return array(
            &apos;dev&apos; =&gt; 16777220,
            &apos;ino&apos; =&gt; 15764,
            &apos;mode&apos; =&gt; 33188,
            &apos;nlink&apos; =&gt; 1,
            &apos;uid&apos; =&gt; 10000,
            &apos;gid&apos; =&gt; 80,
            &apos;rdev&apos; =&gt; 0,
            &apos;size&apos; =&gt; $size,
            &apos;atime&apos; =&gt; 0,
            &apos;mtime&apos; =&gt; 0,
            &apos;ctime&apos; =&gt; 0,
            &apos;blksize&apos; =&gt; 4096,
            &apos;blocks&apos; =&gt; $blocks,
        );
    }
    public function setPath($path)
    {
        $this-&gt;path = $path;
        $this-&gt;fp = fopen($this-&gt;path, &apos;rb&apos;) or die(&apos;Cannot open file:  &apos; . $this-&gt;path);
        $this-&gt;fileStat = $this-&gt;getStat();
    }
    public function read($count) {
        return fread($this-&gt;fp, $count);
    }
    public function getStreamPath()
    {
        return str_replace(array(&apos;ftp://&apos;, &apos;http://&apos;, &apos;https://&apos;), self::WRAPPER_NAME . &apos;://&apos;, $this-&gt;path);
    }
    public function getContext()
    {
        if (!self::$isRegistered) {
            stream_wrapper_register(self::WRAPPER_NAME, get_class());
            self::$isRegistered = true;
        }
        return stream_context_create(
            array(
                self::WRAPPER_NAME =&gt; array(
                    &apos;cb&apos; =&gt; array($this, &apos;read&apos;),
                    &apos;fileStat&apos; =&gt; $this-&gt;fileStat,
                )
            )
        );
    }
    public function stream_open($path, $mode, $options, &amp;$opened_path)
    {
        if (!preg_match(&apos;/^r[bt]?$/&apos;, $mode) || !$this-&gt;context) {
            return false;
        }
        $opt = stream_context_get_options($this-&gt;context);
        if (!is_array($opt[self::WRAPPER_NAME]) ||
            !isset($opt[self::WRAPPER_NAME][&apos;cb&apos;]) ||
            !is_callable($opt[self::WRAPPER_NAME][&apos;cb&apos;])
        ) {
            return false;
        }
        $this-&gt;callBackFunction = $opt[self::WRAPPER_NAME][&apos;cb&apos;];
        $this-&gt;fileStat = $opt[self::WRAPPER_NAME][&apos;fileStat&apos;];

        return true;
    }
    public function stream_read($count)
    {
        if ($this-&gt;eof || !$count) {
            return &apos;&apos;;
        }
        if (($s = call_user_func($this-&gt;callBackFunction, $count)) == &apos;&apos;) {
            $this-&gt;eof = true;
        }
        return $s;
    }
    public function stream_eof()
    {
        return $this-&gt;eof;
    }
    public function stream_stat()
    {
        return $this-&gt;fileStat;
    }
    public function stream_cast($castAs)
    {
        $read = null;
        $write  = null;
        $except = null;
        return @stream_select($read, $write, $except, $castAs);
    }
}
$path = &apos;http://fc04.deviantart.net/fs71/f/2010/227/4/6/PNG_Test_by_Destron23.png&apos;;
echo "File: ", $path, "\n";
$wrapper = new MimeStreamWrapper();
$wrapper-&gt;setPath($path);

$fInfo = new finfo(FILEINFO_MIME);
echo "MIME-type: ", $fInfo-&gt;file($wrapper-&gt;getStreamPath(), FILEINFO_MIME_TYPE, $wrapper-&gt;getContext()), "\n";
?>
```
  

#

Tempting as it may seem to use finfo_file() to validate uploaded image files (Check whether a supposed imagefile really contains an image), the results cannot be trusted. It&apos;s not that hard to wrap harmful executable code in a file identified as a GIF for instance.<br><br>A better &amp; safer option is to check the result of:<br><br>if (!$img = @imagecreatefromgif($uploadedfilename)) {<br>  trigger_error(&apos;Not a GIF image!&apos;,E_USER_WARNING);<br>  // do necessary stuff<br>}  

#

[Official documentation page](https://www.php.net/manual/en/function.finfo-file.php)

**[To root](/README.md)**