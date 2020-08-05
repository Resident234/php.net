# mysql_num_rows



Some user comments on this page, and some resources including the FAQ at :<br><br>http://www.faqts.com/knowledge_base/view.phtml/aid/114/fid/12 suggest using count(*) to count the number of rows<br><br>This is not a particularly universal solution, and those who read these comments on this page should also be aware that <br><br>select count(*) may not give correct results if you are using "group by" or "having" in your query, as count(*) is an agregate function and resets eachtime a group-by column changes.<br><br>select sum(..) ... left join .. group by ... having ...<br><br>can be an alternative to sub-selects in mysql 3, and such queries cannot have the select fields replaced by count(*) to give good results, it just doesn&apos;t work.<br><br>Sam  

---

[Official documentation page](https://www.php.net/manual/en/function.mysql-num-rows.php)

**[To root](/README.md)**