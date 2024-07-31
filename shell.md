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

```
if [ condition ];then...;fi
if [ condition ];then...;else...;fi
if [ condition ];then...;elif...;fi
```

简单的逻辑语句可以使用 &&   ||  代替

[ -e demo2.sh ] && echo exist ||echo not exist

```
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

 

```
1	3
2	13
3	7
4	7
5	16
6 10
7 25
8 6
9 10
11 16
12 2
13 13
14 8
15 4
```

