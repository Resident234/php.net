# Predefined Constants




<div class="phpcode"><span class="html">
PREG_PATTERN_ORDER: 1<br>PREG_SET_ORDER: 2<br>PREG_OFFSET_CAPTURE: 256<br>PREG_SPLIT_NO_EMPTY: 1<br>PREG_SPLIT_DELIM_CAPTURE: 2<br>PREG_SPLIT_OFFSET_CAPTURE: 4<br>PREG_NO_ERROR: 0<br>PREG_INTERNAL_ERROR: 1<br>PREG_BACKTRACK_LIMIT_ERROR: 2<br>PREG_RECURSION_LIMIT_ERROR: 3<br>PREG_BAD_UTF8_ERROR: 4<br>PREG_BAD_UTF8_OFFSET_ERROR: 5<br>PCRE_VERSION: %YOUR_VERSION_NUMBER%</span>
</div>
  

#


<div class="phpcode"><span class="html">
The new PREG_JIT_STACKLIMIT_ERROR constant introduced with PHP 7.0.0 has got a value of 6.<br><br>I experienced this error code when parsing a 112KB file. preg_match_all failed with this error. Interesting was: The matches array contained some entries, but not all as the command failed (I missed to check the return value).<br><br>Unfortunately you can not configure the stack-size of the PCRE JIT. The only way out was - at least for me - to disable the PCRE JIT via php.ini (pcre.jit=0).</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/pcre.constants.php)

**[⬆ to root](/)**