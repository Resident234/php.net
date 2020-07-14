# mb_ereg_match



The behaviour of mb_ereg_match to imply a ^ at the beginning of the pattern stands in stark contrast to the behaviour of mb_ereg where ^ is NOT implied.<br><br>Switching between those two routines (because the need to extract a subpattern changes) requires careful consideration when to compensate for this surprising inconsistence.  

#

[Official documentation page](https://www.php.net/manual/en/function.mb-ereg-match.php)

**[To root](/README.md)**