# imap_headerinfo





When I was testing imap_headerinfo() with an e-mail that had multiple recipients (multiple e-mails in to to: and/or cc: field), I noticed that imap_headerinfo() was failing hard for me on PHP 5.2.10-2ubuntu6.4 with Suhosin-Patch 0.9.7 (cli).

Rather than providing me an array with each and every e-mail address listed in the to and/or cc fields, it was only providing me the first listed.&#xA0; This was disappointing.

&#xA0;&#xA0; [to] =&gt; Array
&#xA0; &#xA0; &#xA0; &#xA0; (&#xA0;&#xA0; 
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; [0] =&gt; stdClass Object
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; (&#xA0;&#xA0; 
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; [mailbox] =&gt; game
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; [host] =&gt; blunts.com
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; )
&#xA0; &#xA0; &#xA0; &#xA0; )

Luckily, there was a cool workaround to this problem:

imap_rfc822_parse_headers(imap_fetchheader( string ); which subsequentally worked like a nice little pet would:

&#xA0;&#xA0; [to] =&gt; Array
&#xA0; &#xA0; &#xA0; &#xA0; (&#xA0;&#xA0; 
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; [0] =&gt; stdClass Object
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; (&#xA0;&#xA0; 
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; [mailbox] =&gt; game
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; [host] =&gt; blunts.com
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; )
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; [1] =&gt; stdClass Object
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; (&#xA0;&#xA0; 
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; [mailbox] =&gt; dutch
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; [host] =&gt; masters.com
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; )
&#xA0; &#xA0; &#xA0; &#xA0; )

TL;DR: 
So in other words, instead of imap_headerinfo() use imap_rfc822_parse_headers(imap_fetchheader()).

Hope this helps anyone else that runs into this issue from now into the future.&#xA0; This post was suggested by people in #PHP on FreeNode IRC.

  

#

[Official documentation page](https://www.php.net/manual/en/function.imap-headerinfo.php)

**[To root](/README.md)**