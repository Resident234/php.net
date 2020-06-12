# Using the bundled PHP




<div class="phpcode"><span class="html">
You only have to uncomment:<br>#LoadModule php5_module&#xA0; &#xA0; &#xA0; &#xA0; libexec/apache2/libphp5.so<br><br>This is gone:<br># AddModule mod_php5.c<br><br>The statement in 3 was changed to:<br>&lt;IfModule mime_module&gt;<br>&#xA0; &#xA0; #<br>&#xA0; &#xA0; # TypesConfig points to the file containing the list of mappings from<br>&#xA0; &#xA0; # filename extension to MIME-type.<br>&#xA0; &#xA0; #<br>&#xA0; &#xA0; TypesConfig /private/etc/apache2/mime.types<br><br>&#xA0; &#xA0; #<br>&#xA0; &#xA0; # AddType allows you to add to or override the MIME configuration<br>&#xA0; &#xA0; # file specified in TypesConfig for specific file types.<br>&#xA0; &#xA0; #<br>&#xA0; &#xA0; #AddType application/x-gzip .tgz<br>&#xA0; &#xA0; #<br>&#xA0; &#xA0; # AddEncoding allows you to have certain browsers uncompress<br>&#xA0; &#xA0; # information on the fly. Note: Not all browsers support this.<br>&#xA0; &#xA0; #<br>&#xA0; &#xA0; #AddEncoding x-compress .Z<br>&#xA0; &#xA0; #AddEncoding x-gzip .gz .tgz<br>&#xA0; &#xA0; #<br>&#xA0; &#xA0; # If the AddEncoding directives above are commented-out, then you<br>&#xA0; &#xA0; # probably should define those extensions to indicate media types:<br>&#xA0; &#xA0; #<br>&#xA0; &#xA0; AddType application/x-compress .Z<br>&#xA0; &#xA0; AddType application/x-gzip .gz .tgz<br><br>&#xA0; &#xA0; #<br>&#xA0; &#xA0; # AddHandler allows you to map certain file extensions to &quot;handlers&quot;:<br>&#xA0; &#xA0; # actions unrelated to filetype. These can be either built into the server<br>&#xA0; &#xA0; # or added with the Action directive (see below)<br>&#xA0; &#xA0; #<br>&#xA0; &#xA0; # To use CGI scripts outside of ScriptAliased directories:<br>&#xA0; &#xA0; # (You will also need to add &quot;ExecCGI&quot; to the &quot;Options&quot; directive.)<br>&#xA0; &#xA0; #<br>&#xA0; &#xA0; #AddHandler cgi-script .cgi<br><br>&#xA0; &#xA0; # For type maps (negotiated resources):<br>&#xA0; &#xA0; #AddHandler type-map var<br><br>&#xA0; &#xA0; #<br>&#xA0; &#xA0; # Filters allow you to process content before it is sent to the client.<br>&#xA0; &#xA0; #<br>&#xA0; &#xA0; # To parse .shtml files for server-side includes (SSI):<br>&#xA0; &#xA0; # (You will also need to add &quot;Includes&quot; to the &quot;Options&quot; directive.)<br>&#xA0; &#xA0; #<br>&#xA0; &#xA0; #AddType text/html .shtml<br>&#xA0; &#xA0; #AddOutputFilter INCLUDES .shtml<br>&lt;/IfModule&gt;<br><br>Extra MIME types can either be added to the file /private/etc/apache2/mime.types or by using an AddType directive as commented on above.</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/install.macosx.bundled.php)

**[⬆ to root](/)**