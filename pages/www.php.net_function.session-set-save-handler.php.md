# session_set_save_handler



After spend so many time to understand how PHP session works with database and unsuccessful attempts to get it right, I decided to rewrite the version from our friend stalker.<br><br>//Database<br>CREATE TABLE `Session` (<br>  `Session_Id` varchar(255) COLLATE utf8_unicode_ci NOT NULL,<br>  `Session_Expires` datetime NOT NULL,<br>  `Session_Data` text COLLATE utf8_unicode_ci,<br>  PRIMARY KEY (`Session_Id`)<br>) ENGINE=InnoDB DEFAULT CHARSET=utf8 COLLATE=utf8_unicode_ci;<br>SELECT * FROM mydatabase.Session;<br><br>

```
<?php
//inc.session.php

class SysSession implements SessionHandlerInterface
{
    private $link;
    
    public function open($savePath, $sessionName)
    {
        $link = mysqli_connect("server","user","pwd","mydatabase");
        if($link){
            $this->link = $link;
            return true;
        }else{
            return false;
        }
    }
    public function close()
    {
        mysqli_close($this->link);
        return true;
    }
    public function read($id)
    {
        $result = mysqli_query($this->link,"SELECT Session_Data FROM Session WHERE Session_Id = '".$id."' AND Session_Expires > '".date('Y-m-d H:i:s')."'");
        if($row = mysqli_fetch_assoc($result)){
            return $row['Session_Data'];
        }else{
            return "";
        }
    }
    public function write($id, $data)
    {
        $DateTime = date('Y-m-d H:i:s');
        $NewDateTime = date('Y-m-d H:i:s',strtotime($DateTime.' + 1 hour'));
        $result = mysqli_query($this->link,"REPLACE INTO Session SET Session_Id = '".$id."', Session_Expires = '".$NewDateTime."', Session_Data = '".$data."'");
        if($result){
            return true;
        }else{
            return false;
        }
    }
    public function destroy($id)
    {
        $result = mysqli_query($this->link,"DELETE FROM Session WHERE Session_Id ='".$id."'");
        if($result){
            return true;
        }else{
            return false;
        }
    }
    public function gc($maxlifetime)
    {
        $result = mysqli_query($this->link,"DELETE FROM Session WHERE ((UNIX_TIMESTAMP(Session_Expires) + ".$maxlifetime.") < ".$maxlifetime.")");
        if($result){
            return true;
        }else{
            return false;
        }
    }
}
$handler = new SysSession();
session_set_save_handler($handler, true);
?>
```




```
<?php
//page 1
require_once('inc.session.php');

session_start();

$_SESSION['var1'] = "My Portuguese text: SOU Gaucho!";
?>
```




```
<?php
//page 2
require_once('inc.session.php');

session_start();

if(isset($_SESSION['var1']){
echo $_SESSION['var1']; 
}
//OUTPUT: My Portuguese text: SOU Gaucho!
?>
```
  

#

As of PHP 7.0, you can implement SessionUpdateTimestampHandlerInterface to <br>define your own session id validating method like validate_sid and the timestamp updating method like update_timestamp in the non-OOP prototype of session_set_save_handler().<br><br>SessionUpdateTimestampHandlerInterface is a new interface introduced in PHP 7.0, which has not been documented yet. It has two abstract methods: SessionUpdateTimestampHandlerInterface :: validateId($sessionId) and SessionUpdateTimestampHandlerInterface :: updateTimestamp($sessionId, $sessionData).<br><br>

```
<?php
    /*
       @author Wu Xiancheng
       Code structure for PHP 7.0+ only because SessionUpdateTimestampHandlerInterface is introduced in PHP 7.0
       With this class you can validate php session id and update the timestamp of php session data
       with the OOP prototype of session_set_save_handler() in PHP 7.0+
    */
    class PHPSessionXHandler implements SessionHandlerInterface, SessionUpdateTimestampHandlerInterface {
        public function close(){
            // return value should be true for success or false for failure
            // ...
        }
        public function destroy($sessionId){
            // return value should be true for success or false for failure
            // ... 
        }
        public function gc($maximumLifetime){
            // return value should be true for success or false for failure
            // ...
        }
        public function open($sessionSavePath, $sessionName){
            // return value should be true for success or false for failure
            // ...
        }
        public function read($sessionId){
            // return value should be the session data or an empty string
            // ...
        }
        public function write($sessionId, $sessionData){
            // return value should be true for success or false for failure
            // ...
        }
        public function create_sid(){
            // available since PHP 5.5.1
            // invoked internally when a new session id is needed
            // no parameter is needed and return value should be the new session id created
            // ...
        }
        public function validateId($sessionId){
            // implements SessionUpdateTimestampHandlerInterface::validateId()
            // available since PHP 7.0
            // return value should be true if the session id is valid otherwise false
            // if false is returned a new session id will be generated by php internally
            // ...
        }
        public function updateTimestamp($sessionId, $sessionData){
            // implements SessionUpdateTimestampHandlerInterface::validateId()
            // available since PHP 7.0
            // return value should be true for success or false for failure
            // ...
        }
    }
?>
```
  

#

[Official documentation page](https://www.php.net/manual/en/function.session-set-save-handler.php)

**[To root](/README.md)**