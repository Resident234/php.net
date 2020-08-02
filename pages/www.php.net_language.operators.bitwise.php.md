# Bitwise Operators



BITWISE FLAGS for Custom PHP Objects<br><br>Sometimes I need a custom PHP Object that holds several boolean TRUE or FALSE values. I could easily include a variable for each of them, but as always, code has a way to get unweildy pretty fast. A more intelligent approach always seems to be the answer, even if it seems to be overkill at first.<br><br>I start with an abstract base class which will hold a single integer variable called $flags. This simple integer can hold 32 TRUE or FALSE boolean values. Another thing to consider is to just set certain BIT values without disturbing any of the other BITS -- so included in the class definition is the setFlag($flag, $value) function, which will set only the chosen bit. Here&apos;s the abstract base class definition: <br><br>

```
<?php

# BitwiseFlag.php

abstract class BitwiseFlag
{
  protected $flags;

  /*
   * Note: these functions are protected to prevent outside code
   * from falsely setting BITS. See how the extending class 'User'
   * handles this.
   *
   */
  protected function isFlagSet($flag)
  {
    return (($this->flags &amp; $flag) == $flag);
  }

  protected function setFlag($flag, $value)
  {
    if($value)
    {
      $this->flags |= $flag;
    }
    else
    {
      $this->flags &amp;= ~$flag;
    }
  }
}

?>
```


The class above is abstract and cannot be instantiated, so an extension is required. Below is a simple extension called User -- which is severely truncated for clarity. Notice I am defining const variables AND methods to use them.



```
<?php

# User.php

require('BitwiseFlag.php');

class User extends BitwiseFlag
{
  const FLAG_REGISTERED = 1; // BIT #1 of $flags has the value 1
  const FLAG_ACTIVE = 2;     // BIT #2 of $flags has the value 2
  const FLAG_MEMBER = 4;     // BIT #3 of $flags has the value 4
  const FLAG_ADMIN = 8;      // BIT #4 of $flags has the value 8

  public function isRegistered(){
    return $this->isFlagSet(self::FLAG_REGISTERED);
  }

  public function isActive(){
    return $this->isFlagSet(self::FLAG_ACTIVE);
  }

  public function isMember(){
    return $this->isFlagSet(self::FLAG_MEMBER);
  }

  public function isAdmin(){
    return $this->isFlagSet(self::FLAG_ADMIN);
  }

  public function setRegistered($value){
    $this->setFlag(self::FLAG_REGISTERED, $value);
  }

  public function setActive($value){
    $this->setFlag(self::FLAG_ACTIVE, $value);
  }

  public function setMember($value){
    $this->setFlag(self::FLAG_MEMBER, $value);
  }

  public function setAdmin($value){
    $this->setFlag(self::FLAG_ADMIN, $value);
  }

  public function __toString(){
    return 'User [' .
      ($this->isRegistered() ? 'REGISTERED' : '') .
      ($this->isActive() ? ' ACTIVE' : '') .
      ($this->isMember() ? ' MEMBER' : '') .
      ($this->isAdmin() ? ' ADMIN' : '') .
    ']';
  }
}

?>
```


This seems like a lot of work, but we have addressed many issues, for example, using and maintaining the code is easy, and the getting and setting of flag values make sense. With the User class, you can now see how easy and intuitive bitwise flag operations become.



```
<?php

require('User.php')

$user = new User();
$user->setRegistered(true);
$user->setActive(true);
$user->setMember(true);
$user->setAdmin(true);

echo $user;  // outputs: User [REGISTERED ACTIVE MEMBER ADMIN]

?>
```
  

---

Initially, I found bitmasking to be a confusing concept and found no use for it. So I&apos;ve whipped up this code snippet in case anyone else is confused:<br><br>

```
<?php

    // The various details a vehicle can have
    $hasFourWheels = 1;
    $hasTwoWheels  = 2;
    $hasDoors      = 4;
    $hasRedColour  = 8;

    $bike          = $hasTwoWheels;
    $golfBuggy     = $hasFourWheels;
    $ford          = $hasFourWheels | $hasDoors;
    $ferrari       = $hasFourWheels | $hasDoors | $hasRedColour;

    $isBike        = $hasFourWheels &amp; $bike; # False, because $bike doens't have four wheels
    $isGolfBuggy   = $hasFourWheels &amp; $golfBuggy; # True, because $golfBuggy has four wheels
    $isFord        = $hasFourWheels &amp; $ford; # True, because $ford $hasFourWheels

?>
```


And you can apply this to a lot of things, for example, security:



```
<?php

    // Security permissions:
    $writePost = 1;
    $readPost = 2;
    $deletePost = 4;
    $addUser = 8;
    $deleteUser = 16;
    
    // User groups:
    $administrator = $writePost | $readPosts | $deletePosts | $addUser | $deleteUser;
    $moderator = $readPost | $deletePost | $deleteUser;
    $writer = $writePost | $readPost;
    $guest = $readPost;

    // function to check for permission
    function checkPermission($user, $permission) {
        if($user &amp; $permission) {
            return true;
        } else {
            return false;
        }
    }

    // Now we apply all of this!
    if(checkPermission($administrator, $deleteUser)) {
        deleteUser("Some User"); # This is executed because $administrator can $deleteUser
    }

?>
```
<br><br>Once you get your head around it, it&apos;s VERY useful! Just remember to raise each value by the power of two to avoid problems  

---

A bitwise operators practical case :<br><br>

```
<?php
    // We want to know the red, green and blue values of this color :
    $color = 0xFEA946 ;

    $red = $color >> 16 ;
    $green = ($color &amp; 0x00FF00) >> 8 ;
    $blue = $color &amp; 0x0000FF ;

    printf('Red : %X (%d), Green : %X (%d), Blue : %X (%d)',
        $red, $red, $green, $green, $blue, $blue) ;

    // Will display...
    // Red : FE (254), Green : A9 (169), Blue : 46 (70)
?>
```
  

---

[Official documentation page](https://www.php.net/manual/en/language.operators.bitwise.php)

**[To root](/README.md)**