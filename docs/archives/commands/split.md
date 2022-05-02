# split

`split`命令用于将大文件分割成较小的文件。语法形式为：

```bash
split [OPTION]... [INPUT [PREFIX]]
```

例如：

```bash
$ split example.txt
```

以上命令将使用默认参数，按每`1000`行一个文件，将文件`example.txt`分割成`xaa`、`xab`、……等较小的文件。

可以指定输出文件的文件名前缀，如果不指定，默认是`x`：

```bash
$ split example.txt example_
```

以上命令将输出`example_aa`、`example_ab`、……等文件。

如果未指定文件名或者文件名为`-`，则从标准输入读取数据：

```bash
# 从标准输入读取数据，无法指定输出文件名前缀
$ cat example.txt | split
# 使用-作为文件名指定从标准输入读取数据时，可以指定输出文件名前缀
$ cat example.txt | split - example_
```

## -a,  --suffix-length=N

可以使用`-a`或者`--suffix-length`选项指定输出文件名后缀长度，如果不指定，默认为`2`：

```bash
$ split -a 1 example.txt
```

以上命令将输出`xa`、`xb`、……等文件。注意 **<font color='red'>长选项的强制参数对于短选项也是强制性的，强制参数与选项之间的等号可以省略，后同</font>**

## --additional-suffix=SUFFIX

使用`--additional-suffix`选项可以为输出文件名添加额外的后缀或扩展名：

```bash
$ split --additional-suffix .txt example.txt
```

以上命令将输出`xaa.txt`、`xab.txt`、……等文件。

## -b, --bytes=SIZE

使用`-b`或`--bytes`选项指定按字节数分割文件，每个输出文件`SIZE`字节。**`SIZE`是一个整数，后面跟一个单位（可选），例如`10M`即`10*1024*1024`。单位`K`、`M`、`G`、`T`、`P`、`E`、`Z`、`Y`是`1024`的幂，单位`KB`、`MB`、……是`1000`的幂。下同。**

```bash
$ split -b 10 example.txt
```

以上命令指定每`10`个字节作为一个输出文件对文件`example.txt`进行分割。

## -C, --line-bytes=SIZE

每个输出文件最多放入`SIZE`个字节，与`-b`选项相似，但是在分割时将尽量维持每行的完整性。

## -d, --numeric-suffixes[=FROM]

输出文件名默认采用字母后缀，使用`-d`或`--numeric-suffixes`选项可以指定输出文件名采用数字后缀，数字默认从`0`开始编号。

```bash
$ split -d example.txt
# 或
$ split --numeric-suffixes example.txt
```

以上命令将输出`x00`、`x01`、……等文件，而不是`xaa`、`xab`、……等文件。

选项`--numeric-suffixes`还允许设置数字起始值：

```bash
$ split --numeric-suffixes=1 example.txt
```

以上命令将输出`x01`、`x02`、……等文件。注意 **<font color='red'>长选项的可选参数对于短选项通常是无效的，可选参数与长选项之间的等号不能省略，后同。</font>**

## -e, --elide-empty-files

不生成空输出文件，和`-n`选项一起使用。

## --filter=COMMAND

写入shell命令`COMMAND`，文件名是`$FILE`。

```bash
$ split -n l/3 --filter='cat -n' example.txt
```

以上命令将输入文件`example.txt`按行数平均分成3部分并分别对每部分添加行号，并不实际生成输出文件。

## -l, --lines=NUMBER

按行数分割文件，每个文件`NUMBER`行。

```bash
$ split -l 10 example.txt
```

以上命令指定每`10`行作为一个输出文件对文件`example.txt`进行分割。

## -n, --number=CHUNKS

按块分割文件。`CHUNKS`的值有如下几种形式：

- `N` - 将输入文件按字节数平均分成`N`个文件。

    ```bash
    $ split -n 3 example.txt
    ```

    以上命令将输入文件`example.txt`按字节数平均分成`3`个文件。

- `K/N` - 将输入文件按字节数平均分成`N`个文件，并将第`K`个输出到标准输出，其余文件并不实际生成。

    ```bash
    $ split -n 2/3 example.txt
    ```

    以上命令将输入文件`example.txt`按字节数平均分成`3`部分，并将第`2`部分的输出到标准输出。

- `l/N` - 按行数平均分成`N`个文件，不分割行。

    ```bash
    $ split -n l/3 example.txt
    ```

    以上命令将输入文件`example.txt`按行数平均分成`3`个文件，每一行的内容不会被分割。

- `l/K/N` - 按行数平均分成`N`个文件，不分割行，并将第`K`个输出到标准输出，其余文件并不实际生成，与`K/N`类似。

- `r/N` - 按行数循环分割成`N`个文件，不分割行。即第`1`行分割到第`1`个文件、第`2`行分割到第`2`个文件、……、第`N`行分割到第`N`个文件，第`N+1`行分割到第`1`个文件，第`N+2`行分割到第`2`个文件，依此类推。

    ```bash
    $ split -n r/3 example.txt
    ```

    以上命令将输入文件`example.txt`按行数循环分割成`3`个文件，即第`1`行分割到文件`xaa`中，第`2`行分割到文件`xab`中，第`3`行分割到文件`xac`中，第`4`行分割到文件`xaa`中，依此类推。

- `r/K/N` - 按行数循环分割成`N`个文件，不分割行，并将第`K`个输出到标准输出，其余文件并不实际生成，与`K/N`类似。

