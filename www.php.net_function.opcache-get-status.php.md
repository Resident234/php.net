# opcache_get_status




<div class="phpcode"><span class="html">
Example output from this function:
<br>
<br>array(8) {
<br>&#xA0; [&quot;opcache_enabled&quot;]=&gt;
<br>&#xA0; bool(true)
<br>&#xA0; [&quot;cache_full&quot;]=&gt;
<br>&#xA0; bool(false)
<br>&#xA0; [&quot;restart_pending&quot;]=&gt;
<br>&#xA0; bool(false)
<br>&#xA0; [&quot;restart_in_progress&quot;]=&gt;
<br>&#xA0; bool(false)
<br>&#xA0; [&quot;memory_usage&quot;]=&gt;
<br>&#xA0; array(4) {
<br>&#xA0; &#xA0; [&quot;used_memory&quot;]=&gt;
<br>&#xA0; &#xA0; int(10936144)
<br>&#xA0; &#xA0; [&quot;free_memory&quot;]=&gt;
<br>&#xA0; &#xA0; int(123281584)
<br>&#xA0; &#xA0; [&quot;wasted_memory&quot;]=&gt;
<br>&#xA0; &#xA0; int(0)
<br>&#xA0; &#xA0; [&quot;current_wasted_percentage&quot;]=&gt;
<br>&#xA0; &#xA0; float(0)
<br>&#xA0; }
<br>&#xA0; [&quot;interned_strings_usage&quot;]=&gt;
<br>&#xA0; array(4) {
<br>&#xA0; &#xA0; [&quot;buffer_size&quot;]=&gt;
<br>&#xA0; &#xA0; int(8388608)
<br>&#xA0; &#xA0; [&quot;used_memory&quot;]=&gt;
<br>&#xA0; &#xA0; int(458480)
<br>&#xA0; &#xA0; [&quot;free_memory&quot;]=&gt;
<br>&#xA0; &#xA0; int(7930128)
<br>&#xA0; &#xA0; [&quot;number_of_strings&quot;]=&gt;
<br>&#xA0; &#xA0; int(5056)
<br>&#xA0; }
<br>&#xA0; [&quot;opcache_statistics&quot;]=&gt;
<br>&#xA0; array(13) {
<br>&#xA0; &#xA0; [&quot;num_cached_scripts&quot;]=&gt;
<br>&#xA0; &#xA0; int(1)
<br>&#xA0; &#xA0; [&quot;num_cached_keys&quot;]=&gt;
<br>&#xA0; &#xA0; int(2)
<br>&#xA0; &#xA0; [&quot;max_cached_keys&quot;]=&gt;
<br>&#xA0; &#xA0; int(7963)
<br>&#xA0; &#xA0; [&quot;hits&quot;]=&gt;
<br>&#xA0; &#xA0; int(0)
<br>&#xA0; &#xA0; [&quot;start_time&quot;]=&gt;
<br>&#xA0; &#xA0; int(1410858101)
<br>&#xA0; &#xA0; [&quot;last_restart_time&quot;]=&gt;
<br>&#xA0; &#xA0; int(0)
<br>&#xA0; &#xA0; [&quot;oom_restarts&quot;]=&gt;
<br>&#xA0; &#xA0; int(0)
<br>&#xA0; &#xA0; [&quot;hash_restarts&quot;]=&gt;
<br>&#xA0; &#xA0; int(0)
<br>&#xA0; &#xA0; [&quot;manual_restarts&quot;]=&gt;
<br>&#xA0; &#xA0; int(0)
<br>&#xA0; &#xA0; [&quot;misses&quot;]=&gt;
<br>&#xA0; &#xA0; int(1)
<br>&#xA0; &#xA0; [&quot;blacklist_misses&quot;]=&gt;
<br>&#xA0; &#xA0; int(0)
<br>&#xA0; &#xA0; [&quot;blacklist_miss_ratio&quot;]=&gt;
<br>&#xA0; &#xA0; float(0)
<br>&#xA0; &#xA0; [&quot;opcache_hit_rate&quot;]=&gt;
<br>&#xA0; &#xA0; float(0)
<br>&#xA0; }
<br>&#xA0; [&quot;scripts&quot;]=&gt;
<br>&#xA0; array(1) {
<br>&#xA0; &#xA0; [&quot;/var/www/opcache.php&quot;]=&gt;
<br>&#xA0; &#xA0; array(6) {
<br>&#xA0; &#xA0; &#xA0; [&quot;full_path&quot;]=&gt;
<br>&#xA0; &#xA0; &#xA0; string(17) &quot;/var/www/opcache.php&quot;
<br>&#xA0; &#xA0; &#xA0; [&quot;hits&quot;]=&gt;
<br>&#xA0; &#xA0; &#xA0; int(0)
<br>&#xA0; &#xA0; &#xA0; [&quot;memory_consumption&quot;]=&gt;
<br>&#xA0; &#xA0; &#xA0; int(1064)
<br>&#xA0; &#xA0; &#xA0; [&quot;last_used&quot;]=&gt;
<br>&#xA0; &#xA0; &#xA0; string(24) &quot;Tue Sep 16 09:01:41 2014&quot;
<br>&#xA0; &#xA0; &#xA0; [&quot;last_used_timestamp&quot;]=&gt;
<br>&#xA0; &#xA0; &#xA0; int(1410858101)
<br>&#xA0; &#xA0; &#xA0; [&quot;timestamp&quot;]=&gt;
<br>&#xA0; &#xA0; &#xA0; int(1410858099)
<br>&#xA0; &#xA0; }
<br>&#xA0; }
<br>}</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/function.opcache-get-status.php)

**[⬆ to root](/)**