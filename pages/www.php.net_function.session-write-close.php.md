# session_write_close




<div class="phpcode"><span class="html">
You can have interesting fun debugging anything with sleep() in it if you have a session still active.&#xA0; For example, a page that makes an ajax request, where the ajax request polls a server-side event (and may not return immediately).<br><br>If the ajax function doesn&apos;t do session_write_close(), then your outer page will appear to hang, and opening other pages in new tabs will also stall.</span>
</div>
  

#


<div class="phpcode"><span class="html">
An easy gotcha here - the $_SESSIONS superglobal does not vanish because you call session_write_close.&#xA0; If subsequent to the write_close call if you manipulate the superglobal the changes will not be saved when the script exists.&#xA0; Likewise a call to session_regenrate_id will fail.<br><br>Closing the session and then manipulating session variables is not something many would do by intent.&#xA0; However, if your sessions suddenly start misbehaving, failing to record changes etc it is well worth checking that the cause is not this one!</span>
</div>
  

#


<div class="phpcode"><span class="html">
Workaround if session_write_close() still doesn&apos;t write sessions fast enough:<br><br>I found with one PHP login system that even session_write_close() was not setting the session variables before I transferred pages with a Location: header.&#xA0; So the user would log in, I would create the $_SESSION variables, call session_write_close() and then transfer to the secure page using header(Location:...).&#xA0; The secure page would check for the session vars, not find them, and force the user to log in again.&#xA0; After the second login the session would be found and they could continue.<br><br>My workaround was to create the $_SESSION variables with 0 values before writing the initial login page.&#xA0; Then I updated the session vars with the login results and used the header() function to switch to the secure location.&#xA0; Once the session vars have already been created, updated values are assigned quickly.&#xA0; Problem solved.&#xA0; Just be sure the secure page checks both that the $_SESSION var exists AND that it&apos;s not 0.</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/function.session-write-close.php)

**[To root](/README.md)**