# imap_headerinfo




<div class="phpcode"><span class="html">
When I was testing imap_headerinfo() with an e-mail that had multiple recipients (multiple e-mails in to to: and/or cc: field), I noticed that imap_headerinfo() was failing hard for me on PHP 5.2.10-2ubuntu6.4 with Suhosin-Patch 0.9.7 (cli).<br><br>Rather than providing me an array with each and every e-mail address listed in the to and/or cc fields, it was only providing me the first listed.&#xA0; This was disappointing.<br><br>&#xA0;&#xA0; [to] =&gt; Array<br>&#xA0; &#xA0; &#xA0; &#xA0; (&#xA0;&#xA0; <br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; [0] =&gt; stdClass Object<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; (&#xA0;&#xA0; <br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; [mailbox] =&gt; game<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; [host] =&gt; blunts.com<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; )<br>&#xA0; &#xA0; &#xA0; &#xA0; )<br><br>Luckily, there was a cool workaround to this problem:<br><br>imap_rfc822_parse_headers(imap_fetchheader( string ); which subsequentally worked like a nice little pet would:<br><br>&#xA0;&#xA0; [to] =&gt; Array<br>&#xA0; &#xA0; &#xA0; &#xA0; (&#xA0;&#xA0; <br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; [0] =&gt; stdClass Object<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; (&#xA0;&#xA0; <br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; [mailbox] =&gt; game<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; [host] =&gt; blunts.com<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; )<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; [1] =&gt; stdClass Object<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; (&#xA0;&#xA0; <br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; [mailbox] =&gt; dutch<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; [host] =&gt; masters.com<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; )<br>&#xA0; &#xA0; &#xA0; &#xA0; )<br><br>TL;DR: <br>So in other words, instead of imap_headerinfo() use imap_rfc822_parse_headers(imap_fetchheader()).<br><br>Hope this helps anyone else that runs into this issue from now into the future.&#xA0; This post was suggested by people in #PHP on FreeNode IRC.</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/function.imap-headerinfo.php)

**[To root](/README.md)**