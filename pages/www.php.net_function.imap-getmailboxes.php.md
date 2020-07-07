# imap_getmailboxes





i am currently develop a simple IMAP client, when i call imap_getmailboxes() i receive a different values in attributes property of the mailbox object the problem is how can i manipulate these attributes to get a meaningful value,
if you make a hard search to find a solution for this, you will
not find any useful documents for this problem, let us take a closer look for this problem.

when i call imap_getmailboxes() against different IMAP servers i got these attribute values

[attributes] =&gt; 9
[attributes] =&gt; 1
[attributes] =&gt; 64
[attributes] =&gt; 32
[attributes] =&gt; 40 

the documentation tell us that we check this attributes against four constants, these contents are

LATT_NOINFERIORS 
LATT_NOSELECT 
LATT_MARKED
LATT_UNMARKED

the value of these constants are 

LATT_NOINFERIORS = 1
LATT_NOSELECT = 2
LATT_MARKED = 4
LATT_UNMARKED = 8

you can got this result by echo each constant, unfortunately the documentation not explain how we can check the attributes against the constants, after a long time of searching i find the answer in source code of c-client 
(you can get the source from ftp://ftp.cac.washington.edu/imap/)
under \src\c-client you will find mail.h open it and you will find this

&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0;&#xA0; /* terminal node in hierarchy */
#define LATT_NOINFERIORS (long) 0x1
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; /* name can not be selected */
#define LATT_NOSELECT (long) 0x2
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; /* changed since last accessed */
#define LATT_MARKED (long) 0x4
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; /* accessed since last changed */
#define LATT_UNMARKED (long) 0x8
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; /* name has referral to remote mailbox */
#define LATT_REFERRAL (long) 0x10
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; /* has selectable inferiors */
#define LATT_HASCHILDREN (long) 0x20
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; /* has no selectable inferiors */
#define LATT_HASNOCHILDREN (long) 0x40

as you notice here these are our four constants and three additional constants 

LATT_REFERRAL
LATT_HASCHILDREN
LATT_HASNOCHILDREN

then what is the value of these 3 attributes 
LATT_REFERRAL 0x10 = 00010000 in binary, the bitmask value is 2^4 = 16 and so on, or simply echo this constant to get the value, then

LATT_REFERRAL = 16
LATT_HASCHILDREN = 32
LATT_HASNOCHILDREN = 64

finally the full list of constants will be
LATT_NOINFERIORS = 1
LATT_NOSELECT = 2
LATT_MARKED = 4
LATT_UNMARKED = 8
LATT_REFERRAL = 16
LATT_HASCHILDREN = 32
LATT_HASNOCHILDREN = 64

ok let&apos;s back to our attributes 
[attributes] =&gt; 9
[attributes] =&gt; 1
[attributes] =&gt; 64
[attributes] =&gt; 32
[attributes] =&gt; 40 

[attributes] =&gt; 9 this mean it&apos;s LATT_UNMARKED and LATT_NOINFERIORS 1+8 =9

[attributes] =&gt; 1 this mean LATT_NOINFERIORS

[attributes] =&gt; 64 this mean LATT_HASNOCHILDREN

[attributes] =&gt; 32 this mean LATT_HASCHILDREN 

[attributes] =&gt; 40 this mean LATT_HASCHILDREN and LATT_UNMARKED 32+8=40

this just like linux file permission 7 mean read, write, and execute 4+2+1 read=4 write=2 execute=1

that is what i found, i hope this can help 

Mohamed Abbas
Nileweb Egypt

  

#

[Official documentation page](https://www.php.net/manual/en/function.imap-getmailboxes.php)

**[To root](/README.md)**