# get_browser



As of PHP 7.0.15 and 7.1.1 and higher, get_browser() now performs much better - reportedly 100x faster.  The Changelog, bug description, and solution are here:<br><br>http://php.net/ChangeLog-7.php (search for get_browser())<br>https://bugs.php.net/bug.php?id=70490<br>https://github.com/php/php-src/pull/2242  

#

This function is too slow for todays needs.<br><br>If you need browser / device / operating system detection, please try one of listed packages here: https://github.com/ThaDafinser/UserAgentParser  

#

Follow up to Francesco R&apos;s post from 2016.<br><br>His function works for most human traffic; added a few lines to cover the most common bot traffic. Also fixed issue with function failing to detect strings at position 0 due to strpos behavior.<br><br>

```
<?php
// Function written and tested December, 2018
function get_browser_name($user_agent)
{
        // Make case insensitive.
        $t = strtolower($user_agent);

        // If the string *starts* with the string, strpos returns 0 (i.e., FALSE). Do a ghetto hack and start with a space.
        // "[strpos()] may return Boolean FALSE, but may also return a non-Boolean value which evaluates to FALSE."
        //     http://php.net/manual/en/function.strpos.php
        $t = " " . $t;

        // Humans / Regular Users      
        if     (strpos($t, &apos;opera&apos;     ) || strpos($t, &apos;opr/&apos;)     ) return &apos;Opera&apos;            ;
        elseif (strpos($t, &apos;edge&apos;      )                           ) return &apos;Edge&apos;             ;
        elseif (strpos($t, &apos;chrome&apos;    )                           ) return &apos;Chrome&apos;           ;
        elseif (strpos($t, &apos;safari&apos;    )                           ) return &apos;Safari&apos;           ;
        elseif (strpos($t, &apos;firefox&apos;   )                           ) return &apos;Firefox&apos;          ;
        elseif (strpos($t, &apos;msie&apos;      ) || strpos($t, &apos;trident/7&apos;)) return &apos;Internet Explorer&apos;;

        // Search Engines  
        elseif (strpos($t, &apos;google&apos;    )                           ) return &apos;[Bot] Googlebot&apos;   ;
        elseif (strpos($t, &apos;bing&apos;      )                           ) return &apos;[Bot] Bingbot&apos;     ;
        elseif (strpos($t, &apos;slurp&apos;     )                           ) return &apos;[Bot] Yahoo! Slurp&apos;;
        elseif (strpos($t, &apos;duckduckgo&apos;)                           ) return &apos;[Bot] DuckDuckBot&apos; ;
        elseif (strpos($t, &apos;baidu&apos;     )                           ) return &apos;[Bot] Baidu&apos;       ;
        elseif (strpos($t, &apos;yandex&apos;    )                           ) return &apos;[Bot] Yandex&apos;      ;
        elseif (strpos($t, &apos;sogou&apos;     )                           ) return &apos;[Bot] Sogou&apos;       ;
        elseif (strpos($t, &apos;exabot&apos;    )                           ) return &apos;[Bot] Exabot&apos;      ;
        elseif (strpos($t, &apos;msn&apos;       )                           ) return &apos;[Bot] MSN&apos;         ;

        // Common Tools and Bots
        elseif (strpos($t, &apos;mj12bot&apos;   )                           ) return &apos;[Bot] Majestic&apos;     ;
        elseif (strpos($t, &apos;ahrefs&apos;    )                           ) return &apos;[Bot] Ahrefs&apos;       ;
        elseif (strpos($t, &apos;semrush&apos;   )                           ) return &apos;[Bot] SEMRush&apos;      ;
        elseif (strpos($t, &apos;rogerbot&apos;  ) || strpos($t, &apos;dotbot&apos;)   ) return &apos;[Bot] Moz or OpenSiteExplorer&apos;;
        elseif (strpos($t, &apos;frog&apos;      ) || strpos($t, &apos;screaming&apos;)) return &apos;[Bot] Screaming Frog&apos;;
        
        // Miscellaneous 
        elseif (strpos($t, &apos;facebook&apos;  )                           ) return &apos;[Bot] Facebook&apos;     ;
        elseif (strpos($t, &apos;pinterest&apos; )                           ) return &apos;[Bot] Pinterest&apos;    ;
        
        // Check for strings commonly used in bot user agents   
        elseif (strpos($t, &apos;crawler&apos; ) || strpos($t, &apos;api&apos;    ) ||
                strpos($t, &apos;spider&apos;  ) || strpos($t, &apos;http&apos;   ) ||
                strpos($t, &apos;bot&apos;     ) || strpos($t, &apos;archive&apos;) || 
                strpos($t, &apos;info&apos;    ) || strpos($t, &apos;data&apos;   )    ) return &apos;[Bot] Other&apos;   ;
        
        return &apos;Other (Unknown)&apos;;
}
?>
```
<br>Post with more depth here:<br>https://www.256kilobytes.com/content/show/1922/how-to-parse-a-user-agent-in-php-with-minimal-effort  

