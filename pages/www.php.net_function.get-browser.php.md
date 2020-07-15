# get_browser



As of PHP 7.0.15 and 7.1.1 and higher, get_browser() now performs much better - reportedly 100x faster.  The Changelog, bug description, and solution are here:<br><br>http://php.net/ChangeLog-7.php (search for get_browser())<br>https://bugs.php.net/bug.php?id=70490<br>https://github.com/php/?>
```
src/pull/2242  

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
        if     (strpos($t, 'opera'     ) || strpos($t, 'opr/')     ) return 'Opera'            ;
        elseif (strpos($t, 'edge'      )                           ) return 'Edge'             ;
        elseif (strpos($t, 'chrome'    )                           ) return 'Chrome'           ;
        elseif (strpos($t, 'safari'    )                           ) return 'Safari'           ;
        elseif (strpos($t, 'firefox'   )                           ) return 'Firefox'          ;
        elseif (strpos($t, 'msie'      ) || strpos($t, 'trident/7')) return 'Internet Explorer';

        // Search Engines  
        elseif (strpos($t, 'google'    )                           ) return '[Bot] Googlebot'   ;
        elseif (strpos($t, 'bing'      )                           ) return '[Bot] Bingbot'     ;
        elseif (strpos($t, 'slurp'     )                           ) return '[Bot] Yahoo! Slurp';
        elseif (strpos($t, 'duckduckgo')                           ) return '[Bot] DuckDuckBot' ;
        elseif (strpos($t, 'baidu'     )                           ) return '[Bot] Baidu'       ;
        elseif (strpos($t, 'yandex'    )                           ) return '[Bot] Yandex'      ;
        elseif (strpos($t, 'sogou'     )                           ) return '[Bot] Sogou'       ;
        elseif (strpos($t, 'exabot'    )                           ) return '[Bot] Exabot'      ;
        elseif (strpos($t, 'msn'       )                           ) return '[Bot] MSN'         ;

        // Common Tools and Bots
        elseif (strpos($t, 'mj12bot'   )                           ) return '[Bot] Majestic'     ;
        elseif (strpos($t, 'ahrefs'    )                           ) return '[Bot] Ahrefs'       ;
        elseif (strpos($t, 'semrush'   )                           ) return '[Bot] SEMRush'      ;
        elseif (strpos($t, 'rogerbot'  ) || strpos($t, 'dotbot')   ) return '[Bot] Moz or OpenSiteExplorer';
        elseif (strpos($t, 'frog'      ) || strpos($t, 'screaming')) return '[Bot] Screaming Frog';
        
        // Miscellaneous 
        elseif (strpos($t, 'facebook'  )                           ) return '[Bot] Facebook'     ;
        elseif (strpos($t, 'pinterest' )                           ) return '[Bot] Pinterest'    ;
        
        // Check for strings commonly used in bot user agents   
        elseif (strpos($t, 'crawler' ) || strpos($t, 'api'    ) ||
                strpos($t, 'spider'  ) || strpos($t, 'http'   ) ||
                strpos($t, 'bot'     ) || strpos($t, 'archive') || 
                strpos($t, 'info'    ) || strpos($t, 'data'   )    ) return '[Bot] Other'   ;
        
        return 'Other (Unknown)';
}
?>
```

Post with more depth here:
https://www.256kilobytes.com/content/show/1922/how-to-parse-a-user-agent-in

```
<??>
```
with-minimal-effort  

#

Since browser detection can be tricky and very slow, I compared a few packages.<br><br>http://thadafinser.github.io/UserAgentParserComparison/v5/index.html<br><br>https://github.com/sinergi/?>
```
browser-detector<br>https://github.com/WhichBrowser/Parser-PHP<br>https://github.com/piwik/device-detector<br>http://php.net/manual/en/function.get-browser.php<br><br>Here are the results:<br><br>User Agent: <br>Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/63.0.3239.84 Safari/537.36<br><br>Sinergi Package<br>---------------<br>Chrome 63.0.3239.84 on Windows 10.0<br>Took 0.0022480487823486 seconds.<br>---------------<br><br>WhichBrowser Package<br>---------------<br>Chrome 63 on Windows 10<br>Took 0.021045207977295 seconds.<br>---------------<br><br>Piwik Package<br>---------------<br>Chrome 63.0 on Windows 10<br>Took 0.079447031021118 seconds.<br>---------------<br><br>get_browser Package<br>---------------<br>Chrome 63.0 on Windows 10<br>Took 0.09611701965332 seconds.<br>---------------<br><br>User Agent: <br>Mozilla/5.0 (Windows NT 10.0; Win64; x64; rv:57.0) Gecko/20100101 Firefox/57.0<br><br>Sinergi Package<br>---------------<br>Firefox 57.0 on Windows 10.0<br>Took 0.0023159980773926 seconds.<br>---------------<br><br>WhichBrowser Package<br>---------------<br>Firefox 57.0 on Windows 10<br>Took 0.019663095474243 seconds.<br>---------------<br><br>Piwik Package<br>---------------<br>Firefox 57.0 on Windows 10<br>Took 0.079678058624268 seconds.<br>---------------<br><br>get_browser Package<br>---------------<br>Firefox 57.0 on Windows 10<br>Took 0.02236008644104 seconds.<br>---------------<br><br>The consistent winner (by speed, not necessarily coverage) by far is:<br>https://github.com/sinergi/?>
```
browser-detector  

