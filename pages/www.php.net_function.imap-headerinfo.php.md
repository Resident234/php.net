# imap_headerinfo



When I was testing imap_headerinfo() with an e-mail that had multiple recipients (multiple e-mails in to to: and/or cc: field), I noticed that imap_headerinfo() was failing hard for me on PHP 5.2.10-2ubuntu6.4 with Suhosin-Patch 0.9.7 (cli).<br><br>Rather than providing me an array with each and every e-mail address listed in the to and/or cc fields, it was only providing me the first listed.  This was disappointing.<br><br>   [to] =&gt; Array<br>        (   <br>            [0] =&gt; stdClass Object<br>                (   <br>                    [mailbox] =&gt; game<br>                    [host] =&gt; blunts.com<br>                )<br>        )<br><br>Luckily, there was a cool workaround to this problem:<br><br>imap_rfc822_parse_headers(imap_fetchheader( string ); which subsequentally worked like a nice little pet would:<br><br>   [to] =&gt; Array<br>        (   <br>            [0] =&gt; stdClass Object<br>                (   <br>                    [mailbox] =&gt; game<br>                    [host] =&gt; blunts.com<br>                )<br>            [1] =&gt; stdClass Object<br>                (   <br>                    [mailbox] =&gt; dutch<br>                    [host] =&gt; masters.com<br>                )<br>        )<br><br>TL;DR: <br>So in other words, instead of imap_headerinfo() use imap_rfc822_parse_headers(imap_fetchheader()).<br><br>Hope this helps anyone else that runs into this issue from now into the future.  This post was suggested by people in #PHP on FreeNode IRC.  

#

[Official documentation page](https://www.php.net/manual/en/function.imap-headerinfo.php)

**[To root](/README.md)**