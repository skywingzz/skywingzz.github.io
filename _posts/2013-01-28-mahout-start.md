---
title: Start Mahout
date: 2013-01-28 17:22:00 +0300
categories: [Machine Learning]
tags: [Mahout, 추천, 설치]
---

### python 설치
```shell
brew install python
```

### Mahout package 다운로드 및 압축해제
tar 를 다운해서 압축을 푸니 maven 프로젝트가 아니라서 이후 진행이 되지 않아
tutorial 나온데로 trunk 소스를 checkout 받아서 진행을 하였다.
```shell
$ wget http://mirror.apache-kr.org/mahout/0.7/mahout-distribution-0.7.tar.gz
$ tar xvzf mahout-distribution-0.7.tar.gz
$ svn co http://svn.apache.org/repos/asf/mahout/trunk
```
 
### 소스 컴파일
압축을 해제한 루트 경로로 이동 후 실행
```shell
$ mvn install
$ cd core
$ mvn compile
```