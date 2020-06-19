# socket_last_error




<div class="phpcode"><span class="html">
This is a bit long, but personally I prefer to use the standard C defines in my code.
<br>
<br><span class="default">&lt;?php
<br>
<br>define</span><span class="keyword">(</span><span class="string">&apos;ENOTSOCK&apos;</span><span class="keyword">,&#xA0; &#xA0; &#xA0; </span><span class="default">88</span><span class="keyword">);&#xA0; &#xA0; </span><span class="comment">/* Socket operation on non-socket */
<br></span><span class="default">define</span><span class="keyword">(</span><span class="string">&apos;EDESTADDRREQ&apos;</span><span class="keyword">,&#xA0; </span><span class="default">89</span><span class="keyword">);&#xA0; &#xA0; </span><span class="comment">/* Destination address required */
<br></span><span class="default">define</span><span class="keyword">(</span><span class="string">&apos;EMSGSIZE&apos;</span><span class="keyword">,&#xA0; &#xA0; &#xA0; </span><span class="default">90</span><span class="keyword">);&#xA0; &#xA0; </span><span class="comment">/* Message too long */
<br></span><span class="default">define</span><span class="keyword">(</span><span class="string">&apos;EPROTOTYPE&apos;</span><span class="keyword">,&#xA0; &#xA0; </span><span class="default">91</span><span class="keyword">);&#xA0; &#xA0; </span><span class="comment">/* Protocol wrong type for socket */
<br></span><span class="default">define</span><span class="keyword">(</span><span class="string">&apos;ENOPROTOOPT&apos;</span><span class="keyword">,&#xA0;&#xA0; </span><span class="default">92</span><span class="keyword">);&#xA0; &#xA0; </span><span class="comment">/* Protocol not available */
<br></span><span class="default">define</span><span class="keyword">(</span><span class="string">&apos;EPROTONOSUPPORT&apos;</span><span class="keyword">, </span><span class="default">93</span><span class="keyword">);&#xA0; </span><span class="comment">/* Protocol not supported */
<br></span><span class="default">define</span><span class="keyword">(</span><span class="string">&apos;ESOCKTNOSUPPORT&apos;</span><span class="keyword">, </span><span class="default">94</span><span class="keyword">);&#xA0; </span><span class="comment">/* Socket type not supported */
<br></span><span class="default">define</span><span class="keyword">(</span><span class="string">&apos;EOPNOTSUPP&apos;</span><span class="keyword">,&#xA0; &#xA0; </span><span class="default">95</span><span class="keyword">);&#xA0; &#xA0; </span><span class="comment">/* Operation not supported on transport endpoint */
<br></span><span class="default">define</span><span class="keyword">(</span><span class="string">&apos;EPFNOSUPPORT&apos;</span><span class="keyword">,&#xA0; </span><span class="default">96</span><span class="keyword">);&#xA0; &#xA0; </span><span class="comment">/* Protocol family not supported */
<br></span><span class="default">define</span><span class="keyword">(</span><span class="string">&apos;EAFNOSUPPORT&apos;</span><span class="keyword">,&#xA0; </span><span class="default">97</span><span class="keyword">);&#xA0; &#xA0; </span><span class="comment">/* Address family not supported by protocol */
<br></span><span class="default">define</span><span class="keyword">(</span><span class="string">&apos;EADDRINUSE&apos;</span><span class="keyword">,&#xA0; &#xA0; </span><span class="default">98</span><span class="keyword">);&#xA0; &#xA0; </span><span class="comment">/* Address already in use */
<br></span><span class="default">define</span><span class="keyword">(</span><span class="string">&apos;EADDRNOTAVAIL&apos;</span><span class="keyword">, </span><span class="default">99</span><span class="keyword">);&#xA0; &#xA0; </span><span class="comment">/* Cannot assign requested address */
<br></span><span class="default">define</span><span class="keyword">(</span><span class="string">&apos;ENETDOWN&apos;</span><span class="keyword">,&#xA0; &#xA0; &#xA0; </span><span class="default">100</span><span class="keyword">);&#xA0;&#xA0; </span><span class="comment">/* Network is down */
<br></span><span class="default">define</span><span class="keyword">(</span><span class="string">&apos;ENETUNREACH&apos;</span><span class="keyword">,&#xA0;&#xA0; </span><span class="default">101</span><span class="keyword">);&#xA0;&#xA0; </span><span class="comment">/* Network is unreachable */
<br></span><span class="default">define</span><span class="keyword">(</span><span class="string">&apos;ENETRESET&apos;</span><span class="keyword">,&#xA0; &#xA0;&#xA0; </span><span class="default">102</span><span class="keyword">);&#xA0;&#xA0; </span><span class="comment">/* Network dropped connection because of reset */
<br></span><span class="default">define</span><span class="keyword">(</span><span class="string">&apos;ECONNABORTED&apos;</span><span class="keyword">,&#xA0; </span><span class="default">103</span><span class="keyword">);&#xA0;&#xA0; </span><span class="comment">/* Software caused connection abort */
<br></span><span class="default">define</span><span class="keyword">(</span><span class="string">&apos;ECONNRESET&apos;</span><span class="keyword">,&#xA0; &#xA0; </span><span class="default">104</span><span class="keyword">);&#xA0;&#xA0; </span><span class="comment">/* Connection reset by peer */
<br></span><span class="default">define</span><span class="keyword">(</span><span class="string">&apos;ENOBUFS&apos;</span><span class="keyword">,&#xA0; &#xA0; &#xA0;&#xA0; </span><span class="default">105</span><span class="keyword">);&#xA0;&#xA0; </span><span class="comment">/* No buffer space available */
<br></span><span class="default">define</span><span class="keyword">(</span><span class="string">&apos;EISCONN&apos;</span><span class="keyword">,&#xA0; &#xA0; &#xA0;&#xA0; </span><span class="default">106</span><span class="keyword">);&#xA0;&#xA0; </span><span class="comment">/* Transport endpoint is already connected */
<br></span><span class="default">define</span><span class="keyword">(</span><span class="string">&apos;ENOTCONN&apos;</span><span class="keyword">,&#xA0; &#xA0; &#xA0; </span><span class="default">107</span><span class="keyword">);&#xA0;&#xA0; </span><span class="comment">/* Transport endpoint is not connected */
<br></span><span class="default">define</span><span class="keyword">(</span><span class="string">&apos;ESHUTDOWN&apos;</span><span class="keyword">,&#xA0; &#xA0;&#xA0; </span><span class="default">108</span><span class="keyword">);&#xA0;&#xA0; </span><span class="comment">/* Cannot send after transport endpoint shutdown */
<br></span><span class="default">define</span><span class="keyword">(</span><span class="string">&apos;ETOOMANYREFS&apos;</span><span class="keyword">,&#xA0; </span><span class="default">109</span><span class="keyword">);&#xA0;&#xA0; </span><span class="comment">/* Too many references: cannot splice */
<br></span><span class="default">define</span><span class="keyword">(</span><span class="string">&apos;ETIMEDOUT&apos;</span><span class="keyword">,&#xA0; &#xA0;&#xA0; </span><span class="default">110</span><span class="keyword">);&#xA0;&#xA0; </span><span class="comment">/* Connection timed out */
<br></span><span class="default">define</span><span class="keyword">(</span><span class="string">&apos;ECONNREFUSED&apos;</span><span class="keyword">,&#xA0; </span><span class="default">111</span><span class="keyword">);&#xA0;&#xA0; </span><span class="comment">/* Connection refused */
<br></span><span class="default">define</span><span class="keyword">(</span><span class="string">&apos;EHOSTDOWN&apos;</span><span class="keyword">,&#xA0; &#xA0;&#xA0; </span><span class="default">112</span><span class="keyword">);&#xA0;&#xA0; </span><span class="comment">/* Host is down */
<br></span><span class="default">define</span><span class="keyword">(</span><span class="string">&apos;EHOSTUNREACH&apos;</span><span class="keyword">,&#xA0; </span><span class="default">113</span><span class="keyword">);&#xA0;&#xA0; </span><span class="comment">/* No route to host */
<br></span><span class="default">define</span><span class="keyword">(</span><span class="string">&apos;EALREADY&apos;</span><span class="keyword">,&#xA0; &#xA0; &#xA0; </span><span class="default">114</span><span class="keyword">);&#xA0;&#xA0; </span><span class="comment">/* Operation already in progress */
<br></span><span class="default">define</span><span class="keyword">(</span><span class="string">&apos;EINPROGRESS&apos;</span><span class="keyword">,&#xA0;&#xA0; </span><span class="default">115</span><span class="keyword">);&#xA0;&#xA0; </span><span class="comment">/* Operation now in progress */
<br></span><span class="default">define</span><span class="keyword">(</span><span class="string">&apos;EREMOTEIO&apos;</span><span class="keyword">,&#xA0; &#xA0;&#xA0; </span><span class="default">121</span><span class="keyword">);&#xA0;&#xA0; </span><span class="comment">/* Remote I/O error */
<br></span><span class="default">define</span><span class="keyword">(</span><span class="string">&apos;ECANCELED&apos;</span><span class="keyword">,&#xA0; &#xA0;&#xA0; </span><span class="default">125</span><span class="keyword">);&#xA0;&#xA0; </span><span class="comment">/* Operation Canceled */
<br></span><span class="default">?&gt;</span>
</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/function.socket-last-error.php)

**[To root](/README.md)**