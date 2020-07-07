# imap_thread





imap_thread() returns threads, but are confined to the current open mailbox you defined in imap_open(). This is not useful for, lets say, getting full threads ( from &quot;Sent Messages&quot; and &quot;Inbox&quot; [took me a day to figure this out]). 



If you compare threads on Outlook vs gmail.com you will find that Outlook determines threads by subject title, not actual parent &gt; child relationships. 



Gmail however, seems to get threads right, but does not include mail you send using their web interface in&#xA0; {imap.google.com:993/imap/ssl}Sent Messages . What this means is that threads using php imap won&apos;t be perfect for gmail.



If you send mail using Outlook (or any mail client), gmail.com does put it in their &quot;Sent Mail&quot;.



So all in all, threads for PHP imap are not perfect. But I blame the imap specifications (DEAR IMAP GUYS, please add better uids and parent ids. thx Chris) more than PHP



So I created the Outlook method for threading (comparing subjects) below:





```
<?php 

$imap&#xA0; &#xA0; &#xA0; &#xA0;&#xA0; = imap_open(&apos;{imap.gmail.com:993/imap/ssl}INBOX&apos;, &apos;youremail@gmail.com&apos;, &apos;yourpassword&apos;);

$subject&#xA0; &#xA0;&#xA0; = &apos;Item b&apos;;

$threads&#xA0; &#xA0;&#xA0; = array();



//remove re: and fwd:

$subject = trim(preg_replace(&quot;/Re\:|re\:|RE\:|Fwd\:|fwd\:|FWD\:/i&quot;, &apos;&apos;, $subject));



//search for subject in current mailbox

$results = imap_search($imap, &apos;SUBJECT &quot;&apos;.$subject.&apos;&quot;&apos;, SE_UID);



//because results can be false

if(is_array($results)) {

&#xA0; &#xA0; //now get all the emails details that were found 

&#xA0; &#xA0; $emails = imap_fetch_overview($imap, implode(&apos;,&apos;, $results), FT_UID);

&#xA0; &#xA0; 

&#xA0; &#xA0; //foreach email 

&#xA0; &#xA0; foreach ($emails as $email) {

&#xA0; &#xA0; &#xA0; &#xA0; //add to threads

&#xA0; &#xA0; &#xA0; &#xA0; //we date date as the key because later we will sort it

&#xA0; &#xA0; &#xA0; &#xA0; $threads[strtotime($email-&gt;date)] = $email;

&#xA0; &#xA0; }&#xA0; &#xA0; 

}



//now reopen sent messages

imap_reopen($imap, &apos;{imap.gmail.com:993/imap/ssl}Sent Messages&apos;);



//and do the same thing



//search for subject in current mailbox

$results = imap_search($imap, &apos;SUBJECT &quot;&apos;.$subject.&apos;&quot;&apos;, SE_UID);



//because results can be false

if(is_array($results)) {

&#xA0; &#xA0; //now get all the emails details that were found 

&#xA0; &#xA0; $emails = imap_fetch_overview($imap, implode(&apos;,&apos;, $results), FT_UID);

&#xA0; &#xA0; 

&#xA0; &#xA0; //foreach email 

&#xA0; &#xA0; foreach ($emails as $email) {

&#xA0; &#xA0; &#xA0; &#xA0; //add to threads

&#xA0; &#xA0; &#xA0; &#xA0; //we date date as the key because later we will sort it

&#xA0; &#xA0; &#xA0; &#xA0; $threads[strtotime($email-&gt;date)] = $email;

&#xA0; &#xA0; }&#xA0; &#xA0; 

}



//sort keys so we get threads in chronological order 

ksort($threads);



echo &apos;&lt;pre&gt;&apos;.print_r($threads, true).&apos;&lt;/pre&gt;&apos;;

exit;

?>
```




so if you are going to use imap_thread() for something useful. This is probably the most optimal way I can think of:





```
<?php 

$imap = imap_open(&apos;{imap.gmail.com:993/imap/ssl}INBOX&apos;, &apos;youremail@gmail.com&apos;, &apos;password&apos;);

$threads = $rootValues = array();



$thread = imap_thread($imap);

$root = 0;



//first we find the root (or parent) value for each email in the thread

//we ignore emails that have no root value except those that are infact

//the root of a thread



//we want to gather the message IDs in a way where we can get the details of

//all emails on one call rather than individual calls ( for performance )



//foreach thread

foreach ($thread as $i =&gt; $messageId) {

&#xA0; &#xA0; //get sequence and type

&#xA0; &#xA0; list($sequence, $type) = explode(&apos;.&apos;, $i);

&#xA0; &#xA0; 

&#xA0; &#xA0; //if type is not num or messageId is 0 or (start of a new thread and no next) or is already set 

&#xA0; &#xA0; if($type != &apos;num&apos; || $messageId == 0 

&#xA0; &#xA0; &#xA0;&#xA0; || ($root == 0 &amp;&amp; $thread[$sequence.&apos;.next&apos;] == 0)

&#xA0; &#xA0; &#xA0;&#xA0; || isset($rootValues[$messageId])) {

&#xA0; &#xA0; &#xA0; &#xA0; //ignore it

&#xA0; &#xA0; &#xA0; &#xA0; continue;

&#xA0; &#xA0; }

&#xA0; &#xA0; 

&#xA0; &#xA0; //if this is the start of a new thread

&#xA0; &#xA0; if($root == 0) {

&#xA0; &#xA0; &#xA0; &#xA0; //set root

&#xA0; &#xA0; &#xA0; &#xA0; $root = $messageId;

&#xA0; &#xA0; }

&#xA0; &#xA0; 

&#xA0; &#xA0; //at this point this will be part of a thread

&#xA0; &#xA0; //let&apos;s remember the root for this email

&#xA0; &#xA0; $rootValues[$messageId] = $root;

&#xA0; &#xA0; 

&#xA0; &#xA0; //if there is no next

&#xA0; &#xA0; if($thread[$sequence.&apos;.next&apos;] == 0) {

&#xA0; &#xA0; &#xA0; &#xA0; //reset root

&#xA0; &#xA0; &#xA0; &#xA0; $root = 0;

&#xA0; &#xA0; }

}



//now get all the emails details in rootValues in one call

//because one call for 1000 rows to a server is better 

//than calling the server 1000 times&#xA0; 

$emails = imap_fetch_overview($imap, implode(&apos;,&apos;, array_keys($rootValues)));



//foreach email 

foreach ($emails as $email) {

&#xA0; &#xA0; //get root

&#xA0; &#xA0; $root = $rootValues[$email-&gt;msgno];

&#xA0; &#xA0; 

&#xA0; &#xA0; //add to threads

&#xA0; &#xA0; $threads[$root][] = $email;

}&#xA0; &#xA0; 



//there is no need to sort, the threads will automagically in chronological order

echo &apos;&lt;pre&gt;&apos;.print_r($threads, true).&apos;&lt;/pre&gt;&apos;;

imap_close($imap);

exit;

?>
```



  

#

[Official documentation page](https://www.php.net/manual/en/function.imap-thread.php)

**[To root](/README.md)**