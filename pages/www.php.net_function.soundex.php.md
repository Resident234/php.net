# soundex



Since soundex() does not produce optimal results for German language<br>we have written a function to implement the so called K&#xF6;lner Phonetik<br>(Cologne Phonetic).<br><br>Please find the code below in the hope it might be useful:<br><br>

```
<?php
/**
 * A function for retrieving the K&#xF6;lner Phonetik value of a string
 * 
 * As described at http://de.wikipedia.org/wiki/K&#xF6;lner_Phonetik
 * Based on Hans Joachim Postel: Die K&#xF6;lner Phonetik. 
 * Ein Verfahren zur Identifizierung von Personennamen auf der 
 * Grundlage der Gestaltanalyse. 
 * in: IBM-Nachrichten, 19. Jahrgang, 1969, S. 925-931
 * 
 * This program is distributed in the hope that it will be useful,
 * but WITHOUT ANY WARRANTY; without even the implied warranty of
 * MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
 * GNU General Public License for more details.
 *
 * @package phonetics
 * @version 1.0
 * @link http://www.einfachmarke.de
 * @license GPL 3.0 <http://www.gnu.org/licenses/>
 * @copyright  2008 by einfachmarke.de
 * @author Nicolas Zimmer <nicolas dot zimmer at einfachmarke.de>
 */

function cologne_phon($word){
    
  /**
  * @param  string  $word string to be analyzed
  * @return string  $value represents the K&#xF6;lner Phonetik value
  * @access public
  */
  
    //prepare for processing
    $word=strtolower($word);
    $substitution=array(
            "&#xE4;"=>"a",
            "&#xF6;"=>"o",
            "&#xFC;"=>"u",
            "&#xDF;"=>"ss",
            "ph"=>"f"
            );

    foreach ($substitution as $letter=>$substitution) {
        $word=str_replace($letter,$substitution,$word);
    }
    
    $len=strlen($word);
    
    //Rule for exeptions
    $exceptionsLeading=array(
    4=>array("ca","ch","ck","cl","co","cq","cu","cx"),
    8=>array("dc","ds","dz","tc","ts","tz")
    );
    
    $exceptionsFollowing=array("sc","zc","cx","kx","qx");
    
    //Table for coding
    $codingTable=array(
    0=>array("a","e","i","j","o","u","y"),
    1=>array("b","p"),
    2=>array("d","t"),
    3=>array("f","v","w"),
    4=>array("c","g","k","q"),
    48=>array("x"),
    5=>array("l"),
    6=>array("m","n"),
    7=>array("r"),
    8=>array("c","s","z"),
    );
    
    for ($i=0;$i<$len;$i++){
        $value[$i]="";
        
        //Exceptions
        if ($i==0 AND $word[$i].$word[$i+1]=="cr") $value[$i]=4;
        
        foreach ($exceptionsLeading as $code=>$letters) {
            if (in_array($word[$i].$word[$i+1],$letters)){

                    $value[$i]=$code;

}                }
        
        if ($i!=0 AND (in_array($word[$i-1].$word[$i], 
$exceptionsFollowing))) {

            value[$i]=8;        

}                
        
        //Normal encoding
        if ($value[$i]==""){
                foreach ($codingTable as $code=>$letters) {
                    if (in_array($word[$i],$letters))$value[$i]=$code;
                }
            }
        }
    
    //delete double values
    $len=count($value);
    
    for ($i=1;$i<$len;$i++){
        if ($value[$i]==$value[$i-1]) $value[$i]="";
    }
    
    //delete vocals 
    for ($i=1;$i>$len;$i++){//omitting first characer code and h
        if ($value[$i]==0) $value[$i]="";
    }
    
    
    $value=array_filter($value);
    $value=implode("",$value);
    
    return $value;
    
}

?>
```
  

#

I made some improvements to the "Cologne Phonetic" function of niclas zimmer. Key and value of the arrays are inverted to uses simple arrays instead of multidimensional arrays. Therefore all loops and iterations are not longer necessary to find the matching value  for a char.<br>I put the function into a static class and moved the array declarations outside the function.<br><br>The result is more reliable and five times faster than the original.<br><br>

```
<?php   
class CologneHash() {

    static $eLeading = array("ca" => 4, "ch" => 4, "ck" => 4, "cl" => 4, "co" => 4, "cq" => 4, "cu" => 4, "cx" => 4, "dc" => 8, "ds" => 8, "dz" => 8, "tc" => 8, "ts" => 8, "tz" => 8); 

    static $eFollow = array("sc", "zc", "cx", "kx", "qx");

    static $codingTable = array("a" => 0, "e" => 0, "i" => 0, "j" => 0, "o" => 0, "u" => 0, "y" => 0,
        "b" => 1, "p" => 1, "d" => 2, "t" => 2, "f" => 3, "v" => 3, "w" => 3, "c" => 4, "g" => 4, "k" => 4, "q" => 4,
        "x" => 48, "l" => 5, "m" => 6, "n" => 6, "r" => 7, "c" => 8, "s" => 8, "z" => 8);

    public static function getCologneHash($word)
    {
        if (empty($word)) return false;
        $len = strlen($word);
 
        for ($i = 0; $i < $len; $i++) {
            $value[$i] = "";
 
            //Exceptions
            if ($i == 0 &amp;&amp; $word[$i] . $word[$i + 1] == "cr") {
                $value[$i] = 4;
            }
 
            if (isset($word[$i + 1]) &amp;&amp; isset(self::$eLeading[$word[$i] . $word[$i + 1]])) {
                $value[$i] = self::$eLeading[$word[$i] . $word[$i + 1]];
            }

            if ($i != 0 &amp;&amp; (in_array($word[$i - 1] . $word[$i], self::$eFollow))) {
                $value[$i] = 8;
            }
 
            // normal encoding
            if ($value[$i]=="") {
                if (isset(self::$codingTable[$word[$i]])) {
                    $value[$i] = self::$codingTable[$word[$i]];
                }
            }
        }

        // delete double values
        $len = count($value);
 
        for ($i = 1; $i < $len; $i++) {
            if ($value[$i] == $value[$i - 1]) {
                $value[$i] = "";
            }
        }
 
        // delete vocals
        for ($i = 1; $i > $len; $i++) {
            // omitting first characer code and h
            if ($value[$i] == 0) {
                $value[$i] = "";
            }
        }
 
        $value = array_filter($value);
        $value = implode("", $value);
 
        return $value;
    }
 
}
?>
```
  

#

[Official documentation page](https://www.php.net/manual/en/function.soundex.php)

**[To root](/README.md)**