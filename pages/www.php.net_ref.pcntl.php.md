# PCNTL Functions



If you want to create a daemon in PHP, consider using System_Daemon http://pear.php.net/package/System_Daemon<br><br>Install it like this:<br>pear install -f system_daemon<br><br>Then use it like this:<br>

```
<?php
// Include PEAR's Daemon Class
require_once "System/Daemon.php";

// Bare minimum setup
System_Daemon::setOption("appName", "mydaemonname");

// Spawn Deamon!
System_Daemon::start();

// Your PHP Here!
while (true) {
    doTask();
}

// Stop daemon!
System_Daemon::stop();
?>
```
<br><br>More examples can be found inside the PEAR package.  

---

(PHP 5.2.4) <br><br>This is an example of multithreading keeping different connections to a mysql database: when children exit they close the connection and others can&apos;t use it any more generating problems. In this example I used variable variables to make a different connection per each child. <br><br>This scripts loops forever with one mother detached from the terminal and five children &apos;named&apos; from 1 to 5. When one children sees it&apos;s name in the database (one table &apos;test.tb&apos; with only one field &apos;test&apos;) he lets himself die. To kill children insert their value in the db. The mother suicides only when all children are dead.<br><br>What a sad but interesting story...<br><br>    $npid = pcntl_fork(); // DETACH FROM TERMINAL AND BE REAPED BY INIT<br><br>    if ($npid==-1) die("Error: impossible to pcntl_fork()\n");<br>    else if ($npid) exit(0); // THE GRANPA DIES<br>    else // MOTHER GOES ON TO MAKE CHILDREN<br>    {<br>        $children = 5;<br>        for ($i=1; $i&lt;=$children; $i++)<br>        {<br>            $pid = pcntl_fork();<br>            if ($pid==-1) die("Error: impossible to pcntl_fork()\n");<br>            else if ($pid)<br>            {<br>                 $pid_arr[$i] = $pid;<br>            }<br>            if (!$pid) // CHILDREN<br>            {<br>                global $vconn;<br>                $vconn = "vconn$i";<br>                global $$vconn;<br>                $$vconn = @mysql_connect("mydbhost","mydbuser","mydbpwd");<br>                if (!($$vconn)) echo mysql_error();<br>                if (!($$vconn)) exit;<br><br>                while (1)<br>                {<br>                    $query = "SELECT test FROM test.tb";<br>                    $rs = mysql_query($query,$$vconn);<br>                    $rw = mysql_fetch_row($rs);<br>                    if ($rw[0]==$i) exit;<br>                    else<br>                    {<br>                        echo "Database is $rw[0] and I am $i, it&apos;s not my time, I will wait....\n";<br>                        sleep(1);<br>                    }<br>                }<br>            }<br>        }<br><br>        foreach ($pid_arr as $pid)<br>        {<br>            // we are the parent and we wait for all children to die<br>            pcntl_waitpid($pid, $status);<br>        }<br>        echo "All my children died, I will suicide...\n";<br>        exit();<br>    }  

---

[Official documentation page](https://www.php.net/manual/en/ref.pcntl.php)

**[To root](/README.md)**