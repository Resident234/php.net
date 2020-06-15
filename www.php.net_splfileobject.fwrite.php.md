# SplFileObject::fwrite




<div class="phpcode"><span class="html">
Your \SplFileObject will not throw an exception when trying to write to a non-writeable stream!<br><br>I forgot to set the second parameter on my \SplFileObject constructor (the mode), costing me minutes to figure out why nothing was writter by the fwrite method...<br><br>Just to let you know!</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/splfileobject.fwrite.php)

**[To root](/README.md)**