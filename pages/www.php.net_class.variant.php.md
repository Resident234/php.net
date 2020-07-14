# variant class



If you are frustrated that print_r($obj) (where $obj is something returned from a call to a function on a COM object) does not return anything helpful, and that variant_get_type($obj) just returns a number, the function you are actually after is:<br>  com_print_typeinfo($obj);<br><br>It lists all functions, variables, their types in a human-readable (well, programmer-readable) format. Lovely!  

#

[Official documentation page](https://www.php.net/manual/en/class.variant.php)

**[To root](/README.md)**