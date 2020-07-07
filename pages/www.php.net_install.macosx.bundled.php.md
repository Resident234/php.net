# Using the bundled PHP





You only have to uncomment:
#LoadModule php5_module&#xA0; &#xA0; &#xA0; &#xA0; libexec/apache2/libphp5.so

This is gone:
# AddModule mod_php5.c

The statement in 3 was changed to:
&lt;IfModule mime_module&gt;
&#xA0; &#xA0; #
&#xA0; &#xA0; # TypesConfig points to the file containing the list of mappings from
&#xA0; &#xA0; # filename extension to MIME-type.
&#xA0; &#xA0; #
&#xA0; &#xA0; TypesConfig /private/etc/apache2/mime.types

&#xA0; &#xA0; #
&#xA0; &#xA0; # AddType allows you to add to or override the MIME configuration
&#xA0; &#xA0; # file specified in TypesConfig for specific file types.
&#xA0; &#xA0; #
&#xA0; &#xA0; #AddType application/x-gzip .tgz
&#xA0; &#xA0; #
&#xA0; &#xA0; # AddEncoding allows you to have certain browsers uncompress
&#xA0; &#xA0; # information on the fly. Note: Not all browsers support this.
&#xA0; &#xA0; #
&#xA0; &#xA0; #AddEncoding x-compress .Z
&#xA0; &#xA0; #AddEncoding x-gzip .gz .tgz
&#xA0; &#xA0; #
&#xA0; &#xA0; # If the AddEncoding directives above are commented-out, then you
&#xA0; &#xA0; # probably should define those extensions to indicate media types:
&#xA0; &#xA0; #
&#xA0; &#xA0; AddType application/x-compress .Z
&#xA0; &#xA0; AddType application/x-gzip .gz .tgz

&#xA0; &#xA0; #
&#xA0; &#xA0; # AddHandler allows you to map certain file extensions to &quot;handlers&quot;:
&#xA0; &#xA0; # actions unrelated to filetype. These can be either built into the server
&#xA0; &#xA0; # or added with the Action directive (see below)
&#xA0; &#xA0; #
&#xA0; &#xA0; # To use CGI scripts outside of ScriptAliased directories:
&#xA0; &#xA0; # (You will also need to add &quot;ExecCGI&quot; to the &quot;Options&quot; directive.)
&#xA0; &#xA0; #
&#xA0; &#xA0; #AddHandler cgi-script .cgi

&#xA0; &#xA0; # For type maps (negotiated resources):
&#xA0; &#xA0; #AddHandler type-map var

&#xA0; &#xA0; #
&#xA0; &#xA0; # Filters allow you to process content before it is sent to the client.
&#xA0; &#xA0; #
&#xA0; &#xA0; # To parse .shtml files for server-side includes (SSI):
&#xA0; &#xA0; # (You will also need to add &quot;Includes&quot; to the &quot;Options&quot; directive.)
&#xA0; &#xA0; #
&#xA0; &#xA0; #AddType text/html .shtml
&#xA0; &#xA0; #AddOutputFilter INCLUDES .shtml
&lt;/IfModule&gt;

Extra MIME types can either be added to the file /private/etc/apache2/mime.types or by using an AddType directive as commented on above.

  

#

[Official documentation page](https://www.php.net/manual/en/install.macosx.bundled.php)

**[To root](/README.md)**