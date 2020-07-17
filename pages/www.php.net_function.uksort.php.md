# uksort



[Editor&apos;s note: the following comment may be factually incorrect]<br><br>uksort is only usable in the UK<br>

```
<?php
if($country=="UK"){
  uksort();
}else{
  echo "You have to live in UK to use uksort().";
}
?>
```
  

#

(about sorting an array of objects by their properties in a class - inspired by webmaster at zeroweb dot org at usort function)<br>I&apos;m using classes as an abstraction for querying records in a database and use arrays of objects to store records that have an 1 to n relationship. E.g. a class "family" has family members stored as an array of objects. Each of those objects prepresents a record in a database related to the family (by it&apos;s familyId).<br><br>To identify members, I&apos;m using their memberId as the key of the array e.g. $family-&gt;members[$memberId].<br>To sort the family members AFTER fetching them with the database query, you can use the functions _objSort and sortMembers which will sort the "members" array by key using it&apos;s properties (for space reasons I didn&apos;t include the methods used to open the records):<br>

```
<?php
class familyMember
{
    var $memberId;
    var $familyId;
    var $firstName;
    var $age;
    var $hairColor;
// ...
}

class family
{
    var $familyId;
    var $name;
    var $members = array(); // array of familyMember objects
    var $sortFields = array();
    var $sortDirections = array();
    // ...
    function _objSort(&amp;$a, &amp;$b, $i = 0)
    {
        $field        = $this->sortFields[$i];
        $direction    = $this->sortDirections[$i];
        
        $diff = strnatcmp($this->details[$a]->$field, $this->details[$b]->$field) * $direction;
        if ($diff == 0 &amp;&amp; isset($this->sortFields[++$i]))
        {
            $diff = $this->_objSort($a, $b, $i);
        }
        
        return $diff;
    }
    
    function sortMembers($sortFields)
    {
        $i = 0;
        foreach ($sortFields as $field => $direction)
        {
            $this->sortFields[$i] = $field;
            $direction == "DESC" ? $this->sortDirections[$i] = -1 : $this->sortDirections[$i] = 1;
            $i++;
        }
        
        uksort($this->details, array($this, "_objSort"));
        
        $this->sortFields = array();
        $this->sortDirections = array();
    }
}
// open a family
$familyId = 5;
$family = new family($familyId);
$family->open(); // this will also fetch all members

// sort members by 3 fields
$family->sortMembers(array("firstName" => "ASC", "age" => "DESC", "hairColor" => "ASC"));
// output all family members
foreach ($family->members as $member)
{
    echo $member->firstName." - ".$member->age." - ".$member->hairColor."<br />";
}
?>
```
<br><br>Note that this might not be the fastest thing on earth and it hasn&apos;t been tested very much yet but I hope it&apos;s useful for someone.  

#

[Official documentation page](https://www.php.net/manual/en/function.uksort.php)

**[To root](/README.md)**