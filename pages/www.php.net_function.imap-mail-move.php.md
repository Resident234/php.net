# imap_mail_move





After using imap_mail_move, imap_mail_copy or imap_delete it is necesary to call imap_expunge() function.

  

#



I had the most trouble with figureing out what the message list was supposed to be.&#xA0; There was one comment by jan@showstar.com but i was still terribly confused.&#xA0; So I searched and searched and searched.&#xA0; I read rfc2060 over 10 times.&#xA0; Then BAM!&#xA0; My brother found it here:

http://www.cis.ohio-state.edu/cgi-bin/rfc/rfc2060.html#sec-6.4.7

Here is the importand stuff. 

Another important field is the SEQUENCE, which identifies a set of messages by consecutive numbers from 1 to n where n is the number of messages in the mailbox.&#xA0; A sequence may consist of a single number, a pair of numbers delimited by colon (equivalent to all numbers between those two numbers), or a list of single numbers or number pairs.&#xA0; For example, the sequence 2,4:7,9,12:15 is equivalent to
&#xA0;&#xA0; 2,4,5,6,7,9,12,13,14,15 and identifies all those messages.

There is what I know about it.&#xA0; BAM!

  

#

[Official documentation page](https://www.php.net/manual/en/function.imap-mail-move.php)

**[To root](/README.md)**