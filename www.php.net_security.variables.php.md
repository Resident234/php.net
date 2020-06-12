# User Submitted Data




<div class="phpcode"><span class="html">
One thing I would repeat in the docs here is what information actually comes from the user. Many people think a Cookie, since it&apos;s written by PHP, was safe. But the fact is that it&apos;s stored on the user&apos;s computer, transferred by the user&apos;s browser, and thus very easy to manipulate.<br><br>So, it&apos;d be handy to mention here again that:<br><br>CGI parameters in the URL, HTTP POST data and cookie variables are considered &quot;user data&quot; and thus need to be validated. Session data and SQL database contents only need to be validated if they came from untrustworthy sources (like the ones just mentioned).<br><br>Not new, but I would have expected this info under this headline, at least as a short recap plus linlk to the actual docs.</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/security.variables.php)

**[To root](/)**