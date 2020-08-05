# Runtime Configuration



On debian (based) systems, changing session.gc_maxlifetime at runtime has no real effect. Debian disables PHP&apos;s own garbage collector by setting session.gc_probability=0. Instead it has a cronjob running every 30 minutes (see /etc/cron.d/php5) that cleans up old sessions. This cronjob basically looks into your php.ini and uses the value of session.gc_maxlifetime there to decide which sessions to clean (see /usr/lib/php5/maxlifetime).<br><br>You can adjust the global value in your php.ini (usually /etc/php5/apache2/php.ini). Or you can change the session.save_path so debian&apos;s cronjob will not clean up your sessions anymore. Then you need to either do your own garbage collection with your own cronjob or enable PHP&apos;s garbage collection (php then needs sufficient privileges on the save_path).<br><br>Why does Debian not use PHP&apos;s garbarage collection?<br>For security reasons, they store session data in a place (/var/lib/php5) with very stringent permissions. With the sticky bit set, only root is allowed to rename or delete files there, so PHP itself cannot clean up old session data. See https://bugs.debian.org/cgi-bin/bugreport.cgi?bug=267720 .  

---

I found out that if you need to set custom session settings, you only need to do it once when session starts. Then session maintains its settings, even if you use ini_set and change them, original session still will use it&apos;s original setting until it expires.<br><br>Just thought it might be useful to someone.  

---

We found a session.save_path depth of 3 led to excessive wastage of inodes and in fact disk space in storing the directory tree. dir_indexes option on ext2/3/4 makes larger directories more feasible anyway, so we decided to move to a depth of 2 instead.<br><br>It took a little puzzling to figure out how to move the existing PHP sessions up one directory tree, but we ended up running this in the root sessions directory:<br><br>#!/bin/sh<br>for a in ./* ; do<br>    cd ./$a<br>    pwd<br>    for b in ./* ; do<br>      cd ./$b<br>      pwd<br>      # Move existing sessions out<br>      find ./* -xdev -type f -print0 | xargs -0 mv -t .<br>      # Remove subdirectories<br>      find ./* -xdev -type d -print0 | xargs -0 rmdir<br>      cd ..<br>  done<br>  cd ..<br>done<br><br>This script may not be the best way to do it, but it got the job done fast. You can modify it for different depths by adding or removing "for" loops.<br><br>The documentation gives a depth of 5 as an example, but five is right out. If you&apos;re going beyond 2, you&apos;re at the scale where you may want to to look at a large memcached or redis instance instead.  

---

[Official documentation page](https://www.php.net/manual/en/session.configuration.php)

**[To root](/README.md)**