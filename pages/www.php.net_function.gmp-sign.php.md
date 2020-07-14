# gmp_sign



Hi !<br><br>If you don&apos;t have the GMP extension, the sign function is really simple to code.<br>Here an example of implementation :<br><br>

```
<?php
function sign( $number ) {
    return ( $number &gt; 0 ) ? 1 : ( ( $number &lt; 0 ) ? -1 : 0 );
}

echo sign( 500 ); // Return 1
echo sign( -500 ); // Return -1
echo sign( 0 ); // Return 0
?>
```
<br><br>Thomas.  

#

[Official documentation page](https://www.php.net/manual/en/function.gmp-sign.php)

**[To root](/README.md)**