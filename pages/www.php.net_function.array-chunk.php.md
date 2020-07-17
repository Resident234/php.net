# array_chunk



Tried to use an example below (#56022) for array_chunk_fixed that would "partition" or divide an array into a desired number of split lists -- a useful procedure for "chunking" up objects or text items into columns, or partitioning any type of data resource. However, there seems to be a flaw with array_chunk_fixed &#x2014; for instance, try it with a nine item list and with four partitions. It results in 3 entries with 3 items, then a blank array.<br><br>So, here is the output of my own dabbling on the matter:<br><br>

```
<?php

function partition( $list, $p ) {
    $listlen = count( $list );
    $partlen = floor( $listlen / $p );
    $partrem = $listlen % $p;
    $partition = array();
    $mark = 0;
    for ($px = 0; $px < $p; $px++) {
        $incr = ($px < $partrem) ? $partlen + 1 : $partlen;
        $partition[$px] = array_slice( $list, $mark, $incr );
        $mark += $incr;
    }
    return $partition;
}

$citylist = array( "Black Canyon City", "Chandler", "Flagstaff", "Gilbert", "Glendale", "Globe", "Mesa", "Miami",
                   "Phoenix", "Peoria", "Prescott", "Scottsdale", "Sun City", "Surprise", "Tempe", "Tucson", "Wickenburg" );
print_r( partition( $citylist, 3 ) );

?>
```
<br><br>Array<br>(<br>    [0] =&gt; Array<br>        (<br>            [0] =&gt; Black Canyon City<br>            [1] =&gt; Chandler<br>            [2] =&gt; Flagstaff<br>            [3] =&gt; Gilbert<br>            [4] =&gt; Glendale<br>            [5] =&gt; Globe<br>        )<br><br>    [1] =&gt; Array<br>        (<br>            [0] =&gt; Mesa<br>            [1] =&gt; Miami<br>            [2] =&gt; Phoenix<br>            [3] =&gt; Peoria<br>            [4] =&gt; Prescott<br>            [5] =&gt; Scottsdale<br>        )<br><br>    [2] =&gt; Array<br>        (<br>            [0] =&gt; Sun City<br>            [1] =&gt; Surprise<br>            [2] =&gt; Tempe<br>            [3] =&gt; Tucson<br>            [4] =&gt; Wickenburg<br>        )<br><br>)  

#

Here my array_chunk_values( ) with values distributed by lines (columns are balanced as much as possible) :<br><br>

```
<?php
    function array_chunk_vertical($data, $columns) {
        $n = count($data) ;
        $per_column = floor($n / $columns) ;
        $rest = $n % $columns ;

        // The map
        $per_columns = array( ) ;
        for ( $i = 0 ; $i < $columns ; $i++ ) {
            $per_columns[$i] = $per_column + ($i < $rest ? 1 : 0) ;
        }

        $tabular = array( ) ;
        foreach ( $per_columns as $rows ) {
            for ( $i = 0 ; $i < $rows ; $i++ ) {
                $tabular[$i][ ] = array_shift($data) ;
            }
        }

        return $tabular ;
    }

    header('Content-Type: text/plain') ;

    $data = array_chunk_vertical(range(1, 31), 7) ;
    foreach ( $data as $row ) {
        foreach ( $row as $value ) {
            printf('[%2s]', $value) ;
        }
        echo "\r\n" ;
    }

    /*
        Output :

        [ 1][ 6][11][16][20][24][28]
        [ 2][ 7][12][17][21][25][29]
        [ 3][ 8][13][18][22][26][30]
        [ 4][ 9][14][19][23][27][31]
        [ 5][10][15]
    */
?>
```
  

#

If you just want to grab one chunk from an array, you should use array_slice().  

#

Response to azspot at gmail dot com function partition.<br><br>$columns = 3;<br>$citylist = array(&apos;Black Canyon City&apos;, &apos;Chandler&apos;, &apos;Flagstaff&apos;, &apos;Gilbert&apos;, &apos;Glendale&apos;, &apos;Globe&apos;, &apos;Mesa&apos;, &apos;Miami&apos;, &apos;Phoenix&apos;, &apos;Peoria&apos;, &apos;Prescott&apos;, &apos;Scottsdale&apos;, &apos;Sun City&apos;, &apos;Surprise&apos;, &apos;Tempe&apos;, &apos;Tucson&apos;, &apos;Wickenburg&apos;);<br>print_r(array_chunk($citylist, ceil(count($citylist) / $columns)));<br><br>Output:<br>Array<br>(<br>    [0] =&gt; Array<br>        (<br>            [0] =&gt; Black Canyon City<br>            [1] =&gt; Chandler<br>            [2] =&gt; Flagstaff<br>            [3] =&gt; Gilbert<br>            [4] =&gt; Glendale<br>            [5] =&gt; Globe<br>        )<br><br>    [1] =&gt; Array<br>        (<br>            [0] =&gt; Mesa<br>            [1] =&gt; Miami<br>            [2] =&gt; Phoenix<br>            [3] =&gt; Peoria<br>            [4] =&gt; Prescott<br>            [5] =&gt; Scottsdale<br>        )<br><br>    [2] =&gt; Array<br>        (<br>            [0] =&gt; Sun City<br>            [1] =&gt; Surprise<br>            [2] =&gt; Tempe<br>            [3] =&gt; Tucson<br>            [4] =&gt; Wickenburg<br>        )<br><br>)  

#

[Official documentation page](https://www.php.net/manual/en/function.array-chunk.php)

**[To root](/README.md)**