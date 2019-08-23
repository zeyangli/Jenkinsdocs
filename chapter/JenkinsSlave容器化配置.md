# Jenkins Slave容器化配置

## 配置说明
- 需要下载jdk、maven/等构建工具
- 需要下载jenkins站点中agent.jar


## Dockerfile

```
FROM jenkinsci/slave

ARG user=jenkins
ARG agent_workdir=/home/${user}/agent
ENV jenkins_script=/usr/local/bin/
USER root

#替换JDK
ADD  buildtools/jdk-8u121-linux-x64.tar.gz /usr/local/

#替换agent.jar和启动脚本
RUN rm -fr ${agent_workdir}/agent.jar
COPY agent.jar ${agent_workdir}/agent.jar
COPY jenkins-slave ${jenkins_script}/jenkins-slave
RUN chown ${user} ${agent_workdir}/agent.jar \
   && chown ${user} ${jenkins_script}/jenkins-slave

#增加构建工具
ADD buildtools/apache-maven-3.5.0.tar.gz /usr/local/
ADD buildtools/sonar-scanner.tar.gz /usr/local/
ADD buildtools/apache-ant-1.9.9-bin.tar.gz /usr/local/
ADD buildtools/apache-jmeter-5.1.1.tgz /usr/local/

ENV JMETER_HOME=/usr/local/apache-jmeter-5.1.1/
ENV ANT_HOME=/usr/local/apache-ant-1.9.9
ENV M3_HOME=/usr/local/apache-maven-3.5.0

#Jmeter插件
COPY jmeter-plugins/ant-jmeter-1.1.1.jar ${ANT_HOME}/lib/ant-jmeter-1.1.1.jar
COPY jmeter-plugins/fastjson-1.2.9.jar  ${JMETER_HOME}/lib/ext/fastjson-1.2.9.jar
COPY jmeter-plugins/jmeter-plugins-cmn-jmeter-0.5.jar  ${JMETER_HOME}/lib/ext/jmeter-plugins-cmn-jmeter-0.5.jar
COPY jmeter-plugins/jmeter-plugins-manager-1.3.jar  ${JMETER_HOME}/lib/ext/jmeter-plugins-manager-1.3.jar
COPY jmeter-plugins/jmeter-results-detail-report_21.xsl  ${JMETER_HOME}/extras/jmeter-results-detail-report_21.xsl
COPY jmeter-plugins/jmeter-results-shanhe-me.xsl  ${JMETER_HOME}/extras/jmeter-results-shanhe-me.xsl
COPY jmeter-plugins/jmeter.properties ${JMETER_HOME}/bin/jmeter.properties
COPY jmeter-plugins/jmeter-plugins-json-2.7.jar ${JMETER_HOME}/lib/ext/jmeter-plugins-json-2.7.jar
COPY jmeter-plugins/Tools.jar ${JMETER_HOME}/lib/ext/Tools.jar

RUN touch ${JMETER_HOME}/bin/jmeter.log \
    && chmod 777 ${JMETER_HOME}/bin/jmeter.log

#环境变量
ENV JAVA_HOME=/usr/loca/jdk1.8.0_121
ENV PATH=${PATH}:${JAVA_HOME}/bin:${ANT_HOME}/bin:${JMETER_HOME}/bin:${M3_HOME}/bin

CMD ["sh","-x","/usr/local/bin/jenkins-slave"]

```


## Slave启动脚本

```
#!/bin/bash

#env | grep "JENKINS" >>env.txt

env | grep "JENKINS"

echo "[DEBUG] ----nohup java -jar /usr/share/jenkins/agent.jar -jnlpUrl ${JENKINS_URL}/computer/${JENKINS_AGENT_NAME}/slave-agent.jnlp -secret ${JENKINS_SECRET} -workDir /home/jenkins/agent >slave.log & -----"


java -jar /home/jenkins/agent/agent.jar -jnlpUrl ${JENKINS_URL}/computer/${JENKINS_AGENT_NAME}/slave-agent.jnlp -secret ${JENKINS_SECRET} -workDir "/home/jenkins/agent "

```