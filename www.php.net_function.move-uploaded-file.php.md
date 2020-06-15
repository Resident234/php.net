# move_uploaded_file




<div class="phpcode"><span class="html">
Security tips you must know before use this function :<br><br>First : make sure that the file is not empty.<br><br>Second : make sure the file name in English characters, numbers and (_-.) symbols, For more protection.<br><br>You can use below function as in example<br><br><span class="default">&lt;?php<br><br></span><span class="comment">/**<br> * Check $_FILES[][name]<br> *<br> * @param (string) $filename - Uploaded file name.<br> * @author Yousef Ismaeil Cliprz<br> */<br></span><span class="keyword">function </span><span class="default">check_file_uploaded_name </span><span class="keyword">(</span><span class="default">$filename</span><span class="keyword">)<br>{<br>&#xA0; &#xA0; (bool) ((</span><span class="default">preg_match</span><span class="keyword">(</span><span class="string">&quot;`^[-0-9A-Z_\.]+$`i&quot;</span><span class="keyword">,</span><span class="default">$filename</span><span class="keyword">)) ? </span><span class="default">true </span><span class="keyword">: </span><span class="default">false</span><span class="keyword">);<br>}<br><br></span><span class="default">?&gt;<br></span><br>Third : make sure that the file name not bigger than 250 characters.<br><br>as in example :<br><br><span class="default">&lt;?php<br><br></span><span class="comment">/**<br> * Check $_FILES[][name] length.<br> *<br> * @param (string) $filename - Uploaded file name.<br> * @author Yousef Ismaeil Cliprz.<br> */<br></span><span class="keyword">function </span><span class="default">check_file_uploaded_length </span><span class="keyword">(</span><span class="default">$filename</span><span class="keyword">)<br>{<br>&#xA0; &#xA0; return (bool) ((</span><span class="default">mb_strlen</span><span class="keyword">(</span><span class="default">$filename</span><span class="keyword">,</span><span class="string">&quot;UTF-8&quot;</span><span class="keyword">) &gt; </span><span class="default">225</span><span class="keyword">) ? </span><span class="default">true </span><span class="keyword">: </span><span class="default">false</span><span class="keyword">);<br>}<br><br></span><span class="default">?&gt;<br></span><br>Fourth: Check File extensions and Mime Types that you want to allow in your project. You can use : pathinfo() <a href="http://php.net/pathinfo" rel="nofollow" target="_blank">http://php.net/pathinfo</a><br><br>or you can use regular expression for check File extensions as in example<br><br>#^(gif|jpg|jpeg|jpe|png)$#i<br><br>or use in_array checking as<br><br><span class="default">&lt;?php<br><br>$ext_type </span><span class="keyword">= array(</span><span class="string">&apos;gif&apos;</span><span class="keyword">,</span><span class="string">&apos;jpg&apos;</span><span class="keyword">,</span><span class="string">&apos;jpe&apos;</span><span class="keyword">,</span><span class="string">&apos;jpeg&apos;</span><span class="keyword">,</span><span class="string">&apos;png&apos;</span><span class="keyword">);<br><br></span><span class="default">?&gt;<br></span><br>You have multi choices to checking extensions and Mime types.<br><br>Fifth: Check file size and make sure the limit of php.ini to upload files is what you want, You can start from <a href="http://www.php.net/manual/en/ini.core.php#ini.file-uploads" rel="nofollow" target="_blank">http://www.php.net/manual/en/ini.core.php#ini.file-uploads</a><br><br>And last but not least : Check the file content if have a bad codes or something like this function <a href="http://php.net/manual/en/function.file-get-contents.php." rel="nofollow" target="_blank">http://php.net/manual/en/function.file-get-contents.php.</a><br><br>You can use .htaccess to stop working some scripts as in example php file in your upload path.<br><br>use :<br><br>AddHandler cgi-script .php .pl .jsp .asp .sh .cgi<br>Options -ExecCGI&#xA0; <br><br>Do not forget this steps for your project protection.</span>
</div>
  

#


<div class="phpcode"><span class="html">
The destination directory must exist; move_uploaded_file() will not automatically create it for you.</span>
</div>
  

#


<div class="phpcode"><span class="html">
For those using PHP on Windows and IIS, you SHOULD set the &quot;upload_tmp_dir&quot; value in php.ini to some directory around where your websites directory is, create that directory, and then set the same permissions on it that you have set for your websites directory. Otherwise, when you upload a file and it goes into C:\WINDOWS\Temp, then you move it to your website directory, its permissions will NOT be set correctly. This will cause you problems if you then want to manipulate that file with something like ImageMagick&apos;s convert utility.</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/function.move-uploaded-file.php)

**[To root](/README.md)**