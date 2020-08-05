# session_save_path



I made a folder next to the public html folder and placed these lines at the very first point in index.php<br><br>Location of session folder:<br><br>/domains/account/session<br><br>location of index.php<br><br>/domains/account/public_html/index.php<br><br>What I placed in index.php at line 0:<br><br>

```
<?php 
ini_set('session.save_path',realpath(dirname($_SERVER['DOCUMENT_ROOT']) . '/../session'));
session_start();

This is the only solution that worked for me. Hope this helps someone.?>
```
  

---

Debian does not use the default garbage collector for sessions. Instead, it sets session.gc_probability to zero and it runs a cron job to clean up old session data in the default directory.<br><br>As a result, if your site sets a custom location with session_save_path() you also need to set a value for session.gc_probability, e.g.:<br><br>

```
<?php
session_save_path('/home/example.com/sessions');
ini_set('session.gc_probability', 1);
?>
```
<br><br>Otherwise, old files in &apos;/home/example.com/sessions&apos; will never get removed!  

---

Session on clustered web servers !<br><br>We had problem in PHP session handling with 2 web server cluster. Problem was one servers session data was not available in other server.<br><br>So I made a simple configuration in both server php.ini file. Changed session.save_path default value to shared folder on both servers (/mnt/session/).<br><br>It works for me. :)  

---

[Official documentation page](https://www.php.net/manual/en/function.session-save-path.php)

**[To root](/README.md)**