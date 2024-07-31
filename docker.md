

# docker常用命令



##### docker部署linux

1. ###### 拉取linux镜像

   ```
   docker pull ubuntu
   ```

2. ###### 运行linux容器

   ```
   docker run -it ubuntu
   ```

3. ###### 进入容器

   ```
   docker exec -it hardcore_mclean /bin/bash
   ```

   

4. ###### 管理容器

   ```
   docker ps 	## 查看正在运行的容器
   docker stop <contain_id>	## 停止正在运行的容器
   docker rm <contain_id>		## 删除正在运行的容器
   ```

   

5. ###### 保存更改

   ```
   docker commit <container_id> my_custom_ubuntu  ## 修改并希望保存这些更改，可以使用 docker commit 命令创建一个新的镜像
   ```



###### 输入和输出重定向

```shell
root@04b25dd4f42a:/data# cat 1	# 查看1文件
hello from otherside
root@04b25dd4f42a:/data# echo "hello to otherside" > 1 # 输出重定向，已覆盖的形式，将字符追加到1中
root@04b25dd4f42a:/data# cat 1	# 查看1文件
hello to otherside
root@04b25dd4f42a:/data# echo "hello from otherside" >> 1	# 输出重定向，以追加的形式，将字符添加到1中
root@04b25dd4f42a:/data# cat 1
hello to otherside
hello from otherside

root@04b25dd4f42a:/data# grep "little"  demo.txt		# 匹配demo.txt文件中包含little的行，并打印
little

root@04b25dd4f42a:/data# grep "hello" demo.txt			# 	默认区分大小写，进行匹配
hello from
root@04b25dd4f42a:/data# grep -i "hello" demo.txt		# grep -i 不区分大小写，进行匹配
hello from
HELLO FROM

root@04b25dd4f42a:/data# cat demo.txt | grep -i "hello"	# 使用管道符，进行不区分大小写的匹配
hello from
HELLO FROM

root@04b25dd4f42a:/data# cat demo.txt | grep -io "hello"	# 参数 -o 只显示筛选的文字
hello
HELLO

root@04b25dd4f42a:/data# echo abcdefg | grep -o "c.*"		# 匹配c后面的所有内容，并输出
cdefg

root@04b25dd4f42a:/data#



abcdefg

root@04b25dd4f42a:/data# echo abcdefg | grep -o "c."	# 匹配c和后面的一个字符，并输出
cd
```



###### awk -F 命令

```
apt-get install xxx	# 如果linux提示 bash: xx: command not found

#!/bin/bash
curl -s http://www.baidu.com/s?wd=mp3 | grep -o "result:[0-9,]*"		# [0-9,]* 能够匹配0-9和逗号的任意字符串

echo "123|456|789" | awk -F '|' '{print $1}'	# awk -f '|' 	# 以 ｜ 为分隔符进行区分
echo "123|456|789" | awk -F '[|+]' '{print $1}'	# awk -f '|+' # 以 +或者 ｜ 为分隔符进行区别
echo "cat dog,fish" | awk -F '[,| ]' '{print $2}'
```



管道符

```
cat passwd | awk -F '[:]' '{print $1}'	读取passwd文件，并将：前面的第一列进行打印
cat passwd | awk -F '[:]' '{print $1,$5}'	读取passwd文件，并将第一列和第五列的内容进行打印
cat passwd | awk -F '[:]' '{print $1"\t"$5}'	读取passwd文件，并在第一列和第五列中间的输出加入制表位置

curl -s "http://www.baidu.com/s?wd=mp3" | grep -o "结果约[0-9,]*" | awk -F '[个约]' '{print $2}'

	1.	curl -s "http://www.baidu.com/s?wd=mp3":	# curl -s 默认不打印统计信息
	•	这会静默地请求百度搜索 mp3 的结果页面。
	2.	管道 |:
	•	将 curl 的输出传递给 grep。
	3.	grep -o "结果约[0-9,]*":
	•	使用正则表达式匹配并仅输出包含搜索结果数量的部分。例如，匹配类似 结果约123,456个 的文本。
	4.	awk -F '[个约]' '{print $2}':
	•	将 grep 输出的文本以 个 和 约 作为字段分隔符，提取第二个字段。
	•	如果 grep 的输出是 结果约123,456个，字段会被分成三个部分：结果、123,456、和空字符，{print $2} 打印第二部分，即 123,456。
```



###### sed  's\old\new' 命令

```
echo "cat dog fish" | sed 's/cat/wulala/'	# 使用wulala替换掉cat,最后要使用/结束整个替换
cat demo.txt | sed 's/hello/he/'					# 这种方法，只是将文件进行其他地方备份更改，并未实际实际替换文件内容
sed -i.bak 's/he/hello/' demo.txt					# 替换内容的同时，生成备份文件
```



###### 参数脚本

```
maiqi@Qis-MacBook-Air shell % cat test1.sh 
echo "获取脚本执行的参数： $0";
echo "获取第一个参数：$1";
echo "获取第二个参数：$2";
echo "获取参数的个数：$#";
echo "获取到的参数(str):$*";
echo "获取到的参数(每一个参数都是一个str): $@";
echo "获取到当前进程的ID(PID):$$";


maiqi@Qis-MacBook-Air shell % ./test1.sh 1 2 3 4 5
获取脚本执行的参数： ./test1.sh
获取第一个参数：1
获取第二个参数：2
获取参数的个数：5
获取到的参数(str):1 2 3 4 5
获取到的参数(每一个参数都是一个str): 1 2 3 4 5
获取到当前进程的ID(PID):30373

```





