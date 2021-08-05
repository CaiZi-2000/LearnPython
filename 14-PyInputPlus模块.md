# 14 -- PyInputPlus模块

## 安装第三方模块

在 Windows 和 macOS 中，pip 会随着 Python 自动安装。可以通过命令行窗口输入 pip 检查是否已经安装。但在 Linux 中，必须由你单独安装。在 Ubuntu Linux 或 Debian Linux 上使用 `sudo apt-get install python3-pip` 安装，在 Fedora Linux 上使用 `sudo yum install python3-pip` 安装。

使用 `pip install --user MODULE` 安装相应的第三方模块。输入验证需要安装的模块为 `PyInputPlus` ，所以我们先使用 `pip install --user pyinputplus` 命令安装。

## PyInputPlus 模块中的方法

### inputStr()

此方法类似于 Python 内置的 input() 函数，但具有一般的 PyInputPlus 功能，例如有效输入时间、有效输入次数等。

```python
>>> import pyinputplus as pyip
>>> result = pyip.inputStr('Enter name> ')
Enter name> Caizi
>>> result
'Caizi'
```

### inputCustom()

此方法类似于 Python 内置的 input() 函数，但具有一般的 PyInputPlus 功能，例如有效输入时间、有效输入次数等。可以传入一个用于验证输入的函数。如果引发异常，验证将失败，并向用户显示异常消息。

```python
>>> import pyinputplus as pyip
>>> def raiseIfUppercase(text):
	if text.isupper():
		raise Exception('Input cannot be uppercase.')

	
>>> x = pyip.inputCustom(raiseIfUppercase)
CAIZI
Input cannot be uppercase.
Caizi
>>> x
'Caizi'
```

### inputNum()

提示用户输入整数或浮点数。方法返回 int 或 float 值（取决于用户是否输入了小数点）。其他公共参数说明：

- `min` 如果不为 `None` ，则表示接受的最小数值，包括最小参数。
- `max` 如果不为 `None` ，则表示接受的最大数值，包括最大参数。
- `greaterThan` 如果不为 `None` ，则为接受的最小数值，不包括 `greaterThan` 参数。
- `lessThan` 如果不为 `None` ，则为接受的最大数值，不包括 `lessThan` 参数。

```python
>>> import pyinputplus as pyip
>>> x = pyip.inputNum('Enter a number> ')
Enter a number> 2.3
>>> x
2.3
>>> x = pyip.inputNum('Enter a number> ')
Enter a number> 2
>>> x
2
```

```python
>>> import pyinputplus as pyip
>>> x = pyip.inputNum('Enter a number(min=10)> ', min=10)
Enter a number(min=10)> 9
Number must be at minimum 10.
Enter a number(min=10)> 10
>>> x
10
```

```python
>>> import pyinputplus as pyip
>>> x = pyip.inputNum('Enter a number(max=10)> ', max=10)
Enter a number(max=10)> 11
Number must be at maximum 10.
Enter a number(max=10)> 10
>>> x
10
```

```python
>>> import pyinputplus as pyip
>>> x = pyip.inputNum('Enter a number greater than 5> ', greaterThan=5)
Enter a number greater than 5> 5
Number must be greater than 5.
Enter a number greater than 5> 6
>>> x
6
```

```python
>>> import pyinputplus as pyip
>>> x = pyip.inputNum('Enter a number less than 5> ', lessThan=5)
Enter a number less than 5> 5
Number must be less than 5.
Enter a number less than 5> 4
>>> x
4
```

### inputInt()

提示用户输入整数值，以整数形式返回。其他公共参数说明：

- `limit`：一个整数值，设置在函数退出前最多能有几次有效输入。
- `timeout`：一个整数值，设置用户只能在几秒内提供有效输入。
- `default`：设置默认返回值，而不是引发异常。

```python
>>> import pyinputplus as pyip
>>> x = pyip.inputInt('Enter a integer number> ')
Enter a integer number> 2.3
'2.3' is not an integer.
Enter a integer number> 2.0
>>> x
2
>>> x = pyip.inputInt('Enter a integer number> ', limit=2, default='N/A')
Enter a integer number> 2.3
'2.3' is not an integer.
Enter a integer number> 2.1
'2.1' is not an integer.
>>> x
'N/A'
>>> x = pyip.inputInt('Enter a integer number> ', timeout=5, default='N/A')
Enter a integer number> 4   # 输入数字后等待超过5秒再回车
>>> x
'N/A'
```

### inputFloat()

提示用户输入浮点值，以浮点形式返回。