#

Since browser detection can be tricky and very slow, I compared a few packages.<br><br>http://thadafinser.github.io/UserAgentParserComparison/v5/index.html<br><br>https://github.com/sinergi/php-browser-detector<br>https://github.com/WhichBrowser/Parser-PHP<br>https://github.com/piwik/device-detector<br>http://php.net/manual/en/function.get-browser.php<br><br>Here are the results:<br><br>User Agent: <br>Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/63.0.3239.84 Safari/537.36<br><br>Sinergi Package<br>---------------<br>Chrome 63.0.3239.84 on Windows 10.0<br>Took 0.0022480487823486 seconds.<br>---------------<br><br>WhichBrowser Package<br>---------------<br>Chrome 63 on Windows 10<br>Took 0.021045207977295 seconds.<br>---------------<br><br>Piwik Package<br>---------------<br>Chrome 63.0 on Windows 10<br>Took 0.079447031021118 seconds.<br>---------------<br><br>get_browser Package<br>---------------<br>Chrome 63.0 on Windows 10<br>Took 0.09611701965332 seconds.<br>---------------<br><br>User Agent: <br>Mozilla/5.0 (Windows NT 10.0; Win64; x64; rv:57.0) Gecko/20100101 Firefox/57.0<br><br>Sinergi Package<br>---------------<br>Firefox 57.0 on Windows 10.0<br>Took 0.0023159980773926 seconds.<br>---------------<br><br>WhichBrowser Package<br>---------------<br>Firefox 57.0 on Windows 10<br>Took 0.019663095474243 seconds.<br>---------------<br><br>Piwik Package<br>---------------<br>Firefox 57.0 on Windows 10<br>Took 0.079678058624268 seconds.<br>---------------<br><br>get_browser Package<br>---------------<br>Firefox 57.0 on Windows 10<br>Took 0.02236008644104 seconds.<br>---------------<br><br>The consistent winner (by speed, not necessarily coverage) by far is:<br>https://github.com/sinergi/php-browser-detector  

#

To my surprise I found that none of the get_browser alternatives output the correct name / version combination that I was looking for using Opera or Chrome. They either give the wrong name eg Safari when in fact it should be Chrome and if the ua string includes a version number as with the latest versions of Chrome and Opera the wrong number is reported. So I took bits and pieces from the various examples and combined them and added a check for version. <br><br>

