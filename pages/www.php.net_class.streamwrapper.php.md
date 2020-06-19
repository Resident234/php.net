# The streamWrapper class




<div class="phpcode"><span class="html">
It&apos;s worth noting that the interface defined by yannick at gmail should not always be implemented by a stream wrapper class, as several of the methods should not be implemented if the class has no use for them (as per the manual). <br><br>Specifically, mkdir, rename, rmdir, and unlink are methods that &quot;should not be defined&quot; if the wrapper has no use for them. The consequence is that the appropriate error message will not be returned. <br><br>If the interface is implemented, you won&apos;t have the flexibility to not implement those methods.<br><br>Not trying to be academic, but it was useful for me.</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/class.streamwrapper.php)

**[To root](/README.md)**