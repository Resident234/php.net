# pthreads



Here are some notes regarding PHP pThreads v3 that I have gathered:<br>-namespace: It does not understand namespaces. <br>-globals: It won&apos;t serialize GLOBALS at all! And will not register new ones.<br>-classes: It registers new classes okay.<br>-functions: Will not register ANY functions - they must all be in static classes. It does understand PHP native functions. <br>-consts: previous constants will transfer over. Don&apos;t make any new ones thou!<br>-pThreads only work in CLI - of course!<br>-If a thread crashes it automatically gets recreated.<br>-In order to &apos;force kill&apos; any thread the parent must be killed. Or wait until all other tasks queued are complete and then terminate.<br>-Anything registered in a pThread does not seem to join the main thread ... which is good!<br>-pThreads seem to be very powerful on multi-core environments, just need to be careful on system resources... it can and will lock up a system if mis-configured.<br>-Finally, finding help for PHP pThreads is slim to none... especially v3!<br><br>Good luck!  

#

[Official documentation page](https://www.php.net/manual/en/book.pthreads.php)

**[To root](/README.md)**