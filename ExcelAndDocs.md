
# Примеры различных кейсов работы с xlsx и doc форматами 

## Конвертировать xlsx в array:

Устанавливаем библиотеку `phpoffice/phpspreadsheet`:

`composer require phpoffice/phpspreadsheet`

Код конвертации xlsx в массив:

```php
<?php

// include the autoloader, so we can use PhpSpreadsheet
require_once(__DIR__ . '/vendor/autoload.php');

# Create a new Xls Reader
$reader = new \PhpOffice\PhpSpreadsheet\Reader\Xlsx();

// Tell the reader to only read the data. Ignore formatting etc.
$reader->setReadDataOnly(true);

// Read the spreadsheet file.
$spreadsheet = $reader->load(__DIR__ . '/path/to/file.xlsx');

$sheet = $spreadsheet->getSheet($spreadsheet->getFirstSheetIndex());
$data = $sheet->toArray();

// output the data to the console, so you can see what there is.
print_r($data, true);
```
