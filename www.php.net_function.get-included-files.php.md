# get_included_files




<div class="phpcode"><span class="html">
As of PHP5, this function seems to return an array with the first index being the script all subsequent scripts are included to.<br>If index.php includes b.php and c.php and calls get_included_files(), the returned array looks as follows:<br><br>index.php<br>a.php<br>b.php<br><br>while in PHP&lt;5 the array would be:<br><br>a.php<br>b.php<br><br>If you want to know which is the script that is including current script you can use $_SERVER[&apos;SCRIPT_FILENAME&apos;] or any other similar server global.<br><br>If you also want to ensure current script is being included and not run independently you should evaluate following expression:<br><br>__FILE__ != $_SERVER[&apos;SCRIPT_FILENAME&apos;]<br><br>If this expression returns TRUE, current script is being included or required.</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/function.get-included-files.php)

**[To root](/README.md)**