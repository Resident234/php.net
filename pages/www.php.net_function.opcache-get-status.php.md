# opcache_get_status





Example output from this function:



array(8) {

&#xA0; [&quot;opcache_enabled&quot;]=&gt;

&#xA0; bool(true)

&#xA0; [&quot;cache_full&quot;]=&gt;

&#xA0; bool(false)

&#xA0; [&quot;restart_pending&quot;]=&gt;

&#xA0; bool(false)

&#xA0; [&quot;restart_in_progress&quot;]=&gt;

&#xA0; bool(false)

&#xA0; [&quot;memory_usage&quot;]=&gt;

&#xA0; array(4) {

&#xA0; &#xA0; [&quot;used_memory&quot;]=&gt;

&#xA0; &#xA0; int(10936144)

&#xA0; &#xA0; [&quot;free_memory&quot;]=&gt;

&#xA0; &#xA0; int(123281584)

&#xA0; &#xA0; [&quot;wasted_memory&quot;]=&gt;

&#xA0; &#xA0; int(0)

&#xA0; &#xA0; [&quot;current_wasted_percentage&quot;]=&gt;

&#xA0; &#xA0; float(0)

&#xA0; }

&#xA0; [&quot;interned_strings_usage&quot;]=&gt;

&#xA0; array(4) {

&#xA0; &#xA0; [&quot;buffer_size&quot;]=&gt;

&#xA0; &#xA0; int(8388608)

&#xA0; &#xA0; [&quot;used_memory&quot;]=&gt;

&#xA0; &#xA0; int(458480)

&#xA0; &#xA0; [&quot;free_memory&quot;]=&gt;

&#xA0; &#xA0; int(7930128)

&#xA0; &#xA0; [&quot;number_of_strings&quot;]=&gt;

&#xA0; &#xA0; int(5056)

&#xA0; }

&#xA0; [&quot;opcache_statistics&quot;]=&gt;

&#xA0; array(13) {

&#xA0; &#xA0; [&quot;num_cached_scripts&quot;]=&gt;

&#xA0; &#xA0; int(1)

&#xA0; &#xA0; [&quot;num_cached_keys&quot;]=&gt;

&#xA0; &#xA0; int(2)

&#xA0; &#xA0; [&quot;max_cached_keys&quot;]=&gt;

&#xA0; &#xA0; int(7963)

&#xA0; &#xA0; [&quot;hits&quot;]=&gt;

&#xA0; &#xA0; int(0)

&#xA0; &#xA0; [&quot;start_time&quot;]=&gt;

&#xA0; &#xA0; int(1410858101)

&#xA0; &#xA0; [&quot;last_restart_time&quot;]=&gt;

&#xA0; &#xA0; int(0)

&#xA0; &#xA0; [&quot;oom_restarts&quot;]=&gt;

&#xA0; &#xA0; int(0)

&#xA0; &#xA0; [&quot;hash_restarts&quot;]=&gt;

&#xA0; &#xA0; int(0)

&#xA0; &#xA0; [&quot;manual_restarts&quot;]=&gt;

&#xA0; &#xA0; int(0)

&#xA0; &#xA0; [&quot;misses&quot;]=&gt;

&#xA0; &#xA0; int(1)

&#xA0; &#xA0; [&quot;blacklist_misses&quot;]=&gt;

&#xA0; &#xA0; int(0)

&#xA0; &#xA0; [&quot;blacklist_miss_ratio&quot;]=&gt;

&#xA0; &#xA0; float(0)

&#xA0; &#xA0; [&quot;opcache_hit_rate&quot;]=&gt;

&#xA0; &#xA0; float(0)

&#xA0; }

&#xA0; [&quot;scripts&quot;]=&gt;

&#xA0; array(1) {

&#xA0; &#xA0; [&quot;/var/www/opcache.php&quot;]=&gt;

&#xA0; &#xA0; array(6) {

&#xA0; &#xA0; &#xA0; [&quot;full_path&quot;]=&gt;

&#xA0; &#xA0; &#xA0; string(17) &quot;/var/www/opcache.php&quot;

&#xA0; &#xA0; &#xA0; [&quot;hits&quot;]=&gt;

&#xA0; &#xA0; &#xA0; int(0)

&#xA0; &#xA0; &#xA0; [&quot;memory_consumption&quot;]=&gt;

&#xA0; &#xA0; &#xA0; int(1064)

&#xA0; &#xA0; &#xA0; [&quot;last_used&quot;]=&gt;

&#xA0; &#xA0; &#xA0; string(24) &quot;Tue Sep 16 09:01:41 2014&quot;

&#xA0; &#xA0; &#xA0; [&quot;last_used_timestamp&quot;]=&gt;

&#xA0; &#xA0; &#xA0; int(1410858101)

&#xA0; &#xA0; &#xA0; [&quot;timestamp&quot;]=&gt;

&#xA0; &#xA0; &#xA0; int(1410858099)

&#xA0; &#xA0; }

&#xA0; }

}

  

#

[Official documentation page](https://www.php.net/manual/en/function.opcache-get-status.php)

**[To root](/README.md)**