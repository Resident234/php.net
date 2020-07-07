# is_dir





Just a note for anyone who encounters is_dir() returning false on CIFS mount points or directories within those mount points on 2.6.31 and newer kernels: Apparently in new kernels they&apos;ve started using the CIFS serverino option by default.&#xA0; With Windows shares this causes huge inode numbers and which apparently can cause is_dir() to return false.&#xA0; Adding the noserverino option to the CIFS mount will prevent this.&#xA0; This may only occur on 32 systems but I don&apos;t have a 64 bit install to test against.

  

#



Note that on Linux is_dir returns FALSE if a parent directory does not have +x (executable) set for the php process.

  

#



This is the &quot;is_dir&quot; function I use to solve the problems :

function Another_is_dir ($file)
{
&#xA0; &#xA0; if ((fileperms(&quot;$file&quot;) &amp; 0x4000) == 0x4000)
&#xA0; &#xA0; &#xA0; &#xA0; return TRUE;
&#xA0; &#xA0; else
&#xA0; &#xA0; &#xA0; &#xA0; return FALSE;
}

or, more simple :

function Another_is_dir ($file)
{ 
return ((fileperms(&quot;$file&quot;) &amp; 0x4000) == 0x4000);
}

I can&apos;t remember where it comes from, but it works fine.

  

#

[Official documentation page](https://www.php.net/manual/en/function.is-dir.php)

**[To root](/README.md)**