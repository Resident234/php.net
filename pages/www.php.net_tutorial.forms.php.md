# Dealing with Forms



According to the HTTP specification, you should use the POST method when you&apos;re using the form to change the state of something on the server end. For example, if a page has a form to allow users to add their own comments, like this page here, the form should use POST. If you click "Reload" or "Refresh" on a page that you reached through a POST, it&apos;s almost always an error -- you shouldn&apos;t be posting the same comment twice -- which is why these pages aren&apos;t bookmarked or cached.<br><br>You should use the GET method when your form is, well, getting something off the server and not actually changing anything.  For example, the form for a search engine should use GET, since searching a Web site should not be changing anything that the client might care about, and bookmarking or caching the results of a search-engine query is just as useful as bookmarking or caching a static HTML page.  

#

Also, don&apos;t ever use GET method in a form that capture passwords and other things that are meant to be hidden.  

#

worth clarifying: <br><br>POST is not more secure than GET. <br><br>The reasons for choosing GET vs POST involve various factors such as intent of the request (are you "submitting" information?), the size of the request (there are limits to how long a URL can be, and GET parameters are sent in the URL), and how easily you want the Action to be shareable -- Example, Google Searches are GET because it makes it easy to copy and share the search query with someone else simply by sharing the URL. <br><br>Security is only a consideration here due to the fact that a GET is easier to share than a POST. Example: you don&apos;t want a password to be sent by GET, because the user might share the resulting URL and inadvertently expose their password.<br><br>However, a GET and a POST are equally easy to intercept by a well-placed malicious person if you don&apos;t deploy TLS/SSL to protect the network connection itself. <br><br>All Forms sent over HTTP (usually port 80) are insecure, and today (2017), there aren&apos;t many good reasons for a public website to not be using HTTPS (which is basically HTTP + Transport Layer Security). <br><br>As a bonus, if you use TLS  you minimise the risk of your users getting code (ADs) injected into your traffic that wasn&apos;t put there by you.  

#

[Official documentation page](https://www.php.net/manual/en/tutorial.forms.php)

**[To root](/README.md)**