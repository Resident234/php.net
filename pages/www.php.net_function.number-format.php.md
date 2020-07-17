# number_format



It&apos;s not explicitly documented; number_format also rounds:<br><br>

```
<?php
$numbers = array(0.001, 0.002, 0.003, 0.004, 0.005, 0.006, 0.007, 0.008, 0.009);
foreach ($numbers as $number)
    print $number."->".number_format($number, 2, '.', ',')."<br>";
?>
```
<br><br>0.001-&gt;0.00<br>0.002-&gt;0.00<br>0.003-&gt;0.00<br>0.004-&gt;0.00<br>0.005-&gt;0.01<br>0.006-&gt;0.01<br>0.007-&gt;0.01<br>0.008-&gt;0.01<br>0.009-&gt;0.01  

#

here is the code to convert number to Indonesian text, this code has limitation as is number_format function. sorry for this.<br>/*<br>* Created : Iwan Sapoetra - Jun 13, 2008<br>* Project : Web<br>* Package : cgaf<br>*<br>*/<br>function terbilang( $num ,$dec=4){<br>    $stext = array(<br>        "Nol",<br>        "Satu",<br>        "Dua",<br>        "Tiga",<br>        "Empat",<br>        "Lima",<br>        "Enam",<br>        "Tujuh",<br>        "Delapan",<br>        "Sembilan",<br>        "Sepuluh",<br>        "Sebelas"<br>    );<br>    $say  = array(<br>        "Ribu",<br>        "Juta",<br>        "Milyar",<br>        "Triliun",<br>        "Biliun", // remember limitation of float<br>        "--apaan---" ///setelah biliun namanya apa?<br>    );<br>    $w = "";<br><br>    if ($num &lt;0 ) {<br>        $w  = "Minus ";<br>        //make positive<br>        $num *= -1;<br>    }<br><br>    $snum = number_format($num,$dec,",",".");<br>    die($snum);<br>    $strnum =  explode(".",substr($snum,0,strrpos($snum,",")));<br>    //parse decimalnya<br>    $koma = substr($snum,strrpos($snum,",")+1);<br><br>    $isone = substr($num,0,1)  ==1;<br>    if (count($strnum)==1) {<br>        $num = $strnum[0];<br>        switch (strlen($num)) {<br>            case 1:<br>            case 2:<br>                if (!isset($stext[$strnum[0]])){<br>                    if($num&lt;19){<br>                        $w .=$stext[substr($num,1)]." Belas";<br>                    }else{<br>                        $w .= $stext[substr($num,0,1)]." Puluh ".<br>                            (intval(substr($num,1))==0 ? "" : $stext[substr($num,1)]);<br>                    }<br>                }else{<br>                    $w .= $stext[$strnum[0]];<br>                }<br>                break;<br>            case 3:<br>                $w .=  ($isone ? "Seratus" : terbilang(substr($num,0,1)) .<br>                    " Ratus").<br>                    " ".(intval(substr($num,1))==0 ? "" : terbilang(substr($num,1)));<br>                break;<br>            case 4:<br>                $w .=  ($isone ? "Seribu" : terbilang(substr($num,0,1)) .<br>                    " Ribu").<br>                    " ".(intval(substr($num,1))==0 ? "" : terbilang(substr($num,1)));<br>                break;<br>            default:<br>                break;<br>        }<br>    }else{<br>        $text = $say[count($strnum)-2];<br>        $w = ($isone &amp;&amp; strlen($strnum[0])==1 &amp;&amp; count($strnum) &lt;=3? "Se".strtolower($text) : terbilang($strnum[0]).&apos; &apos;.$text);<br>        array_shift($strnum);<br>        $i =count($strnum)-2;<br>        foreach ($strnum as $k=&gt;$v) {<br>            if (intval($v)) {<br>                $w.= &apos; &apos;.terbilang($v).&apos; &apos;.($i &gt;=0 ? $say[$i] : "");<br>            }<br>            $i--;<br>        }<br>    }<br>    $w = trim($w);<br>    if ($dec = intval($koma)) {<br>        $w .= " Koma ". terbilang($koma);<br>    }<br>    return trim($w);<br>}<br>//example<br>echo terbilang(999999999999)."\n";<br>/**<br> * result : Sembilan Ratus Sembilan Puluh Sembilan Milyar Sembilan Ratus Sembilan Puluh Sembilan Juta Sembilan Ratus Sembilan Puluh Sembilan Ribu Sembilan Ratus Sembilan Puluh Sembilan<br> */<br>echo terbilang(9999999999999999);<br>/**<br> * todo : fix this bug pleasese<br> * problem : number_format(9999999999999999) &lt;--- 10.000.000.000.000.000,0000<br> * Result : Sepuluh Biliun<br> */  

#

Outputs a human readable number.<br><br>

```
<?php
    #    Output easy-to-read numbers
    #    by james at bandit.co.nz
    function bd_nice_number($n) {
        // first strip any formatting;
        $n = (0+str_replace(",","",$n));
        
        // is this a number?
        if(!is_numeric($n)) return false;
        
        // now filter it;
        if($n>1000000000000) return round(($n/1000000000000),1).' trillion';
        else if($n>1000000000) return round(($n/1000000000),1).' billion';
        else if($n>1000000) return round(($n/1000000),1).' million';
        else if($n>1000) return round(($n/1000),1).' thousand';
        
        return number_format($n);
    }
?>
```
<br><br>Outputs:<br><br>247,704,360 -&gt; 247.7 million<br>866,965,260,000 -&gt; 867 billion  

#

I ran across an issue where I wanted to keep the entered precision of a real value, without arbitrarily rounding off what the user had submitted.<br><br>I figured it out with a quick explode on the number before formatting. I could then format either side of the decimal.<br><br>

```
<?php
      function number_format_unlimited_precision($number,$decimal = '.')
      {
           $broken_number = explode($decimal,$number);
           return number_format($broken_number[0]).$decimal.$broken_number[1];
      }
?>
```
  

#

For Zero fill - just use the sprintf() function<br><br>$pr_id = 1;<br>$pr_id = sprintf("%03d", $pr_id);<br>echo $pr_id;<br><br>//outputs 001<br>-----------------<br><br>$pr_id = 10;<br>$pr_id = sprintf("%03d", $pr_id);<br>echo $pr_id;<br><br>//outputs 010<br>-----------------<br><br>You can change %03d to %04d, etc.  

#

[Official documentation page](https://www.php.net/manual/en/function.number-format.php)

**[To root](/README.md)**