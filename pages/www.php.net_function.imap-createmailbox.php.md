# imap_createmailbox





One should understand that even though it says &quot;create mailbox&quot;, you are really creating a FOLDER. Now, as a imap admin you can create mailboxes and more with this function.

So in reality, you are always creating folders when creating &quot;mailboxes&quot;. Mail admin&apos;s get this, but programmers may not understand the concept completely.

If you auth a single user account and use these functions, they will not create mailboxes where mail is delivered, they will actually create a folder where you can copy messages to.

Here is a translation:
imap_createmailbox = create a folder in the account for the current authenticated user&apos;s imap session (imap_open)
imap_deletemailbox = delete a folder (and the email in it) for the current authenticated user&apos;s imap session (imap_open)
imap_getmailboxes = get all your folders for the current authenticated user&apos;s imap session (imap_open)
imap_renamemailbox = Rename a folder for the current authenticated user&apos;s imap session (imap_open)

================

Here is a quick class to login to an account, generate all of your base folders, and return the connection, success message and returns all the base folders for an imap account using PHP5:



```
<?php

class Imap {
&#xA0; &#xA0; public $folders;
&#xA0; &#xA0; public $connection;

&#xA0; &#xA0; public function login($user, $pass) {
&#xA0; &#xA0; &#xA0; &#xA0; $mbox = @imap_open(&quot;{imap.example.org:143}&quot;, $user, $pass);
&#xA0; &#xA0; &#xA0; &#xA0; if(!$mbox)
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; return (&apos;Your login failed for user &lt;strong&gt;&apos;.$user.&apos;&lt;/strong&gt;. Please try to enter your username and password again.&lt;br /&gt;&apos;);

&#xA0; &#xA0; &#xA0; &#xA0; // Login worked, let us begin!!!!....

&#xA0; &#xA0; &#xA0; &#xA0; // gather folder lost...
&#xA0; &#xA0; &#xA0; &#xA0; $fldrs_made = 0;
&#xA0; &#xA0; &#xA0; &#xA0; $folders = imap_listmailbox($mbox, &quot;{localhost:143}&quot;, &quot;*&quot;);
&#xA0; &#xA0; &#xA0; &#xA0; // create the default folders....
&#xA0; &#xA0; &#xA0; &#xA0; if(1 === mailgui::create_default_folders($mbox,$folders)) {
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $folders = imap_listmailbox($mbox, &quot;{localhost:143}&quot;, &quot;*&quot;);
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $fldrs_made = 1;
&#xA0; &#xA0; &#xA0; &#xA0; }

&#xA0; &#xA0; &#xA0; &#xA0; sort($folders);

&#xA0; &#xA0; &#xA0; &#xA0; $this-&gt;folders = $folders;
&#xA0; &#xA0; &#xA0; &#xA0; $this-&gt;connection = $mbox;

&#xA0; &#xA0; &#xA0; &#xA0; if(1 === $fldrs_made)
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; return (&apos;User logged in successfully as &apos;.$user.&apos;. This is your first time logging in, welcome to our webmail!!!&lt;br /&gt;&apos;);
&#xA0; &#xA0; &#xA0; &#xA0; else
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; return (&apos;User logged in successfully as &apos;.$user.&apos;.&lt;br /&gt;&apos;);
&#xA0; &#xA0; }
&#xA0; &#xA0; private function create_default_folders($imap_stream, $folders) {
&#xA0; &#xA0; &#xA0; &#xA0; $change=0;
&#xA0; &#xA0; &#xA0; &#xA0; if(!in_array(&apos;{imap.example.org}TRASH&apos;,$folders)) {
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; @imap_createmailbox($imap_stream, imap_utf7_encode(&quot;{imap.example.org:143}TRASH&quot;));
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $change=1;
&#xA0; &#xA0; &#xA0; &#xA0; }
&#xA0; &#xA0; &#xA0; &#xA0; if(!in_array(&apos;{imap.example.org}SENT&apos;,$folders)) {
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; @imap_createmailbox($imap_stream, imap_utf7_encode(&quot;{imap.example.org:143}SENT&quot;));
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $change=1;
&#xA0; &#xA0; &#xA0; &#xA0; }
&#xA0; &#xA0; &#xA0; &#xA0; if(!in_array(&apos;{imap.example.org}SPAM&apos;,$folders)) {
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; @imap_createmailbox($imap_stream, imap_utf7_encode(&quot;{imap.example.org:143}SPAM&quot;));
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $change=1;
&#xA0; &#xA0; &#xA0; &#xA0; }
&#xA0; &#xA0; &#xA0; &#xA0; if(!in_array(&apos;{imap.example.org}SENT&apos;,$folders)) {
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; @imap_createmailbox($imap_stream, imap_utf7_encode(&quot;{imap.example.org:143}SENT&quot;));
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $change=1;
&#xA0; &#xA0; &#xA0; &#xA0; }
&#xA0; &#xA0; &#xA0; &#xA0; if(!in_array(&apos;{imap.example.org}SENT&apos;,$folders)) {
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; @imap_createmailbox($imap_stream, imap_utf7_encode(&quot;{imap.example.org:143}DRAFTS&quot;));
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $change=1;
&#xA0; &#xA0; &#xA0; &#xA0; }
&#xA0; &#xA0; &#xA0; &#xA0; if(!in_array(&apos;{imap.example.org}MY_FOLDERS&apos;,$folders)) {
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; @imap_createmailbox($imap_stream, imap_utf7_encode(&quot;{imap.example.org:143}PERSONAL EMAIL&quot;));
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $change=1;
&#xA0; &#xA0; &#xA0; &#xA0; }
&#xA0; &#xA0; &#xA0; &#xA0; return $change;
&#xA0; &#xA0; }
&#xA0; &#xA0; public function close_mail_connection() {
&#xA0; &#xA0; &#xA0; &#xA0; @imap_close($this-&gt;connection);
&#xA0; &#xA0; }
}

// usage, create a form, post it....
if($_POST[&apos;imap_username&apos;] &amp;&amp; $_POST[&apos;imap_password&apos;]) {
&#xA0; &#xA0; $imap_login = new Imap();
&#xA0; &#xA0; $imap_login-&gt;login($_POST[&apos;imap_username&apos;],$_POST[&apos;imap_password&apos;]);
&#xA0; &#xA0; 
&#xA0; &#xA0; // Do some mail stuff here, like get headers...., use obj connection
&#xA0; &#xA0; $message_headers = imap_mailboxmsginfo($imap_login-&gt;connection);
&#xA0; &#xA0; 
&#xA0; &#xA0; // show the folders....
&#xA0; &#xA0; print_r($imap_login-&gt;folders, true);
&#xA0; &#xA0; 
&#xA0; &#xA0; print &apos;&lt;br /&gt;&lt;hr size=&quot;1&quot; noshade /&gt;&apos;;
&#xA0; &#xA0; 
&#xA0; &#xA0; print_r($message_headers, true);
&#xA0; &#xA0; 

&#xA0; &#xA0; // close the connection
&#xA0; &#xA0; $imap_login-&gt;close_mail_connection();
}

?>
```



  

#

[Official documentation page](https://www.php.net/manual/en/function.imap-createmailbox.php)

**[To root](/README.md)**