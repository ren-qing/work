![image-20211206003048287](image-20211206003048287.png)

![image-20211206003243288](image-20211206003243288.png)

![image-20211206003341024](image-20211206003341024.png)

Git\etc\gitconfig

C\user\asua\gitconfig



git config -- global user.name "XXX"

# 核心

## 工作区域

参考：b站狂神

- 工作目录 working directory
  - 本地文件
  - ![image-20211206004805771](image-20211206004805771.png)
- 暂存区 stage
  - 是一个文件,用来保存即将提交到文件列表信息
  - .git
  - ![image-20211206004954685](image-20211206004954685.png)
- 资源库 repository
  - head
  - ![image-20211206005155343](image-20211206005155343.png)

远程git仓库 (github gitee ...)

![image-20211206004631235.png](image-20211206004631235.png)

![image-20211206005746161.png](image-20211206005746161.png)

工作流程

1. 工作目录添加修改文件
2. 需要版本控制的放入暂存区
3. 暂存区提交到git仓库

![image-20211206010245545.png](image-20211206010245545.png)



## 命令

### git init 

### git clone [url]





## 状态

untracked 未跟踪

unmodify 入库、未修改

modified 仅已修改

staged 暂存

![image-20211206011417674.png](image-20211206011417674.png)

![image-20211206011728269](image-20211206011728269.png)

![image-20211206011747198](image-20211206011747198.png)

![image-20211206012121167](image-20211206012121167.png)

## 步骤

### 1.git add .



![image-20211216085748671](image-20211216085748671.png)

### 2.git commit -m“the message”



![image-20211206015911337](image-20211206015911337.png)





### 3.git remote add origin git@github.com:ren-qing/ren-qing-Data-management-and-shopping-websites.git

![image-20211206020047258](image-20211206020047258.png)

### 4.git push --set-upstream origin master 首次

###  如果仓库里已经上传过东西

### git config branch.master.remote origin

###  git config branch.master.merge refs/heads/master

### git pull origin master --allow-unrelated-histories

### git push

![image-20211206020148878](image-20211206020148878.png)

![image-20211206020202226](image-20211206020202226.png)

![image-20211206020217458](image-20211206020217458.png)