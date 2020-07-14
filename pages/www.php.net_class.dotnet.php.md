# The dotnet class



Create an Excel Workbook using DOTNET.<br><br>

```
<?php

$full_assembly_string = &apos;Microsoft.Office.Interop.Excel, Version=14.0.0.0, Culture=neutral, PublicKeyToken=71e9bce111e9429c&apos;;
$full_class_name = &apos;Microsoft.Office.Interop.Excel.ApplicationClass&apos;;

$e = new DOTNET($full_assembly_string, $full_class_name);
$wb = $e-&gt;workbooks-&gt;add();
$Precios = $wb-&gt;Worksheets(1);
$Precios-&gt;Name = &apos;Precios&apos;;
$Venta = $wb-&gt;Worksheets(2);
$Venta-&gt;Name = &apos;Venta&apos;;
$Tons = $wb-&gt;Worksheets(3);
$Tons-&gt;Name = &apos;Tons&apos;;

$Meses = Array(&apos;2014-01&apos;, &apos;2014-02&apos;, &apos;2014-03&apos;, &apos;2014-04&apos;, &apos;2014-05&apos;, &apos;2014-06&apos;, &apos;2014-07&apos;, &apos;2014-08&apos;, &apos;2014-09&apos;, &apos;2014-10&apos;, &apos;2014-11&apos;, &apos;2014-12&apos;);
foreach ($Meses as $Numero =&gt; $Mes) {
   $Precios-&gt;Range("A" . ($Numero+1))-&gt;Value = $Mes;
}

$wb-&gt;SaveAs(&apos;c:\temp\Meta.2014.05.xlsx&apos;);
$wb-&gt;Close();

?>
```
<br><br>Go to c:\windows\assembly to know what value to put in $full_assembly_string.<br><br>If you don&apos;t know the assembly, use http://www.red-gate.com/products/dotnet-development/reflector/ to browse it, use what you learn there to fill $full_class_name.<br><br>Enjoy,<br><br>Ricardo.  

#

[Official documentation page](https://www.php.net/manual/en/class.dotnet.php)

**[To root](/README.md)**