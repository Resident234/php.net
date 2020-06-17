# Dual procedural and object-oriented interface




<div class="phpcode"><span class="html">
Just want to add that both procedural mysqli_connect_errno and mysqli_connect_error DON&apos;T accept any arguments!<br><a href="http://php.net/manual/de/mysqli.connect-errno.php" rel="nofollow" target="_blank">http://php.net/manual/de/mysqli.connect-errno.php</a><br><a href="http://php.net/manual/de/mysqli.connect-error.php" rel="nofollow" target="_blank">http://php.net/manual/de/mysqli.connect-error.php</a><br>&quot;int mysqli_connect_errno ( void )&quot;<br>&quot;string mysqli_connect_error ( void )&quot;<br>It clearly states &quot;void&quot; there.<br><br>Adding the mysqli-Instance as a parameter makes it look like it pulls the error-number out of the provided instance, which is not actually happening. This could end in a hard to detect bug when connecting to multiple SQL servers.<br>And it is confusing for beginners.</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/mysqli.quickstart.dual-interface.php)

**[To root](/README.md)**