```python
>>> import pyinputplus as pyip
>>> x = pyip.inputFloat('Enter a float number> ')
Enter a float number> 3
>>> x
3.0
```

### inputChoice()

提示用户输入提供的选项之一，以字符串形式返回选定的选项。

```python
>>> import pyinputplus as pyip
>>> x = pyip.inputChoice(['apple', 'pear'])
Please select one of: apple, pear
banana
'banana' is not a valid choice.
Please select one of: apple, pear
pear
>>> x
'pear'
```

### inputMenu()

提示用户输入提供的选项之一，还显示带有项目符号、编号或字母选项的小菜单，以字符串形式返回选定的选项。

未指定任何参数，输入不区分大小写。

```python
>>> import pyinputplus as pyip
>>> x = pyip.inputMenu(['apple', 'pear'])
Please select one of the following:
* apple
* pear
apple
>>> x
'apple'
>>> x = pyip.inputMenu(['apple', 'pear'])
Please select one of the following:
* apple
* pear
PEAR
>>> x
'pear'
```

指定了数字标号，可以直接输入数字进行选择。

```python
>>> import pyinputplus as pyip
>>> x = pyip.inputMenu(['apple', 'pear'], numbered=True)
Please select one of the following:
1. apple
2. pear
2
>>> x
'pear'
```

指定了字母标号，可以直接输入字母进行选择。

```python
>>> import pyinputplus as pyip
>>> x = pyip.inputMenu(['apple', 'pear'], lettered=True)
Please select one of the following:
A. apple
B. pear
B
>>> x
'pear'
```

指定了区分大小写，输入必须与选项大小写一致。

```python
>>> import pyinputplus as pyip
>>> x = pyip.inputMenu(['apple', 'pear'], caseSensitive=True)
Please select one of the following:
* apple
* pear
PEAR
'PEAR' is not a valid choice.
Please select one of the following:
* apple
* pear
pear
>>> x
'pear'
```

### inputDate()

提示用户在“格式”列表中输入指定格式的日期，返回 datetime.date 对象。

```python
>>> import pyinputplus as pyip
>>> x = pyip.inputDate()
2021/08/03
>>> x
datetime.date(2021, 8, 3)
>>> x = pyip.inputDate()
08/03/2021
>>> x
datetime.date(2021, 8, 3)
>>> x = pyip.inputDate(formats=['%b %Y'])
Aug 2021
>>> x
datetime.date(2021, 8, 1)
```

### inputDatetime()

提示用户在“格式”列表中输入指定格式的日期时间，返回 datetime.datetime 对象。

```python
>>> import pyinputplus as pyip
>>> x = pyip.inputDatetime()
2021/08/03 20:00:01
>>> x
datetime.datetime(2021, 8, 3, 20, 0, 1)
```

### inputTime()

提示用户在“格式”列表中输入指定格式的时间，返回 datetime.time 对象。

```python
>>> import pyinputplus as pyip
>>> x = pyip.inputTime()
20:00:01
>>> x
datetime.time(20, 0, 1)
```

### inputUSState()

提示用户输入美国州名称或缩写，我们基本用不上。返回状态缩写（除非 returnStateName 参数为 True ，在这种情况下，将返回完整名称）。

```python
>>> import pyinputplus as pyip
>>> x = pyip.inputUSState()
ca
>>> x
'CA'
>>> x = pyip.inputUSState(returnStateName=True)
ca
>>> x
'California'
```

### inputMonth()

提示用户输入月份名称，返回所选月份名称的字符串。

```python
>>> import pyinputplus as pyip
>>> x = pyip.inputMonth()
8
>>> x
'August'
>>> x = pyip.inputMonth()
august
>>> x
'August'
```

### inputDayOfWeek()

提示用户一周中的某一天，返回星期的名称。

```python
>>> import pyinputplus as pyip
>>> x = pyip.inputDayOfWeek()
tue
>>> x
'Tuesday'
```

### inputDayOfMonth()

提示用户输入从1到28、30或31的数字月份（或闰年为29），取决于给定的月份和年份。以整数形式返回输入的日期。其他参数说明：

- `year`：给定的年份，它决定了月份中天数的范围。
- `month`：给定的月份，它决定了可以选择的天数范围。

```python
>>> import pyinputplus as pyip
>>> x = pyip.inputDayOfMonth(2021, 8)
3
>>> x
3
>>> x = pyip.inputDayOfMonth(2000, 2)
29
>>> x
29
>>> x = pyip.inputDayOfMonth(2001, 2)
29
'29' is not a day in the month of February 2001
2
>>> x
2
```

