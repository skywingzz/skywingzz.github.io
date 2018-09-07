---
title: Asciidoc 설치 방법
date: 2018-09-07 10:30:00 +0300
categories: [기타]
tags: [Java, Heap Dump]
---

요즘 개발 분서를 작성할 때 markdown 으로 많이 작성을 하지만 좀 더 구조화 되고 멋있게? 작성 할 필요가 있을 때
한계가 있다. 이것 저것 찾아 보던 중... 
asciiDoc 이란 녀석을 발견...

## 설치
설치는 간단하다.
```shell
# brew
brew install asciidoc

# gem
gem install asciidoc
```

## Build
adoc 으로 작성된 문서를 아래의 명령어를 통해 html 파일로 export 한다.
```shell
asciidoctor README.adoc    
```

## 문서 구조 tree 생성
아래와 같이 생성하면 문서의 구조에 따라 index tree 가 생성 된다. 기본적으로 문서의 상단에 생성
```shell
asciidoctor -a toc README.adoc
```
좌측으로 배열 하고 싶을때 문서의 상단에 아래와 같이 기술
```
:toc: left

= Title

== SubTitle1
    내용

== SubTitle2
    내용
```
![](/assets/images/asciidoc-screenshot.png)


## 참고
- 사용자 가이드 : http://asciidoc.org/userguide.html