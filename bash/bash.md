#Bash
##特殊字符
### 1. `#`注释
 注解`#`(除`#!`),`#!`是用于指定当前脚本的解释器
 ```bash
 #!/bin/bash

 echo "The # here does not begin a comment."
 echo 'The # here does not begin a comment.'
 echo The \# here does not begin a comment.
 echo The # 这里开始一个注释

 echo ${PATH#*:}         # 参数替换，不是一个注释
 echo $(( 2#101011 ))   # 数制转换（使用二进制表示），不是一个注释```
 输出结果：
 >The # here does not begin a comment.
 >The # here does not begin a comment.
 >The # here does not begin a comment.
 >The
 >/user/bin:/bin:/usr.local/games
 >43
 
 ### 2. `;`分号
 1. 命令分隔符
 2. 终止case选项（双分号`;;`）
```bash
 #!/bin/bash
 varname=b
 case "$varname" in
     [a-z]) echo "abc";;
     [0-9]) echo "123";;
 esac```
 结果
 > abc


### 3. 点号`.`
等价于`source`，在当前的bash环境中读取并执行shell脚本

### 4. 引号`""`、`''`
字符串，`''`比`""`能解释大部分的特殊字符

### 5. 斜杠`/`、反斜杠`\`
`/`文件名路径分割符，也可以用作除法
`\`转义符
### 6. 反引号 `
将输出赋值到一个变量中去。在语句中优先执行
```bash
$ cp `mkdir back` test.sh back```

### 7. 冒号`:`
1. 空命令
等价于`NOP`（no op，一个什么都不干的命令）。与内建命令true相同。他的退出码（exit status）是（0）
```bash
#!/bin/bash
while :
do
    echo "endless loop"
done```
等价于
```bash
#!/bin/bash
while true
do
    echo "endless loop"
done```
在if/then中占位，什么多不干
```bash
#!/bin/bash
condition=5
if [ $condition -gt 0 ]
then :   # 什么都不做，退出分支
else
    echo "$condition"
fi```
2. 变量扩展/子串替换
在与`>`重定向操作符结合使用时，将会把一个文件清空，但是并不会修改这个文件的权限。如果之前这个文件并不存在，那么就创建这个文件。
与`>>`合用将不会对预先存在的目标文件产生任何影响。如果这个文件之前并不存在，那么就创建它。
```bash
: > xxxx.txt```
3. 环境变量中的分隔符


###8. 问号`?`
三目运算符

###9. 美元符`$`
1. 变量替换
2. 命令替换（同反引号）
```bash
$ cd $(echo Documents)
$ pwd```
3. bash的前缀标识符



