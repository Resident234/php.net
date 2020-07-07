# Memcached::getResultCode




<div class="phpcode"><span class="html">
00 = MEMCACHED_SUCCESS<br>01 = MEMCACHED_FAILURE<br>02 = MEMCACHED_HOST_LOOKUP_FAILURE // getaddrinfo() and getnameinfo() only<br>03 = MEMCACHED_CONNECTION_FAILURE<br>04 = MEMCACHED_CONNECTION_BIND_FAILURE // DEPRECATED see MEMCACHED_HOST_LOOKUP_FAILURE<br>05 = MEMCACHED_WRITE_FAILURE<br>06 = MEMCACHED_READ_FAILURE<br>07 = MEMCACHED_UNKNOWN_READ_FAILURE<br>08 = MEMCACHED_PROTOCOL_ERROR<br>09 = MEMCACHED_CLIENT_ERROR<br>10 = MEMCACHED_SERVER_ERROR // Server returns &quot;SERVER_ERROR&quot;<br>11 = MEMCACHED_ERROR // Server returns &quot;ERROR&quot;<br>12 = MEMCACHED_DATA_EXISTS<br>13 = MEMCACHED_DATA_DOES_NOT_EXIST<br>14 = MEMCACHED_NOTSTORED<br>15 = MEMCACHED_STORED<br>16 = MEMCACHED_NOTFOUND<br>17 = MEMCACHED_MEMORY_ALLOCATION_FAILURE<br>18 = MEMCACHED_PARTIAL_READ<br>19 = MEMCACHED_SOME_ERRORS<br>20 = MEMCACHED_NO_SERVERS<br>21 = MEMCACHED_END<br>22 = MEMCACHED_DELETED<br>23 = MEMCACHED_VALUE<br>24 = MEMCACHED_STAT<br>25 = MEMCACHED_ITEM<br>26 = MEMCACHED_ERRNO<br>27 = MEMCACHED_FAIL_UNIX_SOCKET // DEPRECATED<br>28 = MEMCACHED_NOT_SUPPORTED<br>29 = MEMCACHED_NO_KEY_PROVIDED /* Deprecated. Use MEMCACHED_BAD_KEY_PROVIDED! */<br>30 = MEMCACHED_FETCH_NOTFINISHED<br>31 = MEMCACHED_TIMEOUT<br>32 = MEMCACHED_BUFFERED<br>33 = MEMCACHED_BAD_KEY_PROVIDED<br>34 = MEMCACHED_INVALID_HOST_PROTOCOL<br>35 = MEMCACHED_SERVER_MARKED_DEAD<br>36 = MEMCACHED_UNKNOWN_STAT_KEY<br>37 = MEMCACHED_E2BIG<br>38 = MEMCACHED_INVALID_ARGUMENTS<br>39 = MEMCACHED_KEY_TOO_BIG<br>40 = MEMCACHED_AUTH_PROBLEM<br>41 = MEMCACHED_AUTH_FAILURE<br>42 = MEMCACHED_AUTH_CONTINUE<br>43 = MEMCACHED_PARSE_ERROR<br>44 = MEMCACHED_PARSE_USER_ERROR<br>45 = MEMCACHED_DEPRECATED<br>46 = MEMCACHED_IN_PROGRESS<br>47 = MEMCACHED_SERVER_TEMPORARILY_DISABLED<br>48 = MEMCACHED_SERVER_MEMORY_ALLOCATION_FAILURE<br>49 = MEMCACHED_MAXIMUM_RETURN /* Always add new error code before */<br>11 = MEMCACHED_CONNECTION_SOCKET_CREATE_FAILURE = MEMCACHED_ERROR</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/memcached.getresultcode.php)

**[To root](/README.md)**