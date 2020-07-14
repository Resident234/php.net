# Collecting Cycles



Memory leak: meaning you keep a reference to it thus preventing the GC from collecting it.  

#

After testing, breaking up memory intensive code into a separate function allows the garbage collection to work.<br><br>For example the original code was like:-<br>while(true){<br>   //do memory intensive code<br>}<br><br>can be turned into something like:-<br>function intensive($parameters){<br>   //do memory intensive code<br>}<br><br>while(true){<br>   intensive($parameters);<br>}  

#

&#x2500;&#x2500; Unused Objects &#x2500;&#x2500;&#x2500; &#x2500; In use Objects<br>&#x2193;                    &#x2193;               &#x2193;<br> _____________________________________<br> |&#x25A1;&#x25A1;&#x25A1;&#x25A1;&#x25A1;&#x25A1;&#x25A1;&#x25A1;&#x25A1;&#x25A1;&#x25A1;&#x25A1;&#x25A1;&#x25A1;&#x25A1;&#x25A1;&#x25A1;|&#x2588;&#x2588;&#x25A0;&#x25A0;&#x25A0;&#x25A0;&#x25A0;&#x25A0;&#x25A0;&#x25A0;&#x25A0;&#x25A0;&#x25A0;&#x25A0;&#x25A0;&#x25A0;&#x25A0;&#x25A0;|<br> |&#x25A1;&#x25A1;&#x25A1;&#x25A1;&#x25A1;&#x25A1;&#x25A1;&#x25A1;&#x25A1;&#x25A1;&#x25A1;&#x25A1;&#x25A1;&#x25A1;&#x25A1;&#x25A1;&#x25A1;|&#x2588;&#x2588;&#x25A0;&#x25A0;&#x25A0;&#x25A0;&#x25A0;&#x25A0;&#x25A0;&#x25A0;&#x25A0;&#x25A0;&#x25A0;&#x25A0;&#x25A0;&#x25A0;&#x25A0;&#x25A0;|<br>&#xAF;&#xAF;&#xAF;&#xAF;&#xAF;&#xAF;&#xAF;&#xAF;&#xAF;&#xAF;&#xAF;&#xAF;&#xAF;&#xAF;&#xAF;&#xAF;&#xAF;&#xAF;&#xAF;&#xAF;&#xAF;&#xAF;&#xAF;&#xAF;&#xAF;&#xAF;&#xAF;&#xAF;&#xAF;&#xAF;&#xAF;&#xAF;&#xAF;&#xAF;&#xAF;&#xAF;&#xAF;<br>          &#x25B2;                  &#x25B2;<br>     Unreferenced        Referenced<br>       Objects             Objects<br><br>&#x2588; Memory leak  

#

[Official documentation page](https://www.php.net/manual/en/features.gc.collecting-cycles.php)

**[To root](/README.md)**