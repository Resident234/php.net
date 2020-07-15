# finfo_file



The way HOWTO get MIME-type of remote file.<br><br>

```
<?php
class MimeStreamWrapper
{
    const WRAPPER_NAME = 'mime';
    public $context;
    private static $isRegistered = false;
    private $callBackFunction;
    private $eof = false;
    private $fp;
    private $path;
    private $fileStat;
    private function getStat()
    {
        if ($fStat = fstat($this->fp)) {
            return $fStat;
        }

        $size = 100;
        if ($headers = get_headers($this->path, true)) {
            $head = array_change_key_case($headers, CASE_LOWER);
            $size = (int)$head['content-length'];
        }
        $blocks = ceil($size / 512);
        return array(
            'dev' => 16777220,
            'ino' => 15764,
            'mode' => 33188,
            'nlink' => 1,
            'uid' => 10000,
            'gid' => 80,
            'rdev' => 0,
            'size' => $size,
            'atime' => 0,
            'mtime' => 0,
            'ctime' => 0,
            'blksize' => 4096,
            'blocks' => $blocks,
        );
    }
    public function setPath($path)
    {
        $this->path = $path;
        $this->fp = fopen($this->path, 'rb') or die('Cannot open file:  ' . $this->path);
        $this->fileStat = $this->getStat();
    }
    public function read($count) {
        return fread($this->fp, $count);
    }
    public function getStreamPath()
    {
        return str_replace(array('ftp://', 'http://', 'https://'), self::WRAPPER_NAME . '://', $this->path);
    }
    public function getContext()
    {
        if (!self::$isRegistered) {
            stream_wrapper_register(self::WRAPPER_NAME, get_class());
            self::$isRegistered = true;
        }
        return stream_context_create(
            array(
                self::WRAPPER_NAME => array(
                    'cb' => array($this, 'read'),
                    'fileStat' => $this->fileStat,
                )
            )
        );
    }
    public function stream_open($path, $mode, $options, &amp;$opened_path)
    {
        if (!preg_match('/^r[bt]?$/', $mode) || !$this->context) {
            return false;
        }
        $opt = stream_context_get_options($this->context);
        if (!is_array($opt[self::WRAPPER_NAME]) ||
            !isset($opt[self::WRAPPER_NAME]['cb']) ||
            !is_callable($opt[self::WRAPPER_NAME]['cb'])
        ) {
            return false;
        }
        $this->callBackFunction = $opt[self::WRAPPER_NAME]['cb'];
        $this->fileStat = $opt[self::WRAPPER_NAME]['fileStat'];

        return true;
    }
    public function stream_read($count)
    {
        if ($this->eof || !$count) {
            return '';
        }
        if (($s = call_user_func($this->callBackFunction, $count)) == '') {
            $this->eof = true;
        }
        return $s;
    }
    public function stream_eof()
    {
        return $this->eof;
    }
    public function stream_stat()
    {
        return $this->fileStat;
    }
    public function stream_cast($castAs)
    {
        $read = null;
        $write  = null;
        $except = null;
        return @stream_select($read, $write, $except, $castAs);
    }
}
$path = 'http://fc04.deviantart.net/fs71/f/2010/227/4/6/PNG_Test_by_Destron23.png';
echo "File: ", $path, "\n";
$wrapper = new MimeStreamWrapper();
$wrapper->setPath($path);

$fInfo = new finfo(FILEINFO_MIME);
echo "MIME-type: ", $fInfo->file($wrapper->getStreamPath(), FILEINFO_MIME_TYPE, $wrapper->getContext()), "\n";
?>
```
  

#

Tempting as it may seem to use finfo_file() to validate uploaded image files (Check whether a supposed imagefile really contains an image), the results cannot be trusted. It&apos;s not that hard to wrap harmful executable code in a file identified as a GIF for instance.<br><br>A better &amp; safer option is to check the result of:<br><br>if (!$img = @imagecreatefromgif($uploadedfilename)) {<br>  trigger_error(&apos;Not a GIF image!&apos;,E_USER_WARNING);<br>  // do necessary stuff<br>}  

#

[Official documentation page](https://www.php.net/manual/en/function.finfo-file.php)

**[To root](/README.md)**