```
a=$(curl -s -L https://testerhome.com/topics | grep -o 'href="/topics/[0-9]*"' | awk -F '[/|"]' '{print $4}')
for id in $a; do
  url="https://testerhome.com/topics/$id"
  page=$(curl -s -L "$url")
  echo "Fetching $url"
  echo "$page" | grep -o -m1 '<span>[0-9]*'
  zan=$(echo "$page" | grep -o -m1 '<span>[0-9]*' | awk -F '>' '{print $2}')
  if [ -n "$zan" ]; then
    echo "$url 点赞人数 $zan"
  else
    echo "$url 点赞人数 -"
  fi
done

这段代码的功能是从 `http://testerhome.com/topics` 页面提取所有主题的 ID，并逐个访问这些主题页面，提取点赞人数并显示每个主题的点赞人数。以下是逐行解释：

href="/topics/1234"

使用分隔符 / 和 " 进行分割：

	1.	第一个分隔符之前是 href=
	2.	第一个分隔符 / 之后是 topics
	3.	第二个分隔符 / 之后是 1234
	4.	引号 " 作为分隔符

实际分割结果：

	•	$1 是 href=
	•	$2 是空字符串（因为第一个引号之前和第一个斜杠之间没有内容）
	•	$3 是 topics
	•	$4 是 1234

再详细拆解下，awk -F '/|"' '{print $4}' 的实际操作过程如下：

	•	href= 和第一个引号 " 之间的部分是 $1，即 href=
	•	第一个引号 " 和第一个斜杠 / 之间的部分是空字符串，因此 $2 是空字符串
	•	第一个斜杠 / 和第二个斜杠 / 之间的部分是 topics，因此 $3 是 topics
	•	第二个斜杠 / 之后的部分是 1234，因此 $4 是 1234


1. `a=$(curl -s -L https://testerhome.com/topics | grep -o 'href="/topics/[0-9]*"' | awk -F '/|"' '{print $4}')`
   - `curl -s -L https://testerhome.com/topics`：使用 `curl` 静默地下载 `http://testerhome.com/topics` 页面内容，并跟随重定向。
   - `grep -o 'href="/topics/[0-9]*"'`：从页面内容中提取所有匹配 `href="/topics/[数字]` 格式的字符串。
   - `awk -F '/|"' '{print $4}'`：使用斜杠 `/` 和引号 `"` 作为分隔符，提取匹配字符串中的数字部分（主题 ID）。
   - `a=...`：将提取的所有主题 ID 赋值给变量 `a`。

2. `for id in $a; do`
   - 开始一个循环，遍历变量 `a` 中的每个主题 ID，并将当前主题 ID 赋值给变量 `id`。

3. `url="https://testerhome.com/topics/$id"`
   - 构造主题 URL，将变量 `id` 拼接到 `https://testerhome.com/topics/` 后，赋值给变量 `url`。

4. `page=$(curl -s -L "$url")`
   - 使用 `curl` 静默地下载 `url` 页面内容，并跟随重定向，将页面内容赋值给变量 `page`。

5. `echo "Fetching $url"`
   - 打印当前正在处理的主题 URL。

6. `echo "$page" | grep -o -m1 '<span>[0-9]*'`
   - 打印页面内容，并使用 `grep` 提取第一个匹配 `<span>` 标签的字符串，这通常包含点赞人数。

7. `zan=$(echo "$page" | grep -o -m1 '<span>[0-9]*' | awk -F '>' '{print $2}')`
   - 从页面内容中提取第一个匹配 `<span>` 标签的字符串，并使用 `awk` 提取 `<span>` 标签中的数字部分（点赞人数），赋值给变量 `zan`。

8. `if [ -n "$zan" ]; then`
   - 判断变量 `zan` 是否非空（即是否成功提取到点赞人数）。

9. `echo "$url 点赞人数 $zan"`
   - 如果变量 `zan` 非空，打印当前主题 URL 和点赞人数。

10. `else`
    - 如果变量 `zan` 为空，执行以下操作。

11. `echo "$url 点赞人数 -"`
    - 打印当前主题 URL 和 `-`（表示未找到点赞人数）。

12. `fi`
    - 结束 `if` 条件判断。

13. `done`
    - 结束 `for` 循环。

整体代码会依次提取每个主题的 ID，访问对应的页面，尝试提取点赞人数，并打印每个主题的 URL 和点赞人数。

如果点赞数始终为空，可能是因为页面结构与假设的不一致。在这种情况下，需要仔细检查页面内容并调整 `grep` 和 `awk` 命令以正确匹配点赞数。
```



###### echo -e 开启转译

echo -e "1|2|3\n4|5|6\n7|8|9"

```
echo -e "1|2|3\n4|5|6\n7|8|9" | awk -F '|' 'BEGIN{a=0} {a+=$2} END{print a}'

awf -F '|' 以分号逐行读取
awk语法中规定：BEGIN在所有操作之前执行，END在所有操作只有执行
BEGIN 和 END 必须大写
BEGIN 和 END 模块必须写在单引号内部
```



###### shell 算数的四则运算

```
read -p "Input Number_1" num_1
read -p "Input Number_2" num_2
echo "(($num_1+$num2))"		# 计算加法
echo "(($num_1-$num2))"		# 计算减法
echo "(($num_1*$num2))"		# 计算乘法
awk -v num1="$num_1" -v num2="$num_2" 'BEGIN {print num1/num2}'	需要使用awk -k从awk外部传入参数，再在awk内部使用'BEGIN {print xxx}'语句进行运算
```



###### shell   直到输入quit才终止

```
while true
	do read -p "Please Input:" var_1
    if [ "var_1"="quit" ];then
      break
    else
      echo "$var_1"
    fi
	done
```



