# getmxrr



An other way to do mx-lookup on a windows platform.<br>Rewrote this from an other class i wrote for DNS lookup - so it might be a bit messy - but hope you get the idea.<br><br>Big thanks to the rfc community.<br><br>

```
<?php

class mxlookup
{
      var $dns_socket = NULL;
      var $QNAME = "";
      var $dns_packet= NULL;
      var $ANCOUNT = 0;
      var $cIx = 0;
      var $dns_repl_domain;
      var $arrMX = array();

      function mxlookup($domain, $dns="192.168.2.1")
      {
         $this-&gt;QNAME($domain);
         $this-&gt;pack_dns_packet();
         $dns_socket = fsockopen("udp://$dns", 53);

         fwrite($dns_socket,$this-&gt;dns_packet,strlen($this-&gt;dns_packet));
         $this-&gt;dns_reply  = fread($dns_socket,1);
         $bytes = stream_get_meta_data($dns_socket);
         $this-&gt;dns_reply .= fread($dns_socket,$bytes[&apos;unread_bytes&apos;]);
         fclose($dns_socket);
         $this-&gt;cIx=6;
         $this-&gt;ANCOUNT   = $this-&gt;gord(2);
         $this-&gt;cIx+=4;
         $this-&gt;parse_data($this-&gt;dns_repl_domain);
         $this-&gt;cIx+=7;

         for($ic=1;$ic&lt;=$this-&gt;ANCOUNT;$ic++)
         {
           $QTYPE = ord($this-&gt;gdi($this-&gt;cIx));
           if($QTYPE!==15){print("[MX Record not returned]"); die();}
           $this-&gt;cIx+=9;
           $mxPref = ord($this-&gt;gdi($this-&gt;cIx));
           $this-&gt;parse_data($curmx);
           $this-&gt;arrMX[] = array("MX_Pref" =&gt; $mxPref, "MX" =&gt; $curmx);
           $this-&gt;cIx+=3;
         }
      }

      function parse_data(&amp;$retval)
      {
        $arName = array();
        $byte = ord($this-&gt;gdi($this-&gt;cIx));
        while($byte!==0)
        {
          if($byte==192) //compressed
          {
            $tmpIx = $this-&gt;cIx;
            $this-&gt;cIx = ord($this-&gt;gdi($cIx));
            $tmpName = $retval;
            $this-&gt;parse_data($tmpName);
            $retval=$retval.".".$tmpName;
            $this-&gt;cIx = $tmpIx+1;
            return;
          }
          $retval="";
          $bCount = $byte;
          for($b=0;$b&lt;$bCount;$b++)
          {
            $retval .= $this-&gt;gdi($this-&gt;cIx);
          }
          $arName[]=$retval;
         $byte = ord($this-&gt;gdi($this-&gt;cIx));
       }
       $retval=join(".",$arName);
     }

     function gdi(&amp;$cIx,$bytes=1)
     {
       $this-&gt;cIx++;
       return(substr($this-&gt;dns_reply, $this-&gt;cIx-1, $bytes));
     }

      function QNAME($domain)
      {
        $dot_pos = 0; $temp = "";
        while($dot_pos=strpos($domain,"."))
        {
          $temp   = substr($domain,0,$dot_pos);
          $domain = substr($domain,$dot_pos+1);
          $this-&gt;QNAME .= chr(strlen($temp)).$temp;
        }
        $this-&gt;QNAME .= chr(strlen($domain)).$domain.chr(0);
      }

      function gord($ln=1)
      {
        $reply="";
        for($i=0;$i&lt;$ln;$i++){
         $reply.=ord(substr($this-&gt;dns_reply,$this-&gt;cIx,1));
         $this-&gt;cIx++;
         }

        return $reply;
      }

      function pack_dns_packet()
      {
        $this-&gt;dns_packet = chr(0).chr(1).
                            chr(1).chr(0).
                            chr(0).chr(1).
                            chr(0).chr(0).
                            chr(0).chr(0).
                            chr(0).chr(0).
                            $this-&gt;QNAME.
                            chr(0).chr(15).
                            chr(0).chr(1);
      }

}

?>
```




```
<?php

/* Exampe of use: */
$mx = new mxlookup("php.net");

print $mx-&gt;ANCOUNT." MX Records\n";
print "Records returned for ".$mx-&gt;dns_repl_domain.":\n&lt;pre&gt;";
print_r($mx-&gt;arrMX);

?>
```
<br><br>Return:<br><br>02 MX Records Records returned for php.net:<br><br>Array<br>(<br>    [0] =&gt; Array<br>        (<br>            [MX_Pref] =&gt; 15<br>            [MX] =&gt; smtp.osuosl.org<br>        )<br><br>    [1] =&gt; Array<br>        (<br>            [MX_Pref] =&gt; 5<br>            [MX] =&gt; osu1.php.net<br>        )<br><br>)  

#

I tried using getmxrr() to validate the domain portion of email addresses in enquiry submission forms, and there is a curious effect with some top-level domains when checking non-existant domains.<br><br>With sdlkfjsdl.com, since the domain does not exist, getmxrr() returns false, as expected, and the returned mxhosts array is empty.<br><br>But with sdlkfjsdl.gov, getmxrr() returns true,  and the returned mxhosts array contains one element: NULL<br><br>With sdlkfjsdl.org, getmxrr() returns true,  and the returned mxhosts array contains one element: &apos;0.0.0.0&apos;<br><br>With sdlkfjsdl.co.uk, getmxrr()  returns true and supplies one MX record: uk-net-wildcard-null-mx.centralnic.net<br><br>So to validate the email domain, it would seem one has to check the returned mxhosts array to exclude the possibility of mxhosts being returned as NULL, 0.0.0.0 and wildcard ...  

#

[Official documentation page](https://www.php.net/manual/en/function.getmxrr.php)

**[To root](/README.md)**