# Variables From External Sources



The full list of field-name characters that PHP converts to _ (underscore) is the following (not just dot):<br>chr(32) ( ) (space)<br>chr(46) (.) (dot)<br>chr(91) ([) (open square bracket)<br>chr(128) - chr(159) (various)<br><br>PHP irreversibly modifies field names containing these characters in an attempt to maintain compatibility with the deprecated register_globals feature.  

#

Important:  Pay attention to the following security concerns when handling user submitted  data :<br><br>http://www.php.net/manual/en/security.registerglobals.php<br>http://www.php.net/manual/en/security.variables.php  

#

This post is with regards to handling forms that have more than one submit button.<br><br>Suppose we have an HTML form with a submit button specified like this:<br><br>&lt;input type="submit" value="Delete" name="action_button"&gt;<br><br>Normally the &apos;value&apos; attribute of the HTML &apos;input&apos; tag (in this case "Delete") that creates the submit button can be accessed in PHP after post like this:<br><br>

```
<?php
$_POST['action_button'];
?>
```


We of course use the 'name' of the button as an index into the $_POST array.

This works fine, except when we want to pass more information with the click of this particular button.

Imagine a scenario where you're dealing with user management in some administrative interface.  You are presented with a list of user names queried from a database and wish to add a "Delete" and "Modify" button next to each of the names in the list.  Naturally the 'value' of our buttons in the HTML form that we want to display will be "Delete" and "Modify" since that's what we want to appear on the buttons' faceplates.

Both buttons (Modify and Delete) will be named "action_button" since that's what we want to index the $_POST array with.  In other words, the 'name' of the buttons along cannot carry any uniquely identifying information if we want to process them systematically after submit. Since these buttons will exist for every user in the list, we need some further way to distinguish them, so that we know for which user one of the buttons has been pressed.

Using arrays is the way to go.  Assuming that we know the unique numerical identifier of each user, such as their primary key from the database, and we DON'T wish to protect that number from the public, we can make the 'action_button' into an array and use the user's unique numerical identifier as a key in this array.

Our HTML code to display the buttons will become:

&lt;input type="submit" value="Delete" name="action_button[0000000002]"&gt;
&lt;input type="submit" value="Modify" name="action_button[0000000002]"&gt;

The 0000000002 is of course the unique numerical identifier for this particular user.

Then when we handle this form in PHP we need to do the following to extract both the 'value' of the button ("Delete" or "Modify") and the unique numerical identifier of the user we wish to affect (0000000002 in this case). The following will print either "Modify" or "Delete", as well as the unique number of the user:



```
<?php
$submitted_array = array_keys($_POST['action_button']);
echo ($_POST['action_button'][$submitted_array[0]] . " " . $submitted_array[0]);
?>
```
<br><br>$submitted_array[0] carries the 0000000002.<br>When we index that into the $_POST[&apos;action_button&apos;], like we did above, we will extract the string that was used as &apos;value&apos; in the HTML code &apos;input&apos; tag that created this button.<br><br>If we wish to protect the unique numerical identifier, we must use some other uniquely identifying attribute of each user. Possibly that attribute should be encrypted when output into the form for greater security.<br><br>Enjoy!  

#

[Official documentation page](https://www.php.net/manual/en/language.variables.external.php)

**[To root](/README.md)**