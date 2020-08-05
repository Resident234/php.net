# mysqli_stmt::$num_rows



Please be advised, for people who sometimes miss to read this important Manual entry for this function:<br><br>If you do not use mysqli_stmt_store_result( ), and immediatley call this function after executing a prepared statement, this function will usually return 0 as it has no way to know how many rows are in the result set as the result set is not saved in memory yet.<br><br>mysqli_stmt_store_result( ) saves the result set in memory thus you can immedietly use this function after you both execute the statement AND save the result set.<br><br>If you do not save the result set but still want to use this function you have to actually loop through the result set one row at a time using mysqli_stmt_fetch( ) before using this function to determine the number of rows.<br><br>A thought though, if you want to determine the number of rows without storing the result set and after looping through it, why not just simply keep an internal counter in your loop every time a row is fetched and save the function call.<br><br>In short, this function is only really useful if you save the result set and want to determine the number of rows before looping through it, otherwise you can pretty much recreate its use like I suggested.  

---

[Official documentation page](https://www.php.net/manual/en/mysqli-stmt.num-rows.php)

**[To root](/README.md)**