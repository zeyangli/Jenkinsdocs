

# Jenkins+InfluxDB+Grafana 收集构建数据

## 0.具体步骤
- 1.搭建jenkins、InfluxDB、Grafana。
- 2.Jenkins安装插件、配置数据库地址、编写Pipeline、自定义数据。
- 3.InfluxDB创建数据库、创建用户。
- 4.Grafana建立统一的度量模板，使用变量替换固定的项目名称。

## 1.准备工作
###  1.1 创建数据库

``` 
CREATE DATABASE jenkins
CREATE USER "jenkins" WITH PASSWORD 'root123' WITH ALL PRIVILEGES
```


### 1.2 配置Jenkins
安装influxdb插件、配置Influxdb信息。

![image](https://github.com/zeyangli/Jenkins-docs/raw/master/chapter/influxdb/01.png)

![image](https://github.com/zeyangli/Jenkins-docs/raw/master/chapter/influxdb/02.png)

### 1.3 编写pipeline代码

```
try {
    //pipeline
    currentBuild.description = "构建成功"   //定义Jenkins构建描述，默认无。
} catch(err){
    currentBuild.description = "构建失败"   //定义Jenkins构建描述，默认无。
    throw err
    
}finally{
    step([$class: 'InfluxDbPublisher',
        customData: null,
        customDataMap: null,
        customPrefix: null,
        target: 'influxdb',         // Jenkins中配置的Influxdb。
        selectedTarget: 'influxdb', // Jenkins中配置的Influxdb。
        //jenkinsEnvParameterTag: 'KEY=' + env.PARAM,     // OPTIONAL,自定义tag
        jenkinsEnvParameterField: 'build_agent_name=' + 'master' + '\n' + 'build_status_message=' + currentBuild.description, // OPTIONAL, 自定义字段
        measurementName: 'jenkins_data', // OPTIONAL, 表名称
        replaceDashWithUnderscore: false, // OPTIONAL, 是否替换 "-"为 "_"。
    ])
}


//基于插件自定义字段名称（非完全自定义）
/*
step([$class: 'InfluxDbPublisher',
            customData: null,
            customDataMap: null,
            customPrefix: null,
            target: 'influxdb',
            selectedTarget: 'influxdb', 
            jenkinsEnvParameterTag: 'project_name=' + "${JOB_NAME}".split('/')[-1] ,     // OPTIONAL, 自定义project_name
            jenkinsEnvParameterField: 'build_agent_name=' + 'master'  +  "\n" +           //自定义参数列表，每个参数加上一个换行符
                                      'build_status_message=' + currentBuild.description +  "\n" + 
                                      'midwareType=' + "${midwareType}" +  "\n" + 
                                      'listenPort=' + "${port}" +  "\n" + 
                                      'runUser=' + "${user}" +  "\n" +
                                      'repoName=' + "${srcUrl}".split("/")[-1] - '.git' +  '\n' + 
                                      'project_name=' + "${JOB_NAME}".split('/')[-1]  +  '\n' +
                                      'deployHosts=' + "${targetHosts}" ,
            measurementName: 'jenkins_data', 
            replaceDashWithUnderscore: false,
        ])
*/

```

### 1.4 构建测试
查看Influxdb数据

```
> USE jenkins
Using database jenkins
> SHOW SERIES
key
---
jenkins_data,build_result=SUCCESS,project_name=cxy-influxdb-demo_PROD
jenkins_data,build_result=SUCCESS,project_name=cxy-influxdb1-demo_PROD

> select * from jenkins_data
name: jenkins_data
time                build_agent_name build_exec_time build_measured_time build_number build_result build_result_1 build_result_ordinal build_scheduled_time build_status_message build_successful build_time deployHosts    last_stable_build last_successful_build listenPort midwareType project_build_health project_name            project_name_1          project_path            repoName         runUser
----                ---------------- --------------- ------------------- ------------ ------------ -------------- -------------------- -------------------- -------------------- ---------------- ---------- -----------    ----------------- --------------------- ---------- ----------- -------------------- ------------            --------------          ------------            --------         -------
1559345943038000000 master           1559345921459   1559345943038       2132         SUCCESS      SUCCESS        0                    1559345921426        构建成功                 true             21612      VM_7_14_centos 2131              2131                  80         Nginx       100                  cxy-influxdb-demo_PROD  cxy-influxdb-demo_PROD  cxy-influxdb-demo_PROD  devops-tools-web nginx
1559345945612000000 master           1559345927303   1559345945612       2091         SUCCESS      SUCCESS        0                    1559345927254        构建成功                 true             18358      VM_7_14_centos 2090              2090                  80         Nginx       80                   cxy-influxdb1-demo_PROD cxy-influxdb1-demo_PROD cxy-influxdb1-demo_PROD devops-tools-web nginx
1559345963040000000 master           1559345940685   1559345963040       2092         SUCCESS      SUCCESS        0                    1559345940669        构建成功                 true             22371      VM_7_14_centos 2091              2091                  80         Nginx       80                   cxy-influxdb1-demo_PROD cxy-influxdb1-demo_PROD cxy-influxdb1-demo_PROD devops-tools-web nginx
1559345964061000000 master           1559345940689   1559345964061       2133         SUCCESS      SUCCESS        0                    1559345940686        构建成功                 true             23375      VM_7_14_centos 2132              2132                  80         Nginx       100                  cxy-influxdb-demo_PROD  cxy-influxdb-demo_PROD  cxy-influxdb-demo_PROD  devops-tools-web nginx
```


## 2.配置Grafana
### 2.1 设置项目变量 
dashboard ->templating
![image](https://github.com/zeyangli/Jenkins-docs/raw/master/chapter/influxdb/03.png)


效果图

![image](https://github.com/zeyangli/Jenkins-docs/raw/master/chapter/influxdb/04.png)


## 3. FAQ
- 这个插件是根据多分支流水线项目设计的，项目名称开头会有null_，目前自定义项目名称完成。
- 参考资料：https://wiki.jenkins.io/display/JENKINS/InfluxDB+Plugin   插件说明
