# Welcome to txl Pages

## 1.常见配置和安装问题的解决

### 添加.到PATH环境变量中去--当前用户级别

+ vim .bashrc
+ export PATH=$PATH:.
+ source .bashrc
![Image of Yaktocat](assets\images\bashrc.png)

---


## 2. shell语法--记不住系列

### 执行顺序
1. shell先进行变量的替换
2. 文件名的替换
3. 命令行解析成参数

---

### 单引号 与 双引号
> 单引号:不管什么都是原字符输出
> 双引号比单引号弱点,不忽略三种特使字符
> 1. $   #美元符号
> 2. ``  # 反引号
> 3. \   # 反斜杠


---

### 命令替换
1. 反引号
2. $()
---

### 参数
1. $1 $2 ... 位置参数
2. $# 参数个数
3. $* 所有参数
4. 当参数是两位数的时候 用${数字}
5. shift 左移一个参数

---

### test

操作符 | 满足为真
------------ | -------------
-d file | 目录
-e file | 存在
-f file | 普通文件
-r file | 可读
-s file | 不是空文件
-w file | 可写
-x file | 可执行
-L file | 符号链接


---


### cp命令的实现
```shell
## copy files

numargs=$#
filelist=
copylist=

#处理用户参数 分解成filelist 和 to
while [ "$#" -gt 1 ];do
	filelist="$filelist $1"
	shift
done

to="$1"

# 如果参数数目小于2 或者 参数数目大于2 但是to不是目录文件
if [ "$numargs" -lt 2 -o "$numargs" -gt 2 -a ! -d "$to" ];then
	echo "Usage:mycp file1 file2"
	echo "      mycp file(s) dir"
	exit 1
fi

# 遍历每一个文件
for from in $filelist;do
    # 判断是否是目录
	if [ -d "$to" ];then
		tofile="$to/$(basename $from)" # basename 命令 截取该文件名
	else
		tofile="$to"
	fi
    #判断文件是否存在
	if [ -e "$tofile" ] ;then
		echo "$tofile already exists;overwrite (yes/no)? \c"
		read answer

		if [ "$answer" = yes ];then
			copylist="$copylist $from"
		fi
	else
		copylist="$copylist $from"
	fi
done

if [ -n "$copylist" ] ;then
	cp $copylist $to
fi


```

### 菜单程序

> 目录结构
![Image of Yaktocat](assets\images\rolo1.png)

> rolo
```shell

# 为了便于修改和发行
PHONEBOOK=$HOME/phonebook
export PHONEBOOK

if [ ! -f "$PHONEBOOK" ];then
	echo "No phone book file in $HOME "
	exit 1
fi

if [ "$#" -ne 0 ];then
	lu "$@"
	exit
fi
validchoice=""

until [ -n "$validchoice" ]
do
echo -n '
	Would you like to:
		1.Look someone up
		2.add someone to the phone book
		3.Remove someone from the phone book
		q.quit
	Please select one of the above (1-3):'

read choice

echo ""
case "$choice"
in
	1) echo -n "Enter name to look up:"
	   read name
	   lu "$name";;
	2) echo -n "Enter name to be added:"
	   read name
	   echo -n "Enter number:"
	   read number
	   add "$name" "$number";;
	3) echo -n "Enter name to be remove:"
	   read name
	   rem "$name";;
	q) echo "quit"
           exit 1;;
	*) echo "Bad choice";;
esac
done

```

>rem
```shell
if [ "$#" -ne 1 ]
then
	echo "Incorrect number of arguments."
	echo "Usage:rem name"
	exit 1
fi

name=$1

matches=$(grep "$name" $PHONEBOOK | wc -l)

if [ "$matches" -gt 1 ]
then
	echo "More than one match;Please qualify further"
elif [ "$matches" -eq 1 ]
then
	grep -v "$name" $PHONEBOOK > /tmp/phonebook$$
	mv /tmp/phonebook$$ $PHONEBOOK
else
	echo "I could not find $name in the phone book"
fi


```

> lu
```shell
if [ "$#" -ne 1 ]
then
	echo "Incorrect nmber of arguments."
	echo "Usage:lu name"
	exit 1
fi


name=$1


if grep "$name" $PHONEBOOK 
then
	:
else
	echo "Not found or error occurs"
fi

```

>add
```shell
if [ "$#" -ne 2 ]
then
	echo "Incorrect numbers of arguments."
	echo "Usage:add name phone"
	exit 1
fi

name=$1
phone=$2

echo "$1	$2" >> $PHONEBOOK
sort -o $PHONEBOOK $PHONEBOOK

```

![Image of Yaktocat](assets\images\rolo.png)

---


## 3.有意思的命令(安装)

+ sudo apt-get install screenfetch
![Image of Yaktocat](assets\images\screenfetch.png)



## 4.环境

### export 命令
> 要明白子shell的概念 每当执行一个命令时,都是启动一个子shell来执行,所以相当于两个环境.父shell使用export将局部变量传递给所有的子shell(复制export变量到子shell中),而子shell修改export变量不影响父shell的export变量

+ export -p #输出所有的export变量
![Image of Yaktocat](assets\images\export.png)

---
### PS1 和 PS2

