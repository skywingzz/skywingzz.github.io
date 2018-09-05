---
title: Hadoop 설치
date: 2013-01-30 14:17:00 +0300
tags:
  - Hadoop
  - 설치
---
1. Hadoop 다운로드
http://apache.tt.co.kr/hadoop/common/hadoop-2.0.2-alpha/hadoop-2.0.2-alpha.tar.gz
2.0.2 Alpha 버전 기준이므로 알맞은 버전을 다운로드 하여 설치하면 됩니다.

2.  tar 압축 해제 
전 ~/app 폴더를 생성하여 그 하위에 압축을 풀었습니다.
`$ tar xvzf hadoop-2.0.2-alpha.tar.gz`

3. 환경 변수 설정
```shell
$ vi /home/계정/.profile
# 하위에 추가, JAVA_HOME PATH 가 잡혀있지 않으면 같이 추가해 줍니다.
export HADOOP_INSTALL=/home/계정/app/hadoop-2.0.2-alpha
export HADOOP_HOME=/home/계정/app/hadoop-2.0.2-alpha
export PATH=$PATH:$HADOOP_INSTALL/bin
$ source ~/.profile
$ hadoop version 
Hadoop 2.0.2-alpha
Subversion https://svn.apache.org/repos/asf/hadoop/common/branches/branch-2.0.2-alpha/hadoop-common-project/hadoop-common -r 1392682
Compiled by hortonmu on Tue Oct  2 00:44:10 UTC 2012
From source with checksum efbdb59af73bfc103f1945d65dbf3071
##위와같이 출력되면 정상적으로 설치된 것임
```

4. Hadoop 환경 설정 (설정 파일 위치 : $HADOOP_HOME/etc/hadoop)
 core-site.xml
```xml
<configuration>
        <property>
                <name>fs.default.name</name>
                <value>hdfs://localhost:9000</value>
        </property>
</configuration>
```
hdfs-site.xml
네임노드, 보조네임노드, 데이타노드 와 같이 hdfs 의 데몬을 위한 환경을 설정하는 파일 입니다.
```xml
<configuration>
        <property>
                <name>dfs.name.dir</name>
                <value>/home/skywing/app/hdfs/name</value>
        </property>

        <property>
                <name>dfs.name.edits.dir</name>
                <value>${dfs.name.dir}</value>
        </property>

        <property>
                <name>dfs.data.dir</name>
                <value>/home/skywing/app/hdfs/data</value>
        </property>
</configuration>
```
mapred-site.xml
jobtracker 와 tasktracker 와 같은 데몬을 위한 환경 설정 파일
```xml
<configuration>
        <property>
                <name>mapred.job.tracker</name>
                <value>localhost:9001</value>
        </property>

        <property>
                <name>mapred.local.dir</name>
                <value>/home/skywing/app/hdfs/mapred/local</value>
        </property>

        <property>
                <name>mapred.system.dir</name>
                <value>/home/skywing/app/hdfs/mapred/system</value>
        </property>
</configuration>
```

 

 