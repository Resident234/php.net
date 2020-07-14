# GearmanClient::addTaskBackground



It is unlikely this example works quite as advertised.<br><br>The foreground job will block, however the background job should not.. (and if it does that&apos;s not documented Gearman behaviour far as I know and would not make sense for a Background job).<br><br>So, if the foreground job completes, then we would expect runTasks() to return at that moment, regardless of the status of the background job(s). With nothing else to do, the php script (the client) in this example would exit at that point. <br><br>To fully-utilize background jobs, it&apos;s reasonable to assume that we still wish to know their status. To do that, you need a polling loop that "checks up on them" and waits until their completion. <br><br>That eliminates the point of background jobs of course - because the client would still be Blocking (in a polling loop) more or less occupied, and unable to exit/finish. <br><br>So, in practice, background jobs mean you are decoupled from the client upon execution (because the main point is to free up the client, otherwise just use foreground execution), which means the client is likely going to exit, meaning the jobs themselves should be reporting their status and final result independently, not leaving it up to the client. <br><br>It&apos;s a significantly different setup compared to foreground jobs. In fact this example is kind of silly to even mix the two. <br><br>Nobody will ever see this post, because apparently nobody in the world has ever commented on the php gearman client, but it&apos;s a good post never the less.  

#

[Official documentation page](https://www.php.net/manual/en/gearmanclient.addtaskbackground.php)

**[To root](/README.md)**