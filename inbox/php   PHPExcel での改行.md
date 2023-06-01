#php 

---
2022-03-01  14:33

# php   PHPExcel で同一セル内の改行

PHP_EOL がOSごとの改行コードとのこと。

```php
  $agency = $data[$i]['de_master']['management_agency'] . PHP_EOL . $data[$i]['de_master']['representation'];
  $sheet->setCellValueExplicitByColumnAndRow(COL_C, 5 + $i*3, $agency, PHPExcel_Cell_DataType::TYPE_STRING);
```

https://www.tailtension.com/php/1503/

