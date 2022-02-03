> 教程 **BY Sky**

# 准备

>### 1.手动创建一个仓库：
>
>- #### 新建仓库并填写相应信息：
>
>![image-20220203093051570](C:\Users\HP\AppData\Roaming\Typora\typora-user-images\image-20220203093051570.png)
>
>- #### 手动初始化厂库：
>
>![image-20220203093244588](C:\Users\HP\AppData\Roaming\Typora\typora-user-images\image-20220203093244588.png)
>
>### 2.下载 git工具 [下载地址]([Git - Downloads (git-scm.com)](https://git-scm.com/downloads))
>
>![image-20220203093609338](C:\Users\HP\AppData\Roaming\Typora\typora-user-images\image-20220203093609338.png)
>
>





# 教程开始



# 1.将要上传的东东放在一个文件夹里（个人喜好）

## PS. 比如这里我要上传一个 Sky的笔记 文件夹

![image-20220203093845784](C:\Users\HP\AppData\Roaming\Typora\typora-user-images\image-20220203093845784.png)

# 2.鼠标右键选择Git Bash Here(所以需要git工具)

![image-20220203094216205](C:\Users\HP\AppData\Roaming\Typora\typora-user-images\image-20220203094216205.png)

# 3.输入命令git init（初始化的意思）

![image-20220203094434368](C:\Users\HP\AppData\Roaming\Typora\typora-user-images\image-20220203094434368.png)

- ## 初始化后会多一个.git文件夹（隐藏的，设置后可见）

![image-20220203094546536](C:\Users\HP\AppData\Roaming\Typora\typora-user-images\image-20220203094546536.png)

# 4.设置用户名称和邮箱（第一次需要，后面忽略）

- git config --global user.name "xxx"                 //名字随便来

- git config --global user.email "xxx@xx.xx"     //你的邮箱

![image-20220203095053996](C:\Users\HP\AppData\Roaming\Typora\typora-user-images\image-20220203095053996.png)

# 5.去仓库复制仓库链接，进行连接仓库

- git remote add origin 仓库地址

> 右键Paste是粘贴的意思

![image-20220203095533878](C:\Users\HP\AppData\Roaming\Typora\typora-user-images\image-20220203095533878.png)

# 6.推送(三部曲)

- git add .      //把当前文件夹所有文件添加到暂存区（记得有个点）

![image-20220203095830336](C:\Users\HP\AppData\Roaming\Typora\typora-user-images\image-20220203095830336.png)

- git commit -m '注释'     //添加到归档区

  ![image-20220203100152986](C:\Users\HP\AppData\Roaming\Typora\typora-user-images\image-20220203100152986.png)

- git push origin master    //推送成功！！

  > 上传空文件夹它不会上传！！！

![image-20220203100256473](C:\Users\HP\AppData\Roaming\Typora\typora-user-images\image-20220203100256473.png)

# 补充内容

- git init 初始化，创建本地仓库
- git add . 添加到本地仓库
- git commit -m "注释" 添加注释
- git remote add origin 仓库地址 连接远程仓库
- git pull origin master 同步仓库内容
- git push -u origin master 上传到远程仓库

> 教程 **BY Sky**
