# gnupg_decrypt




<div class="phpcode"><span class="html">
As of gnupg version 2, it is not possible to pass a plain password any more. The parameter is simply ignored. Instead, a pinentry application will be launched in case of php running in cli mode. In cgi or apache mode, opening the key will fail.<br>The simplest solution is to use keys without passwords.</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/function.gnupg-decrypt.php)

**[To root](/README.md)**