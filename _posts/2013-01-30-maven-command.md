---
title: "Maven 기본 명령어"
date: 2013-01-30 17:47:00 +0300
img:  
tags: [Maven, 명령어]
---

**$ mvn compile**
: 컴파일 후 target 폴더 생성 및 class 파일 생성

**$ mvn test**
: target/test-classes 안에 있는 junit 단위 테스트 클래스들을 테스트

**$ mvn javadoc:javadoc**
: maven에서는 javadoc 작성이나 프로젝트 사이트의 작성등, 도큐멘테이션에 관련하는 기능 프로젝트 폴더의 target/site/apidocs 폴더가 생성되어 index를 웹화면을 볼수 있음

**$ mvn site**
: 프로젝트 사이트 작성, target/site 폴더에 생성 되며 index를 통해 웹화면으로 볼 수 있음

**$ mvn package**
: target 폴더에 JAR 및 WAR 파일 생성

**$ mvn install**
: package 에 의해 생성된 jar, war 파일을 로컬 repository에 등록

**$ mvn clean**
: target 디렉토리 삭제

**$ mvn eclipse:eclipse**
: 이클립스에서 import 가능 하도록 이클립스 프로젝트로 변경