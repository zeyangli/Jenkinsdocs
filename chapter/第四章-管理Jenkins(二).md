# 第三章 管理Jenkins（项目、视图、插件）
## 1. 项目管理

### 1.1 命名规范
- 业务名称-应用名称-应用类型_环境类型： cxy-wlck-ui_TEST, 只有命名规范才方便管理项目。


### 1.2 新建项目
![管理页面](static/image/04/01-create.png)

- 设置构建历史
![管理页面](static/image/04/02-buildhistory.png)

- 选择参数化构建
![管理页面](static/image/04/03-argsbuild.png)

- 设置构建触发器
![管理页面](static/image/04/04-triggerbuild.png)

- 设置Jenkinsfile
![管理页面](static/image/04/05-jenkinsfile.png)

- 构建项目
![管理页面](static/image/04/06-buildjobs.png)

- 查看构建日志
![管理页面](static/image/04/07-buildlogs.png)

- 调试Pipeline
![管理页面](static/image/04/08-debuglogs.png)

### 1.3 删除/禁用项目
![管理页面](static/image/04/09-disablejob.png)



### 1.4 项目分类
- 以业务简称为名，创建工程文件夹。将同一个业务的工程全部放到同一个文件夹中。
![管理页面](static/image/04/10-typejob.png)

- 移动项目
![管理页面](static/image/04/11-movejob.png)
![管理页面](static/image/04/11-movejob2.png)

- 外部
![管理页面](static/image/04/12-movejob.png)

- 内部
![管理页面](static/image/04/12-movejob2.png)


## 2. 视图管理
- 默认会创建一个all视图里面存放所有的项目。

### 2.1 创建视图
- 凭据-> 系统-> 全局凭据
![管理页面](static/image/04/13-views-create1.png)
![管理页面](static/image/04/13-views-create2.png)
![管理页面](static/image/04/13-views-create3.png)
![管理页面](static/image/04/13-views-create4.png)

### 2.2 删除视图
![管理页面](static/image/04/13-views-delete1.png)


### 2.3 更新视图
![管理页面](static/image/04/13-views-update1.png)


## 3. 插件管理
-  系统设置->插件管理。
![管理页面](static/image/04/14-plugin1.png)

### 3.1 安装插件
- 勾选要安装的插件，选择安装后不重启。（有些插件需要安装后重启）

![管理页面](static/image/04/14-plugin2.png)

- 安装
![管理页面](static/image/04/14-plugin3.png)


### 3.2 卸载插件
![管理页面](static/image/04/14-plugin4.png)

### 3.3 上传插件
![管理页面](static/image/04/14-plugin5.png)

### 3.4 切换插件更新站点
![管理页面](static/image/04/14-plugin6.png)


### [下一章](./第五章-集成Jenkins(一).md)       <--------->  [上一章](./第三章-管理Jenkins(一).md)