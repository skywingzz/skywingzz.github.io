---
title: 디렉토리 list 에 git branch명 보이기
date: 2018-09-06 17:09:00 +0300
categories: [Git]
tags: [Git, Tip]
---

### 터미널에서 여러 Git Repository 를 관리할때 브랜치명을 폴더명 옆에 같이 보여주면 좋을 것 같아 찾아보다
아래 Python 프로그램 설치 후 손쉽게 사용 할 수 있었다.
- 참고 자료 : https://github.com/MichaelKim0407/mk-ls-git

.bash_profile 에 아래와 같이 llg 명령어 alias 자동 등록됨
```shell
alias llg='ls-git -laGF'
```
명령어 입력시 아래 화면과 같이 폴더명 옆에 branch 명 표시됨
![](/assets/images/git-multi-llg.png)


### 추가 유용한 기능
Multi 로 Git명령어를 보낼때 .bash_profile 에 아래와 같이 alias 설정 후사용
```shell
alias gitm='ls | xargs -P10 -I{} git -C {}'

#전체 폴더에 존재하는 전체 Repository 에 pull 명령어를 보냄
gitm pull
```
