# Defining multiple namespaces in the same file





using of global namespaces and multiple namespaces in one PHP file increase the complexity and decrease readability of the code.
Let&apos;s try not use this scheme even it&apos;s very necessary (although there is not)

  

#





```
<?php

// You cannot mix bracketed namespace declarations with unbracketed namespace declarations - will result in a Fatal error

namespace a;

echo &quot;I belong to namespace a&quot;;

namespace b {
&#xA0; &#xA0; echo &quot;I&apos;m from namespace b&quot;;
}


  

#





```
<?php
//Namespace can be used in this way also
namespace MyProject {

function connect() { echo &quot;ONE&quot;;&#xA0; }
&#xA0; &#xA0; Sub\Level\connect();
}

namespace MyProject\Sub {
&#xA0; &#xA0; 
function connect() { echo &quot;TWO&quot;;&#xA0; }
&#xA0; &#xA0; Level\connect();
}

namespace MyProject\Sub\Level {
&#xA0; &#xA0; 
&#xA0; &#xA0; function connect() { echo &quot;THREE&quot;;&#xA0; }&#xA0; &#xA0; 
&#xA0; &#xA0; \MyProject\Sub\Level\connect(); // OR we can use this as below
&#xA0; &#xA0; connect();
}


  

#

[Official documentation page](https://www.php.net/manual/en/language.namespaces.definitionmultiple.php)

**[To root](/README.md)**