# Runtime Configuration




<div class="phpcode"><span class="html">
Please note that these three ini settings will also affect the behaviour of your soap server (and clients as well) :<br><br>1. default_socket_timeout default 60 secs<br>Large or slow requests to your soap server or long processes at your soap server will return soap fault after 60 secs like : Error Fetching http headers.<br><br>2. max_execution_time default 30 secs<br>This can be the next bottleneck (but only when your default_socket_timeout is larger then this setting). Your soap server will not return anything, no faults no output, just an empty string.<br><br>3. memory_limit default 128M<br>Will throw fatal errors when the soap server script itself has low memory or will let your services return empty strings when the data it processes puts memory usage over this limit.<br><br>Other max POST settings luckily (but a bit suprisingly to me) have _no_ effect for your soap server. Those are : <br><br>max_input_time<br>max_input_nesting_level<br>max_input_vars<br>post_max_size<br>suhosin.post.max_array_depth<br>suhosin.post.max_array_index_length<br>suhosin.post.max_name_length<br>suhosin.post.max_totalname_length<br>suhosin.post.max_vars<br>suhosin.post.max_value_length</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/soap.configuration.php)

**[To root](/README.md)**