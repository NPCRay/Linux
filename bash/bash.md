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
`$*`位置参数被看作一个单词
`$@`位置参数被看作独立引用的单词同数组

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

##操作符
###赋值`=`
###算术
`+`	加法计算
`-`	减法计算
`*`	乘法计算
`/`	除法计算
`**`	幂运算
`%`	模运算，或者是求余运算（返回一次除法运算的余数）
###位操作符
`<<` 左移一位（每次左移都相当于乘以2）
`<<=` “左移-赋值”
`let "var <<= 2" #这句的结果就是变量var左移2位(就是乘以4)`
`\>>` 右移一位（每次右移都将除以2）
`\>>=` “右移-赋值”（与<<=正好相反）
`&` 按位与
`&=` “按位与-赋值”
`|` 按位或
`|=` “按位或-赋值”
`～` 按位反
`!` 按位非
`^` 按位异或XOR
`^=` “按位异或-赋值”
###逻辑运算符
`&&`和`||`



##退出码
每一条命令执行后都会产生退出状态码`$?`用来表示上一条运行的结果，成功返回0，不成功就是非0值
```
echo $?```


##条件判断
### **if/then**
`[ ]`等价于`test `
```
if [ 1 -eq 1 ]
then echo "equal"
else echo "unequal"
fi

if test 1 -lt 1
then echo "equal"
else echo "unequal"
fi

if grep -q bash file
then echo "FIle contauns at least one occurrence of bash"
fi```


### **多级比较（不是嵌套条件分支）**
```
# 这里应该理解为子if/then当做一个整体作为测试条件
if echo "Next *if* is part of the comparison for the first *if*."
   if [[ $comparison = "integer" ]]
     then (( a < b )) # (( 算数表达式 ))， 用作算数运算
   else
     [[ $a < $b ]]
   fi
then
 echo '$a is less than $b'
fi```
根据内if执行语句返回的退出码来进行判断

### `test`即`[ ]`真假判断
所有的字符串，数字都为真，未定义的变量，定义了未赋值的变量，“NULL”，空值为假

###**((  ))**
用于计算并测试算术表达式的结果,内部用的是C语言的形式
结果为0或false退出码返回大于`1`的数，为除0以外或者true时，返回退出码为`0`
`a=$(( 5 + 3 ))`，将把变量“`a`”设为“`5 + 3`”，或者`8`。


### **操作符列举**
就是`test`即`[ ]`加参数
`-e`	文件存在
`-a`	文件存在，这个选项的效果与 -e 相同。但是它已经被“弃用”了，并且不鼓励使用。
`-f`	表示这个文件是一个一般文件（并不是目录或者设备文件）
`-s`	文件大小不为零
`-d`	表示这是一个目录
`-b`	表示这是一个块设备（软盘，光驱，等等）
`-c`	表示这是一个字符设备（键盘，modem，声卡，等等）
`-p`	这个文件是一个管道
`-h`	这是一个符号链接
`-L`	这是一个符号链接
`-S`	表示这是一个socket
`-t`	文件（描述符）被关联到一个终端设备上，这个测试选项一般被用来检测脚本中的 stdin([ -t 0 ]) 或者 stdout([ -t 1 ])是否来自于一个终端
`-r`	文件是否具有可读权限（指的是正在运行这个测试命令的用户是否具有读权限）
`-w`	文件是否具有可写权限（指的是正在运行这个测试命令的用户是否具有写权限）
`-x`	文件是否具有可执行权限（指的是正在运行这个测试命令的用户是否具有可执行权限）
`-g`	set-group-id(sgid)标记被设置到文件或目录上
`-k`	设置粘贴位
`-O`	判断你是否是文件的拥有者
`-G`	文件的group-id是否与你的相同
`-N`	从文件上一次被读取到现在为止, 文件是否被修改过
`f1 -nt f2`	文件f1比文件f2新
`f1 -ot f2`	文件f1比文件f2旧
`f1 -ef f2`	文件f1和文件f2是相同文件的硬链接
`!`	“非”，反转上边所有测试的结果（如果没给出条件，那么返回真）

