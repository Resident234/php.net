# Tidy example




<div class="phpcode"><span class="html">
If you are looking for HTML beautifier (a tool to indent HTML output produced by your script), Tidy extension might not be the right tool for the job.<br><br>First and foremost, you should not be using either Tidy or alternatives (e.g. HTML Purifier) in the production code. HTML post procession is relatively resource demanding task, esp. if the underlying implementation relies on DOM API. However, beyond performance, HTML beautification in production might hide far more serious output issues that will be hard to trace back, because output will not align with the input.<br><br>If you are indenting to use indentation (consistent, readable formatting of the output) for development purposes only then you might consider implementation that relies on regular expression. I have written, <a href="https://github.com/gajus/dindent" rel="nofollow" target="_blank">https://github.com/gajus/dindent</a> for this purpose. The difference between earlier mentioned implementation and the latter is that regular expression based implementation does not attempt to sanitise, validate or otherwise manipulate your output beyond ensuring proper indentation.</span>
</div>
  

#

[Official documentation page](https://www.php.net/manual/en/tidy.examples.basic.php)

**[To root](/README.md)**