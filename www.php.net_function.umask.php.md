# umask




<div class="phpcode"><span class="html">
I think that the best way to understand umask is to say that umask is used to revoke permissions, not to set permissions.<br><br>umask sets which permissions must be removed from the system default when you create a file or a directory.<br><br>For example, a mask 0022 means that you don&apos;t want group and others modify the file. <br><br>default 0666 rw-.rw-.rw- <br>umask&#xA0;&#xA0; 0022 ---.-w-.-w-<br>Final&#xA0;&#xA0; 0644 rw-.r--.r--<br><br>That means that any file from now on will have 0644 permissions.<br><br>It is important to understand that umask revokes, deletes permissions from system default, so it can&#xB4;t grant permissions the system default hasn&apos;t. In the example above, with the 666 system default, there is no way you can use umask to create a file with execute permission. If you want to grant more permissions, use chmod.<br><br>Be aware that system default permissions are not related to PHP (they depends upon server configuration). PHP has a default umask that is applied after system default base permissions. And there are different system default base permissions for files and directories.<br><br>Usually, system default permissions for files are 666 and for directories 0777. And usually, default PHP umask is 0022</span>
</div>
  

#


<div class="phpcode"><span class="html">
&quot;It is better to change the file permissions with chmod() after creating the file.&quot;<br><br>The usual lacking of security knowledge within the PHP team rears its head once again.&#xA0; You *always* want to have the file created with the proper permission.&#xA0; Let me illustrate why:<br><br>(a) you create new file with read permissions<br>(b) an attacking script opens the file<br>(c) you chmod the file to remove read permissions<br>(d) you write sensitive data to the file<br><br>Now, you might think that the changes of an attacking script getting to open the file before you chmod them are low.&#xA0; And you&apos;re right.&#xA0; But low changes are never low enough - you want zero chance.<br><br>When creating a file that needs increased permissions, you always need to create the file with the proper permissions, and also create it with O_EXCL set.&#xA0; If you don&apos;t do an exclusive create, you end up with this scenario:<br><br>(a) attacker creates the file, makes it writable to everyone<br>(b) you open the file with restricted permissions, but since it already exists, the file is merely opened and the permissions left alone<br>(c) you write sensitive data into the insecure file<br><br>Detecting the latter scenario is possible, but it requires a bit of work.&#xA0; You have to check that the file&apos;s owner and group match the script&apos;s (that is, posix_geteuid(), not myuid()) and check the permissions - if any of those are incorrect, then the file is insecure - you can attempt to unlink() it and try again while logging a warning, of course.<br><br>The only time when it is reasonable or safe to chmod() a file after creating it is when you want to grant extra permissions instead of removing them.&#xA0; For example, it is completely safe to set the umask to 0077 and then chmoding the files you create afterward.<br><br>Doing truly secure programming in PHP is difficult as is, and advice like this in the documentation just makes things worse.&#xA0; Remember, kids, anything that applies to security in the C or UNIX worlds is 100% applicable to PHP.&#xA0; The best thing you can possibly do for yourself as a PHP programmer is to learn and understand secure C and UNIX programming techniques.</span>
</div>
  

#


<div class="phpcode"><span class="html">
In case you don&apos;t understand why you need to &quot;Avoid using this function in multithreaded webservers&quot;:<br><br>It&apos;s because this function changes the umask at the process level, rather than only for PHP or for the current script.&#xA0; If there are multiple simultaneous threads running in the process in which your PHP script is running, the change will apply to all of those threads at the same time hence why this is not safe for multithreaded use.<br><br>I understand that if you are using the PHP module and Apache&apos;s prefork MPM, which is not multi-threaded, then you at least won&apos;t get race-condition problems such as this.&#xA0; However, it is still worth noting that the umask setting, if not re-set, will persist for the life of that process even if the process is re-used to serve future PHP or non-PHP requests.</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/function.umask.php)

**[⬆ to root](/)**