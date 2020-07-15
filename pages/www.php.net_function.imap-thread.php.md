# imap_thread



imap_thread() returns threads, but are confined to the current open mailbox you defined in imap_open(). This is not useful for, lets say, getting full threads ( from "Sent Messages" and "Inbox" [took me a day to figure this out]). <br><br>If you compare threads on Outlook vs gmail.com you will find that Outlook determines threads by subject title, not actual parent &gt; child relationships. <br><br>Gmail however, seems to get threads right, but does not include mail you send using their web interface in  {imap.google.com:993/imap/ssl}Sent Messages . What this means is that threads using php imap won&apos;t be perfect for gmail.<br><br>If you send mail using Outlook (or any mail client), gmail.com does put it in their "Sent Mail".<br><br>So all in all, threads for PHP imap are not perfect. But I blame the imap specifications (DEAR IMAP GUYS, please add better uids and parent ids. thx Chris) more than PHP<br><br>So I created the Outlook method for threading (comparing subjects) below:<br><br>

```
<?php 
$imap         = imap_open('{imap.gmail.com:993/imap/ssl}INBOX', 'youremail@gmail.com', 'yourpassword');
$subject     = 'Item b';
$threads     = array();

//remove re: and fwd:
$subject = trim(preg_replace("/Re\:|re\:|RE\:|Fwd\:|fwd\:|FWD\:/i", '', $subject));

//search for subject in current mailbox
$results = imap_search($imap, 'SUBJECT "'.$subject.'"', SE_UID);

//because results can be false
if(is_array($results)) {
    //now get all the emails details that were found 
    $emails = imap_fetch_overview($imap, implode(',', $results), FT_UID);
    
    //foreach email 
    foreach ($emails as $email) {
        //add to threads
        //we date date as the key because later we will sort it
        $threads[strtotime($email->date)] = $email;
    }    
}

//now reopen sent messages
imap_reopen($imap, '{imap.gmail.com:993/imap/ssl}Sent Messages');

//and do the same thing

//search for subject in current mailbox
$results = imap_search($imap, 'SUBJECT "'.$subject.'"', SE_UID);

//because results can be false
if(is_array($results)) {
    //now get all the emails details that were found 
    $emails = imap_fetch_overview($imap, implode(',', $results), FT_UID);
    
    //foreach email 
    foreach ($emails as $email) {
        //add to threads
        //we date date as the key because later we will sort it
        $threads[strtotime($email->date)] = $email;
    }    
}

//sort keys so we get threads in chronological order 
ksort($threads);

echo '<pre>'.print_r($threads, true).'</pre>';
exit;
?>
```


so if you are going to use imap_thread() for something useful. This is probably the most optimal way I can think of:



```
<?php 
$imap = imap_open('{imap.gmail.com:993/imap/ssl}INBOX', 'youremail@gmail.com', 'password');
$threads = $rootValues = array();

$thread = imap_thread($imap);
$root = 0;

//first we find the root (or parent) value for each email in the thread
//we ignore emails that have no root value except those that are infact
//the root of a thread

//we want to gather the message IDs in a way where we can get the details of
//all emails on one call rather than individual calls ( for performance )

//foreach thread
foreach ($thread as $i => $messageId) {
    //get sequence and type
    list($sequence, $type) = explode('.', $i);
    
    //if type is not num or messageId is 0 or (start of a new thread and no next) or is already set 
    if($type != 'num' || $messageId == 0 
       || ($root == 0 &amp;&amp; $thread[$sequence.'.next'] == 0)
       || isset($rootValues[$messageId])) {
        //ignore it
        continue;
    }
    
    //if this is the start of a new thread
    if($root == 0) {
        //set root
        $root = $messageId;
    }
    
    //at this point this will be part of a thread
    //let's remember the root for this email
    $rootValues[$messageId] = $root;
    
    //if there is no next
    if($thread[$sequence.'.next'] == 0) {
        //reset root
        $root = 0;
    }
}

//now get all the emails details in rootValues in one call
//because one call for 1000 rows to a server is better 
//than calling the server 1000 times  
$emails = imap_fetch_overview($imap, implode(',', array_keys($rootValues)));

//foreach email 
foreach ($emails as $email) {
    //get root
    $root = $rootValues[$email->msgno];
    
    //add to threads
    $threads[$root][] = $email;
}    

//there is no need to sort, the threads will automagically in chronological order
echo '<pre>'.print_r($threads, true).'</pre>';
imap_close($imap);
exit;
?>
```
  

#

[Official documentation page](https://www.php.net/manual/en/function.imap-thread.php)

**[To root](/README.md)**