# InstallationConfiguring PHP with OCI8Installing OCI8 as a Shared ExtensionInstalling OCI8 as a Statically Compiled ExtensionInstalling OCI8 from PECLInstalling OCI8 on WindowsSetting the Oracle EnvironmentTroubleshooting




<div class="phpcode"><span class="html">
If you&apos;ve followed the instructions and you can&apos;t even connect to the DB server, welcome to the Oracle hell. Most of the information you&apos;ll find is deprecated, incomplete, not for your platform, unnecessary or just plain wrong.
<br>
<br>Typically, you won&apos;t need at all those complicate setups you&apos;ll read about and they&apos;ll probably make things harder. I suggest you get Systernal&apos;s &quot;Filemon&quot; utility (for Windows, in Unix you may do with strace) and find out what exact config files and DLLs are being tried by php.exe (or httpd.exe if PHP runs as Apache module or...). Pretty often, the issue is that (e.g.) TNSNAMES.ORA does not have the correct line ending or Apache is looking for a DLL that does not even exist in your hard disc; learning that prevents you to waste time adding more and more useless environmental variables.
<br>
<br>Goog luck.</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/oci8.installation.php)

**[To root](/README.md)**