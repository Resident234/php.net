# The SplFixedArray class




<div class="phpcode"><span class="html">
As the documentation says, SplFixedArray is meant to be *faster* than array. Do not blindly believe other people&apos;s benchmarks, and beextra careful with the user comments on php.net. For instance, nairbv&apos;s benchmark code is completely wrong. Among other errors, it intends to increase the size of the arrays, but always initialize a 20 elements SplFixedArray.<br><br>On a PHP 5.4 64 bits linux server, I found SplFixedArray to be always faster than array().<br>* small data (1,000):<br>&#xA0; &#xA0; * write: SplFixedArray is 15 % faster<br>&#xA0; &#xA0; * read:&#xA0; SplFixedArray is&#xA0; 5 % faster<br>* larger data (512,000):<br>&#xA0; &#xA0; * write: SplFixedArray is 33 % faster<br>&#xA0; &#xA0; * read:&#xA0; SplFixedArray is 10 % faster</span>
</div>
  

#


<div class="phpcode"><span class="html">
Note, that this is considerably faster and should be used when the size of the array is known. Here are some very basic bench marks:
<br>
<br><span class="default">&lt;?php
<br></span><span class="keyword">for(</span><span class="default">$size </span><span class="keyword">= </span><span class="default">1000</span><span class="keyword">; </span><span class="default">$size </span><span class="keyword">&lt; </span><span class="default">50000000</span><span class="keyword">; </span><span class="default">$size </span><span class="keyword">*= </span><span class="default">2</span><span class="keyword">) {
<br>&#xA0; &#xA0; echo </span><span class="default">PHP_EOL </span><span class="keyword">. </span><span class="string">&quot;Testing size: </span><span class="default">$size</span><span class="string">&quot; </span><span class="keyword">. </span><span class="default">PHP_EOL</span><span class="keyword">;
<br>&#xA0; &#xA0; for(</span><span class="default">$s </span><span class="keyword">= </span><span class="default">microtime</span><span class="keyword">(</span><span class="default">true</span><span class="keyword">), </span><span class="default">$container </span><span class="keyword">= Array(), </span><span class="default">$i </span><span class="keyword">= </span><span class="default">0</span><span class="keyword">; </span><span class="default">$i </span><span class="keyword">&lt; </span><span class="default">$size</span><span class="keyword">; </span><span class="default">$i</span><span class="keyword">++) </span><span class="default">$container</span><span class="keyword">[</span><span class="default">$i</span><span class="keyword">] = </span><span class="default">NULL</span><span class="keyword">;
<br>&#xA0; &#xA0; echo </span><span class="string">&quot;Array(): &quot; </span><span class="keyword">. (</span><span class="default">microtime</span><span class="keyword">(</span><span class="default">true</span><span class="keyword">) - </span><span class="default">$s</span><span class="keyword">) . </span><span class="default">PHP_EOL</span><span class="keyword">;
<br>
<br>&#xA0; &#xA0; for(</span><span class="default">$s </span><span class="keyword">= </span><span class="default">microtime</span><span class="keyword">(</span><span class="default">true</span><span class="keyword">), </span><span class="default">$container </span><span class="keyword">= new </span><span class="default">SplFixedArray</span><span class="keyword">(</span><span class="default">$size</span><span class="keyword">), </span><span class="default">$i </span><span class="keyword">= </span><span class="default">0</span><span class="keyword">; </span><span class="default">$i </span><span class="keyword">&lt; </span><span class="default">$size</span><span class="keyword">; </span><span class="default">$i</span><span class="keyword">++) </span><span class="default">$container</span><span class="keyword">[</span><span class="default">$i</span><span class="keyword">] = </span><span class="default">NULL</span><span class="keyword">;
<br>&#xA0; &#xA0; echo </span><span class="string">&quot;SplArray(): &quot; </span><span class="keyword">. (</span><span class="default">microtime</span><span class="keyword">(</span><span class="default">true</span><span class="keyword">) - </span><span class="default">$s</span><span class="keyword">) . </span><span class="default">PHP_EOL</span><span class="keyword">;
<br>}
<br></span><span class="default">?&gt;
<br></span>
<br>OUTPUT
<br>Testing size: 1000
<br>Array(): 0.00046396255493164
<br>SplArray(): 0.00023293495178223
<br>
<br>Testing size: 2000
<br>Array(): 0.00057101249694824
<br>SplArray(): 0.0003058910369873
<br>
<br>Testing size: 4000
<br>Array(): 0.0015869140625
<br>SplArray(): 0.00086307525634766
<br>
<br>Testing size: 8000
<br>Array(): 0.0024251937866211
<br>SplArray(): 0.00211501121521
<br>
<br>Testing size: 16000
<br>Array(): 0.0057680606842041
<br>SplArray(): 0.0041120052337646
<br>
<br>Testing size: 32000
<br>Array(): 0.011334896087646
<br>SplArray(): 0.007631778717041
<br>
<br>Testing size: 64000
<br>Array(): 0.021990060806274
<br>SplArray(): 0.013560056686401
<br>
<br>Testing size: 128000
<br>Array(): 0.053267002105713
<br>SplArray(): 0.030976057052612
<br>
<br>Testing size: 256000
<br>Array(): 0.10280108451843
<br>SplArray(): 0.056283950805664
<br>
<br>Testing size: 512000
<br>Array(): 0.20657992362976
<br>SplArray(): 0.11510300636292
<br>
<br>Testing size: 1024000
<br>Array(): 0.4138810634613
<br>SplArray(): 0.21826505661011
<br>
<br>Testing size: 2048000
<br>Array(): 0.85640096664429
<br>SplArray(): 0.46247816085815
<br>
<br>Testing size: 4096000
<br>Array(): 1.7242450714111
<br>SplArray(): 0.95304894447327
<br>
<br>Testing size: 8192000
<br>Array(): 3.448086977005
<br>SplArray(): 1.96746301651</span>
</div>
  

#


<div class="phpcode"><span class="html">
Memory footprint of splFixedArray is about 37% of a regular &quot;array&quot; of the same size. <br>I was hoping for more, but that&apos;s also significant, and that&apos;s where you should expect to see difference, not in &quot;performance&quot;.</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/class.splfixedarray.php)

**[To root](/README.md)**