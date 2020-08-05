# Defining multiple namespaces in the same file



using of global namespaces and multiple namespaces in one PHP file increase the complexity and decrease readability of the code.<br>Let&apos;s try not use this scheme even it&apos;s very necessary (although there is not)  

---



```
<?php

// You cannot mix bracketed namespace declarations with unbracketed namespace declarations - will result in a Fatal error

namespace a;

echo "I belong to namespace a";

namespace b {
    echo "I'm from namespace b";
}?>
```
  

---



```
<?php
//Namespace can be used in this way also
namespace MyProject {

function connect() { echo "ONE";  }
    Sub\Level\connect();
}

namespace MyProject\Sub {
    
function connect() { echo "TWO";  }
    Level\connect();
}

namespace MyProject\Sub\Level {
    
    function connect() { echo "THREE";  }    
    \MyProject\Sub\Level\connect(); // OR we can use this as below
    connect();
}?>
```
  

---

[Official documentation page](https://www.php.net/manual/en/language.namespaces.definitionmultiple.php)

**[To root](/README.md)**