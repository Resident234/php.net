# imap_thread




<div class="phpcode"><span class="html">
imap_thread() returns threads, but are confined to the current open mailbox you defined in imap_open(). This is not useful for, lets say, getting full threads ( from &quot;Sent Messages&quot; and &quot;Inbox&quot; [took me a day to figure this out]). 
<br>
<br>If you compare threads on Outlook vs gmail.com you will find that Outlook determines threads by subject title, not actual parent &gt; child relationships. 
<br>
<br>Gmail however, seems to get threads right, but does not include mail you send using their web interface in&#xA0; {imap.google.com:993/imap/ssl}Sent Messages . What this means is that threads using php imap won&apos;t be perfect for gmail.
<br>
<br>If you send mail using Outlook (or any mail client), gmail.com does put it in their &quot;Sent Mail&quot;.
<br>
<br>So all in all, threads for PHP imap are not perfect. But I blame the imap specifications (DEAR IMAP GUYS, please add better uids and parent ids. thx Chris) more than PHP
<br>
<br>So I created the Outlook method for threading (comparing subjects) below:
<br>
<br><span class="default">&lt;?php 
<br>$imap&#xA0; &#xA0; &#xA0; &#xA0;&#xA0; </span><span class="keyword">= </span><span class="default">imap_open</span><span class="keyword">(</span><span class="string">&apos;{imap.gmail.com:993/imap/ssl}INBOX&apos;</span><span class="keyword">, </span><span class="string">&apos;youremail@gmail.com&apos;</span><span class="keyword">, </span><span class="string">&apos;yourpassword&apos;</span><span class="keyword">);
<br></span><span class="default">$subject&#xA0; &#xA0;&#xA0; </span><span class="keyword">= </span><span class="string">&apos;Item b&apos;</span><span class="keyword">;
<br></span><span class="default">$threads&#xA0; &#xA0;&#xA0; </span><span class="keyword">= array();
<br>
<br></span><span class="comment">//remove re: and fwd:
<br></span><span class="default">$subject </span><span class="keyword">= </span><span class="default">trim</span><span class="keyword">(</span><span class="default">preg_replace</span><span class="keyword">(</span><span class="string">&quot;/Re\:|re\:|RE\:|Fwd\:|fwd\:|FWD\:/i&quot;</span><span class="keyword">, </span><span class="string">&apos;&apos;</span><span class="keyword">, </span><span class="default">$subject</span><span class="keyword">));
<br>
<br></span><span class="comment">//search for subject in current mailbox
<br></span><span class="default">$results </span><span class="keyword">= </span><span class="default">imap_search</span><span class="keyword">(</span><span class="default">$imap</span><span class="keyword">, </span><span class="string">&apos;SUBJECT &quot;&apos;</span><span class="keyword">.</span><span class="default">$subject</span><span class="keyword">.</span><span class="string">&apos;&quot;&apos;</span><span class="keyword">, </span><span class="default">SE_UID</span><span class="keyword">);
<br>
<br></span><span class="comment">//because results can be false
<br></span><span class="keyword">if(</span><span class="default">is_array</span><span class="keyword">(</span><span class="default">$results</span><span class="keyword">)) {
<br>&#xA0; &#xA0; </span><span class="comment">//now get all the emails details that were found 
<br>&#xA0; &#xA0; </span><span class="default">$emails </span><span class="keyword">= </span><span class="default">imap_fetch_overview</span><span class="keyword">(</span><span class="default">$imap</span><span class="keyword">, </span><span class="default">implode</span><span class="keyword">(</span><span class="string">&apos;,&apos;</span><span class="keyword">, </span><span class="default">$results</span><span class="keyword">), </span><span class="default">FT_UID</span><span class="keyword">);
<br>&#xA0; &#xA0; 
<br>&#xA0; &#xA0; </span><span class="comment">//foreach email 
<br>&#xA0; &#xA0; </span><span class="keyword">foreach (</span><span class="default">$emails </span><span class="keyword">as </span><span class="default">$email</span><span class="keyword">) {
<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="comment">//add to threads
<br>&#xA0; &#xA0; &#xA0; &#xA0; //we date date as the key because later we will sort it
<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$threads</span><span class="keyword">[</span><span class="default">strtotime</span><span class="keyword">(</span><span class="default">$email</span><span class="keyword">-&gt;</span><span class="default">date</span><span class="keyword">)] = </span><span class="default">$email</span><span class="keyword">;
<br>&#xA0; &#xA0; }&#xA0; &#xA0; 
<br>}
<br>
<br></span><span class="comment">//now reopen sent messages
<br></span><span class="default">imap_reopen</span><span class="keyword">(</span><span class="default">$imap</span><span class="keyword">, </span><span class="string">&apos;{imap.gmail.com:993/imap/ssl}Sent Messages&apos;</span><span class="keyword">);
<br>
<br></span><span class="comment">//and do the same thing
<br>
<br>//search for subject in current mailbox
<br></span><span class="default">$results </span><span class="keyword">= </span><span class="default">imap_search</span><span class="keyword">(</span><span class="default">$imap</span><span class="keyword">, </span><span class="string">&apos;SUBJECT &quot;&apos;</span><span class="keyword">.</span><span class="default">$subject</span><span class="keyword">.</span><span class="string">&apos;&quot;&apos;</span><span class="keyword">, </span><span class="default">SE_UID</span><span class="keyword">);
<br>
<br></span><span class="comment">//because results can be false
<br></span><span class="keyword">if(</span><span class="default">is_array</span><span class="keyword">(</span><span class="default">$results</span><span class="keyword">)) {
<br>&#xA0; &#xA0; </span><span class="comment">//now get all the emails details that were found 
<br>&#xA0; &#xA0; </span><span class="default">$emails </span><span class="keyword">= </span><span class="default">imap_fetch_overview</span><span class="keyword">(</span><span class="default">$imap</span><span class="keyword">, </span><span class="default">implode</span><span class="keyword">(</span><span class="string">&apos;,&apos;</span><span class="keyword">, </span><span class="default">$results</span><span class="keyword">), </span><span class="default">FT_UID</span><span class="keyword">);
<br>&#xA0; &#xA0; 
<br>&#xA0; &#xA0; </span><span class="comment">//foreach email 
<br>&#xA0; &#xA0; </span><span class="keyword">foreach (</span><span class="default">$emails </span><span class="keyword">as </span><span class="default">$email</span><span class="keyword">) {
<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="comment">//add to threads
<br>&#xA0; &#xA0; &#xA0; &#xA0; //we date date as the key because later we will sort it
<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$threads</span><span class="keyword">[</span><span class="default">strtotime</span><span class="keyword">(</span><span class="default">$email</span><span class="keyword">-&gt;</span><span class="default">date</span><span class="keyword">)] = </span><span class="default">$email</span><span class="keyword">;
<br>&#xA0; &#xA0; }&#xA0; &#xA0; 
<br>}
<br>
<br></span><span class="comment">//sort keys so we get threads in chronological order 
<br></span><span class="default">ksort</span><span class="keyword">(</span><span class="default">$threads</span><span class="keyword">);
<br>
<br>echo </span><span class="string">&apos;&lt;pre&gt;&apos;</span><span class="keyword">.</span><span class="default">print_r</span><span class="keyword">(</span><span class="default">$threads</span><span class="keyword">, </span><span class="default">true</span><span class="keyword">).</span><span class="string">&apos;&lt;/pre&gt;&apos;</span><span class="keyword">;
<br>exit;
<br></span><span class="default">?&gt;
<br></span>
<br>so if you are going to use imap_thread() for something useful. This is probably the most optimal way I can think of:
<br>
<br><span class="default">&lt;?php 
<br>$imap </span><span class="keyword">= </span><span class="default">imap_open</span><span class="keyword">(</span><span class="string">&apos;{imap.gmail.com:993/imap/ssl}INBOX&apos;</span><span class="keyword">, </span><span class="string">&apos;youremail@gmail.com&apos;</span><span class="keyword">, </span><span class="string">&apos;password&apos;</span><span class="keyword">);
<br></span><span class="default">$threads </span><span class="keyword">= </span><span class="default">$rootValues </span><span class="keyword">= array();
<br>
<br></span><span class="default">$thread </span><span class="keyword">= </span><span class="default">imap_thread</span><span class="keyword">(</span><span class="default">$imap</span><span class="keyword">);
<br></span><span class="default">$root </span><span class="keyword">= </span><span class="default">0</span><span class="keyword">;
<br>
<br></span><span class="comment">//first we find the root (or parent) value for each email in the thread
<br>//we ignore emails that have no root value except those that are infact
<br>//the root of a thread
<br>
<br>//we want to gather the message IDs in a way where we can get the details of
<br>//all emails on one call rather than individual calls ( for performance )
<br>
<br>//foreach thread
<br></span><span class="keyword">foreach (</span><span class="default">$thread </span><span class="keyword">as </span><span class="default">$i </span><span class="keyword">=&gt; </span><span class="default">$messageId</span><span class="keyword">) {
<br>&#xA0; &#xA0; </span><span class="comment">//get sequence and type
<br>&#xA0; &#xA0; </span><span class="keyword">list(</span><span class="default">$sequence</span><span class="keyword">, </span><span class="default">$type</span><span class="keyword">) = </span><span class="default">explode</span><span class="keyword">(</span><span class="string">&apos;.&apos;</span><span class="keyword">, </span><span class="default">$i</span><span class="keyword">);
<br>&#xA0; &#xA0; 
<br>&#xA0; &#xA0; </span><span class="comment">//if type is not num or messageId is 0 or (start of a new thread and no next) or is already set 
<br>&#xA0; &#xA0; </span><span class="keyword">if(</span><span class="default">$type </span><span class="keyword">!= </span><span class="string">&apos;num&apos; </span><span class="keyword">|| </span><span class="default">$messageId </span><span class="keyword">== </span><span class="default">0 
<br>&#xA0; &#xA0; &#xA0;&#xA0; </span><span class="keyword">|| (</span><span class="default">$root </span><span class="keyword">== </span><span class="default">0 </span><span class="keyword">&amp;&amp; </span><span class="default">$thread</span><span class="keyword">[</span><span class="default">$sequence</span><span class="keyword">.</span><span class="string">&apos;.next&apos;</span><span class="keyword">] == </span><span class="default">0</span><span class="keyword">)
<br>&#xA0; &#xA0; &#xA0;&#xA0; || isset(</span><span class="default">$rootValues</span><span class="keyword">[</span><span class="default">$messageId</span><span class="keyword">])) {
<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="comment">//ignore it
<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="keyword">continue;
<br>&#xA0; &#xA0; }
<br>&#xA0; &#xA0; 
<br>&#xA0; &#xA0; </span><span class="comment">//if this is the start of a new thread
<br>&#xA0; &#xA0; </span><span class="keyword">if(</span><span class="default">$root </span><span class="keyword">== </span><span class="default">0</span><span class="keyword">) {
<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="comment">//set root
<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$root </span><span class="keyword">= </span><span class="default">$messageId</span><span class="keyword">;
<br>&#xA0; &#xA0; }
<br>&#xA0; &#xA0; 
<br>&#xA0; &#xA0; </span><span class="comment">//at this point this will be part of a thread
<br>&#xA0; &#xA0; //let&apos;s remember the root for this email
<br>&#xA0; &#xA0; </span><span class="default">$rootValues</span><span class="keyword">[</span><span class="default">$messageId</span><span class="keyword">] = </span><span class="default">$root</span><span class="keyword">;
<br>&#xA0; &#xA0; 
<br>&#xA0; &#xA0; </span><span class="comment">//if there is no next
<br>&#xA0; &#xA0; </span><span class="keyword">if(</span><span class="default">$thread</span><span class="keyword">[</span><span class="default">$sequence</span><span class="keyword">.</span><span class="string">&apos;.next&apos;</span><span class="keyword">] == </span><span class="default">0</span><span class="keyword">) {
<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="comment">//reset root
<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$root </span><span class="keyword">= </span><span class="default">0</span><span class="keyword">;
<br>&#xA0; &#xA0; }
<br>}
<br>
<br></span><span class="comment">//now get all the emails details in rootValues in one call
<br>//because one call for 1000 rows to a server is better 
<br>//than calling the server 1000 times&#xA0; 
<br></span><span class="default">$emails </span><span class="keyword">= </span><span class="default">imap_fetch_overview</span><span class="keyword">(</span><span class="default">$imap</span><span class="keyword">, </span><span class="default">implode</span><span class="keyword">(</span><span class="string">&apos;,&apos;</span><span class="keyword">, </span><span class="default">array_keys</span><span class="keyword">(</span><span class="default">$rootValues</span><span class="keyword">)));
<br>
<br></span><span class="comment">//foreach email 
<br></span><span class="keyword">foreach (</span><span class="default">$emails </span><span class="keyword">as </span><span class="default">$email</span><span class="keyword">) {
<br>&#xA0; &#xA0; </span><span class="comment">//get root
<br>&#xA0; &#xA0; </span><span class="default">$root </span><span class="keyword">= </span><span class="default">$rootValues</span><span class="keyword">[</span><span class="default">$email</span><span class="keyword">-&gt;</span><span class="default">msgno</span><span class="keyword">];
<br>&#xA0; &#xA0; 
<br>&#xA0; &#xA0; </span><span class="comment">//add to threads
<br>&#xA0; &#xA0; </span><span class="default">$threads</span><span class="keyword">[</span><span class="default">$root</span><span class="keyword">][] = </span><span class="default">$email</span><span class="keyword">;
<br>}&#xA0; &#xA0; 
<br>
<br></span><span class="comment">//there is no need to sort, the threads will automagically in chronological order
<br></span><span class="keyword">echo </span><span class="string">&apos;&lt;pre&gt;&apos;</span><span class="keyword">.</span><span class="default">print_r</span><span class="keyword">(</span><span class="default">$threads</span><span class="keyword">, </span><span class="default">true</span><span class="keyword">).</span><span class="string">&apos;&lt;/pre&gt;&apos;</span><span class="keyword">;
<br></span><span class="default">imap_close</span><span class="keyword">(</span><span class="default">$imap</span><span class="keyword">);
<br>exit;
<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/function.imap-thread.php)

**[⬆ to root](/)**