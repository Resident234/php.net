# The dotnet class



Create an Excel Workbook using DOTNET.<br><br>

```
<?php

$full_assembly_string = 'Microsoft.Office.Interop.Excel, Version=14.0.0.0, Culture=neutral, PublicKeyToken=71e9bce111e9429c';
$full_class_name = 'Microsoft.Office.Interop.Excel.ApplicationClass';

$e = new DOTNET($full_assembly_string, $full_class_name);
$wb = $e->workbooks->add();
$Precios = $wb->Worksheets(1);
$Precios->Name = 'Precios';
$Venta = $wb->Worksheets(2);
$Venta->Name = 'Venta';
$Tons = $wb->Worksheets(3);
$Tons->Name = 'Tons';

$Meses = Array('2014-01', '2014-02', '2014-03', '2014-04', '2014-05', '2014-06', '2014-07', '2014-08', '2014-09', '2014-10', '2014-11', '2014-12');
foreach ($Meses as $Numero => $Mes) {
   $Precios->Range("A" . ($Numero+1))->Value = $Mes;
}

$wb->SaveAs('c:\temp\Meta.2014.05.xlsx');
$wb->Close();

?>
```
<br><br>Go to c:\windows\assembly to know what value to put in $full_assembly_string.<br><br>If you don&apos;t know the assembly, use http://www.red-gate.com/products/dotnet-development/reflector/ to browse it, use what you learn there to fill $full_class_name.<br><br>Enjoy,<br><br>Ricardo.  

#

[Official documentation page](https://www.php.net/manual/en/class.dotnet.php)

**[To root](/README.md)**