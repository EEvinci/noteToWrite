# pandas的csv相关方法

## read_csv()

**read_csv()是pandas的方法**

```python
pandas.read_csv(filepath_or_buffer, sep=', ', delimiter=None, header='infer', names=None, 
index_col=None, usecols=None, squeeze=False, prefix=None, mangle_dupe_cols=True, dtype=None, 
engine=None, converters=None, true_values=None, false_values=None, skipinitialspace=False, 
skiprows=None, nrows=None, na_values=None, keep_default_na=True, na_filter=True, verbose=False, 
skip_blank_lines=True, parse_dates=False, infer_datetime_format=False, keep_date_col=False, 
date_parser=None, dayfirst=False, iterator=False, chunksize=None, compression='infer', 
thousands=None, decimal=b'.', lineterminator=None, quotechar='"', quoting=0, escapechar=None, 
comment=None, encoding=None, dialect=None, tupleize_cols=None, error_bad_lines=True, 
warn_bad_lines=True, skipfooter=0, skip_footer=0, doublequote=True, delim_whitespace=False, 
as_recarray=None, compact_ints=None, use_unsigned=None, low_memory=True, buffer_lines=None, 
memory_map=False, float_precision=None )
```

```python
import pandas as pd
filepath = "\\\"
file = pd.read_csv(filepath, 参数1= , 参数2= , ......)
```

关于read_csv的常用参数如下:

 **`filepath`**, 路径 URL 可以是http, ftp, **s3,** 和 file.(这个s3是啥东西我不知道)

 `sep`, 指定分割符，默认是’,’ (C引擎不能自动检测分隔符，但Python解析引擎可以)

**`delimiter`,**作用和`sep`一样, 是`sep`的别名

**delimiter_whitespace:** True or False 默认False, 用空格作为分隔符等价于spe=’\s+’如果该参数被调用，则delimite不会起作用

**`header`,** 指定第几行作为列名(忽略注解行)，**如果没有指定列名，默认header=0;** **如果指定了列名header=None**

**`names`**, 指定列名，如果文件中不包含header的行，应该显性表示header=None

**`index_col`**, 默认为None, 用列名作为DataFrame的行标签，如果给出序列，则使用MultiIndex。如果读取某文件,该文件每行末尾都有带分隔符，考虑使用index_col=False使panadas不用第一列作为行的名称。

**`usecols`：** 默认None 可以使用列序列也可以使用列名，如 [0, 1, 2] or [‘foo’, ‘bar’, ‘baz’]，选取的列

> read_csv读取时会**自动识别表头**:
>
> - 数据有表头时不能设置[header](https://so.csdn.net/so/search?q=header&spm=1001.2101.3001.7020)为空（默认读取第一行，即`header=0`)；
>
> - 数据无表头时，**若不设置header，第一行数据会被视为表头**，应传入`names`参数设置表头名称或设置**header=None**

> pandas是如何识别或区分数据和表头名称的 ？
>
> - 对于index_col来说:
>
>   - 若数据都是相同类型，比如数值型，则表示无index，输出默认index为0,1,2,…；
>
>   - 若数据第一列为字符，其他列为数值，则会将第一列视为index；
>   - 若设置index_col=False, 则表示无index（默认将0, 1, 2,…作为数据的index)
>
> - 对header:
>
>   - 当第一行为字符，则第一行默认为表头；
>   - 当第一行与其他数据类型相同时，也会把第一行当作表头，所以无表头时应设置header=None

**as_recarray：**默认False , 将读入的数据按照numpy array的方式存储，0.19.0版本后使用 pd.read_csv(…).to_records()。 注意，这种方式读入的na数据不是显示na,而是给以个莫名奇妙的值

**squeeze:** 默认为False, True的情况下返回的类型为Series

**`prefix`:**默认为none, 当header =None 或者没有header的时候有效，例如’x’ 列名效果 X0, X1, …

**mangle_dupe_cols ：**默认为True,重复的列将被指定为’X.0’…’X.N’，而不是’X’…’X’。如果传入False，当列中存在重复名称，则会导致数据被覆盖。

**`dtype`:** E.g. {‘a’: np.float64, ‘b’: np.int32} 指定数据类型

**engine:** {‘c’, ‘python’}, optional 选择读取的引擎目前来说C更快，但是Python的引擎有更多选择的操作

**skipinitialspace:** 忽略分隔符后的空格,默认false

**skiprows:** list-like or integer or callable, default None 忽略某几行或者从开始算起的几行

**skipfooter:** 从底端算起的几行，不支持C引擎

**nrows：** int 读取的行数

**na_values**: 默认None NaN包含哪些情况，默认情况下, ‘#N/A’, ‘#N/A N/A’, ‘#NA’, ‘-1.#IND’, ‘-1.#QNAN’, ‘-NaN’, ‘-nan’, ‘1.#IND’, ‘1.#QNAN’, ‘N/A’, ‘NA’, ‘NULL’, ‘NaN’, ‘n/a’, ‘nan’, ‘null’. 都表现为NAN

**keep_default_na:** 如果na_values被定义,keep_default_na为False那么默认的NAN会被改写。 默认为True

**na_filter:** 默认为True, 针对没有NA的文件，使用na_filter=false能够提高读取效率

**skip_blank_lines** 默认为True,跳过blank lines 而且不是定义为NAN

**thousands** 千分位符号，默认‘，’

**decimal** 小数点符号，默认‘.’

**`encoding`:** 编码方式

**memory_map**, 如果为filepath_or_buffer提供了文件路径，则将文件对象直接映射到内存上，并直接从那里访问数据。使用此选项可以提高性能，因为不再有任何I / O开销。

