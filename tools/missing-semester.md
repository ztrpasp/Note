## shell

### 常见指令

* echo
* date
* $PATH
  * 环境变量的路径
* cat
* cd

```shell
cd - //切换到你之前所在的目录
```

* ls

在使用指令时，有很多的flags和arguements可以使用，通常以`-`开头

例如`--help` 

* 权限问题 `ls -l` or `ll`
* mv
  * 可以重命名一个文件，例如 `mv text.md my.md`
  * 可以移动文件
* cp
* rm
  * rm 默认不是递归删除，如果想要删除一个目录使用 `-r`来进行递归删除
  * `-f`指令即使原档案属性设为唯读，亦直接删除，无需逐一确认。
* mkdir
* man

即manus pages，它以其他的指令名作为输入，如`man ls`与`ls --help`效果类似。

----

### 输入流和输出流

重定向输入流与输出流，使用 `< >` angle bracket signs

`<` 表示将这个程序的输入重定向为这个文件的内容

`>` 表示将前面程序的输出层重定向到这个文件中

```shell
echo hello > hello.txt //会创建一个hello.txt的文件

cat hello.txt

cat < hello.txt  //

cat < hello.txt > hello2.txt // 实现了类似 cp的操作
```



`>`表示写入，但`>>`表示追加



> 管道符 `|`

==将左边程序的输出作为右边程序的输入==



```
ls -l | 
```

-------------------

### grep

Linux grep (global regular expression) 命令用于查找文件里符合条件的字符串或正则表达式。

grep 指令用于查找内容包含指定的范本样式的文件，如果发现某文件的内容符合所指定的范本样式，预设 grep 指令会把含有范本样式的那一列显示出来。若不指定任何文件名称，或是所给予的文件名为 **-**，则 grep 指令会从标准输入设备读取数据。

语法

```
grep [options] pattern [files]
或
grep [-abcEFGhHilLnqrsvVwxy][-A<显示行数>][-B<显示列数>][-C<显示列数>][-d<进行动作>][-e<范本样式>][-f<范本文件>][--help][范本样式][文件或目录...]
```

- pattern - 表示要查找的字符串或正则表达式。
- files - 表示要查找的文件名，可以同时查找多个文件，如果省略 files 参数，则默认从标准输入中读取数据。

**常用选项：**：

- `-i`：忽略大小写进行匹配。
- `-v`：反向查找，只打印不匹配的行。
- `-n`：显示匹配行的行号。
- `-r`：递归查找子目录中的文件。
- `-l`：只打印匹配的文件名。
- `-c`：只打印匹配的行数。



==ripgrep是开源社区正在进行的 RIIR（用 [Rust](https://zhida.zhihu.com/search?content_id=200992856&content_type=Article&match_order=1&q=Rust&zhida_source=entity) 重写）努力的一个优秀成果。，它旨在成为经典grep 命令的高级替代品。==

使用 ripgrep 的语法如下：

```text
rg <pattern> [files/directories]
```

使用 ripgrep，无需提及文件名。如果未提供文件名，则搜索所有文件，如果您不知道哪个文件包含您搜索的模式，这将非常有用。



--------

### more interesting

`sudo` 超级用户



当我们访问内核的文件时，需要root或者超级用户的权限，想当然的

```shell
sudo echo 500 > /sys/intel/brightness
//但是结果是permission denied
//因为 > 的重定向是shell来做的，sudo命令是sudo echo。写入brightness文件是shell做的，shell本身并没有超级用户权限
```

解决办法：

1. ==`sudo su`  以超级用户身份获得一个`shell`==

2. ``` shell
   echo 1060 | sudo tee brightness
   ```



> tee
>
> tee命令在Linux中用于从标准输入读取数据，并将其写入到标准输出和一个或多个文件中。tee命令通常与其他命令一起通过管道使用。
>
> `tee [OPTIONS] [FILE]`
>
> -a, --append	不覆盖文件，而是将输出追加到给定的文件中



-----------------



## shell工具和脚本

### 变量的定义 

`foo=bar`

变量的访问，使用`$`，例如`echo $foo`



字符串

单引号

```
str='this is a string'
```

单引号字符串的限制：

- 单引号里的任何字符都会原样输出，单引号字符串中的变量是无效的；
- 单引号字符串中不能出现单独一个的单引号（对单引号使用转义符后也不行），但可成对出现，作为字符串拼接使用。

双引号



```shell
your_name="runoob"
str="Hello, I know you are $your_name"
```

输出结果为：

```
Hello, I know you are "runoob"! 
```

双引号的优点：

- 双引号里可以有变量
- 双引号里可以出现转义字符

-------

### Shell 传递参数

我们可以在执行 Shell 脚本时，向脚本传递参数，脚本内获取参数的格式为 **$n**，**n** 代表一个数字，**1** 为执行脚本的第一个参数，**2** 为执行脚本的第二个参数。

例如可以使用 **$1、$2** 等来引用传递给脚本的参数，其中 **$1** 表示第一个参数，**$2** 表示第二个参数，依此类推。



另外，还有几个特殊字符用来处理参数：

| 参数处理 | 说明                                                         |
| :------- | :----------------------------------------------------------- |
| $#       | 传递到脚本的参数个数                                         |
| $*       | 以一个单字符串显示所有向脚本传递的参数。 如"\$"用「"」括起来的情况、以"\$1 \$2 … $n"的形式输出所有参数。 |
| $$       | 脚本运行的当前进程ID号                                       |
| $!       | 后台运行的最后一个进程的ID号                                 |
| $@       | 与$*相同，但是使用时加引号，并在引号中返回每个参数。 如"$@"用「"」括起来的情况、以"$1" "$2" … "$n" 的形式输出所有参数。 |
| $-       | 显示Shell使用的当前选项，与[set命令](https://www.runoob.com/linux/linux-comm-set.html)功能相同。 |
| $?       | 显示最后命令的退出状态。0表示没有错误，其他任何值表明有错误。 |
| $0       | 该bash文件名                                                 |



`!!`表示上一次执行的指令



### 标准重定向与错误重定向

```shell
>
2>
```



------------

### 检查shell脚本

`shellcheck`



----

### 工具-tldr

==tldr== 

==`TLDR` (Too Long; Didn’t Read) 是一个由社区维护的帮助页面集合, 相较于传统的使用手册, TLDR 旨在以更简单, 更易理解的方式为命令提供说明, 对于命令行新手十分友好==



### 查找文件

`find`	



### 历史指令

* `history` 会打印所有的bash指令
* 可以使用管道和grep处理输出，例如 `history | grep cd` 则会打印所有含有cd的指令
* 或者 `ctrl+r`反向搜索



### 模糊查找

`fzf`



### 目录和文件列表

`tree`

`broot`

