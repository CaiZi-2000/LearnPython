# 18 -- 处理Excel表格

## 安装 openpyxl 模块

在 Python 中没有自带的处理 Excel 表格的模块，所以我们在 Windows上使用 `pip install --user openpyxl` 命令安装第三方模块 `openpyxl` 。

## 读取 Excel 表格

使用 `openpyxl.load_workbook()` 函数，传入路径和文件名，返回 `workbook` 数据类型的值。 `workbook` 的 `sheetnames` 属性，获取 Excel 文档所有表名的列表。

每个表都是由一个 `Worksheet` 对象，使用中括号 `workbook['sheetname']` 可以获取相应表的 `Worksheet` 对象。

`Worksheet` 对象的属性和方法：

- `title` 属性，获取字符串格式的表名。
- `active` 属性，获取活动表的 `Worksheet` 对象。
- `max_row` 属性，获取表的总行数。
- `max_column` 属性，获取表的总列数。
- `cell(row, column)` 方法，获取单元格 `Cell` 对象。

使用 `worksheet['site']` 获取单元格 `Cell` 对象。

`Cell` 对象的属性：

- `value` 属性可以获取单元格的值。
- `row` 属性可以获取单元格的行值。
- `column` 属性可以获取单元格的列值。
- `coordinate` 属性可以获取单元格的位置信息。

下面的例子演示了如何读取 Excel 表格，以及如何获取特定单元行和列的值。

example.xlsx 表如下：

