# 第四章 Jenkins集成Gitlab
## 1. SSO集成

### 1.1 Gitlab配置
- 应用管理->创建应用->配置重定向url
![管理页面](static/image/07/01.png)

### 1.2 Jenkins配置
- 配置前备份config.xml文件，
- 如果jenkins前面有代理需要改一下upstream名称，因为这个名称会自动填充为redirect_url地址。
![管理页面](static/image/07/02.png)


## 2. WebHook集成

### 2.1 Jenkins配置

- 安装gitlab hook插件
![管理页面](static/image/07/03.png)

- 选择一个项目，在构建触发器模块选择通过gitlab触发。生成token
![管理页面](static/image/07/04.png)


### 2.2 Gitlab配置
- 进入项目-> 项目设置-> 集成
![管理页面](static/image/07/05.png)

- 保存后测试连接
![管理页面](static/image/07/06.png)

- 返回成功信息，此时可以查看jenkins项目是否正在building。
![管理页面](static/image/07/07.png)



### [下一章](./第八章-集成Jenkins(四).md)    <--------->  [上一章](./第六章-集成Jenkins(二).md)