**low_memory**, 默认为True 在块内部处理文件，导致分析时内存使用量降低，但可能数据类型混乱。要确保没有混合类型设置为False，或者使用dtype参数指定类型。请注意，不管怎样，整个文件都读入单个DataFrame中，请使用chunksize或iterator参数以块形式返回数据。 （仅在C语法分析器中有效）

### tips:

- delimiter是sep的别名，功能是一样的， 两者设置其中一个就可以了，如果同时设置，就会报错
- 设置sep=None, 就会有个告警，因为c engin不支持sep=None, 如果指定engin='python',就不会有告警
- 另外，如果分隔符超过1个字符，就会被认为是正则，c引擎不支持，会强制使用python解析引擎，如果不指定引擎是python，就会给个告警

## to_csv()

**to_csv()是DataFrame类的方法**

[pandas的to_csv()使用方法](https://blog.csdn.net/toshibahuai/article/details/79034829)

```python
DataFrame.to_csv(path_or_buf=None, sep=', ', na_rep='', float_format=None, columns=None, 
header=True, index=True, index_label=None, mode='w', encoding=None, compression=None, 
quoting=None, quotechar='"', line_terminator='\n', chunksize=None, tupleize_cols=None, 
date_format=None, doublequote=True, escapechar=None, decimal='.')
```

> **path_or_buf=None：** string or file handle, default None 
>
> File path or object, if None is provided the result is returned as a string. 
> 字符串或文件句柄，默认无文件 
> 路径或对象，如果没有提供，结果将返回为字符串。
>
> **sep :** character, default ‘,’ 
> Field delimiter for the output file. 
> 默认字符 ‘ ，’ 
> 输出文件的字段分隔符。
>
> **na_rep :** string, default ‘’ 
> Missing data representation 
> 字符串，默认为 ‘’ 
> 浮点数格式字符串
>
> **float_format :** string, default None 
> Format string for floating point numbers 
> 字符串，默认为 None 
> 浮点数格式字符串
>
> **columns :** sequence, optional Columns to write 
> 顺序，可选列写入
>
> **header :** boolean or list of string, default True 
> Write out the column names. If a list of strings is given it is assumed to be aliases for the column names 
> 字符串或布尔列表，默认为true 
> 写出列名。如果给定字符串列表，则假定为列名的别名。
>
> **index :** boolean, default True 
> Write row names (index) 
> 布尔值，默认为Ture 
> 写入行名称（索引）
>
> **index_label :** string or sequence, or False, default None 
> Column label for index column(s) if desired. If None is given, and header and index are True, then the index names are used. A sequence should be given if the DataFrame uses MultiIndex. If False do not print fields for index names. Use index_label=False for easier importing in R 
> 字符串或序列，或False,默认为None 
> 如果需要，可以使用索引列的列标签。如果没有给出，且标题和索引为True，则使用索引名称。如果数据文件使用多索引，则应该使用这个序列。如果值为False，不打印索引字段。在R中使用index_label=False 更容易导入索引.
>
> **mode :** str 
> 模式：值为‘str’，字符串 
> Python写模式，默认“w”
>
> **encoding :** string, optional 
> 编码：字符串，可选 
> 表示在输出文件中使用的编码的字符串，Python 2上默认为“ASCII”和Python 3上默认为“UTF-8”。
>
> **compression :** string, optional 
> 字符串，可选项 
> 表示在输出文件中使用的压缩的字符串，允许值为“gzip”、“bz2”、“xz”，仅在第一个参数是文件名时使用。
>
> **line_terminator :** string, default ‘\n’ 
> 字符串，默认为 ‘\n’ 
> 在输出文件中使用的换行字符或字符序列
>
> **quoting :** optional constant from csv module 
> CSV模块的可选常量 
> 默认值为to_csv.QUOTE_MINIMAL。如果设置了浮点格式，那么浮点将转换为字符串，因此csv.QUOTE_NONNUMERIC会将它们视为非数值的。
>
> **quotechar :** string (length 1), default ‘”’ 
> 字符串（长度1），默认“” 
> 用于引用字段的字符
>
> **doublequote :** boolean, default True 
> 布尔，默认为Ture 
> 控制一个字段内的quotechar
>
> **escapechar :** string (length 1), default None 
> 字符串（长度为1），默认为None 
> 在适当的时候用来转义sep和quotechar的字符
>
> **chunksize :** int or None 
> int或None 
> 一次写入行
>
> **tupleize_cols :** boolean, default False 
> 布尔值 ，默认为False 
> 从版本0.21.0中删除：此参数将被删除，并且总是将多索引的每行写入CSV文件中的单独行 
> （如果值为false）将多索引列作为元组列表（如果TRUE）或以新的、扩展的格式写入，其中每个多索引列是CSV中的一行。
>
> **date_format :** string, default None 
> 字符串，默认为None 
> 字符串对象转换为日期时间对象
>
> **decimal:** string, default ‘.’ 
> 字符串，默认’.’ 
> 字符识别为小数点分隔符。例如。欧洲数据使用 ​​’，’



### to_csv()在已有数据的情况下，是否会覆盖数据？

当我们在使用到to_csv()方法的时候，循环追加数据会发现最后得到的数据是最后一条，

原因是**to_csv()方法mode默认为w**，**而 w 模式 会清空文件再重新写入新的数据**，

加上mode='a'，便可以追加写入数据。**a 模式** 为追加写入数据就**不会清空前面的数据**，而是会**在原文件的基础上增加新的数据**

```dataframe.to_csv('best_sellers.csv', mode='a', index=True, sep=',')```
