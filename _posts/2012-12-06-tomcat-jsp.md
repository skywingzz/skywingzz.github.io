---
title: Tomcat jsp 컴파일 오류, jsp 라인수 제한
date: 2012-12-06 18:09:00 +0300
categories: [Tomcat]
tags: [Tomcat, JSP, 오류]
---

Local 에서 tomcat 구동 후 이유 없이 jsp 컴파일 오류가 날 경우
아래의 경우를 의심해 보아야 한다.
Tomcat 에서 jsp 하나의 라인수가 5000라인이 넘어가면 컴파일을 하지 못하여
Exception 을 일으킨다.
이 경우 web.xml 에 아래와 같이 추가해 주면 해결 된다.
각 parameter 에 대한 자세한 내용은 아직 분석해 보진 못하였다.

```xml
<servlet>
   <servlet-name>jsp</servlet-name>
   <servlet-class>org.apache.jasper.servlet.JspServlet</servlet-class>
    
   <init-param>
      <param-name>mappedfile</param-name>
      <param-value>false</param-value>
   </init-param>
</servlet>
```