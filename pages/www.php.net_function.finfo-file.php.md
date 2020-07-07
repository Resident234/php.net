# finfo_file





The way HOWTO get MIME-type of remote file.



```
<?php
class MimeStreamWrapper
{
&#xA0; &#xA0; const WRAPPER_NAME = &apos;mime&apos;;
&#xA0; &#xA0; public $context;
&#xA0; &#xA0; private static $isRegistered = false;
&#xA0; &#xA0; private $callBackFunction;
&#xA0; &#xA0; private $eof = false;
&#xA0; &#xA0; private $fp;
&#xA0; &#xA0; private $path;
&#xA0; &#xA0; private $fileStat;
&#xA0; &#xA0; private function getStat()
&#xA0; &#xA0; {
&#xA0; &#xA0; &#xA0; &#xA0; if ($fStat = fstat($this-&gt;fp)) {
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; return $fStat;
&#xA0; &#xA0; &#xA0; &#xA0; }

&#xA0; &#xA0; &#xA0; &#xA0; $size = 100;
&#xA0; &#xA0; &#xA0; &#xA0; if ($headers = get_headers($this-&gt;path, true)) {
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $head = array_change_key_case($headers, CASE_LOWER);
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $size = (int)$head[&apos;content-length&apos;];
&#xA0; &#xA0; &#xA0; &#xA0; }
&#xA0; &#xA0; &#xA0; &#xA0; $blocks = ceil($size / 512);
&#xA0; &#xA0; &#xA0; &#xA0; return array(
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &apos;dev&apos; =&gt; 16777220,
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &apos;ino&apos; =&gt; 15764,
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &apos;mode&apos; =&gt; 33188,
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &apos;nlink&apos; =&gt; 1,
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &apos;uid&apos; =&gt; 10000,
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &apos;gid&apos; =&gt; 80,
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &apos;rdev&apos; =&gt; 0,
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &apos;size&apos; =&gt; $size,
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &apos;atime&apos; =&gt; 0,
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &apos;mtime&apos; =&gt; 0,
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &apos;ctime&apos; =&gt; 0,
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &apos;blksize&apos; =&gt; 4096,
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &apos;blocks&apos; =&gt; $blocks,
&#xA0; &#xA0; &#xA0; &#xA0; );
&#xA0; &#xA0; }
&#xA0; &#xA0; public function setPath($path)
&#xA0; &#xA0; {
&#xA0; &#xA0; &#xA0; &#xA0; $this-&gt;path = $path;
&#xA0; &#xA0; &#xA0; &#xA0; $this-&gt;fp = fopen($this-&gt;path, &apos;rb&apos;) or die(&apos;Cannot open file:&#xA0; &apos; . $this-&gt;path);
&#xA0; &#xA0; &#xA0; &#xA0; $this-&gt;fileStat = $this-&gt;getStat();
&#xA0; &#xA0; }
&#xA0; &#xA0; public function read($count) {
&#xA0; &#xA0; &#xA0; &#xA0; return fread($this-&gt;fp, $count);
&#xA0; &#xA0; }
&#xA0; &#xA0; public function getStreamPath()
&#xA0; &#xA0; {
&#xA0; &#xA0; &#xA0; &#xA0; return str_replace(array(&apos;ftp://&apos;, &apos;http://&apos;, &apos;https://&apos;), self::WRAPPER_NAME . &apos;://&apos;, $this-&gt;path);
&#xA0; &#xA0; }
&#xA0; &#xA0; public function getContext()
&#xA0; &#xA0; {
&#xA0; &#xA0; &#xA0; &#xA0; if (!self::$isRegistered) {
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; stream_wrapper_register(self::WRAPPER_NAME, get_class());
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; self::$isRegistered = true;
&#xA0; &#xA0; &#xA0; &#xA0; }
&#xA0; &#xA0; &#xA0; &#xA0; return stream_context_create(
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; array(
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; self::WRAPPER_NAME =&gt; array(
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &apos;cb&apos; =&gt; array($this, &apos;read&apos;),
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &apos;fileStat&apos; =&gt; $this-&gt;fileStat,
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; )
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; )
&#xA0; &#xA0; &#xA0; &#xA0; );
&#xA0; &#xA0; }
&#xA0; &#xA0; public function stream_open($path, $mode, $options, &amp;$opened_path)
&#xA0; &#xA0; {
&#xA0; &#xA0; &#xA0; &#xA0; if (!preg_match(&apos;/^r[bt]?$/&apos;, $mode) || !$this-&gt;context) {
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; return false;
&#xA0; &#xA0; &#xA0; &#xA0; }
&#xA0; &#xA0; &#xA0; &#xA0; $opt = stream_context_get_options($this-&gt;context);
&#xA0; &#xA0; &#xA0; &#xA0; if (!is_array($opt[self::WRAPPER_NAME]) ||
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; !isset($opt[self::WRAPPER_NAME][&apos;cb&apos;]) ||
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; !is_callable($opt[self::WRAPPER_NAME][&apos;cb&apos;])
&#xA0; &#xA0; &#xA0; &#xA0; ) {
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; return false;
&#xA0; &#xA0; &#xA0; &#xA0; }
&#xA0; &#xA0; &#xA0; &#xA0; $this-&gt;callBackFunction = $opt[self::WRAPPER_NAME][&apos;cb&apos;];
&#xA0; &#xA0; &#xA0; &#xA0; $this-&gt;fileStat = $opt[self::WRAPPER_NAME][&apos;fileStat&apos;];

&#xA0; &#xA0; &#xA0; &#xA0; return true;
&#xA0; &#xA0; }
&#xA0; &#xA0; public function stream_read($count)
&#xA0; &#xA0; {
&#xA0; &#xA0; &#xA0; &#xA0; if ($this-&gt;eof || !$count) {
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; return &apos;&apos;;
&#xA0; &#xA0; &#xA0; &#xA0; }
&#xA0; &#xA0; &#xA0; &#xA0; if (($s = call_user_func($this-&gt;callBackFunction, $count)) == &apos;&apos;) {
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $this-&gt;eof = true;
&#xA0; &#xA0; &#xA0; &#xA0; }
&#xA0; &#xA0; &#xA0; &#xA0; return $s;
&#xA0; &#xA0; }
&#xA0; &#xA0; public function stream_eof()
&#xA0; &#xA0; {
&#xA0; &#xA0; &#xA0; &#xA0; return $this-&gt;eof;
&#xA0; &#xA0; }
&#xA0; &#xA0; public function stream_stat()
&#xA0; &#xA0; {
&#xA0; &#xA0; &#xA0; &#xA0; return $this-&gt;fileStat;
&#xA0; &#xA0; }
&#xA0; &#xA0; public function stream_cast($castAs)
&#xA0; &#xA0; {
&#xA0; &#xA0; &#xA0; &#xA0; $read = null;
&#xA0; &#xA0; &#xA0; &#xA0; $write&#xA0; = null;
&#xA0; &#xA0; &#xA0; &#xA0; $except = null;
&#xA0; &#xA0; &#xA0; &#xA0; return @stream_select($read, $write, $except, $castAs);
&#xA0; &#xA0; }
}
$path = &apos;http://fc04.deviantart.net/fs71/f/2010/227/4/6/PNG_Test_by_Destron23.png&apos;;
echo &quot;File: &quot;, $path, &quot;\n&quot;;
$wrapper = new MimeStreamWrapper();
$wrapper-&gt;setPath($path);

$fInfo = new finfo(FILEINFO_MIME);
echo &quot;MIME-type: &quot;, $fInfo-&gt;file($wrapper-&gt;getStreamPath(), FILEINFO_MIME_TYPE, $wrapper-&gt;getContext()), &quot;\n&quot;;
?>
```



  

#



Tempting as it may seem to use finfo_file() to validate uploaded image files (Check whether a supposed imagefile really contains an image), the results cannot be trusted. It&apos;s not that hard to wrap harmful executable code in a file identified as a GIF for instance.

A better &amp; safer option is to check the result of:

if (!$img = @imagecreatefromgif($uploadedfilename)) {
&#xA0; trigger_error(&apos;Not a GIF image!&apos;,E_USER_WARNING);
&#xA0; // do necessary stuff
}

  

#

[Official documentation page](https://www.php.net/manual/en/function.finfo-file.php)

**[To root](/README.md)**