# 发布maven项目



## 新建项目
![images](./images/04.png)
![images](./images/05.png)


## 上传测试代码

```
#测试demo
git clone https://github.com/TTFHW/jenkins_pipeline_java_maven.git 
#项目地址
git clone http://desktop-m12emcf:8080/tfs/DefaultCollection/_git/devops-service  

mv jenkins_pipeline_java_maven/* devops-service/
git add *
git commit -m 'test'
git push origin master

Enumerating objects: 15, done.
Counting objects: 100% (15/15), done.
Delta compression using up to 4 threads.
Compressing objects: 100% (10/10), done.
Writing objects: 100% (15/15), 2.17 KiB | 2.17 MiB/s, done.
Total 15 (delta 0), reused 0 (delta 0)
remote: Analyzing objects... (15/15) (81 ms)
remote: Storing packfile... done (58 ms)
remote: Storing index... done (149 ms)
To http://desktop-m12emcf:8080/tfs/DefaultCollection/_git/devops-service
 * [new branch]      master -> master
```

![images](./images/06.png)

## 使用持续集成
![images](./images/07.png)

![images](./images/08.png)

### 选择构建代理
![images](./images/09.png)

### 设置获取源
![images](./images/10.png)

### 执行构建
![images](./images/14.png)

![images](./images/15.png)

### 保存配置
![images](./images/12.png)
### 运行
![images](./images/13.png)

### 结果
![images](./images/16.png)