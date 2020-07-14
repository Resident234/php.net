# serialize



&lt;?<br>/*<br>Anatomy of a serialize()&apos;ed value:<br><br> String<br> s:size:value;<br><br> Integer<br> i:value;<br><br> Boolean<br> b:value; (does not store "true" or "false", does store &apos;1&apos; or &apos;0&apos;)<br><br> Null<br> N;<br><br> Array<br> a:size:{key definition;value definition;(repeated per element)}<br><br> Object<br> O:strlen(object name):object name:object size:{s:strlen(property name):property name:property definition;(repeated per property)}<br><br> String values are always in double quotes<br> Array keys are always integers or strings<br>    "null =&gt; &apos;value&apos;" equates to &apos;s:0:"";s:5:"value";&apos;,<br>    "true =&gt; &apos;value&apos;" equates to &apos;i:1;s:5:"value";&apos;,<br>    "false =&gt; &apos;value&apos;" equates to &apos;i:0;s:5:"value";&apos;,<br>    "array(whatever the contents) =&gt; &apos;value&apos;" equates to an "illegal offset type" warning because you can&apos;t use an<br>    array as a key; however, if you use a variable containing an array as a key, it will equate to &apos;s:5:"Array";s:5:"value";&apos;,<br>     and<br>    attempting to use an object as a key will result in the same behavior as using an array will.<br>*/<br>?>
```
  

#

Please! please! please! DO NOT serialize data and place it into your database. Serialize can be used that way, but that&apos;s missing the point of a relational database and the datatypes inherent in your database engine. Doing this makes data in your database non-portable, difficult to read, and can complicate queries. If you want your application to be portable to other languages, like let&apos;s say you find that you want to use Java for some portion of your app that it makes sense to use Java in, serialization will become a pain in the buttocks. You should always be able to query and modify data in the database without using a third party intermediary tool to manipulate data to be inserted. <br><br>I&apos;ve encountered this too many times in my career, it makes for difficult to maintain code, code with portability issues, and data that is it more difficult to migrate to other RDMS systems, new schema, etc. It also has the added disadvantage of making it messy to search your database based on one of the fields that you&apos;ve serialized. <br><br>That&apos;s not to say serialize() is useless. It&apos;s not... A good place to use it may be a cache file that contains the result of a data intensive operation, for instance. There are tons of others... Just don&apos;t abuse serialize because the next guy who comes along will have a maintenance or migration nightmare.  

#

I did the same test as MiChAeLoKGB but in another version of PHP.<br>I don&apos;t post his code because I executed exactly the same script (after correcting a small variable naming error and add a line to display the PHP version).<br><br>PHP version: 7.1.9<br>PHP serialized in 0.0032186679840088 seconds average<br>JSON encoded in 0.0035939190387726 seconds average<br>serialize() was roughly 11.66% faster than json_encode()<br>Test took 6.8132710456848 seconds with 1000 iterations.<br><br>It would seem that in the current version of PHP the serialize function is faster.  

#

I did some testing to see the speed differences between serialize and json_encode, and my results with 250 iterations are:<br><br>PHP serialized in 0.0651714730263 seconds average<br>JSON encoded in 0.0254955434799 seconds average<br>json_encode() was roughly 155.62% faster than serialize()<br>Test took 27.2039430141 seconds with 300 iretations.<br><br>PHP serialized in 0.0564563179016 seconds average<br>JSON encoded in 0.0249140485128 seconds average<br>json_encode() was roughly 126.60% faster than serialize()<br>Test took 24.4148340225 seconds with 300 iretations.<br><br>From all my tests it looks like json_encode is on average about 120% faster (sometimes it gets to about 85% and sometimes to 150%).<br><br>Here is the PHP code you can run on your server to try it out:<br><br>

```
<?php

// fillArray function myde by Peter Bailey
function fillArray($depth, $max){
    static $seed;
    if (is_null($seed)){
        $seed = array(&apos;a&apos;, 2, &apos;c&apos;, 4, &apos;e&apos;, 6, &apos;g&apos;, 8, &apos;i&apos;, 10);
    }
    if ($depth &lt; $max){
        $node = array();
        foreach ($seed as $key){
            $node[$key] = fillArray( $depth + 1, $max );
        }
        return $node;
    }
    return &apos;empty&apos;;
}

function testSpeed($testArray, $iterations = 100){

    $json_time = array();
    $serialize_time = array();
    $test_start = microtime(true);

    for ($x = 1; $x &lt;= $iterations; $x++){
        $start = microtime(true);
        json_encode($testArray);
        $json_time[] = microtime(true) - $start;

        $start = microtime(true);
        serialize($testArray);
        $serialize_time[] = microtime(true) - $start;
    }

    $test_lenght = microtime(true) - $test_start;
    $json_average = array_sum($json_time) / count($json_time);
    $serialize_average = array_sum($serialize_time) / count($serialize_time);

    $result = "PHP serialized in ".$serialize_average." seconds average&lt;br&gt;";
    $result .= "JSON encoded in ".$json_average." seconds average&lt;br&gt;";

    if ($json_average &lt; $serialize_average){
        $result .= "json_encode() was roughly ".number_format( ($serialize_average / $json_average - 1 ) * 100, 2 )."% faster than serialize()&lt;br&gt;";
    } else if ( $serializeTime &lt; $jsonTime ){
        $result .= "serialize() was roughly ".number_format( ($json_average / $serialize_average - 1 ) * 100, 2 )."% faster than json_encode()&lt;br&gt;";
    } else {
        $result .= "No way!&lt;br&gt;";
    }

    $result .= "Test took ".$test_lenght." seconds with ".$iterations." iterations.";

    return $result;

}

// Change the number of iterations (250) to lower if you exceed your maximum execution time
echo testSpeed(fillArray(0, 5), 250);

?>
```
  

#

Serializing floating point numbers leads to weird precision offset errors:<br><br>

```
<?php

echo round(96.670000000000002, 2);
// 96.67

echo serialize(round(96.670000000000002, 2));
// d:96.670000000000002;

echo serialize(96.67);
// d:96.670000000000002;

?>
```
<br><br>Not only is this wrong, but it adds a lot of unnecessary bulk to serialized data. Probably better to use json_encode() instead (which apparently is faster than serialize(), anyway).  

#

If you are going to serialie an object which contains references to other objects you want to serialize some time later, these references will be lost when the object is unserialized.<br>The references can only be kept if all of your objects are serialized at once.<br>That means:<br><br>$a = new ClassA(); <br>$b = new ClassB($a); //$b containes a reference to $a;<br><br>$s1=serialize($a);<br>$s2=serialize($b);<br><br>$a=unserialize($s1);<br>$b=unserialize($s2);<br><br>now b references to an object of ClassA which is not $a. $a is another object of Class A.<br><br>use this:<br>$buf[0]=$a;<br>$buf[1]=$b;<br>$s=serialize($buf);<br>$buf=unserialize($s);<br>$a=$buf[0];<br>$b=$buf[1];<br><br>all references are intact.  

#

[Official documentation page](https://www.php.net/manual/en/function.serialize.php)

**[To root](/README.md)**