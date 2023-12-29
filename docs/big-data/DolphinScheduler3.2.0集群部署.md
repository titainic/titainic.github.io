---
layout: default
title: DolphinScheduler3.2.0集群部署
parent: Big Data
nav_order: 1
---

{: .highlight }
本章节详细介DolphinScheduler3.2.0集群部署细节

# 完成以下部署工作
{: .fs-6 }
- 集群需要zookeeper集群，此处省略部署zk 
- 修改DolphinScheduler3.2.0配置文件
- 集群ssh免密码登录
- 分发各集群节点
- 启动DolphinScheduler集群


部署说明 :
{: .fs-6 }
- 省略集群ssh免密码登录
- 用户需sudo免密码输入
- 省略zk部署

----

{: .note-title }
> 安装psmisc
>
> sudo apt install psmisc

---

## 拷贝mysql驱动到以下目录
- cp mysql-connector-java-8.0.16.jar alert-server/libs/
- cp mysql-connector-java-8.0.16.jar api-server/libs/
- cp mysql-connector-java-8.0.16.jar master-server/libs/
- cp mysql-connector-java-8.0.16.jar standalone-server/libs/
- cp mysql-connector-java-8.0.16.jar tools/libs/
- cp mysql-connector-java-8.0.16.jar worker-server/libs/
---

## 配置文件修改

修改配置文件DolphinScheduler3.2.0/bin/env/下 install_env.sh和dolphinscheduler_env.sh

<div class="code-example" markdown="1">
根据需要修改install_env.sh配置文件，如下：
</div>
```markdown
#集群所有节点host
ips="binend1,binend2,binend3,binend4,binend5,binend6,binend7,binend8"

#集群远程登录端口，默认不动
sshPort="22"

#master节点host
masters="binend1,binend2"

#work节点host
workers="binend3:default,binend4:default,binend5:default,binend6:default"

#alertServer节点host
alertServer="binend7"

#apiServers节点host
apiServers="binend8"

#所有host节点安装dolphinscheduler目录
installPath="/home/binend/soft/bigdata/dolphinscheduler-3.2.0"

#远程部署的用户，修改为ssh免密码登录用户，不适用官方dolphinscheduler用户
deployUser="binend"

#zk节路径
zkRoot="/dolphinscheduler-pi"
```

---

<div class="code-example" markdown="1">
根据需要修改dolphinscheduler_env.sh配置文件，如下：
</div>
```markdown
# jdk路径
export JAVA_HOME=/usr/lib/jvm/jdk1.8.0_301

# 修改源数据库，注意下面mysql，需要双引号，需要完全按照以下格式完全copy
export DATABASE="mysql"
export SPRING_PROFILES_ACTIVE=${DATABASE}
export SPRING_DATASOURCE_URL=jdbc:mysql://binend7:3306/dolphinscheduler?useUnicode=true&characterEncoding=UTF-8&useSSL=false
export SPRING_DATASOURCE_USERNAME=root
export SPRING_DATASOURCE_PASSWORD=a30986125A

# 照抄
export SPRING_CACHE_TYPE=${SPRING_CACHE_TYPE:-none}
export SPRING_JACKSON_TIME_ZONE=${SPRING_JACKSON_TIME_ZONE:-UTC}
export MASTER_FETCH_COMMAND_NUM=${MASTER_FETCH_COMMAND_NUM:-10}

# zk节点信息
export REGISTRY_TYPE=zookeeper
export REGISTRY_ZOOKEEPER_CONNECT_STRING=binend2:2181,binend5:2181,binend6:2181

# 集群组件路径
export HADOOP_HOME=/home/binend/soft/bigdata/hadoop-3.3.3
export HADOOP_CONF_DIR=/home/binend/soft/bigdata/hadoop-3.3.3/etc/hadoop
export SPARK_HOME=/home/binend/soft/bigdata/spark-3.3.0-bin-hadoop3
export PYTHON_LAUNCHER=/usr/bin/python3
export HIVE_HOME=/home/binend/soft/bigdata/hive-3.1.3-bin
export FLINK_HOME=/home/binend/soft/bigdata/flink-1.17.1
#export DATAX_LAUNCHER=${DATAX_LAUNCHER:-/opt/soft/datax/bin/python3}

export PATH=$HADOOP_HOME/bin:$SPARK_HOME/bin:$PYTHON_LAUNCHER:$JAVA_HOME/bin:$HIVE_HOME/bin:$FLINK_HOME/bin:$PATH
```
---

修改组件mysql链接 :
{: .fs-6 }
- DolphinScheduler3.2.0/alert-server/conf/application.yaml
- DolphinScheduler3.2.0/api-server/conf/application.yaml
- DolphinScheduler3.2.0/master-server/conf/application.yaml
- DolphinScheduler3.2.0/worker-server/conf/application.yaml

---

{: .important-title }
> 初始化mysql数据库
>
> DolphinScheduler3.2.0/tools/bin目录下运行以下脚本
>
> ./upgrade-schema.sh
---

<div class="code-example" markdown="1">
alert-server/conf/application.yaml mysql配置文件，如下：
</div>
```markdown
spring:
  config:
    activate:
      on-profile: mysql
  datasource:
    driver-class-name: com.mysql.cj.jdbc.Driver
    url: jdbc:mysql://binend7:3306/dolphinscheduler?useUnicode=true&characterEncoding=UTF-8&useSSL=false
    username: root
    password: a30986125A

```

---

<div class="code-example" markdown="1">
api-server/conf/application.yaml mysql配置文件，如下：
</div>
```markdown
spring:
  config:
    activate:
      on-profile: mysql
  datasource:
    driver-class-name: com.mysql.cj.jdbc.Driver
    url: jdbc:mysql://binend7:3306/dolphinscheduler?useUnicode=true&characterEncoding=UTF-8&useSSL=false
    username: root
    password: a30986125A
  quartz:
    properties:
      org.quartz.jobStore.driverDelegateClass: org.quartz.impl.jdbcjobstore.StdJDBCDelegate

```

---

<div class="code-example" markdown="1">
amaster-server/conf/application.yaml mysql配置文件，如下：
</div>
```markdown
spring:
  config:
    activate:
      on-profile: mysql
  datasource:
    driver-class-name: com.mysql.cj.jdbc.Driver
    url: jdbc:mysql://binend7:3306/dolphinscheduler?useUnicode=true&characterEncoding=UTF-8&useSSL=false
    username: root
    password: a30986125A
  quartz:
    properties:
      org.quartz.jobStore.driverDelegateClass: org.quartz.impl.jdbcjobstore.StdJDBCDelegate

```

---


<div class="code-example" markdown="1">
worker-server/conf/application.yaml zk配置文件，如下：
</div>
```markdown
registry:
  type: zookeeper
  zookeeper:
    namespace: dolphinscheduler
    connect-string: binend2:2181,binend5:2181,binend6:2181
    retry-policy:
      base-sleep-time: 60ms
      max-sleep: 300ms
      max-retries: 5
    session-timeout: 30s
    connection-timeout: 9s
    block-until-connected: 600ms
    digest: ~


```

---


## 部署分发安装到其他节点
- 先启动zk集群
- 运行DolphinScheduler3.2.0/bin/install.sh 部署安装

## 一键启动停止集群
- DolphinScheduler3.2.0/bin下
- start-all.sh 启动集群
- stop-all.sh 关闭集群


