# IMAP Functions




<div class="phpcode"><span class="html">
Since this library at a whole is fairly poorly documented, and it doesn&apos;t help that there&apos;s 30-something functions, and many of the functions do the same things, I have gone through and categorized the functions.&#xA0; Hopefully this will help somebody else, I know it will help me!! -Justin
<br>
<br>CONNECTION, ERRORS &amp; QUOTAS
<br>===========================
<br>imap_timeout 
<br>imap_ping 
<br>imap_open 
<br>imap_reopen 
<br>imap_close 
<br>imap_check **(fairly useless)
<br>imap_alerts 
<br>imap_errors
<br>imap_last_error 
<br>imap_get_quota 
<br>imap_get_quotaroot 
<br>imap_set_quota 
<br>
<br>MESSAGES - READING
<br>==================
<br>imap_uid 
<br>imap_msgno 
<br>imap_fetchbody 
<br>imap_fetchheader 
<br>imap_fetchstructure 
<br>imap_fetch_overview 
<br>imap_body 
<br>imap_rfc822_parse_adrlist 
<br>imap_rfc822_parse_headers 
<br>
<br>MESSAGES - WRITING
<br>==================
<br>imap_mail_compose 
<br>imap_mail
<br>imap_append 
<br>imap_rfc822_write_address 
<br>
<br>MESSAGES - OPERATIONS
<br>=====================
<br>imap_undelete 
<br>imap_thread
<br>imap_delete 
<br>imap_mail_copy 
<br>imap_mail_move 
<br>imap_expunge 
<br>imap_clearflag_full 
<br>imap_setflag_full 
<br>
<br>MESSAGES - DECODE/ENCODE
<br>========================
<br>imap_utf7_decode 
<br>imap_utf7_encode 
<br>imap_utf8
<br>imap_8bit 
<br>imap_base64 
<br>imap_binary 
<br>imap_mime_header_decode 
<br>imap_qprint 
<br>
<br>FOLDERS
<br>=======
<br>imap_createmailbox 
<br>imap_deletemailbox 
<br>imap_getmailboxes 
<br>imap_mailboxmsginfo 
<br>imap_renamemailbox 
<br>imap_headers **(fairly useless)
<br>imap_status 
<br>imap_sort 
<br>imap_search
<br>imap_listscan
<br>
<br>NNTP
<br>====
<br>imap_unsubscribe 
<br>imap_subscribe 
<br>imap_getsubscribed 
<br>
<br>Others
<br>=============================
<br>imap_num_msg - use imap_mailboxmsginfo()
<br>imap_num_recent - use imap_mailboxmsginfo() 
<br>imap_header - alias of imap_headerinfo()
<br>imap_scanmailbox - alias of imap_listscan()
<br>imap_listsubscribed - alias of imap_lsub()
<br>imap_listmailbox - alias of imap_list()
<br>imap_lsub - use imap_getsubscribed()
<br>imap_list - use imap_getmailboxes()
<br>imap_bodystruct - not documented
<br>imap_getacl - not documented
<br>imap_setacl - not documented
<br>imap_headerinfo - use imap_fetch_overview()</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/ref.imap.php)

**[To root](/README.md)**