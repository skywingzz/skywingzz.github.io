---
title: "Mobile 디버깅 방법 "
date: 2015-08-13 09:25:00 +0300
categories: [Mobile]
tags: [mobile, debugging, Weinre, fiddler, charles]
---

모바일 웹페이지 개발시 PC처럼 디버깅이 쉽지가 않다. 이런 어려움은 개발 생산성을 떨어뜨릴 뿐 아니라 운영에도 크게 어려움을 준다.
디버깅을 최대한 쉽고 편하게 할 수 있는 몇가지 방법을 소개한다.
 
## Web Inspector
**단말기와 PC 의 USB 연결을 통한 디버깅**
- Android 디버깅
	- Chrome Inspector
		- 실행 조건
			1. 단말기와 PC 에 모두 Chrome 브라우저가 설치 되어 있어야 한다. (Android 4.0+, Chome32+)
			2. PC 에 Android SDK 및 해당 단말기 USB 드라이버가 설치되어 있어야 한다. (Mac 은 드라이버 설치 불필요)
		- 실행 방법
			1. 단말기의 USB디버깅 모드 활성화 : 설정 > 개발자 옵션 (개발자옵션 비활성화시 폰정보도의 빌드넘버를 7번 클릭시 활성화됨, 설정 > 휴대전화 정보 > 빌드 번호)
			2. 단말기와  PC USB 연결
			3. Chrome 설정 > 도구 더보기 > 기기검사 또는 주소창에 chrome://inspect 입력
			4. Discover USB devices 체크
			5. Chome Inspector 의 Devices목록에서 페이지를 선택 후 디버깅 
	- APP 웹뷰 디버깅(안드로이드 4.4 이상)
		- 실행 조건
			1. Android 4.4+, App 에서 Webview 디버깅이 활성화 되어 있어야 함
		- 실행 방법
			1. App 의 Webview 디버깅 활성화(관리자 모드 진입 후 체크)
			2. 위의 Chrome Inspector 와 동일하게 사용
- IOS 디버깅
	- Safari 개발자 도구(WebView 디버깅은 사용불가)
		- 실행조건
			- IOS 6.0+, Safari 6+ 부터 사용가능
		- 실행방법
			1. 단말기의 설정 > Safari > 고급 > 웹속성 enable
			2. USB 연결
			3. 단말기에서 Safari 실행
			4. PC Safari 실행
			5. PC 상단메뉴 > 개발자용 > IOS Device 이름 > 창 이름선택하면 Inspector 실행


## Weinre
nodejs 기반의 디버깅 툴로서 내장 디버깅 툴이 없는 모바일 브라우저를 위한 원격 디버팅 툴 로서 Webkit 계열의 개발자 도구인 Web Inspector 를 사용할수 있도록 해준다. 
- 설치 & 다운로드 : https://people.apache.org/~pmuellr/weinre/docs/latest/Home.html

## Proxy 서버를 통한 디버깅 
개인 PC 에 프록시 서버를 두고 단말기에서 이 프록시 서버를 통해 통신함으로서 단말기가 주고 받는 데이터를 모니터링
- Charles(Mac) 사용법
	1. PC Proxy 서버 셋팅
		- 설치 : www.charlesproxy.com/
		- Charles 는 기본적으로 Proxy 서버가 셋팅되어 있음(포트 8888)

	2. 단말기 Proxy 셋팅
		- 현재 PC 의 ip 확인 : 쉘에서 ifconfig 또는 시스템환경설정 > 네트워크 메뉴를 통해 확인
		- 단말기에서 현재 연결되어 있는 AP 수정
		- 고급 옵션의 프록시 설정 수동으로 변경 후 pc ip 와 port입력 후 확인

- Fiddler(Window) 사용법
	- 다운로드 및 사용법 : http://www.telerik.com/fiddler 
