# Filesystem Security




<div class="phpcode"><span class="html">
(A) Better not to create files or folders with user-supplied names. If you do not validate enough, you can have trouble. Instead create files and folders with randomly generated names like fg3754jk3h and store the username and this file or folder name in a table named, say, user_objects. This will ensure that whatever the user may type, the command going to the shell will contain values from a specific set only and no mischief can be done.<br><br>(B) The same applies to commands executed based on an operation that the user chooses. Better not to allow any part of the user&apos;s input to go to the command that you will execute. Instead, keep a fixed set of commands and based on what the user has input, and run those only. <br><br>For example,<br>(A) Keep a table named, say, user_objects with values like:<br>username|chosen_name&#xA0;&#xA0; |actual_name|file_or_dir<br>--------|--------------|-----------|-----------<br>jdoe&#xA0; &#xA0; |trekphotos&#xA0; &#xA0; |m5fg767h67 |D<br>jdoe&#xA0; &#xA0; |notes.txt&#xA0; &#xA0;&#xA0; |nm4b6jh756 |F<br>tim1997 |_imp_ folder&#xA0; |45jkh64j56 |D<br><br>and always use the actual_name in the filesystem operations rather than the user supplied names.<br><br>(B)<br><span class="default">&lt;?php<br>$op </span><span class="keyword">= </span><span class="default">$_POST</span><span class="keyword">[</span><span class="string">&apos;op&apos;</span><span class="keyword">];</span><span class="comment">//after a lot of validations <br></span><span class="default">$dir </span><span class="keyword">= </span><span class="default">$_POST</span><span class="keyword">[</span><span class="string">&apos;dirname&apos;</span><span class="keyword">];</span><span class="comment">//after a lot of validations or maybe you can use technique (A)<br></span><span class="keyword">switch(</span><span class="default">$op</span><span class="keyword">){<br>&#xA0; &#xA0; case </span><span class="string">&quot;cd&quot;</span><span class="keyword">:<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">chdir</span><span class="keyword">(</span><span class="default">$dir</span><span class="keyword">);<br>&#xA0; &#xA0; &#xA0; &#xA0; break;<br>&#xA0; &#xA0; case </span><span class="string">&quot;rd&quot;</span><span class="keyword">:<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">rmdir</span><span class="keyword">(</span><span class="default">$dir</span><span class="keyword">);<br>&#xA0; &#xA0; &#xA0; &#xA0; break;<br>&#xA0; &#xA0; .....<br>&#xA0; &#xA0; default:<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">mail</span><span class="keyword">(</span><span class="string">&quot;webmaster@example.com&quot;</span><span class="keyword">, </span><span class="string">&quot;Mischief&quot;</span><span class="keyword">, </span><span class="default">$_SERVER</span><span class="keyword">[</span><span class="string">&apos;REMOTE_ADDR&apos;</span><span class="keyword">].</span><span class="string">&quot; is probably attempting an attack.&quot;</span><span class="keyword">);<br>}</span>
</span>
</div>
  

#


<div class="phpcode"><span class="html">
Well, the fact that all users run under the same UID is a big problem. Userspace&#xA0; security hacks (ala safe_mode) should not be substitution for proper kernel level security checks/accounting.
<br>Good news: Apache 2 allows you to assign UIDs for different vhosts.
<br>devik</span>
</div>
  

#


<div class="phpcode"><span class="html">
All of the fixes here assume that it is necessary to allow the user to enter system sensitive information to begin with. The proper way to handle this would be to provide something like a numbered list of files to perform an unlink action on and then the chooses the matching number. There is no way for the user to specify a clever attack circumventing whatever pattern matching filename exclusion syntax that you may have.<br><br>Anytime you have a security issue, the proper behaviour is to deny all then allow specific instances, not allow all and restrict. For the simple reason that you may not think of every possible restriction.</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/security.filesystem.php)

**[⬆ to root](/)**