```
<?php
function getBrowser() 
{ 
    $u_agent = $_SERVER[&apos;HTTP_USER_AGENT&apos;]; 
    $bname = &apos;Unknown&apos;;
    $platform = &apos;Unknown&apos;;
    $version= "";

    //First get the platform?
    if (preg_match(&apos;/linux/i&apos;, $u_agent)) {
        $platform = &apos;linux&apos;;
    }
    elseif (preg_match(&apos;/macintosh|mac os x/i&apos;, $u_agent)) {
        $platform = &apos;mac&apos;;
    }
    elseif (preg_match(&apos;/windows|win32/i&apos;, $u_agent)) {
        $platform = &apos;windows&apos;;
    }
    
    // Next get the name of the useragent yes seperately and for good reason
    if(preg_match(&apos;/MSIE/i&apos;,$u_agent) &amp;&amp; !preg_match(&apos;/Opera/i&apos;,$u_agent)) 
    { 
        $bname = &apos;Internet Explorer&apos;; 
        $ub = "MSIE"; 
    } 
    elseif(preg_match(&apos;/Firefox/i&apos;,$u_agent)) 
    { 
        $bname = &apos;Mozilla Firefox&apos;; 
        $ub = "Firefox"; 
    } 
    elseif(preg_match(&apos;/Chrome/i&apos;,$u_agent)) 
    { 
        $bname = &apos;Google Chrome&apos;; 
        $ub = "Chrome"; 
    } 
    elseif(preg_match(&apos;/Safari/i&apos;,$u_agent)) 
    { 
        $bname = &apos;Apple Safari&apos;; 
        $ub = "Safari"; 
    } 
    elseif(preg_match(&apos;/Opera/i&apos;,$u_agent)) 
    { 
        $bname = &apos;Opera&apos;; 
        $ub = "Opera"; 
    } 
    elseif(preg_match(&apos;/Netscape/i&apos;,$u_agent)) 
    { 
        $bname = &apos;Netscape&apos;; 
        $ub = "Netscape"; 
    } 
    
    // finally get the correct version number
    $known = array(&apos;Version&apos;, $ub, &apos;other&apos;);
    $pattern = &apos;#(?&lt;browser&gt;&apos; . join(&apos;|&apos;, $known) .
    &apos;)[/ ]+(?&lt;version&gt;[0-9.|a-zA-Z.]*)#&apos;;
    if (!preg_match_all($pattern, $u_agent, $matches)) {
        // we have no matching number just continue
    }
    
    // see how many we have
    $i = count($matches[&apos;browser&apos;]);
    if ($i != 1) {
        //we will have two since we are not using &apos;other&apos; argument yet
        //see if version is before or after the name
        if (strripos($u_agent,"Version") &lt; strripos($u_agent,$ub)){
            $version= $matches[&apos;version&apos;][0];
        }
        else {
            $version= $matches[&apos;version&apos;][1];
        }
    }
    else {
        $version= $matches[&apos;version&apos;][0];
    }
    
    // check if we have a number
    if ($version==null || $version=="") {$version="?";}
    
    return array(
        &apos;userAgent&apos; =&gt; $u_agent,
        &apos;name&apos;      =&gt; $bname,
        &apos;version&apos;   =&gt; $version,
        &apos;platform&apos;  =&gt; $platform,
        &apos;pattern&apos;    =&gt; $pattern
    );
} 

// now try it
$ua=getBrowser();
$yourbrowser= "Your browser: " . $ua[&apos;name&apos;] . " " . $ua[&apos;version&apos;] . " on " .$ua[&apos;platform&apos;] . " reports: &lt;br &gt;" . $ua[&apos;userAgent&apos;];
print_r($yourbrowser);
?>
```
  

#

If you ONLY need a very fast and simple function to detect the browser name (update to May 2016):<br><br>

```
<?php

function get_browser_name($user_agent)
{
    if (strpos($user_agent, &apos;Opera&apos;) || strpos($user_agent, &apos;OPR/&apos;)) return &apos;Opera&apos;;
    elseif (strpos($user_agent, &apos;Edge&apos;)) return &apos;Edge&apos;;
    elseif (strpos($user_agent, &apos;Chrome&apos;)) return &apos;Chrome&apos;;
    elseif (strpos($user_agent, &apos;Safari&apos;)) return &apos;Safari&apos;;
    elseif (strpos($user_agent, &apos;Firefox&apos;)) return &apos;Firefox&apos;;
    elseif (strpos($user_agent, &apos;MSIE&apos;) || strpos($user_agent, &apos;Trident/7&apos;)) return &apos;Internet Explorer&apos;;
    
    return &apos;Other&apos;;
}

// Usage:

echo get_browser_name($_SERVER[&apos;HTTP_USER_AGENT&apos;]);

?>
```
<br><br>This function also resolves the trouble with Edge (that contains in the user agent the string "Safari" and "Chrome"), with Chrome (contains the string "Safari") and IE11 (that do not contains &apos;MSIE&apos; like all other IE versions).<br><br>Note that "strpos" is the fastest function to check a string (far better than "preg_match") and Opera + Edge + Chrome + Safari + Firefox + Internet Explorer are the most used browsers today (over 97%).  

#

Good news! The latest version of PHP has a performance fix for this function. It&apos;s reportedly now 100x faster. See the ChangeLog for specifics.  

#

[Official documentation page](https://www.php.net/manual/en/function.get-browser.php)

**[To root](/README.md)**