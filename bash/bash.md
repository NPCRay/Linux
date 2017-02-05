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

###10. 小括号`( )`
1. 命令组
内部变量为局部变量
2. 初始化数组
```bash
arr=(12 34 56 78)
a=123
(a=321；)
echo "a=$a"
echo ${arr[3]}```
结果是：
>a=123
>56


###11. 大括号`{ }`
1. 文件名扩展
```bash
cp t.{txt,bak}  #将文件t.txt拷贝到t.bak```
2. 代码块
同c和java中的`{}`
```bash
a=123
{ a=321; }
echo "a=$a"```
结果是
>a=321

###12. 中括号`[ ]` 
1. 条件测试
条件测试表达式放在`[ ]`中。值得注意的是`[`是shell内建test命令的一部分，并不是/usr/bin/test中的外部命令的一个链接。
2. 数组元素
```bash
arr=(12 34 56 78)
if[${arr[0]} -lt 20] #-lt (less then)表示小于
then
      echo "${arr[0]}"
else
       echo 'arr>10'
fi```

###13. 尖括号`< >`
1. 重定向


###14. 竖线`|`
管道
test.sh文件
```bash
#!/bin/bash
tr 'a-z' 'A-Z'
exit 0```
运行以下命令
```bash
chmod 755 test.sh
ls -l | ./test.sh```
输出的内容全部变成了大写字母

###15. 破折号`-`
1. 命令前缀
2. 用于重定向stdin或者stdout

###16. 波浪号`~`
home目录

##变量
###1.变量替换
```bash
a=123
hello=$a```

```
*******************************************************************************
# 强烈注意, 在赋值的的时候, 等号前后一定不要有空格.
#  "VARIABLE =value"
#                   ^
#% 脚本将尝试运行一个"VARIABLE"的命令, 带着一个"=value"参数.
#  "VARIABLE= value"
#                      ^
#% 脚本将尝试运行一个"value"的命令, 
#+ 并且带着一个被赋值成""的环境变量"VARIABLE". 
#*******************************************************************************```

```bash
echo hello    # hello 没有变量引用, 只是个hello字符串.

echo $hello  # 123
echo ${hello} # 123

echo "$hello" # 123
echo '$hello' # $hello
echo "${hello}" # 123```
>全引用（单引号）的作用将会导致"$"被解释为单独的字符, 而不是变量前缀. 
使用单引号引用变量时候，变量的值不会被引用，只是简单的保持原始字符串.
 
 在bash中，当变量中有空格、tab之类的字符时候，
 如果需要打印这些字符，需要用双引号进行引用 "$hello".
 
```bash
hello="A B  C     D"
echo $hello   # A B C D
echo "$hello" # A B  C     D```


```
hello=    # 设置为空值.
echo "\$hello (null value) = $hello"
#  注意设置一个变量为null, 与unset这个变量, 并不是一回事

echo "uninitialized_variable = $uninitialized_variable"
# Uninitialized变量为null(就是没有值). 
uninitialized_variable=   #  声明, 但是没有初始化这个变量,其实和前边设置为空值的作用是一样的. 
echo "uninitialized_variable = $uninitialized_variable"  # 还是一个空值.
uninitialized_variable=123       # 赋值.
unset uninitialized_variable    # Unset这个变量.
echo "uninitialized_variable = $uninitialized_variable" # 还是一个空值.```


###2. 变量赋值
1. 简单的赋值和变量替换一样
2. 结果赋值
```bash
a=`echo Hello\!` #注意是反引号
echo $a
a=`ls -l`
echo "$a"```

###3. 位置参数 
$0，$1，$2，$3...
>$0就是脚本文件自身的名字，$1 是第一个参数，$2 是第二个参数，$3 是第三个参数，然后是第四个。$9 之后的位置参数就必须用大括号括起来了，比如，${10}，${11}，${12}
**两个比较特殊的变量 $* 和 $@ 表示所有的位置参数**

##引用和转义
`\`作为转义符
`" "`部分应用
`' '`全引用（所有的特殊字符将全部失效）
在echo命令中有如下的转义命令：
>`\n` 新的一行
>`\r` 回车
>`\t` 水平制表
>`\v` 垂直制表
>`\b` 后退符
>`\a` 表示“alert”
>`\0xx` 转换为八进制的ASCII码
>`\"` 双引号
>`\'` 单引号
>`\\` 反斜杠
>`\$` $本身
>`\ ` 转义空格
>`\`+`enter` 转义enter

***注：在使用echo时，需加上-e 或者$'\X'这样的格式才可以实现转义命令***
```bash
echo "\v\v\v\v"      # 逐字的打印\v\v\v\v.
# 使用-e选项的'echo'命令来打印转义符.
echo -e "\v\v\v\v"   # 打印4个垂直制表符.```
```bash
echo -e "\042"       # 打印" (引号, 8进制的ASCII 码就是42).
# 如果使用$'\X'结构,那-e选项就不必要了.
echo $'\n'           # 新行.
echo $'\a'           # 警告(蜂鸣).
# 使用16进制的值,使用$'\xhhh' 结构.
echo $'\t \x22 \t'  # 被水平制表符括起来的引号(").
quote=$'\042'        # " 被赋值到变量中.
echo "$quote This is a quoted string, $quote and this lies outside the quotes."
# 变量中的连续的ASCII字符.
triple_underline=$'\137\137\137'  # 137是八进制的'_'.
echo "$triple_underline UNDERLINE $triple_underline"
ABC=$'\101\102\103\010'           # 101, 102, 103是八进制码的A, B, C.
echo $ABC
escape=$'\033'                    # 033 是八进制码的esc.
echo "\"escape\" echoes as $escape"
#                                   没有变量被输出.
exit 0```

**转义符`\`**
```bash
echo \z #z
echo "\z" #\z
echo \\z #\z
echo "\\z" #\z
echo \\\z #\z
echo "\\\z" #\\z
echo \\\\z #\\z
echo "\\\\z" #\\z```

##条件判断
