# Jmeter接口测试

## 简介
JMeter是开源软件Apache基金会下的一个性能测试工具，用来测试部署在服务器端的应用程序的性能。这里我们用来做接口测试。


## 安装
下载地址： http://jmeter.apache.org/download_jmeter.cgi

![images](./01.png)

```shell
#解压到/usr/local
unzip apache-jmeter-5.1.1.zip

#声明环境变量
export JMETER_HOME=/usr/local/apache-jmeter-5.1.1
export PATH=$PATH:$JMETER_HOME/bin

```

## 启动

```
jmeter

```
![images](./02.png)


## 创建测试计划

创建线程组
![images](./03.png)

创建请求
![images](./04.png)
![images](./05.png)

添加结果树
![images](./06.png)

## 运行测试

![images](./07.png)

![images](./08.png)
