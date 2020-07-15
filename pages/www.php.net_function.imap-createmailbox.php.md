# imap_createmailbox



One should understand that even though it says "create mailbox", you are really creating a FOLDER. Now, as a imap admin you can create mailboxes and more with this function.<br><br>So in reality, you are always creating folders when creating "mailboxes". Mail admin&apos;s get this, but programmers may not understand the concept completely.<br><br>If you auth a single user account and use these functions, they will not create mailboxes where mail is delivered, they will actually create a folder where you can copy messages to.<br><br>Here is a translation:<br>imap_createmailbox = create a folder in the account for the current authenticated user&apos;s imap session (imap_open)<br>imap_deletemailbox = delete a folder (and the email in it) for the current authenticated user&apos;s imap session (imap_open)<br>imap_getmailboxes = get all your folders for the current authenticated user&apos;s imap session (imap_open)<br>imap_renamemailbox = Rename a folder for the current authenticated user&apos;s imap session (imap_open)<br><br>================<br><br>Here is a quick class to login to an account, generate all of your base folders, and return the connection, success message and returns all the base folders for an imap account using PHP5:<br><br>

```
<?php

class Imap {
    public $folders;
    public $connection;

    public function login($user, $pass) {
        $mbox = @imap_open("{imap.example.org:143}", $user, $pass);
        if(!$mbox)
            return ('Your login failed for user &lt;strong&gt;'.$user.'&lt;/strong&gt;. Please try to enter your username and password again.&lt;br /&gt;');

        // Login worked, let us begin!!!!....

        // gather folder lost...
        $fldrs_made = 0;
        $folders = imap_listmailbox($mbox, "{localhost:143}", "*");
        // create the default folders....
        if(1 === mailgui::create_default_folders($mbox,$folders)) {
            $folders = imap_listmailbox($mbox, "{localhost:143}", "*");
            $fldrs_made = 1;
        }

        sort($folders);

        $this->folders = $folders;
        $this->connection = $mbox;

        if(1 === $fldrs_made)
            return ('User logged in successfully as '.$user.'. This is your first time logging in, welcome to our webmail!!!&lt;br /&gt;');
        else
            return ('User logged in successfully as '.$user.'.&lt;br /&gt;');
    }
    private function create_default_folders($imap_stream, $folders) {
        $change=0;
        if(!in_array('{imap.example.org}TRASH',$folders)) {
            @imap_createmailbox($imap_stream, imap_utf7_encode("{imap.example.org:143}TRASH"));
            $change=1;
        }
        if(!in_array('{imap.example.org}SENT',$folders)) {
            @imap_createmailbox($imap_stream, imap_utf7_encode("{imap.example.org:143}SENT"));
            $change=1;
        }
        if(!in_array('{imap.example.org}SPAM',$folders)) {
            @imap_createmailbox($imap_stream, imap_utf7_encode("{imap.example.org:143}SPAM"));
            $change=1;
        }
        if(!in_array('{imap.example.org}SENT',$folders)) {
            @imap_createmailbox($imap_stream, imap_utf7_encode("{imap.example.org:143}SENT"));
            $change=1;
        }
        if(!in_array('{imap.example.org}SENT',$folders)) {
            @imap_createmailbox($imap_stream, imap_utf7_encode("{imap.example.org:143}DRAFTS"));
            $change=1;
        }
        if(!in_array('{imap.example.org}MY_FOLDERS',$folders)) {
            @imap_createmailbox($imap_stream, imap_utf7_encode("{imap.example.org:143}PERSONAL EMAIL"));
            $change=1;
        }
        return $change;
    }
    public function close_mail_connection() {
        @imap_close($this->connection);
    }
}

// usage, create a form, post it....
if($_POST['imap_username'] &amp;&amp; $_POST['imap_password']) {
    $imap_login = new Imap();
    $imap_login->login($_POST['imap_username'],$_POST['imap_password']);
    
    // Do some mail stuff here, like get headers...., use obj connection
    $message_headers = imap_mailboxmsginfo($imap_login->connection);
    
    // show the folders....
    print_r($imap_login->folders, true);
    
    print '&lt;br /&gt;&lt;hr size="1" noshade /&gt;';
    
    print_r($message_headers, true);
    

    // close the connection
    $imap_login->close_mail_connection();
}

?>
```
  

#

[Official documentation page](https://www.php.net/manual/en/function.imap-createmailbox.php)

**[To root](/README.md)**