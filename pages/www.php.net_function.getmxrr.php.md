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
         $this->QNAME($domain);
         $this->pack_dns_packet();
         $dns_socket = fsockopen("udp://$dns", 53);

         fwrite($dns_socket,$this->dns_packet,strlen($this->dns_packet));
         $this->dns_reply  = fread($dns_socket,1);
         $bytes = stream_get_meta_data($dns_socket);
         $this->dns_reply .= fread($dns_socket,$bytes['unread_bytes']);
         fclose($dns_socket);
         $this->cIx=6;
         $this->ANCOUNT   = $this->gord(2);
         $this->cIx+=4;
         $this->parse_data($this->dns_repl_domain);
         $this->cIx+=7;

         for($ic=1;$ic&lt;=$this->ANCOUNT;$ic++)
         {
           $QTYPE = ord($this->gdi($this->cIx));
           if($QTYPE!==15){print("[MX Record not returned]"); die();}
           $this->cIx+=9;
           $mxPref = ord($this->gdi($this->cIx));
           $this->parse_data($curmx);
           $this->arrMX[] = array("MX_Pref" => $mxPref, "MX" => $curmx);
           $this->cIx+=3;
         }
      }

      function parse_data(&amp;$retval)
      {
        $arName = array();
        $byte = ord($this->gdi($this->cIx));
        while($byte!==0)
        {
          if($byte==192) //compressed
          {
            $tmpIx = $this->cIx;
            $this->cIx = ord($this->gdi($cIx));
            $tmpName = $retval;
            $this->parse_data($tmpName);
            $retval=$retval.".".$tmpName;
            $this->cIx = $tmpIx+1;
            return;
          }
          $retval="";
          $bCount = $byte;
          for($b=0;$b&lt;$bCount;$b++)
          {
            $retval .= $this->gdi($this->cIx);
          }
          $arName[]=$retval;
         $byte = ord($this->gdi($this->cIx));
       }
       $retval=join(".",$arName);
     }

     function gdi(&amp;$cIx,$bytes=1)
     {
       $this->cIx++;
       return(substr($this->dns_reply, $this->cIx-1, $bytes));
     }

      function QNAME($domain)
      {
        $dot_pos = 0; $temp = "";
        while($dot_pos=strpos($domain,"."))
        {
          $temp   = substr($domain,0,$dot_pos);
          $domain = substr($domain,$dot_pos+1);
          $this->QNAME .= chr(strlen($temp)).$temp;
        }
        $this->QNAME .= chr(strlen($domain)).$domain.chr(0);
      }

      function gord($ln=1)
      {
        $reply="";
        for($i=0;$i&lt;$ln;$i++){
         $reply.=ord(substr($this->dns_reply,$this->cIx,1));
         $this->cIx++;
         }

        return $reply;
      }

      function pack_dns_packet()
      {
        $this->dns_packet = chr(0).chr(1).
                            chr(1).chr(0).
                            chr(0).chr(1).
                            chr(0).chr(0).
                            chr(0).chr(0).
                            chr(0).chr(0).
                            $this->QNAME.
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

print $mx->ANCOUNT." MX Records\n";
print "Records returned for ".$mx->dns_repl_domain.":\n&lt;pre&gt;";
print_r($mx->arrMX);

?>
```
<br><br>Return:<br><br>02 MX Records Records returned for php.net:<br><br>Array<br>(<br>    [0] =&gt; Array<br>        (<br>            [MX_Pref] =&gt; 15<br>            [MX] =&gt; smtp.osuosl.org<br>        )<br><br>    [1] =&gt; Array<br>        (<br>            [MX_Pref] =&gt; 5<br>            [MX] =&gt; osu1.php.net<br>        )<br><br>)  

#

I tried using getmxrr() to validate the domain portion of email addresses in enquiry submission forms, and there is a curious effect with some top-level domains when checking non-existant domains.<br><br>With sdlkfjsdl.com, since the domain does not exist, getmxrr() returns false, as expected, and the returned mxhosts array is empty.<br><br>But with sdlkfjsdl.gov, getmxrr() returns true,  and the returned mxhosts array contains one element: NULL<br><br>With sdlkfjsdl.org, getmxrr() returns true,  and the returned mxhosts array contains one element: &apos;0.0.0.0&apos;<br><br>With sdlkfjsdl.co.uk, getmxrr()  returns true and supplies one MX record: uk-net-wildcard-null-mx.centralnic.net<br><br>So to validate the email domain, it would seem one has to check the returned mxhosts array to exclude the possibility of mxhosts being returned as NULL, 0.0.0.0 and wildcard ...  

#

[Official documentation page](https://www.php.net/manual/en/function.getmxrr.php)

**[To root](/README.md)**