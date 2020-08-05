# xmlrpc_decode



Note that from libxml 2.7.9+ there is a limit of 10MB for the XML-RPC response.<br><br>If the response is larger, xmlrpc_decode will simply return NULL.<br><br>There is currently no way to override this limit like we can with the other xml functions (LIBXML_PARSEHUGE)  

---

[Official documentation page](https://www.php.net/manual/en/function.xmlrpc-decode.php)

**[To root](/README.md)**