+ PS1 保存命令行提示符的环境变量
![Image of Yaktocat](assets\images\PS1.png)

+ PS2 保存辅助命令行提示符
![Image of Yaktocat](assets\images\PS2.png)

---

### HOME 
> 用于保存用户登陆系统后所处的位置 不要轻易改变

![Image of Yaktocat](assets\images\HOME.png)


### PATH
> PATH 环境变量的作用: 当你输入程序名的时候,shell会在一个目录列表里面查找指定的程序,直到找到为止,而这个目录列表就是HOME变量保存的目录,搜索顺序为谁在前,谁先搜索

![Image of Yaktocat](assets\images\PATH.png)

## 0.常见命令

### nl 命令 输出行号
+ nl file
![Image of Yaktocat](assets\images\nl.png)

> 简单实现

```shell
lineno=1
cat $* |
while read line
do
	echo "    $lineno : $line"
	lineno=$((lineno + 1))
done
```

![Image of Yaktocat](assets\images\nl2.png)


### 正则

```shell
.:任意字符
*:多个字符
+:重复之前的正则一次或多次
^:行首
$:行尾
[chars]: chars中的任意字符
[^chars]:不在chars中任意字符
\{min,max\}:重复之前的正则至少min次,最多max次
\(...\):将括号中匹配的字符保存到接下来的寄存器中(1-9)

```
---

### 重定向
+ ls n* 2>errors

ls n* 是没有该文件,所以会向标准错误输出一条信息,但是标准错误被重定向到文件errors

---

### ps

+ ps
+ ps -f

![Image of Yaktocat](assets\images\ps.png)

---

### ed 
+ ed file
+ 1,$p
+ / ... /
+ /the/
![Image of Yaktocat](assets\images\ed1.png)

+ 1,$s/^/>>/
+ 1,$p
+ /\.$/
+ 1,$s/..$//
+ 1,$p
![Image of Yaktocat](assets\images\ed2.png)

---

### cut -- 拆分行

> 默认情况下 分隔符为制表符

+ who \| cut -c1-3
+ who

![Image of Yaktocat](assets\images\cut1.png)

+ cut -d: -f1 /etc/passwd
![Image of Yaktocat](assets\images\cut2.png)


+ cut -d: -f1,6 /etc/passwd
![Image of Yaktocat](assets\images\cut3.png)

---

### paste -- 合并行

+ cat names
+ cat numbers
+ paste names numbers

![Image of Yaktocat](assets\images\paste1.png)

+ paste -d':' names numbes
![Image of Yaktocat](assets\images\paste2.png)

+ ls \| paste -d'&' -s - 
![Image of Yaktocat](assets\images\paste3.png)

---

### sed 流编辑器 命令类似于ed

> 不改变原始文件 
> sed命令放入单引号--好习惯
> 默认情况下 sed将输入的每一行都写入到标准输出中,不管其内容是否发生变化

+ sed -n l sth

![Image of Yaktocat](assets\images\sed1.png)

+ sed 's/Unix/UNIX/' file
+ sed 's/Unix/UNIX/g' file  
![Image of Yaktocat](assets\images\sed2.png)

+ sed '1,2p' file
+ sed -n '1,2p' file  
![Image of Yaktocat](assets\images\sed3.png)

+ sed '/Unix/d' file  
![Image of Yaktocat](assets\images\sed4.png)

+ sed '1,2d' file  
![Image of Yaktocat](assets\images\sed5.png)

### tr

字符 | 八进制值
------------ | -------------
Bell|7
Backspace|10
Tab|11
Newline|12
---

+ data \| tr ' ' ' ' '\12'  
![Image of Yaktocat](assets\images\tr1.png)

+ tr e x < file
![Image of Yaktocat](assets\images\tr2.png)

+ cat lostaspaces
![Image of Yaktocat](assets\images\tr3.png)

+ tr -d ; ' < file
![Image of Yaktocat](assets\images\tr4.png)

---

### grep
> 可以在单个文件 或 多个文件中 查找

![Image of Yaktocat](assets\images\grep1.png)
![Image of Yaktocat](assets\images\grep2.png)
![Image of Yaktocat](assets\images\grep3.png)
![Image of Yaktocat](assets\images\grep4.png)
![Image of Yaktocat](assets\images\grep5.png)
![Image of Yaktocat](assets\images\grep5-1.png)
![Image of Yaktocat](assets\images\grep6.png)

---

### sort

+ sort -u names #消除重复项
![Image of Yaktocat](assets\images\sort.png)

+ sort -r names #逆序
![Image of Yaktocat](assets\images\sort1.png)

+ sort names -o sorted_names # 将已排好序的输出到指定文件
![Image of Yaktocat](assets\images\sort2.png)

+ sort -k2n num #按第二个字段以算术排序 默认以制表符或着空格分隔 指定分隔符 -t 
![Image of Yaktocat](assets\images\sort4.png)

+ sort num 
+ sort -n num
![Image of Yaktocat](assets\images\sort3.png)

---

### uniq

+ uniq -d names # 列出重复项
![Image of Yaktocat](assets\images\uniq.png)


+ sort names | uniq -c #统计出现次数
![Image of Yaktocat](assets\images\uniq2.png)

---