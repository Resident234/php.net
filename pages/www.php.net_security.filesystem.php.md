# Filesystem Security





(A) Better not to create files or folders with user-supplied names. If you do not validate enough, you can have trouble. Instead create files and folders with randomly generated names like fg3754jk3h and store the username and this file or folder name in a table named, say, user_objects. This will ensure that whatever the user may type, the command going to the shell will contain values from a specific set only and no mischief can be done.

(B) The same applies to commands executed based on an operation that the user chooses. Better not to allow any part of the user&apos;s input to go to the command that you will execute. Instead, keep a fixed set of commands and based on what the user has input, and run those only. 

For example,
(A) Keep a table named, say, user_objects with values like:
username|chosen_name&#xA0;&#xA0; |actual_name|file_or_dir
--------|--------------|-----------|-----------
jdoe&#xA0; &#xA0; |trekphotos&#xA0; &#xA0; |m5fg767h67 |D
jdoe&#xA0; &#xA0; |notes.txt&#xA0; &#xA0;&#xA0; |nm4b6jh756 |F
tim1997 |_imp_ folder&#xA0; |45jkh64j56 |D

and always use the actual_name in the filesystem operations rather than the user supplied names.

(B)


```
<?php
$op = $_POST[&apos;op&apos;];//after a lot of validations 
$dir = $_POST[&apos;dirname&apos;];//after a lot of validations or maybe you can use technique (A)
switch($op){
&#xA0; &#xA0; case &quot;cd&quot;:
&#xA0; &#xA0; &#xA0; &#xA0; chdir($dir);
&#xA0; &#xA0; &#xA0; &#xA0; break;
&#xA0; &#xA0; case &quot;rd&quot;:
&#xA0; &#xA0; &#xA0; &#xA0; rmdir($dir);
&#xA0; &#xA0; &#xA0; &#xA0; break;
&#xA0; &#xA0; .....
&#xA0; &#xA0; default:
&#xA0; &#xA0; &#xA0; &#xA0; mail(&quot;webmaster@example.com&quot;, &quot;Mischief&quot;, $_SERVER[&apos;REMOTE_ADDR&apos;].&quot; is probably attempting an attack.&quot;);
}


  

#



Well, the fact that all users run under the same UID is a big problem. Userspace&#xA0; security hacks (ala safe_mode) should not be substitution for proper kernel level security checks/accounting.

Good news: Apache 2 allows you to assign UIDs for different vhosts.

devik

  

#



All of the fixes here assume that it is necessary to allow the user to enter system sensitive information to begin with. The proper way to handle this would be to provide something like a numbered list of files to perform an unlink action on and then the chooses the matching number. There is no way for the user to specify a clever attack circumventing whatever pattern matching filename exclusion syntax that you may have.

Anytime you have a security issue, the proper behaviour is to deny all then allow specific instances, not allow all and restrict. For the simple reason that you may not think of every possible restriction.

  

#

[Official documentation page](https://www.php.net/manual/en/security.filesystem.php)

**[To root](/README.md)**