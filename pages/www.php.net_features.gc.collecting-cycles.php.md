# Collecting Cycles





Memory leak: meaning you keep a reference to it thus preventing the GC from collecting it.

  

#



After testing, breaking up memory intensive code into a separate function allows the garbage collection to work.

For example the original code was like:-
while(true){
&#xA0;&#xA0; //do memory intensive code
}

can be turned into something like:-
function intensive($parameters){
&#xA0;&#xA0; //do memory intensive code
}

while(true){
&#xA0;&#xA0; intensive($parameters);
}

  

#



&#x2500;&#x2500; Unused Objects &#x2500;&#x2500;&#x2500; &#x2500; In use Objects
&#x2193;&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#x2193;&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; &#x2193;
 _____________________________________
 |&#x25A1;&#x25A1;&#x25A1;&#x25A1;&#x25A1;&#x25A1;&#x25A1;&#x25A1;&#x25A1;&#x25A1;&#x25A1;&#x25A1;&#x25A1;&#x25A1;&#x25A1;&#x25A1;&#x25A1;|&#x2588;&#x2588;&#x25A0;&#x25A0;&#x25A0;&#x25A0;&#x25A0;&#x25A0;&#x25A0;&#x25A0;&#x25A0;&#x25A0;&#x25A0;&#x25A0;&#x25A0;&#x25A0;&#x25A0;&#x25A0;|
 |&#x25A1;&#x25A1;&#x25A1;&#x25A1;&#x25A1;&#x25A1;&#x25A1;&#x25A1;&#x25A1;&#x25A1;&#x25A1;&#x25A1;&#x25A1;&#x25A1;&#x25A1;&#x25A1;&#x25A1;|&#x2588;&#x2588;&#x25A0;&#x25A0;&#x25A0;&#x25A0;&#x25A0;&#x25A0;&#x25A0;&#x25A0;&#x25A0;&#x25A0;&#x25A0;&#x25A0;&#x25A0;&#x25A0;&#x25A0;&#x25A0;|
&#xAF;&#xAF;&#xAF;&#xAF;&#xAF;&#xAF;&#xAF;&#xAF;&#xAF;&#xAF;&#xAF;&#xAF;&#xAF;&#xAF;&#xAF;&#xAF;&#xAF;&#xAF;&#xAF;&#xAF;&#xAF;&#xAF;&#xAF;&#xAF;&#xAF;&#xAF;&#xAF;&#xAF;&#xAF;&#xAF;&#xAF;&#xAF;&#xAF;&#xAF;&#xAF;&#xAF;&#xAF;
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#x25B2;&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#x25B2;
&#xA0; &#xA0;&#xA0; Unreferenced&#xA0; &#xA0; &#xA0; &#xA0; Referenced
&#xA0; &#xA0; &#xA0;&#xA0; Objects&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; Objects

&#x2588; Memory leak

  

#

[Official documentation page](https://www.php.net/manual/en/features.gc.collecting-cycles.php)

**[To root](/README.md)**