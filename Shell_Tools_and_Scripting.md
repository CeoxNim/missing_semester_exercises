# Shell_Tools_and_Scripting

1. 阅读man ls，使用ls命令进行如下操作

```bash
ls -aht --color=auto
```

- -a：所有文件，包括隐藏文件
- -h：文件打印以人类可以理解的格式输出（如454M）
- -t：文件以最近访问顺序排序
- —color=auto：以彩色文本显示输出结果

1. 编写两个bash函数。每当执行macro时，当前的工作目录应当以某种形式保存；当执行polo时，无论现在处在什么目录下，都应当cd回到当时执行的macro目录。

```bash
#!/bin/bash
macro(){
	echo "$(pwd)" > $HOME/macro_history.log
	echo "save pwd $(pwd)"
}

polo(){
	cd "$(cat "$HOME/macro_history.log")"
}
```

1. 编写一段脚本，运行如下的脚本直到它出错，将它的标准输出和标准错误记录到文件，并在最后输出所有的内容。加分项：报告脚本在失败前共运行了多少次。

```bash
# buggy.sh
#!/usr/bin/env bash

 n=$(( RANDOM % 100 ))

 if [[ n -eq 42 ]]; then
     echo "Something went wrong"
     >&2 echo "The error was using magic numbers"
     exit 1
 fi

 echo "Everything went according to plan"
```

```bash
for ((count=1;;count++)); do 
    ./buggy.sh 2> out.log
    if [[ $? -ne 0 ]]; then 
        echo "failed after $count times"
        cat out.log 
        break 
    fi 
done
```

1. 编写一个命令，它可以递归查找文件夹中所有的HTML文件，并将它们压缩成zip文件

```bash
find . -name "*.html" -print0 | xargs -0 tar cvf html.zip
```

1. 编写一个命令或脚本递归的查找文件夹中最近使用的文件。更通用的做法，你可以按照最近的使用时间列出文件吗？

```bash
find . -type f -mmin 60 -print0 | xargs -0 ls -lt | head -10
```