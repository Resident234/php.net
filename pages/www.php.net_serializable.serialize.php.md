# Serializable::serialize



The documentation here is somewhat misleading. Where it says "This method acts as the destructor of the object. The __destruct() method will not be called after this method," I believe the intent is not that the destructor is not run on the object itself, but that the destructor is not called /as part of the serialization process/. <br><br>That is, the object will still be destructed as it goes out of scope as normal, but the destructor is not called as a part of the object&apos;s serialization.  

---

[Official documentation page](https://www.php.net/manual/en/serializable.serialize.php)

**[To root](/README.md)**