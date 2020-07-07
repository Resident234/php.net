# Handling file uploads





You&apos;d better check $_FILES structure and values throughly.
The following code cannot cause any errors absolutely.

Example:


```
<?php

header(&apos;Content-Type: text/plain; charset=utf-8&apos;);

try {
&#xA0; &#xA0; 
&#xA0; &#xA0; // Undefined | Multiple Files | $_FILES Corruption Attack
&#xA0; &#xA0; // If this request falls under any of them, treat it invalid.
&#xA0; &#xA0; if (
&#xA0; &#xA0; &#xA0; &#xA0; !isset($_FILES[&apos;upfile&apos;][&apos;error&apos;]) ||
&#xA0; &#xA0; &#xA0; &#xA0; is_array($_FILES[&apos;upfile&apos;][&apos;error&apos;])
&#xA0; &#xA0; ) {
&#xA0; &#xA0; &#xA0; &#xA0; throw new RuntimeException(&apos;Invalid parameters.&apos;);
&#xA0; &#xA0; }

&#xA0; &#xA0; // Check $_FILES[&apos;upfile&apos;][&apos;error&apos;] value.
&#xA0; &#xA0; switch ($_FILES[&apos;upfile&apos;][&apos;error&apos;]) {
&#xA0; &#xA0; &#xA0; &#xA0; case UPLOAD_ERR_OK:
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; break;
&#xA0; &#xA0; &#xA0; &#xA0; case UPLOAD_ERR_NO_FILE:
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; throw new RuntimeException(&apos;No file sent.&apos;);
&#xA0; &#xA0; &#xA0; &#xA0; case UPLOAD_ERR_INI_SIZE:
&#xA0; &#xA0; &#xA0; &#xA0; case UPLOAD_ERR_FORM_SIZE:
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; throw new RuntimeException(&apos;Exceeded filesize limit.&apos;);
&#xA0; &#xA0; &#xA0; &#xA0; default:
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; throw new RuntimeException(&apos;Unknown errors.&apos;);
&#xA0; &#xA0; }

&#xA0; &#xA0; // You should also check filesize here. 
&#xA0; &#xA0; if ($_FILES[&apos;upfile&apos;][&apos;size&apos;] &gt; 1000000) {
&#xA0; &#xA0; &#xA0; &#xA0; throw new RuntimeException(&apos;Exceeded filesize limit.&apos;);
&#xA0; &#xA0; }

&#xA0; &#xA0; // DO NOT TRUST $_FILES[&apos;upfile&apos;][&apos;mime&apos;] VALUE !!
&#xA0; &#xA0; // Check MIME Type by yourself.
&#xA0; &#xA0; $finfo = new finfo(FILEINFO_MIME_TYPE);
&#xA0; &#xA0; if (false === $ext = array_search(
&#xA0; &#xA0; &#xA0; &#xA0; $finfo-&gt;file($_FILES[&apos;upfile&apos;][&apos;tmp_name&apos;]),
&#xA0; &#xA0; &#xA0; &#xA0; array(
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &apos;jpg&apos; =&gt; &apos;image/jpeg&apos;,
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &apos;png&apos; =&gt; &apos;image/png&apos;,
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &apos;gif&apos; =&gt; &apos;image/gif&apos;,
&#xA0; &#xA0; &#xA0; &#xA0; ),
&#xA0; &#xA0; &#xA0; &#xA0; true
&#xA0; &#xA0; )) {
&#xA0; &#xA0; &#xA0; &#xA0; throw new RuntimeException(&apos;Invalid file format.&apos;);
&#xA0; &#xA0; }

&#xA0; &#xA0; // You should name it uniquely.
&#xA0; &#xA0; // DO NOT USE $_FILES[&apos;upfile&apos;][&apos;name&apos;] WITHOUT ANY VALIDATION !!
&#xA0; &#xA0; // On this example, obtain safe unique name from its binary data.
&#xA0; &#xA0; if (!move_uploaded_file(
&#xA0; &#xA0; &#xA0; &#xA0; $_FILES[&apos;upfile&apos;][&apos;tmp_name&apos;],
&#xA0; &#xA0; &#xA0; &#xA0; sprintf(&apos;./uploads/%s.%s&apos;,
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; sha1_file($_FILES[&apos;upfile&apos;][&apos;tmp_name&apos;]),
&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; $ext
&#xA0; &#xA0; &#xA0; &#xA0; )
&#xA0; &#xA0; )) {
&#xA0; &#xA0; &#xA0; &#xA0; throw new RuntimeException(&apos;Failed to move uploaded file.&apos;);
&#xA0; &#xA0; }

&#xA0; &#xA0; echo &apos;File is uploaded successfully.&apos;;

} catch (RuntimeException $e) {

&#xA0; &#xA0; echo $e-&gt;getMessage();

}

?>
```



  

#



If &quot;large files&quot; (ie: 50 or 100 MB) fail, check this:

