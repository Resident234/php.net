# imap_headers





Ok, this page is driving me crazy. Parsing the lines returned in the array is simple enough but there is no definitions on what the flags mean. So I searched the web to find the answer and this is what I was able to gather:

A - Answered: email has been replied to
N - New: Recent and not seen
R - Recent: Recent and seen
U - Unread: The message has not been read yet
F - Flagged: Message is &quot;flagged&quot; for urgent/special attention
D - Deleted: Message is &quot;deleted&quot; for removal by later EXPUNGE
X - Draft: Message has not completed composition (marked as a draft).

please correct me if I am wrong...

  

#

[Official documentation page](https://www.php.net/manual/en/function.imap-headers.php)

**[To root](/README.md)**