

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





###### 输出重定向

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

root@04b25dd4f42a:/data# echo abcdefg | grep -o ".*c.*"	# 匹配c前面和后面均有多个字符，并输出

abcdefg

root@04b25dd4f42a:/data# echo abcdefg | grep -o "c."	# 匹配c和后面的一个字符，并输出
cd
```





```
apt-get install xxx	# 如果linux提示 bash: xx: command not found

#!/bin/bash
curl -s http://www.baidu.com/s?wd=mp3 | grep -o "result:[0-9,]*"		# [0-9,]* 能够匹配0-9和逗号的任意字符串
```

