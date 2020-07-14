# pfsockopen



pfsockopen() works great on IIS/Windows7 installation, it keeps connection open which is good for performance. However, there is one caveat: when connection is broken because of physical net failure, pfsockopen() returns handle as if connection was working. Subsequent call to fwrite() returns false so you have information about error. The problem is that after physical net connection is restored the situation doesn&apos;t change: pfsockopen() still returns handle and fwrite() returns false. In other words, PHP sticks to old connection that is not working (if you use fsockopen() instead, it will connect properly). Situation goes back to normal after 30 minutes when PHP closes unused connection.<br>The solution to this problem is to call fclose() on socket handle when fwrite() returns false.  

#

To see if it&apos;s really a new connection, or a reused one, you can use ftell() - and see if ther&apos;s been any traffic on the connection. If it&apos;s more than 0, then it&apos;s a reused connection.  

#

[Official documentation page](https://www.php.net/manual/en/function.pfsockopen.php)

**[To root](/README.md)**