#

To my surprise I found that none of the get_browser alternatives output the correct name / version combination that I was looking for using Opera or Chrome. They either give the wrong name eg Safari when in fact it should be Chrome and if the ua string includes a version number as with the latest versions of Chrome and Opera the wrong number is reported. So I took bits and pieces from the various examples and combined them and added a check for version. <br><br>

```
<?php
function getBrowser() 
{ 
    $u_agent = $_SERVER['HTTP_USER_AGENT']; 
    $bname = 'Unknown';
    $platform = 'Unknown';
    $version= "";

    //First get the platform?
    if (preg_match('/linux/i', $u_agent)) {
        $platform = 'linux';
    }
    elseif (preg_match('/macintosh|mac os x/i', $u_agent)) {
        $platform = 'mac';
    }
    elseif (preg_match('/windows|win32/i', $u_agent)) {
        $platform = 'windows';
    }
    
    // Next get the name of the useragent yes seperately and for good reason
    if(preg_match('/MSIE/i',$u_agent) &amp;&amp; !preg_match('/Opera/i',$u_agent)) 
    { 
        $bname = 'Internet Explorer'; 
        $ub = "MSIE"; 
    } 
    elseif(preg_match('/Firefox/i',$u_agent)) 
    { 
        $bname = 'Mozilla Firefox'; 
        $ub = "Firefox"; 
    } 
    elseif(preg_match('/Chrome/i',$u_agent)) 
    { 
        $bname = 'Google Chrome'; 
        $ub = "Chrome"; 
    } 
    elseif(preg_match('/Safari/i',$u_agent)) 
    { 
        $bname = 'Apple Safari'; 
        $ub = "Safari"; 
    } 
    elseif(preg_match('/Opera/i',$u_agent)) 
    { 
        $bname = 'Opera'; 
        $ub = "Opera"; 
    } 
    elseif(preg_match('/Netscape/i',$u_agent)) 
    { 
        $bname = 'Netscape'; 
        $ub = "Netscape"; 
    } 
    
    // finally get the correct version number
    $known = array('Version', $ub, 'other');
    $pattern = '#(?<browser>' . join('|', $known) .
    ')[/ ]+(?<version>[0-9.|a-zA-Z.]*)#';
    if (!preg_match_all($pattern, $u_agent, $matches)) {
        // we have no matching number just continue
    }
    
    // see how many we have
    $i = count($matches['browser']);
    if ($i != 1) {
        //we will have two since we are not using 'other' argument yet
        //see if version is before or after the name
        if (strripos($u_agent,"Version") < strripos($u_agent,$ub)){
            $version= $matches['version'][0];
        }
        else {
            $version= $matches['version'][1];
        }
    }
    else {
        $version= $matches['version'][0];
    }
    
    // check if we have a number
    if ($version==null || $version=="") {$version="?";}
    
    return array(
        'userAgent' => $u_agent,
        'name'      => $bname,
        'version'   => $version,
        'platform'  => $platform,
        'pattern'    => $pattern
    );
} 

// now try it
$ua=getBrowser();
$yourbrowser= "Your browser: " . $ua['name'] . " " . $ua['version'] . " on " .$ua['platform'] . " reports: <br >" . $ua['userAgent'];
print_r($yourbrowser);
?>
```
  

#

If you ONLY need a very fast and simple function to detect the browser name (update to May 2016):<br><br>

```
<?php

function get_browser_name($user_agent)
{
    if (strpos($user_agent, 'Opera') || strpos($user_agent, 'OPR/')) return 'Opera';
    elseif (strpos($user_agent, 'Edge')) return 'Edge';
    elseif (strpos($user_agent, 'Chrome')) return 'Chrome';
    elseif (strpos($user_agent, 'Safari')) return 'Safari';
    elseif (strpos($user_agent, 'Firefox')) return 'Firefox';
    elseif (strpos($user_agent, 'MSIE') || strpos($user_agent, 'Trident/7')) return 'Internet Explorer';
    
    return 'Other';
}

// Usage:

echo get_browser_name($_SERVER['HTTP_USER_AGENT']);

?>
```
<br><br>This function also resolves the trouble with Edge (that contains in the user agent the string "Safari" and "Chrome"), with Chrome (contains the string "Safari") and IE11 (that do not contains &apos;MSIE&apos; like all other IE versions).<br><br>Note that "strpos" is the fastest function to check a string (far better than "preg_match") and Opera + Edge + Chrome + Safari + Firefox + Internet Explorer are the most used browsers today (over 97%).  

#

Good news! The latest version of PHP has a performance fix for this function. It&apos;s reportedly now 100x faster. See the ChangeLog for specifics.  

#

[Official documentation page](https://www.php.net/manual/en/function.get-browser.php)

**[To root](/README.md)**