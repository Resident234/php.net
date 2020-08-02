# imap_headers



Ok, this page is driving me crazy. Parsing the lines returned in the array is simple enough but there is no definitions on what the flags mean. So I searched the web to find the answer and this is what I was able to gather:<br><br>A - Answered: email has been replied to<br>N - New: Recent and not seen<br>R - Recent: Recent and seen<br>U - Unread: The message has not been read yet<br>F - Flagged: Message is "flagged" for urgent/special attention<br>D - Deleted: Message is "deleted" for removal by later EXPUNGE<br>X - Draft: Message has not completed composition (marked as a draft).<br><br>please correct me if I am wrong...  

---

[Official documentation page](https://www.php.net/manual/en/function.imap-headers.php)

**[To root](/README.md)**