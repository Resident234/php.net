# Debugging in PHP



I find it very useful to print out to the browsers console instead of just var_dumping:<br><br>function console_log( $data ){<br>  echo &apos;&lt;script&gt;&apos;;<br>  echo &apos;console.log(&apos;. json_encode( $data ) .&apos;)&apos;;<br>  echo &apos;&lt;/script&gt;&apos;;<br>}<br><br>Usage:<br>$myvar = array(1,2,3);<br>console_log( $myvar ); // [1,2,3]  

#

I would like to add http://phpdebugbar.com/ which is also a handy tool for debugging/profiling data. and you could use it whenever framework you are using. :-)  

#

[Official documentation page](https://www.php.net/manual/en/debugger.php)

**[To root](/README.md)**