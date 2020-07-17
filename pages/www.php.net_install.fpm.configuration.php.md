# Configuration



It seems there is no way to get informed about the access log format codes that are used or can be used. All I found is the source code.<br><br>It would really help, not to have open questions when deploying php-fpm. I constantly struggle with file paths for example, but that is another topic.<br><br>                                case &apos;%&apos;: /* &apos;%&apos; */<br>                                case &apos;C&apos;: /* %CPU */<br>                                case &apos;d&apos;: /* duration &#xB5;s */<br>                                case &apos;e&apos;: /* fastcgi env  */<br>                                case &apos;f&apos;: /* script */<br>                                case &apos;l&apos;: /* content length */<br>                                case &apos;m&apos;: /* method */<br>                                case &apos;M&apos;: /* memory */<br>                                case &apos;n&apos;: /* pool name */<br>                                case &apos;o&apos;: /* header output  */<br>                                case &apos;p&apos;: /* PID */<br>                                case &apos;P&apos;: /* PID */<br>                                case &apos;q&apos;: /* query_string */<br>                                case &apos;Q&apos;: /* &apos;?&apos; */<br>                                case &apos;r&apos;: /* request URI */<br>                                case &apos;R&apos;: /* remote IP address */<br>                                case &apos;s&apos;: /* status */<br>                                case &apos;T&apos;:<br>                                case &apos;t&apos;: /* time */<br>                                case &apos;u&apos;: /* remote user */  

#

the doc is lacking a lot of things it seems.<br><br>  The php fpm exemple config file indicate different thing, more option etc... I wonder why the main documentation is less verbose that the configuration file that user can have .. or not have ?  

#

[Official documentation page](https://www.php.net/manual/en/install.fpm.configuration.php)

**[To root](/README.md)**