### inputIP()

提示用户输入 IPv4 或 IPv6 地址，以字符串形式返回输入的 IP 地址。

```python
>>> import pyinputplus as pyip
>>> x = pyip.inputIP()
192.168.0.1
>>> x
'192.168.0.1'
```

### inputRegex()

提示用户输入与提供的正则表达式字符串（或正则表达式对象）和标志匹配的字符串，返回输入的字符串。

```python
>>> import pyinputplus as pyip
>>> x = pyip.inputRegex('\d\d')
2
'2' does not match the specified pattern.
23
>>> x
'23'
```

### inputRegexStr()

提示用户输入正则表达式字符串（只支持 Python 风格正则表达式，不支持 Perl 或 JavaScript 风格），返回输入的正则表达式字符串。

```python
>>> import pyinputplus as pyip
>>> x = pyip.inputRegexStr()
\d\d
>>> x
re.compile('\\d\\d')
```

### inputURL()

提示用户输入 URL ，以字符串形式返回 URL 。

```python
>>> import pyinputplus as pyip
>>> x = pyip.inputURL()
Caizi
'Caizi' is not a valid URL.
https://www.baidu.com
>>> x
'https://www.baidu.com'
```

### inputYesNo()

提示用户输入 `yes` / `no`，用户还可以输入 `y` / `n` 。根据用户的选择返回自定义的“yesVal”或“noVal”参数（默认为“yes”和“no”）。

```python
>>> import pyinputplus as pyip
>>> x = pyip.inputYesNo()
yes
>>> x
'yes'
>>> x = pyip.inputYesNo()
n
>>> x
'no'
>>> x = pyip.inputYesNo(yesVal='yeah', noVal='no')
y
>>> x
'yeah'
```

### inputBool()

提示用户输入 `True` / `False` ，用户还可以在任何情况下输入 `t` / `f` ，返回一个布尔值。

```python
>>> import pyinputplus as pyip
>>> x = pyip.inputBool()
false
>>> x
False
>>> x = pyip.inputBool()
t
>>> x
True
```

### inputZip()

提示用户输入3到5位数的美国邮政编码，这个基本也用不上。以字符串形式返回 zipcode 。

```python
>>> import pyinputplus as pyip
>>> x = pyip.inputZip()
123
>>> x
'123'
```

### inputFilename()

提示用户输入文件名，文件名不能包含：

```
\\  /   :  *    ?  "   <   >  |    或者以空格结束
```

请注意，这将验证文件名，而不是文件路径。/ 和 \\\\ 字符对于文件名无效，以字符串形式返回文件名。

```python
>>> import pyinputplus as pyip
>>> x = pyip.inputFilename()
C:\Users\1.txt
'C:\\Users\\1.txt' is not a valid filename.
1.txt
>>> x
'1.txt'
```

### inputFilepath()

提示用户输入文件路径，如果 mustExist 为 True ，则此文件路径必须存在于本地文件系统上，以字符串形式返回文件路径。

```python
>>> import pyinputplus as pyip
>>> x = pyip.inputFilepath()
C:\Users
>>> x
'C:\\Users'
```

### inputEmail()

提示用户输入电子邮件地址，以字符串形式返回电子邮件地址。

```python
>>> import pyinputplus as pyip
>>> x = pyip.inputEmail()
Caizi
'Caizi' is not a valid email address.
Caizi@qq.com
>>> x
'Caizi@qq.com'
```

### inputPassword()

提示用户输入密码。将显示掩码字符而不是实际的字符。

- 如果 `correctPassword` 为 `None` ，则任何输入由 `inputPassword()` 接受并返回。
- `strip` 的默认值为 `''` ，因此不会出现空白。
- 默认情况下， `limit` 设置为 `1` ，因此错误的密码尝试会导致引发 `RetryLimitException` 。
- 如果将 `limit` 设置为 `None` ，则会再次要求用户输入正确的密码。每当用户输入错误的密码时，都会显示 `ErrorPasswordMsg` 字符串。
- 如果 `correctPassword` 设置为 `None` ，则接受所有输入。
- `mask` 是用于显示的保护密码字符，而不是实际的字符。它可以设置为 `None` （不隐藏字符）、空白字符串（用户键入时不显示任何内容）或单个字符串（显示此字符而不是按键）。不能将其设置为多字符的字符串。

```python
# 测试时请切回命令行界面运行！
>>> import pyinputplus as pyip
>>> x = pyip.inputPassword()
******
>>> x
'123456'
```
