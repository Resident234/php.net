# The PDO class




<div class="phpcode"><span class="html">
&quot;And storing username/password inside class is not a very good idea for production code.&quot;<br><br>Good idea is to store database connection settings in *.ini files but you have to restrict access to them. For example this way:<br><br>my_setting.ini:<br>[database]<br>driver = mysql<br>host = localhost<br>;port = 3306<br>schema = db_schema<br>username = user<br>password = secret<br><br>Database connection:<br><span class="default">&lt;?php<br></span><span class="keyword">class </span><span class="default">MyPDO </span><span class="keyword">extends </span><span class="default">PDO<br></span><span class="keyword">{<br>&#xA0; &#xA0; public function </span><span class="default">__construct</span><span class="keyword">(</span><span class="default">$file </span><span class="keyword">= </span><span class="string">&apos;my_setting.ini&apos;</span><span class="keyword">)<br>&#xA0; &#xA0; {<br>&#xA0; &#xA0; &#xA0; &#xA0; if (!</span><span class="default">$settings </span><span class="keyword">= </span><span class="default">parse_ini_file</span><span class="keyword">(</span><span class="default">$file</span><span class="keyword">, </span><span class="default">TRUE</span><span class="keyword">)) throw new </span><span class="default">exception</span><span class="keyword">(</span><span class="string">&apos;Unable to open &apos; </span><span class="keyword">. </span><span class="default">$file </span><span class="keyword">. </span><span class="string">&apos;.&apos;</span><span class="keyword">);<br>&#xA0; &#xA0; &#xA0; &#xA0; <br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$dns </span><span class="keyword">= </span><span class="default">$settings</span><span class="keyword">[</span><span class="string">&apos;database&apos;</span><span class="keyword">][</span><span class="string">&apos;driver&apos;</span><span class="keyword">] .<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="string">&apos;:host=&apos; </span><span class="keyword">. </span><span class="default">$settings</span><span class="keyword">[</span><span class="string">&apos;database&apos;</span><span class="keyword">][</span><span class="string">&apos;host&apos;</span><span class="keyword">] .<br>&#xA0; &#xA0; &#xA0; &#xA0; ((!empty(</span><span class="default">$settings</span><span class="keyword">[</span><span class="string">&apos;database&apos;</span><span class="keyword">][</span><span class="string">&apos;port&apos;</span><span class="keyword">])) ? (</span><span class="string">&apos;;port=&apos; </span><span class="keyword">. </span><span class="default">$settings</span><span class="keyword">[</span><span class="string">&apos;database&apos;</span><span class="keyword">][</span><span class="string">&apos;port&apos;</span><span class="keyword">]) : </span><span class="string">&apos;&apos;</span><span class="keyword">) .<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="string">&apos;;dbname=&apos; </span><span class="keyword">. </span><span class="default">$settings</span><span class="keyword">[</span><span class="string">&apos;database&apos;</span><span class="keyword">][</span><span class="string">&apos;schema&apos;</span><span class="keyword">];<br>&#xA0; &#xA0; &#xA0; &#xA0; <br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">parent</span><span class="keyword">::</span><span class="default">__construct</span><span class="keyword">(</span><span class="default">$dns</span><span class="keyword">, </span><span class="default">$settings</span><span class="keyword">[</span><span class="string">&apos;database&apos;</span><span class="keyword">][</span><span class="string">&apos;username&apos;</span><span class="keyword">], </span><span class="default">$settings</span><span class="keyword">[</span><span class="string">&apos;database&apos;</span><span class="keyword">][</span><span class="string">&apos;password&apos;</span><span class="keyword">]);<br>&#xA0; &#xA0; }<br>}<br></span><span class="default">?&gt;<br></span><br>Database connection parameters are accessible via human readable ini file for those who screams even if they see one PHP/HTML/any_other command.</span>
</div>
  

#


<div class="phpcode"><span class="html">
PDO and Dependency Injection<br><br>Dependency injection is good for testing.&#xA0; But for anyone wanting various data mapper objects to have a database connection, dependency injection can make other model code very messy because database objects have to be instantiated all over the place and given to the data mapper objects.<br><br>The code below is a good way to maintain dependency injection while keeping clean and minimal model code.<br><br><span class="default">&lt;?php<br><br></span><span class="keyword">class </span><span class="default">DataMapper<br></span><span class="keyword">{<br>&#xA0; &#xA0; public static </span><span class="default">$db</span><span class="keyword">;<br>&#xA0; &#xA0; <br>&#xA0; &#xA0; public static function </span><span class="default">init</span><span class="keyword">(</span><span class="default">$db</span><span class="keyword">)<br>&#xA0; &#xA0; {<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">self</span><span class="keyword">::</span><span class="default">$db </span><span class="keyword">= </span><span class="default">$db</span><span class="keyword">;<br>&#xA0; &#xA0; }<br>}<br><br>class </span><span class="default">VendorMapper </span><span class="keyword">extends </span><span class="default">DataMapper<br></span><span class="keyword">{<br>&#xA0; &#xA0; public static function </span><span class="default">add</span><span class="keyword">(</span><span class="default">$vendor</span><span class="keyword">)<br>&#xA0; &#xA0; {<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$st </span><span class="keyword">= </span><span class="default">self</span><span class="keyword">::</span><span class="default">$db</span><span class="keyword">-&gt;</span><span class="default">prepare</span><span class="keyword">(<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="string">&quot;insert into vendors set<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; first_name = :first_name,<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; last_name = :last_name&quot;<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="keyword">);<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="default">$st</span><span class="keyword">-&gt;</span><span class="default">execute</span><span class="keyword">(array(<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="string">&apos;:first_name&apos; </span><span class="keyword">=&gt; </span><span class="default">$vendor</span><span class="keyword">-&gt;</span><span class="default">first_name</span><span class="keyword">,<br>&#xA0; &#xA0; &#xA0; &#xA0; &#xA0; &#xA0; </span><span class="string">&apos;:last_name&apos; </span><span class="keyword">=&gt; </span><span class="default">$vendor</span><span class="keyword">-&gt;</span><span class="default">last_name<br>&#xA0; &#xA0; &#xA0; &#xA0; </span><span class="keyword">));<br>&#xA0; &#xA0; }<br>}<br><br></span><span class="comment">// In your bootstrap<br></span><span class="default">$db </span><span class="keyword">= new </span><span class="default">PDO</span><span class="keyword">(...);<br></span><span class="default">DataMapper</span><span class="keyword">::</span><span class="default">init</span><span class="keyword">(</span><span class="default">$db</span><span class="keyword">);<br><br></span><span class="comment">// In your model logic<br></span><span class="default">$vendor </span><span class="keyword">= new </span><span class="default">Vendor</span><span class="keyword">(</span><span class="string">&apos;John&apos;</span><span class="keyword">, </span><span class="string">&apos;Doe&apos;</span><span class="keyword">);<br></span><span class="default">VendorMapper</span><span class="keyword">::</span><span class="default">add</span><span class="keyword">(</span><span class="default">$vendor</span><span class="keyword">);<br><br></span><span class="default">?&gt;</span>
</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/class.pdo.php)

**[To root](/README.md)**