![excel](https://user-images.githubusercontent.com/57750019/128684029-d1af46c2-a2cb-4d74-8273-07e3d6aac96f.png)

```python
>>> import openpyxl
>>> wb = openpyxl.load_workbook('D:\\example.xlsx')
>>> type(wb)
<class 'openpyxl.workbook.workbook.Workbook'>
>>> wb.sheetnames
['Sheet1', 'Sheet2']
>>> sheetA = wb['Sheet2']
>>> sheetA
<Worksheet "Sheet2">
>>> type(sheetA)
<class 'openpyxl.worksheet.worksheet.Worksheet'>
>>> sheetA.title
'Sheet2'
>>> sheetB = wb.active
>>> sheetB
<Worksheet "Sheet1">
>>> sheetB.cell(row=2, column=1)
<Cell 'Sheet1'.A2>
>>> sheetB.max_row
7
>>> sheetB.max_column
2
>>> sheetB['A2']
<Cell 'Sheet1'.A2>
>>> sheetB['A2'].value
'spite'
>>> sheetB['A2'].row
2
>>> sheetB['A2'].column
1
>>> sheetB['A2'].coordinate
'A2'
>>> for i in range(1, 6):
	print(i, sheetB.cell(row=i, column=1).value, sheetB.cell(row=i, column=2).value)

	
1 Words Meaning
2 spite n.恶意,怨恨;不顾vt.刁难,欺侮
3 pyramid n.金字塔;椎体
4 tenure n.任期;(土地)保有权,保有期;(大学教师等被授予的)终身职位
5 carbohydrate n.碳水化合物;糖类;[pl.]含碳水化合物的食物
```

使用 `openpyxl.utils` 模块中的 `get_column_letter()` 函数可以将表示列的数字值转换为字母值， `column_index_from_string()` 函数可以将表示列的字母值转换为数字值。

```python
>>> import openpyxl
>>> from openpyxl.utils import get_column_letter, column_index_from_string
>>> get_column_letter(3)
'C'
>>> column_index_from_string('D')
4
```

## 创建和保存 Excel 表格

使用 `openpyxl.Workbook()` 可以创建一个空的 `Workbook` 对象，通过调用 `sheetnames` 属性可以看到，它自动为我们创建了一个名为 `Sheet` 的表，可以直接用赋值语句修改表名，切记，只有在调用 `save(filename)` 方法后才会对表格进行保存。

```python
>>> import openpyxl
>>> wb = openpyxl.Workbook()
>>> wb.sheetnames
['Sheet']
>>> sheet = wb.active
>>> sheet.title
'Sheet'
>>> sheet.title = 'table'
>>> sheet.title
'table'
>>> wb.save('D:\\excelTest.xlsx')
```

## 增加或删除表

使用 `create_sheet(index, title)` 方法可以在 `Excel` 文档中添加一个新的空表，参数 `index` 表示创建表的索引，参数 `title` 表示表的名称，在为指定参数的情况下，默认创建的新表在其他表格的最后，该方法会返回一个名为 `SheetX` 的 `Worksheet` 对象。使用 `del` 操作符可以删除指定的表。记住，在修改后要调用 `save()` 方法后才会对表格的修改进行保存。

```python
>>> import openpyxl
>>> wb = openpyxl.Workbook()
>>> wb.sheetnames
['Sheet']
>>> wb.create_sheet('New Sheet')
<Worksheet "New Sheet">
>>> wb.sheetnames
['Sheet', 'New Sheet']
>>> wb.create_sheet(index = 0, title = 'First Sheet')
<Worksheet "First Sheet">
>>> wb.sheetnames
['First Sheet', 'Sheet', 'New Sheet']
>>> del wb['New Sheet']
>>> wb.sheetnames
['First Sheet', 'Sheet']
>>> wb.save('D:\\excelTest.xlsx')
```

## 修改单元格的值

使用赋值语句可以直接修改单元格内容。

```python
>>> import openpyxl
>>> wb = openpyxl.Workbook()
>>> sheet = wb['Sheet']
>>> sheet['A1'] = 'Caizi'
>>> sheet['A1'].value
'Caizi'
>>> wb.save('D:\\excelTest.xlsx')
```

## 设置单元格字体

调用 `openpyxl.styles` 的 `Font(name, size, bold, italic)` 函数创建一个 `Font` 对象，将该对象赋给 `Cell` 对象的 `font` 属性，即可对单元格字体格式进行修改。参数 `name` 为字体名称的字符串形式，参数 `size` 为字体大小的整数，参数 `bold` 表示字体是否设置加粗（ `True` 表示粗体），参数 `italic` 表示字体是否设置为斜体（ `True` 表示斜体）。

```python
>>> import openpyxl
>>> from openpyxl.styles import Font
>>> wb = openpyxl.Workbook()
>>> sheet = wb['Sheet']
>>> boldItalic20Font = Font(size=20, bold=True, italic=True)
>>> sheet['A1'] = 'Caizi'
>>> sheet['A1'].font = boldItalic20Font
>>> wb.save('D:\\font.xlsx')
```

## 为单元格添加公式

为单元格添加公式和我们直接在 Excel 中操作一致，直接把公式字符串赋给单元格。

```python
>>> import openpyxl
>>> wb = openpyxl.Workbook()
>>> sheet = wb.active
>>> sheet['A1'] = 100
>>> sheet['A2'] = 200
>>> sheet['A3'] = 300
>>> sheet['A4'] = 400
>>> sheet['A5'] = '=SUM(A1:A4)'
>>> wb.save('D:\\sum.xlsx')
```

## 设置行高和列宽

使用 `sheet.row_dimensions[row].height` 可以指定 `row` 行的行高，使用 `sheet.column_dimensions[column].width` 可以指定 `column` 列的列宽。

```python
>>> import openpyxl
>>> wb = openpyxl.Workbook()
>>> sheet = wb.active
>>> sheet.row_dimensions[1].height = 100
>>> sheet.column_dimensions['C'].width = 200
>>> wb.save('D:\\dimension.xlsx')
```

## 合并和拆分单元格

使用 `sheet.merge_cells()` 方法可以将指定区域的单元格进行合并。只需要设置合并前左上角的单元格的值就可以对合并的单元格进行赋值。使用 `sheet`.`unmerge_cells()` 方法可以将指定区域的合并单元格进行拆分。

```python
>>> import openpyxl
>>> wb = openpyxl.Workbook()
>>> sheet = wb.active
>>> sheet.merge_cells('A1:C4')
>>> sheet['A1'] = 'Caizi'
>>> sheet.merge_cells('A5:B7')
>>> sheet['A5'] = 'excel'
>>> sheet.unmerge_cells('A5:B7')
>>> wb.save('D:\\merge.xlsx')
```

## 冻结单元格

在显示一些特别大数据量的 Excel 表格中，我们经常冻结首行或者首列单元格，方便查看数据对应的属性。使用 `sheet.freeze_panes` 属性可以将指定单元格的左边所有列和上面的所有行进行冻结，下面的例子设置为 `C3` ，即把单元格 `C3` 上面的所有行（行1和行2）和左边的所有列（列A和列B）进行冻结。

```python
>>> import openpyxl
>>> wb = openpyxl.Workbook()
>>> sheet = wb.active
>>> sheet.freeze_panes = 'C3'
>>> wb.save('D:\\freeze.xlsx')
```
