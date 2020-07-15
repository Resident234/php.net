# Handling file uploads



You&apos;d better check $_FILES structure and values throughly.<br>The following code cannot cause any errors absolutely.<br><br>Example:<br>

```
<?php

header('Content-Type: text/plain; charset=utf-8');

try {
    
    // Undefined | Multiple Files | $_FILES Corruption Attack
    // If this request falls under any of them, treat it invalid.
    if (
        !isset($_FILES['upfile']['error']) ||
        is_array($_FILES['upfile']['error'])
    ) {
        throw new RuntimeException('Invalid parameters.');
    }

    // Check $_FILES['upfile']['error'] value.
    switch ($_FILES['upfile']['error']) {
        case UPLOAD_ERR_OK:
            break;
        case UPLOAD_ERR_NO_FILE:
            throw new RuntimeException('No file sent.');
        case UPLOAD_ERR_INI_SIZE:
        case UPLOAD_ERR_FORM_SIZE:
            throw new RuntimeException('Exceeded filesize limit.');
        default:
            throw new RuntimeException('Unknown errors.');
    }

    // You should also check filesize here. 
    if ($_FILES['upfile']['size'] > 1000000) {
        throw new RuntimeException('Exceeded filesize limit.');
    }

    // DO NOT TRUST $_FILES['upfile']['mime'] VALUE !!
    // Check MIME Type by yourself.
    $finfo = new finfo(FILEINFO_MIME_TYPE);
    if (false === $ext = array_search(
        $finfo->file($_FILES['upfile']['tmp_name']),
        array(
            'jpg' => 'image/jpeg',
            'png' => 'image/png',
            'gif' => 'image/gif',
        ),
        true
    )) {
        throw new RuntimeException('Invalid file format.');
    }

    // You should name it uniquely.
    // DO NOT USE $_FILES['upfile']['name'] WITHOUT ANY VALIDATION !!
    // On this example, obtain safe unique name from its binary data.
    if (!move_uploaded_file(
        $_FILES['upfile']['tmp_name'],
        sprintf('./uploads/%s.%s',
            sha1_file($_FILES['upfile']['tmp_name']),
            $ext
        )
    )) {
        throw new RuntimeException('Failed to move uploaded file.');
    }

    echo 'File is uploaded successfully.';

} catch (RuntimeException $e) {

    echo $e->getMessage();

}

?>
```
  

#

If "large files" (ie: 50 or 100 MB) fail, check this:<br><br>It may happen that your outgoing connection to the server is slow, and it may timeout not the "execution time" but the "input time", which for example in our system defaulted to 60s. In our case a large upload could take 1 or 2 hours.<br><br>Additionally we had "session settings" that should be preserved after upload.<br><br>1) You might want review those ini entries:<br><br>* session.gc_maxlifetime<br>* max_input_time<br>* max_execution_time<br>* upload_max_filesize<br>* post_max_size<br><br>2) Still fails? Caution, not all are changeable from the script itself. ini_set() might fail to override.<br><br>More info here:<br>http://www.php.net/manual/es/ini.list.php<br><br>You can see that the "upload_max_filesize", among others, is PHP_INI_PERDIR and not PHP_INI_ALL. This invalidates to use ini_set():<br>http://www.php.net/manual/en/configuration.changes.modes.php<br><br>Use .htaccess instead.<br><br>3) Still fails?. Just make sure you enabled ".htaccess" to overwrite your php settings. This is made in the apache file. You need at least AllowOverride Options.<br><br>See this here:<br>http://www.php.net/manual/en/configuration.changes.php<br><br>You will necessarily allow this manually in the case your master files come with AllowOverride None.<br><br>Conclussion:<br><br>Depending on the system, to allow "large file uploads" you must go up and up and up and touch your config necessarily up to the apache config.<br><br>Sample files:<br><br>These work for me, for 100MB uploads, lasting 2 hours:<br><br>In apache-virtual-host:<br>-----------------------------------------------------------<br>&lt;Directory /var/www/MyProgram&gt;<br>    AllowOverride Options<br>&lt;/Directory&gt;<br>-----------------------------------------------------------<br><br>In .htaccess:<br>-----------------------------------------------------------<br>php_value session.gc_maxlifetime 10800<br>php_value max_input_time         10800<br>php_value max_execution_time     10800<br>php_value upload_max_filesize    110M<br>php_value post_max_size          120M<br>-----------------------------------------------------------<br><br>In the example,<br>- As I last 1 to 2 hours, I allow 3 hours (3600x3)<br>- As I need 100MB, I allow air above for the file (110M) and a bit more for the whole post (120M).  

#

Caution: *DO NOT* trust $_FILES[&apos;userfile&apos;][&apos;type&apos;] to verify the uploaded filetype; if you do so your server could be compromised.  I&apos;ll show you why below:<br><br>The manual (if you scroll above) states: $_FILES[&apos;userfile&apos;][&apos;type&apos;] -  The mime type of the file, if the browser provided this information. An example would be "image/gif".<br><br>Be reminded that this mime type can easily be faked as PHP doesn&apos;t go very far in verifying whether it really is what the end user reported!<br><br>So, someone could upload a nasty .php script as an "image/gif" and execute the url to the "image".<br><br>My best bet would be for you to check the extension of the file and using exif_imagetype() to check for valid images.  Many people have suggested the use of getimagesize() which returns an array if the file is indeed an image and false otherwise, but exif_imagetype() is much faster. (the manual says it so)  

#

Using /var/www/uploads in the example code is just criminal, imnsho.<br><br>One should *NOT* upload untrusted files into your web tree, on any server.<br><br>Nor should any directory within your web tree have permissions sufficient for an upload to succeed, on a shared server. Any other user on that shared server could write a PHP script to dump anything they want in there!<br><br>The $_FILES[&apos;userfile&apos;][&apos;type&apos;] is essentially USELESS.<br>A. Browsers aren&apos;t consistent in their mime-types, so you&apos;ll never catch all the possible combinations of types for any given file format.<br>B. It can be forged, so it&apos;s crappy security anyway.<br><br>One&apos;s code should INSPECT the actual file to see if it looks kosher.<br><br>For example, images can quickly and easily be run through imagegetsize and you at least know the first N bytes LOOK like an image.  That doesn&apos;t guarantee it&apos;s a valid image, but it makes it much less likely to be a workable security breaching file.<br><br>For Un*x based servers, one could use exec and &apos;file&apos; command to see if the Operating System thinks the internal contents seem consistent with the data type you expect.<br><br>I&apos;ve had trouble in the past with reading the &apos;/tmp&apos; file in a file upload.  It would be nice if PHP let me read that file BEFORE I tried to move_uploaded_file on it, but PHP won&apos;t, presumably under the assumption that I&apos;d be doing something dangerous to read an untrusted file.  Fine.   One should move the uploaded file to some staging directory.  Then you check out its contents as thoroughly as you can.  THEN, if it seems kosher, move it into a directory outside your web tree.  Any access to that file should be through a PHP script which reads the file.  Putting it into your web tree, even with all the checks you can think of, is just too dangerous, imnsho.<br><br>There are more than a few User Contributed notes here with naive (bad) advice.  Be wary.  

#

[Official documentation page](https://www.php.net/manual/en/features.file-upload.php)

**[To root](/README.md)**