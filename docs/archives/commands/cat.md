# cat

`cat`命令有三个功能：显示文件内容、连接多个文件或标准输入的内容、创建文件。语法形式为：

```bash
cat [OPTION]... [FILE]...
```

本文使用的数据文件内容如下：

```bash
[dsn@CentOS7 tmp]$ cat example.txt 
Hello World!


There is a tab	character.
[dsn@CentOS7 tmp]$ cat example2.txt 
This is a dos file
```

## 显示文件内容

### -n, --number

从`1`开始对所有输出行编号。

```bash
[dsn@CentOS7 tmp]$ cat -n example.txt 
     1	Hello World!
     2	
     3	
     4	There is a tab	character.
[dsn@CentOS7 tmp]$ cat -n example2.txt 
     1	This is a dos file
```

### -b, --number-nonblank

和`-n`相似，只不过对于空白行不编号。

```bash
[dsn@CentOS7 tmp]$ cat -b example.txt 
     1	Hello World!


     2	There is a tab	character.
```

### -s, --squeeze-blank

当遇到有连续两行以上的空白行，就代换为一行的空白行。

```bash
[dsn@CentOS7 tmp]$ cat -ns example.txt 
     1	Hello World!
     2	
     3	There is a tab	character.
```

### -E, --show-ends

在每行结束处显示`$`。

```bash
[dsn@CentOS7 tmp]$ cat -nE example.txt 
     1	Hello World!$
     2	$
     3	$
     4	There is a tab	character.$
```

### -T, --show-tabs

将`TAB`字符显示为`^I`。

```bash
[dsn@CentOS7 tmp]$ cat -nT example.txt 
     1	Hello World!
     2	
     3	
     4	There is a tab^Icharacter.
```

### -v, --show-nonprinting

使用`^`和`M-`符号显示不可打印字符，除了`LFD`和`TAB`之外。

```bash
[dsn@CentOS7 tmp]$ cat -nv example2.txt 
     1	This is a dos file^M
[dsn@CentOS7 tmp]$ file example2.txt 
example2.txt: ASCII text, with CRLF line terminators
```

### -e

等价于`-vE`。

## -t

等价于`-vT`。

### -A, --show-all

等价于`-vET`。

## 连接多个文件或标准输入的内容

将文件`example.txt`和`example2.txt`的内容连接起来，并从`1`开始对所有输出行编号：

```bash
[dsn@CentOS7 tmp]$ cat -n example.txt example2.txt 
     1	Hello World!
     2	
     3	
     4	There is a tab	character.
     5	This is a dos file
```

将标准输入和文件内容连接在一起：

```bash
[dsn@CentOS7 tmp]$ echo "hahaha" | cat -n example.txt - example2.txt 
     1	Hello World!
     2	
     3	
     4	There is a tab	character.
     5	hahaha
     6	This is a dos file
```

## 创建文件

将文件`example.txt`和`example2.txt`的内容连接起来保存为新文件`example3.txt`：

```bash
[dsn@CentOS7 tmp]$ cat example.txt example2.txt > example3.txt
[dsn@CentOS7 tmp]$ cat -n example3.txt 
     1	Hello World!
     2	
     3	
     4	There is a tab	character.
     5	This is a dos file
```

将文件`example2.txt`的内容追加到`example.txt`文件中：

```bash
[dsn@CentOS7 tmp]$ cat example2.txt >> example.txt 
[dsn@CentOS7 tmp]$ cat -n example.txt 
     1	Hello World!
     2	
     3	
     4	There is a tab	character.
     5	This is a dos file
```

`example.txt`文件内容与`example3.txt`文件内容是等价的。

从键盘创建文件：

```bash
[dsn@CentOS7 tmp]$ cat > file.txt
abc
def
[dsn@CentOS7 tmp]$ cat file.txt
abc
def
```

注意**执行`cat > file.txt`命令后，等待输入文件内容，输入结束后需要按下`Ctrl+D`按钮输出`EOF`标识来结束输入。如果`file.txt`原来已经存在，其内容将会被覆盖。**

清空文件内容：

```bash
[dsn@CentOS7 tmp]$ cat /dev/null > file.txt 
[dsn@CentOS7 tmp]$ cat file.txt
```

以上命令可以清空`file.txt`文件中的内容，`/dev/null`称为空设备，是一个特殊的设备文件，其会丢弃一切写入其中的数据，但报告写入操作成功，读取它则会立即得到一个`EOF`。