###**二元比较操作符**
####**整数比较**
`-eq` 等于
`-ne` 不等于
`-gt` 大于
`-ge` 大于等于
`-lt` 小于
`-le` 小于等于
`<` 小于(在双括号中使用)
`<=` 小于等于(在双括号中使用)
`>`大于(在双括号中使用)
`=` 大于等于(在双括号中使用)
####**字符串比较**
`=` 等于
`==` 等于，与=等价
`!=` 不等号
`<` 小于，按照ASCII字符进行排序
注意"<"使用在[ ]结构中的时候需要被转义否则变成管道输出
`>` 大于，按照ASCII字符进行排序
注意“>”使用在[ ]结构中的时候需要被转义
`-z` 字符串为“null”，意思就是字符串长度为零 -n 字符串不为“null”

##循环和分支
###`for`循环
```bash
for planet in Mercury Venus Earth Mars Jupiter Saturn Uranus Neptune Pluto
do
echo $planet  # 每个行星都被单独打印在一行上.
done

echo

for planet in "Mercury Venus Earth Mars Jupiter Saturn Uranus Neptune Pluto"
# 所有的行星名称都打印在同一行上.
# 整个'list'都被双引号封成了一个变量.
do
echo $planet
done```
###`while`循环
```bash
var0=0
LIMIT=10
while [ "$var0" -lt "$LIMIT" ]
do
  echo -n "$var0 "        # -n 将会阻止产生新行.

  var0=`expr $var0 + 1`   # var0=$(($var0+1))  也可以.
  # var0=$((var0 + 1)) 也可以.
  # let "var0 += 1"    也可以.
done                      # 使用其他的方法也行.```

###`until`循环
```bash
END_CONDITION=end
until [ "$var1" = "$END_CONDITION" ]
# 在循环的顶部进行条件判断.
do
  echo "Input variable #1 "
  echo "($END_CONDITION to exit)"
  read var1
  echo "variable #1 = $var1"
  echo
done```

###嵌套循环
```
outer=1             # 设置外部循环计数.

# 开始外部循环.
for a in 1 2 3 4 5
do
  echo "Pass $outer in outer loop."
  echo "---------------------"
  inner=1           # 重置内部循环计数.
  # ===============================================
  # 开始内部循环.
  for b in 1 2 3 4 5
  do
    echo "Pass $inner in inner loop."
    let "inner+=1"  # 增加内部循环计数.
  done
# 内部循环结束.
# ===============================================
  let "outer+=1"    # 增加外部循环的计数.
  echo              # 每次外部循环之间的间隔.
done
# 外部循环结束.```
###循环控制
1. `break`and`continue`
###测试与分支
`case（in）`等同于其他语言的`switch case`
```bash
echo "Hit a key, then hit return."
read Keypress
case "$Keypress" in
[[:lower:]]   ) echo "Lowercase letter";;
[[:upper:]]   ) echo "Uppercase letter";;
[0-9]         ) echo "Digit";;
 *             ) echo "Punctuation, whitespace, or other";;
esac      #  允许字符串的范围出现在[中括号]中```

2. `select`
```bash
PS3='Choose your favorite vegetable: ' # 设置提示符字串.
echo
select vegetable in "beans" "carrots" "potatoes" "onions" "rutabagas"
do
  echo
  echo "Your favorite veggie is $vegetable."
  echo "Yuck!"
  echo
  break  # 如果这里没有 'break' 会发生什么?
done```

##内置变量
`$BASH`,`$FUNCNAME`,`$IFS`(内部域分隔符),`$REPLY`(最后read匿名输入变量)

```bash
# $IFS 处理空白与处理其他字符不同.
output_args_one_per_line()
{
  for arg
  do echo "[$arg]"
  done
}

echo; echo "IFS=\" \""
echo "-------"

IFS=" "
var=" a  b c   "
output_args_one_per_line $var  # output_args_one_per_line `echo " a  b c   "`
#
# [a]
# [b]
# [c]

echo; echo "IFS=:"
echo "-----"
IFS=:
var=":a::b:c:::"               # 与上边一样, 但是用" "替换了":".
output_args_one_per_line $var
#
# []
# [a]
# []
# [b]
# [c]
# []
# []
# []

# 同样的事情也会发生在awk的"FS"域中.
echo```







