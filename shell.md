# shell



```shell
a=hello；echo $a

# 如果打印的字符之间有空格，需要用引号

b="hello_world"；echo $b



# 单引号和双引号的区别
x=5；y=7

# 会将x=5的部分打印，如果是单引号，则不会引用x=5
echo "abc $x" 

# 为了避免歧义，将变量使用{}包围
d=abc；echo ${d}_1；echo ${pwd}；echo $HOME；echo $PATH

# 定义数组
a=(1 2 3 4 5) 

# 输出数组中的所有元素
echo ${a[@]}	

# 输出数组中的所有元素
echo ${a[*]}	

# 输出数组中的元素个数
echo ${#a[*]}	

# 未开启转译模式，输出 a\nbb
echo "a\nbb"  

# 开启转译模式 输出 a 换行 bb
echo -e "a\nbb" echo -e 

x=4；y=6
# 	(( ... )) 进行算术比较。具体来说，它比较 x 和 y 的大小。如果 x 小于 y，则表达式返回 true，否则返回 false。
((x < y)) 		

#	在 Shell 中，命令的退出状态码是一个整数，0 表示成功（或 true），非 0 表示失败（或 false）
echo $?	
```



###### 算术比较

```shell
[ 2 -eq 2 ]	相等				-eq equal
[ 2 -ne 2 ] 不等				-ne not equal
[ 3 -gt 1 ] 大于 				-gt great than
[ 3 -lt 4 ] 小于		 		-lt less  than
[ 3 -ge 3 ] 大于等于		 -gt great equal
[ 3 -le 3 ] 小于等于		 -le less  equal
```



###### if结构

```shell
if [ condition ];then...;fi
if [ condition ];then...;else...;fi
if [ condition ];then...;elif...;fi
```

简单的逻辑语句可以使用 &&   ||  代替

[ -e demo2.sh ] && echo exist ||echo not exist

```shell
# 判断demo2.sh 是否存在， && 前面条件为true时，执行 echo exist  ｜｜ 前面条件为False时，执行 echo not exist
[ -e deom2.sh ] && echo exist || echo not exist
```



###### while结构

```
while xxx;do xxx;done
```



shell的各种括号

```
()		# 在子shell中运行
$(ls)	# 表示执行ls后的结果
{}		# 当前shell中执行
$$ 		# 当前脚本执行的pid
& 		#	后台执行
$! 		# 运行在后台的最后一个作业的pid（进程ID）
```

 

###### grep命令

```shell
curl https://testing-studio.com ｜ grep href

grep -o 将筛选后的内容 进行逐行展示
grep -A	将命中行的后面几行打印出来	
maiqi@Qis-MacBook-Air shell % seq 10 | grep -A 2 3  	# 将3和后面2行打印
3
4
5

grep -B 将命中行的前面几行打印出来
maiqi@Qis-MacBook-Air shell % seq 10 | grep -B 2 3		# 将3和前面2行打印
1
2
3

grep -C 将命中行的前后几行打印出来
maiqi@Qis-MacBook-Air shell % seq 10 | grep -C 2 3		# 将3和前后各2行打印
1
2
3
4
5

curl https://ke.qq.com/ | grep href

# curl 不显示统计信息
# grep -o 显示筛选出的数据，并单独成行
"http[^\"']*" 匹配http开头，
[^\"']任何不是双引号或单引号的字符
*匹配后面一个或者多个字符
curl -s https://ke.qq.com/ | grep href | grep -o "http[^\"']*"   

# curl -I 发送一个http请求获取制定url的相应头信息，返回200则正常
curl -I https://ke.qq.com/cource/254956
```



```shell
-v 	 		在verbase中打开。
2>&1 		将标准错误输出（文件描述符2）重定向到标准输入（文件描述符1）
|		 		管道符号，将前一个命令的输出传递给后一个命令作为输入
less		分页符号，用于逐页查看长文本文件
curl -s https://ke.qq.com/cource/254956 -v 2>&1 | less
```



```shell
curl -s https://testing-studio.com/ | grep href | grep -o "http[^\"']*" | while read line;do curl -s -I $line | grep 200 &&echo 200 $line || echo $line Error;done
```



```
grep -v  file 不显示匹配的行
```



##### 三剑客

###### grep	用于数据定位查找	类比SQL：grep=select * from table

###### awk	 用于数据切片		类比SQL：awk=select field from table

###### **sed 	 用于数据修改		类比SQL：sed=update table set field=new where field=old**



###### awk命令

```shell
# awk 匹配log.txt 中所有为200的行，并进行分组和去重
awk '/200/ {print} log.txt | sort | uniq -c
awk '$0~/200/ {print }' log.txt | sort | uniq -c

# 匹配第2列的含有200的行
awk '$2~/200/ {print}' log.txt | sort | uniq -c
```



```shell
awk的正则匹配
seq 10 | awk '/^6$/'	# awk的正则匹配，要以/一对进行包裹，^表示开头，$表示结尾
6
seq 10 | awk '/^[5-9]$/' # awk正则匹配，输出5-9
5 6 7 8 9

seq 20 | awk '/^[5-9]$/ | /1[5-9]$/'
5 6 7 8 9 15 16 17 18 19

seq 20 | awk '/15/,/19/' # awk 正则匹配，15开始到19结束的所有数字

awk -F
echo " 1|2_3#4" | awk -F '[#|_]' '{print $3}' awk -F 以正则的方式，# | _分隔数字，拆分输入行

awk 'BEGIN{RS=":"}{print $0}'
root@04b25dd4f42a:/# echo $PATH
/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin

RS为记录分隔符，这里的记录分隔符为:
echo $PATH | awk 'BEGIN{RS=":"}{print $0}'
/usr/local/sbin
/usr/local/bin
/usr/sbin
/usr/bin
/sbin
/bin

$0	表示原来的行；$1	表示第一个字符；$N	表示第N个字符；$NF	表示最后一个字符
```



###### sed命令

```shell
替换	s--substitute（替换）

替换全部行
# 将input.txt文本中所有的【foo】替换为【bar】
sed 's/旧文本/新文本'
eg:	's/foo/bar/' input.txt

替换指定行
# 
sed '2s/foo/bra/'		# 只替换第二行的第一个匹配项
sed '2,5s/foo/bar/'	# 替换第二行到第五行的所有匹配项

删除行	d--delete（删除）
sed '删除命令' 文件
sed '2d' input.txt

插入和追加	i---insert（插入）  a ---append（追加）
sed '2i\Inserted line' input.txt	# 在第二行插入inserted line
sed '3a\Appended line' input.txt	# 在第三行追加Appended line

$a\	命令指示sed在最后一行后面追加内容
$'\n'插入一个换行符号
sed '$a\'$'\n''end of the file' input.txt
```



###### seq命令

```
# seq 10 生成1-10的序列数字
grep -E ‘3｜7’	正则匹配 3和 7
seq 10 | grep -E '3|7' 
```





###### 正则表达式

```
基本式	
^ 开头			$ 结尾			[0-9][a-z]区间			* 0个或多个
```



