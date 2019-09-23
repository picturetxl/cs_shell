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

## 0.常见命令

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