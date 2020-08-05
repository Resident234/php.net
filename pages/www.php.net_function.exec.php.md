# exec



This will execute $cmd in the background (no cmd window) without PHP waiting for it to finish, on both Windows and Unix.<br><br>

```
<?php
function execInBackground($cmd) {
    if (substr(php_uname(), 0, 7) == "Windows"){
        pclose(popen("start /B ". $cmd, "r"));  
    }
    else {
        exec($cmd . " > /dev/null &amp;");   
    }
}
?>
```
  

---

(This is for linux users only).<br><br>We know now how we can fork a process in linux with the &amp; operator.<br>And by using command: nohup MY_COMMAND &gt; /dev/null 2&gt;&amp;1 &amp; echo $! we can return the pid of the process.<br><br>This small class is made so you can keep in track of your created processes ( meaning start/stop/status ).<br><br>You may use it to start a process or join an exisiting PID process.<br><br>

```
<?php
    // You may use status(), start(), and stop(). notice that start() method gets called automatically one time.
    $process = new Process('ls -al');

    // or if you got the pid, however here only the status() metod will work.
    $process = new Process();
    $process.setPid(my_pid);
?>
```




```
<?php
    // Then you can start/stop/ check status of the job.
    $process.stop();
    $process.start();
    if ($process.status()){
        echo "The process is currently running";
    }else{
        echo "The process is not running.";
    }
?>
```




```
<?php
/* An easy way to keep in track of external processes.
 * Ever wanted to execute a process in php, but you still wanted to have somewhat controll of the process ? Well.. This is a way of doing it.
 * @compability: Linux only. (Windows does not work).
 * @author: Peec
 */
class Process{
    private $pid;
    private $command;

    public function __construct($cl=false){
        if ($cl != false){
            $this->command = $cl;
            $this->runCom();
        }
    }
    private function runCom(){
        $command = 'nohup '.$this->command.' > /dev/null 2>&amp;1 &amp; echo $!';
        exec($command ,$op);
        $this->pid = (int)$op[0];
    }

    public function setPid($pid){
        $this->pid = $pid;
    }

    public function getPid(){
        return $this->pid;
    }

    public function status(){
        $command = 'ps -p '.$this->pid;
        exec($command,$op);
        if (!isset($op[1]))return false;
        else return true;
    }

    public function start(){
        if ($this->command != '')$this->runCom();
        else return true;
    }

    public function stop(){
        $command = 'kill '.$this->pid;
        exec($command);
        if ($this->status() == false)return true;
        else return false;
    }
}
?>
```
  

---

Can&#x2019;t get the output from your exec&#x2019;d command to appear in the $output array?<br>Is it echo&#x2019;ing all over your shell instead?<br><br>Append "2&gt;&amp;1" to the end of your command, for example:<br><br>exec("xmllint --noout ~/desktop/test.xml 2&gt;&amp;1", $retArr, $retVal);<br><br>Will fill the array $retArr with the expected output; one line per array key.  

---

I too wrestled with getting a program to run in the background in Windows while the script continues to execute.  This method unlike the other solutions allows you to start any program minimized, maximized, or with no window at all.  llbra@phpbrasil&apos;s solution does work but it sometimes produces an unwanted window on the desktop when you really want the task to run hidden.<br><br>start Notepad.exe minimized in the background:<br><br>

```
<?php
$WshShell = new COM("WScript.Shell");
$oExec = $WshShell->Run("notepad.exe", 7, false);
?>
```


start a shell command invisible in the background:


```
<?php
$WshShell = new COM("WScript.Shell");
$oExec = $WshShell->Run("cmd /C dir /S %windir%", 0, false);
?>
```


start MSPaint maximized and wait for you to close it before continuing the script:


```
<?php
$WshShell = new COM("WScript.Shell");
$oExec = $WshShell->Run("mspaint.exe", 3, true);
?>
```
<br><br>For more info on the Run() method go to:<br>http://msdn.microsoft.com/library/en-us/script56/html/wsMthRun.asp  

---

[Official documentation page](https://www.php.net/manual/en/function.exec.php)

**[To root](/README.md)**