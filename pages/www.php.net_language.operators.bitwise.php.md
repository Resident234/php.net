# Bitwise Operators





BITWISE FLAGS for Custom PHP Objects

Sometimes I need a custom PHP Object that holds several boolean TRUE or FALSE values. I could easily include a variable for each of them, but as always, code has a way to get unweildy pretty fast. A more intelligent approach always seems to be the answer, even if it seems to be overkill at first.

I start with an abstract base class which will hold a single integer variable called $flags. This simple integer can hold 32 TRUE or FALSE boolean values. Another thing to consider is to just set certain BIT values without disturbing any of the other BITS -- so included in the class definition is the setFlag($flag, $value) function, which will set only the chosen bit. Here&apos;s the abstract base class definition: 



```
<?php

# BitwiseFlag.php

abstract class BitwiseFlag
{
&#xA0; protected $flags;

&#xA0; /*
&#xA0;&#xA0; * Note: these functions are protected to prevent outside code
&#xA0;&#xA0; * from falsely setting BITS. See how the extending class &apos;User&apos;
&#xA0;&#xA0; * handles this.
&#xA0;&#xA0; *
&#xA0;&#xA0; */
&#xA0; protected function isFlagSet($flag)
&#xA0; {
&#xA0; &#xA0; return (($this-&gt;flags &amp; $flag) == $flag);
&#xA0; }

&#xA0; protected function setFlag($flag, $value)
&#xA0; {
&#xA0; &#xA0; if($value)
&#xA0; &#xA0; {
&#xA0; &#xA0; &#xA0; $this-&gt;flags |= $flag;
&#xA0; &#xA0; }
&#xA0; &#xA0; else
&#xA0; &#xA0; {
&#xA0; &#xA0; &#xA0; $this-&gt;flags &amp;= ~$flag;
&#xA0; &#xA0; }
&#xA0; }
}

?>
```


The class above is abstract and cannot be instantiated, so an extension is required. Below is a simple extension called User -- which is severely truncated for clarity. Notice I am defining const variables AND methods to use them.



```
<?php

# User.php

require(&apos;BitwiseFlag.php&apos;);

class User extends BitwiseFlag
{
&#xA0; const FLAG_REGISTERED = 1; // BIT #1 of $flags has the value 1
&#xA0; const FLAG_ACTIVE = 2;&#xA0; &#xA0;&#xA0; // BIT #2 of $flags has the value 2
&#xA0; const FLAG_MEMBER = 4;&#xA0; &#xA0;&#xA0; // BIT #3 of $flags has the value 4
&#xA0; const FLAG_ADMIN = 8;&#xA0; &#xA0; &#xA0; // BIT #4 of $flags has the value 8

&#xA0; public function isRegistered(){
&#xA0; &#xA0; return $this-&gt;isFlagSet(self::FLAG_REGISTERED);
&#xA0; }

&#xA0; public function isActive(){
&#xA0; &#xA0; return $this-&gt;isFlagSet(self::FLAG_ACTIVE);
&#xA0; }

&#xA0; public function isMember(){
&#xA0; &#xA0; return $this-&gt;isFlagSet(self::FLAG_MEMBER);
&#xA0; }

&#xA0; public function isAdmin(){
&#xA0; &#xA0; return $this-&gt;isFlagSet(self::FLAG_ADMIN);
&#xA0; }

&#xA0; public function setRegistered($value){
&#xA0; &#xA0; $this-&gt;setFlag(self::FLAG_REGISTERED, $value);
&#xA0; }

&#xA0; public function setActive($value){
&#xA0; &#xA0; $this-&gt;setFlag(self::FLAG_ACTIVE, $value);
&#xA0; }

&#xA0; public function setMember($value){
&#xA0; &#xA0; $this-&gt;setFlag(self::FLAG_MEMBER, $value);
&#xA0; }

&#xA0; public function setAdmin($value){
&#xA0; &#xA0; $this-&gt;setFlag(self::FLAG_ADMIN, $value);
&#xA0; }

&#xA0; public function __toString(){
&#xA0; &#xA0; return &apos;User [&apos; .
&#xA0; &#xA0; &#xA0; ($this-&gt;isRegistered() ? &apos;REGISTERED&apos; : &apos;&apos;) .
&#xA0; &#xA0; &#xA0; ($this-&gt;isActive() ? &apos; ACTIVE&apos; : &apos;&apos;) .
&#xA0; &#xA0; &#xA0; ($this-&gt;isMember() ? &apos; MEMBER&apos; : &apos;&apos;) .
&#xA0; &#xA0; &#xA0; ($this-&gt;isAdmin() ? &apos; ADMIN&apos; : &apos;&apos;) .
&#xA0; &#xA0; &apos;]&apos;;
&#xA0; }
}

?>
```


