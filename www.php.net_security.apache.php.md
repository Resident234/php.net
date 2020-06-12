# Installed as an Apache module




<div class="phpcode"><span class="html">
doc_root already limits apache/php script folder locations.<br><br>open_basedir is better used to restrict script access to folders<br>which do NOT contain scripts. Can be a sub-folder of doc_root as in php doc example doc_root/tmp, but better yet in a separate folder tree, like ~user/open_basedir_root/. Harmful scripts could modify other scripts if doc_root (or include_path) and open_basedir overlap.<br>If apache/php can&apos;t browse scripts in open_basedir, even if malicious scripts uploaded more bad scripts there, they won&apos;t be browse-able (executable). <br><br>One should also note that the many shell execute functions are effectively a way to bypass open_basedir limits, and such functions should be disabled if security demands strict folder access control. Harmful scripts can do the unix/windows version of &quot;delete */*/*/*&quot; if allowed to execute native os shell commands via those functions. OS Shell commands could similarly bypass redirect restrictions and upload file restrictions by just brute force copying files into the doc_root tree. It would be nice if they could be disabled as a group or class of functions, but it is still possible to disable them one by one if needed for security.<br><br>PS. currently there is a bug whereby the documented setting of open_basedir to docroot/tmp will not work if any include or require statements are done. Right now include will fail if the included php file is not in BOTH the open_basedir tree and the doc_root+include_path trees. Which is the opposite of safe.<br>This means by any included php file must be in open_basedir, so is vulnerable to harmful scripts and php viruses like Injektor.</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/security.apache.php)

**[⬆ to root](/)**