# Predefined Constants



Volker&apos;s getOS() function needs to have the order of cases changed in the switch statement since "darwin" contains "win", which means that both "windows" and "darwin" will return self::OS_WIN. I&apos;ve moved the &apos;dar&apos; case above the &apos;win&apos; case:<br><br>

```
<?php
class System {

    const OS_UNKNOWN = 1;
    const OS_WIN = 2;
    const OS_LINUX = 3;
    const OS_OSX = 4;

    /**
     * @return int
     */
    static public function getOS() {
        switch (true) {
            case stristr(PHP_OS, 'DAR'): return self::OS_OSX;
            case stristr(PHP_OS, 'WIN'): return self::OS_WIN;
            case stristr(PHP_OS, 'LINUX'): return self::OS_LINUX;
            default : return self::OS_UNKNOWN;
        }
    }

}
?>
```
  

---

[Official documentation page](https://www.php.net/manual/en/reserved.constants.php)

**[To root](/README.md)**