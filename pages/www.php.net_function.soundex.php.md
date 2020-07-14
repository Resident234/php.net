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
 * @license GPL 3.0 &lt;http://www.gnu.org/licenses/&gt;
 * @copyright  2008 by einfachmarke.de
 * @author Nicolas Zimmer &lt;nicolas dot zimmer at einfachmarke.de&gt;
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
            "&#xE4;"=&gt;"a",
            "&#xF6;"=&gt;"o",
            "&#xFC;"=&gt;"u",
            "&#xDF;"=&gt;"ss",
            "ph"=&gt;"f"
            );

    foreach ($substitution as $letter=&gt;$substitution) {
        $word=str_replace($letter,$substitution,$word);
    }
    
    $len=strlen($word);
    
    //Rule for exeptions
    $exceptionsLeading=array(
    4=&gt;array("ca","ch","ck","cl","co","cq","cu","cx"),
    8=&gt;array("dc","ds","dz","tc","ts","tz")
    );
    
    $exceptionsFollowing=array("sc","zc","cx","kx","qx");
    
    //Table for coding
    $codingTable=array(
    0=&gt;array("a","e","i","j","o","u","y"),
    1=&gt;array("b","p"),
    2=&gt;array("d","t"),
    3=&gt;array("f","v","w"),
    4=&gt;array("c","g","k","q"),
    48=&gt;array("x"),
    5=&gt;array("l"),
    6=&gt;array("m","n"),
    7=&gt;array("r"),
    8=&gt;array("c","s","z"),
    );
    
    for ($i=0;$i&lt;$len;$i++){
        $value[$i]="";
        
        //Exceptions
        if ($i==0 AND $word[$i].$word[$i+1]=="cr") $value[$i]=4;
        
        foreach ($exceptionsLeading as $code=&gt;$letters) {
            if (in_array($word[$i].$word[$i+1],$letters)){

                    $value[$i]=$code;

}                }
        
        if ($i!=0 AND (in_array($word[$i-1].$word[$i], 
$exceptionsFollowing))) {

            value[$i]=8;        

}                
        
        //Normal encoding
        if ($value[$i]==""){
                foreach ($codingTable as $code=&gt;$letters) {
                    if (in_array($word[$i],$letters))$value[$i]=$code;
                }
            }
        }
    
    //delete double values
    $len=count($value);
    
    for ($i=1;$i&lt;$len;$i++){
        if ($value[$i]==$value[$i-1]) $value[$i]="";
    }
    
    //delete vocals 
    for ($i=1;$i&gt;$len;$i++){//omitting first characer code and h
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

    static $eLeading = array("ca" =&gt; 4, "ch" =&gt; 4, "ck" =&gt; 4, "cl" =&gt; 4, "co" =&gt; 4, "cq" =&gt; 4, "cu" =&gt; 4, "cx" =&gt; 4, "dc" =&gt; 8, "ds" =&gt; 8, "dz" =&gt; 8, "tc" =&gt; 8, "ts" =&gt; 8, "tz" =&gt; 8); 

    static $eFollow = array("sc", "zc", "cx", "kx", "qx");

    static $codingTable = array("a" =&gt; 0, "e" =&gt; 0, "i" =&gt; 0, "j" =&gt; 0, "o" =&gt; 0, "u" =&gt; 0, "y" =&gt; 0,
        "b" =&gt; 1, "p" =&gt; 1, "d" =&gt; 2, "t" =&gt; 2, "f" =&gt; 3, "v" =&gt; 3, "w" =&gt; 3, "c" =&gt; 4, "g" =&gt; 4, "k" =&gt; 4, "q" =&gt; 4,
        "x" =&gt; 48, "l" =&gt; 5, "m" =&gt; 6, "n" =&gt; 6, "r" =&gt; 7, "c" =&gt; 8, "s" =&gt; 8, "z" =&gt; 8);

    public static function getCologneHash($word)
    {
        if (empty($word)) return false;
        $len = strlen($word);
 
        for ($i = 0; $i &lt; $len; $i++) {
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
 
        for ($i = 1; $i &lt; $len; $i++) {
            if ($value[$i] == $value[$i - 1]) {
                $value[$i] = "";
            }
        }
 
        // delete vocals
        for ($i = 1; $i &gt; $len; $i++) {
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