This seems like a lot of work, but we have addressed many issues, for example, using and maintaining the code is easy, and the getting and setting of flag values make sense. With the User class, you can now see how easy and intuitive bitwise flag operations become.



```
<?php

require(&apos;User.php&apos;)

$user = new User();
$user-&gt;setRegistered(true);
$user-&gt;setActive(true);
$user-&gt;setMember(true);
$user-&gt;setAdmin(true);

echo $user;&#xA0; // outputs: User [REGISTERED ACTIVE MEMBER ADMIN]

?>
```



  

#



Initially, I found bitmasking to be a confusing concept and found no use for it. So I&apos;ve whipped up this code snippet in case anyone else is confused:



```
<?php

&#xA0; &#xA0; // The various details a vehicle can have
&#xA0; &#xA0; $hasFourWheels = 1;
&#xA0; &#xA0; $hasTwoWheels&#xA0; = 2;
&#xA0; &#xA0; $hasDoors&#xA0; &#xA0; &#xA0; = 4;
&#xA0; &#xA0; $hasRedColour&#xA0; = 8;

&#xA0; &#xA0; $bike&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; = $hasTwoWheels;
&#xA0; &#xA0; $golfBuggy&#xA0; &#xA0;&#xA0; = $hasFourWheels;
&#xA0; &#xA0; $ford&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; = $hasFourWheels | $hasDoors;
&#xA0; &#xA0; $ferrari&#xA0; &#xA0; &#xA0;&#xA0; = $hasFourWheels | $hasDoors | $hasRedColour;

&#xA0; &#xA0; $isBike&#xA0; &#xA0; &#xA0; &#xA0; = $hasFourWheels &amp; $bike; # False, because $bike doens&apos;t have four wheels
&#xA0; &#xA0; $isGolfBuggy&#xA0;&#xA0; = $hasFourWheels &amp; $golfBuggy; # True, because $golfBuggy has four wheels
&#xA0; &#xA0; $isFord&#xA0; &#xA0; &#xA0; &#xA0; = $hasFourWheels &amp; $ford; # True, because $ford $hasFourWheels

?>
```


And you can apply this to a lot of things, for example, security:



```
<?php

&#xA0; &#xA0; // Security permissions:
&#xA0; &#xA0; $writePost = 1;
&#xA0; &#xA0; $readPost = 2;
&#xA0; &#xA0; $deletePost = 4;
&#xA0; &#xA0; $addUser = 8;
&#xA0; &#xA0; $deleteUser = 16;
&#xA0; &#xA0; 
&#xA0; &#xA0; // User groups:
&#xA0; &#xA0; $administrator = $writePost | $readPosts | $deletePosts | $addUser | $deleteUser;
&#xA0; &#xA0; $moderator = $readPost | $deletePost | $deleteUser;
&#xA0; &#xA0; $writer = $writePost | $readPost;
&#xA0; &#xA0; $guest = $readPost;

&#xA0; &#xA0; // function to check for permission
&#xA0; &#xA0; function checkPermission($user, $permission) {
&#xA0; &#xA0; &#xA0; &#xA0; if($user &amp; $permission) {
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; return true;
&#xA0; &#xA0; &#xA0; &#xA0; } else {
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; return false;
&#xA0; &#xA0; &#xA0; &#xA0; }
&#xA0; &#xA0; }

&#xA0; &#xA0; // Now we apply all of this!
&#xA0; &#xA0; if(checkPermission($administrator, $deleteUser)) {
&#xA0; &#xA0; &#xA0; &#xA0; deleteUser(&quot;Some User&quot;); # This is executed because $administrator can $deleteUser
&#xA0; &#xA0; }

?>
```


Once you get your head around it, it&apos;s VERY useful! Just remember to raise each value by the power of two to avoid problems

  

#



A bitwise operators practical case :



```
<?php
&#xA0; &#xA0; // We want to know the red, green and blue values of this color :
&#xA0; &#xA0; $color = 0xFEA946 ;

&#xA0; &#xA0; $red = $color &gt;&gt; 16 ;
&#xA0; &#xA0; $green = ($color &amp; 0x00FF00) &gt;&gt; 8 ;
&#xA0; &#xA0; $blue = $color &amp; 0x0000FF ;

&#xA0; &#xA0; printf(&apos;Red : %X (%d), Green : %X (%d), Blue : %X (%d)&apos;,
&#xA0; &#xA0; &#xA0; &#xA0; $red, $red, $green, $green, $blue, $blue) ;

&#xA0; &#xA0; // Will display...
&#xA0; &#xA0; // Red : FE (254), Green : A9 (169), Blue : 46 (70)
?>
```



  

#

[Official documentation page](https://www.php.net/manual/en/language.operators.bitwise.php)

**[To root](/README.md)**