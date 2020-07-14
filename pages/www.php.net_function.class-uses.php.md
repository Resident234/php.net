# class_uses



To get ALL traits including those used by parent classes and other traits, use this function:<br><br>

```
<?php
function class_uses_deep($class, $autoload = true) {
    $traits = [];
    do {
        $traits = array_merge(class_uses($class, $autoload), $traits);
    } while($class = get_parent_class($class));
    foreach ($traits as $trait =&gt; $same) {
        $traits = array_merge(class_uses($trait, $autoload), $traits);
    }
    return array_unique($traits);
}
?>
```
  

#

A slighly modified version from Stealz that also checks all the "parent" traits used by the traits:<br><br>

```
<?php
public static function class_uses_deep($class, $autoload = true)
    {
        $traits = [];

        // Get traits of all parent classes
        do {
            $traits = array_merge(class_uses($class, $autoload), $traits);
        } while ($class = get_parent_class($class));

        // Get traits of all parent traits
        $traitsToSearch = $traits;
        while (!empty($traitsToSearch)) {
            $newTraits = class_uses(array_pop($traitsToSearch), $autoload);
            $traits = array_merge($newTraits, $traits);
            $traitsToSearch = array_merge($newTraits, $traitsToSearch);
        };

        foreach ($traits as $trait =&gt; $same) {
            $traits = array_merge(class_uses($trait, $autoload), $traits);
        }

        return array_unique($traits);
    }
?>
```
  

#

[Official documentation page](https://www.php.net/manual/en/function.class-uses.php)

**[To root](/README.md)**