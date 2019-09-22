## Welcome to txl Pages


###正则
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

###重定向
> ls n* 是没有该文件,所以会向标准错误输出一条信息,但是标准错误被重定向到文件errors
```shell
ls n* 2>errors
```
### ps
+ ps
+ ps -f 
![Image of Yaktocat](assets\images\ps.png)

### ed 
![Image of Yaktocat](assets\images\ed1.png)
![Image of Yaktocat](assets\images\ed2.png)

###cut -- 拆分行
> 默认情况下 分隔符为制表符

![Image of Yaktocat](assets\images\cut1.png)
![Image of Yaktocat](assets\images\cut2.png)
![Image of Yaktocat](assets\images\cut3.png)

###paste -- 合并行
```shell
paste -d':' names numbers #以:将names的每行和numbers的每行连接起来

ls | paste -d'&' -s - #-s表示paste只从单个文件中粘贴行 -表示标准输入

```
![Image of Yaktocat](assets\images\paste1.png)
![Image of Yaktocat](assets\images\paste2.png)
![Image of Yaktocat](assets\images\paste3.png)


###sed 流编辑器 命令类似于ed
> 不改变原始文件 
> sed命令放入单引号--好习惯
> 默认情况下 sed将输入的每一行都写入到标准输出中,不管其内容是否发生变化

```shell
sed -n l filename
```
![Image of Yaktocat](assets\images\sed1.png)
![Image of Yaktocat](assets\images\sed2.png)
![Image of Yaktocat](assets\images\sed3.png)
![Image of Yaktocat](assets\images\sed4.png)
![Image of Yaktocat](assets\images\sed5.png)

###tr
```shell
tr -s ' ' ' ' <file # 将多个空格压缩成一个空格 -s
tr -d ' ' <file #删除空格
```

字符 | 八进制值
------------ | -------------
Bell|7
Backspace|10
Tab|11
Newline|12

![Image of Yaktocat](assets\images\tr1.png)
![Image of Yaktocat](assets\images\tr2.png)
![Image of Yaktocat](assets\images\tr3.png)
![Image of Yaktocat](assets\images\tr4.png)


###grep
> 可以在单个文件 或 多个文件中 查找

![Image of Yaktocat](assets\images\grep1.png)
![Image of Yaktocat](assets\images\grep2.png)
![Image of Yaktocat](assets\images\grep3.png)
![Image of Yaktocat](assets\images\grep4.png)
![Image of Yaktocat](assets\images\grep5.png)
![Image of Yaktocat](assets\images\grep5-1.png)
![Image of Yaktocat](assets\images\grep6.png)

###sort
```shell
sort -u names #消除重复项
sort -r names #逆序
sort names -o sorted_names # 将已排好序的输出到指定文件
sort -k2n num #按第二个字段以算术排序 默认以制表符或着空格分隔 指定分隔符 -t 
```
![Image of Yaktocat](assets\images\sort.png)
![Image of Yaktocat](assets\images\sort1.png)
![Image of Yaktocat](assets\images\sort2.png)
![Image of Yaktocat](assets\images\sort3.png)
![Image of Yaktocat](assets\images\sort4.png)

###uniq
```shell
uniq -d names # 列出重复项
sort names | uniq -c #统计出现次数
```

![Image of Yaktocat](assets\images\uniq.png)
![Image of Yaktocat](assets\images\uniq2.png)