# Dual procedural and object-oriented interface



Just want to add that both procedural mysqli_connect_errno and mysqli_connect_error DON&apos;T accept any arguments!<br>http://php.net/manual/de/mysqli.connect-errno.php<br>http://php.net/manual/de/mysqli.connect-error.php<br>"int mysqli_connect_errno ( void )"<br>"string mysqli_connect_error ( void )"<br>It clearly states "void" there.<br><br>Adding the mysqli-Instance as a parameter makes it look like it pulls the error-number out of the provided instance, which is not actually happening. This could end in a hard to detect bug when connecting to multiple SQL servers.<br>And it is confusing for beginners.  

---

[Official documentation page](https://www.php.net/manual/en/mysqli.quickstart.dual-interface.php)

**[To root](/README.md)**