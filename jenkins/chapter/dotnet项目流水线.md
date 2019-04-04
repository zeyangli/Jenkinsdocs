# dotnet项目发布

demo地址: https://github.com/zeyangli/dotnet-HelloWorld.git

## 安装dotnet开发环境（centos7）

```
rpm --import https://packages.microsoft.com/keys/microsoft.asc

[root@VM_0_12_centos ~]# cat /etc/yum.repos.d/dotnetdev.repo 
[packages-microsoft-com-prod]
name=packages-microsoft-com-prod 
baseurl= https://packages.microsoft.com/yumrepos/microsoft-rhel7.3-prod
enabled=1
gpgcheck=1
gpgkey=https://packages.microsoft.com/keys/microsoft.asc

yum install libunwind libicu
yum install dotnet-sdk-2.1.103

dotnet --version

```


## 创建Jenkins项目

![iamges](./dotnet/01.png)



## Jenkinsfile

```

String buildShell = "${env.buildShell}"
String targetDir  = "${env.targetDir}"

node("runner"){
    stage("checkout"){
        checkout scm
    }
    
    
    stage("build"){
        sh " ${buildShell} "
    }
    
    
    stage("publish"){
        sh " mkdir -p ${targetDir} "
        sh " dotnet publish -o ${targetDir}  && ls -l ${targetDir}"
        
    }
}


```

## 构建日志

```
成功 Console Output
Started by user admin
Obtained new-jenkinsfile from git https://github.com/zeyangli/dotnet-HelloWorld.git
Running in Durability level: MAX_SURVIVABILITY
[Pipeline] Start of Pipeline
[Pipeline] node
Running on runner in /var/lib/jenkins/workspace/demo/demo-dotnet-service
[Pipeline] {
[Pipeline] stage
[Pipeline] { (checkout)
[Pipeline] checkout
using credential 24982560-17fc-4589-819b-bc5bea89da77
Fetching changes from the remote Git repository
 > /root/bin/git rev-parse --is-inside-work-tree # timeout=10
 > /root/bin/git config remote.origin.url https://github.com/zeyangli/dotnet-HelloWorld.git # timeout=10
Fetching upstream changes from https://github.com/zeyangli/dotnet-HelloWorld.git
 > /root/bin/git --version # timeout=10
using GIT_ASKPASS to set credentials gitlab
 > /root/bin/git fetch --tags --progress https://github.com/zeyangli/dotnet-HelloWorld.git +refs/heads/*:refs/remotes/origin/*
Checking out Revision ed6460aaa029e1a29654c3eec369ec97783a8fb1 (refs/remotes/origin/master)
Commit message: "Update new-jenkinsfile"
[Pipeline] }
[Pipeline] // stage
[Pipeline] stage
[Pipeline] { (build)
[Pipeline] sh
 > /root/bin/git rev-parse refs/remotes/origin/master^{commit} # timeout=10
 > /root/bin/git rev-parse refs/remotes/origin/origin/master^{commit} # timeout=10
 > /root/bin/git config core.sparsecheckout # timeout=10
 > /root/bin/git checkout -f ed6460aaa029e1a29654c3eec369ec97783a8fb1
 > /root/bin/git rev-list --no-walk 75976c755992589ce9ebb4799d623e6ed3447a2e # timeout=10
+ dotnet restore
  Restore completed in 107.58 ms for /var/lib/jenkins/workspace/demo/demo-dotnet-service/HelloWorld/HelloWorld.csproj.
+ dotnet build
Microsoft (R) Build Engine version 15.6.82.30579 for .NET Core
Copyright (C) Microsoft Corporation. All rights reserved.

  Restore completed in 49.87 ms for /var/lib/jenkins/workspace/demo/demo-dotnet-service/HelloWorld/HelloWorld.csproj.
  HelloWorld -> /var/lib/jenkins/workspace/demo/demo-dotnet-service/HelloWorld/bin/Debug/netcoreapp2.0/HelloWorld.dll

Build succeeded.
    0 Warning(s)
    0 Error(s)

Time Elapsed 00:00:02.30
[Pipeline] }
[Pipeline] // stage
[Pipeline] stage
[Pipeline] { (publish)
[Pipeline] sh
+ mkdir -p /opt/dotnet
[Pipeline] sh
+ dotnet publish -o /opt/dotnet
Microsoft (R) Build Engine version 15.6.82.30579 for .NET Core
Copyright (C) Microsoft Corporation. All rights reserved.

  Restore completed in 54 ms for /var/lib/jenkins/workspace/demo/demo-dotnet-service/HelloWorld/HelloWorld.csproj.
  HelloWorld -> /var/lib/jenkins/workspace/demo/demo-dotnet-service/HelloWorld/bin/Debug/netcoreapp2.0/HelloWorld.dll
  HelloWorld -> /opt/dotnet/
+ ls -l /opt/dotnet
total 20
-rw-r--r-- 1 root root  440 Apr  4 21:03 HelloWorld.deps.json
-rw-r--r-- 1 root root 7168 Apr  4 21:02 HelloWorld.dll
-rw-r--r-- 1 root root 1364 Apr  4 21:02 HelloWorld.pdb
-rw-r--r-- 1 root root  146 Apr  4 21:03 HelloWorld.runtimeconfig.json
[Pipeline] }
[Pipeline] // stage
[Pipeline] }
[Pipeline] // node
[Pipeline] End of Pipeline
Finished: SUCCESS

```