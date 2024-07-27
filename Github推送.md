# GitHub

###### 如何将本地Typora推送到GitHub？

1. 创建GitHub仓库
   1. 登陆到Github
   2. 点击右上角的 + 按钮，选择 New repository
   3. 填写仓库的名称和其他信息，点击 Create repository

2. 在本地初始化仓库

   ```
   cd /path/to/your/typora/files
   git init		# 在该目录下初始化文件
   ```

   ​	

3. 添加和提交文件

   ```
   git add . # 将Typora文件添加到git仓库
   git commit -m "Initial commit with Typora files"	# 提交这些文件
   git remote add origin YOUR_GITHUB_REPOSITORY_URL	# 连接到Github仓库
   ```

   

4. 推送到Github仓库

   ```
   git push -u origin main # 将本地的更改推送到Github仓库
   ```



5. 使用Typora编辑并同步文件

   ```
   git add your-file.md
   git commit -m "Updated the file"
   ```

   

6. 推送更改

   ```
   git push
   ```

   