It may happen that your outgoing connection to the server is slow, and it may timeout not the &quot;execution time&quot; but the &quot;input time&quot;, which for example in our system defaulted to 60s. In our case a large upload could take 1 or 2 hours.

Additionally we had &quot;session settings&quot; that should be preserved after upload.

1) You might want review those ini entries:

* session.gc_maxlifetime
* max_input_time
* max_execution_time
* upload_max_filesize
* post_max_size

2) Still fails? Caution, not all are changeable from the script itself. ini_set() might fail to override.

More info here:
http://www.php.net/manual/es/ini.list.php

You can see that the &quot;upload_max_filesize&quot;, among others, is PHP_INI_PERDIR and not PHP_INI_ALL. This invalidates to use ini_set():
http://www.php.net/manual/en/configuration.changes.modes.php

Use .htaccess instead.

3) Still fails?. Just make sure you enabled &quot;.htaccess&quot; to overwrite your php settings. This is made in the apache file. You need at least AllowOverride Options.

See this here:
http://www.php.net/manual/en/configuration.changes.php

You will necessarily allow this manually in the case your master files come with AllowOverride None.

Conclussion:

Depending on the system, to allow &quot;large file uploads&quot; you must go up and up and up and touch your config necessarily up to the apache config.

Sample files:

These work for me, for 100MB uploads, lasting 2 hours:

In apache-virtual-host:
-----------------------------------------------------------
&lt;Directory /var/www/MyProgram&gt;
&#xA0; &#xA0; AllowOverride Options
&lt;/Directory&gt;
-----------------------------------------------------------

In .htaccess:
-----------------------------------------------------------
php_value session.gc_maxlifetime 10800
php_value max_input_time&#xA0; &#xA0; &#xA0; &#xA0;&#xA0; 10800
php_value max_execution_time&#xA0; &#xA0;&#xA0; 10800
php_value upload_max_filesize&#xA0; &#xA0; 110M
php_value post_max_size&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; 120M
-----------------------------------------------------------

In the example,
- As I last 1 to 2 hours, I allow 3 hours (3600x3)
- As I need 100MB, I allow air above for the file (110M) and a bit more for the whole post (120M).

  

#



Caution: *DO NOT* trust $_FILES[&apos;userfile&apos;][&apos;type&apos;] to verify the uploaded filetype; if you do so your server could be compromised.&#xA0; I&apos;ll show you why below:

The manual (if you scroll above) states: $_FILES[&apos;userfile&apos;][&apos;type&apos;] -&#xA0; The mime type of the file, if the browser provided this information. An example would be &quot;image/gif&quot;.

Be reminded that this mime type can easily be faked as PHP doesn&apos;t go very far in verifying whether it really is what the end user reported!

So, someone could upload a nasty .php script as an &quot;image/gif&quot; and execute the url to the &quot;image&quot;.

My best bet would be for you to check the extension of the file and using exif_imagetype() to check for valid images.&#xA0; Many people have suggested the use of getimagesize() which returns an array if the file is indeed an image and false otherwise, but exif_imagetype() is much faster. (the manual says it so)

  

#



Using /var/www/uploads in the example code is just criminal, imnsho.

One should *NOT* upload untrusted files into your web tree, on any server.

Nor should any directory within your web tree have permissions sufficient for an upload to succeed, on a shared server. Any other user on that shared server could write a PHP script to dump anything they want in there!

The $_FILES[&apos;userfile&apos;][&apos;type&apos;] is essentially USELESS.
A. Browsers aren&apos;t consistent in their mime-types, so you&apos;ll never catch all the possible combinations of types for any given file format.
B. It can be forged, so it&apos;s crappy security anyway.

One&apos;s code should INSPECT the actual file to see if it looks kosher.

For example, images can quickly and easily be run through imagegetsize and you at least know the first N bytes LOOK like an image.&#xA0; That doesn&apos;t guarantee it&apos;s a valid image, but it makes it much less likely to be a workable security breaching file.

For Un*x based servers, one could use exec and &apos;file&apos; command to see if the Operating System thinks the internal contents seem consistent with the data type you expect.

I&apos;ve had trouble in the past with reading the &apos;/tmp&apos; file in a file upload.&#xA0; It would be nice if PHP let me read that file BEFORE I tried to move_uploaded_file on it, but PHP won&apos;t, presumably under the assumption that I&apos;d be doing something dangerous to read an untrusted file.&#xA0; Fine.&#xA0;&#xA0; One should move the uploaded file to some staging directory.&#xA0; Then you check out its contents as thoroughly as you can.&#xA0; THEN, if it seems kosher, move it into a directory outside your web tree.&#xA0; Any access to that file should be through a PHP script which reads the file.&#xA0; Putting it into your web tree, even with all the checks you can think of, is just too dangerous, imnsho.

There are more than a few User Contributed notes here with naive (bad) advice.&#xA0; Be wary.

  

#

[Official documentation page](https://www.php.net/manual/en/features.file-upload.php)

**[To root](/README.md)**