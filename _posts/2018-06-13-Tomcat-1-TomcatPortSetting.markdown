---
layout: post
title: 자바 System Property를 이용한 Tomcat (톰캣) 포트 동적 설정
date: 2018-06-13 17:00:00 +0900
description: You’ll find this post in your `_posts` directory. Go ahead and edit it and re-build the site to see your changes. # Add post description (optional)
img: Tomcat.png # Add image post (optional)
fig-caption: # Add figcaption (optional)
tags: [Tomcat]
---

## 1. 개요
Multi Instance 구성이나, 자동 배포 등을 할 때 걸림돌이 되는 것이 포트 구성인데, Java System Property를 이용하여 server.xml 수정없이 포트 구성을 할 수 있는 방법을 고민하였습니다.
물론 최초에 template이 되는 server.xml은 한 번 만들어야 합니다.

## 2. 설정
원래 server.xml에는 다음과 같이 총 4개의 포트 구성이 있습니다.

{% highlight xml %}
<Server port="8005" shutdown="SHUTDOWN">
<Connector port="8080" protocol="HTTP/1.1" ...
<Connector port="8009" protocol="AJP/1.3" ...
... redirectPort="8443" />

다음과 같이 수정합니다.
{% endhighlight %}

- 8005 -> ${port.shutdown}
- 8080 -> ${port.http}
- 8009 -> ${port.ajp}
- 8443 -> ${port.https}

즉, 이렇게 되겠죠.

{% highlight xml %}
<Server port="${port.shutdown}" shutdown="SHUTDOWN">
<Connector port="${port.http}" protocol="HTTP/1.1" ...
<Connector port="${port.ajp}" protocol="AJP/1.3" ...
... redirectPort="${port.https}" />
{% endhighlight %}

그리고 적절한 기동 스크립트에 다음과 같이 System Property를 적용합니다.

{% highlight xml %}
JAVA_OPTS=" ${JAVA_OPTS} -Dport.http=${INST_PORT}"
JAVA_OPTS=" ${JAVA_OPTS} -Dport.https=`expr ${INST_PORT} + 363`"
JAVA_OPTS=" ${JAVA_OPTS} -Dport.ajp=`expr ${INST_PORT} - 71`"
JAVA_OPTS=" ${JAVA_OPTS} -Dport.shutdown=`expr ${INST_PORT} - 75`" 
{% endhighlight %}


### 출처
- https://sarc.io/index.php/tomcat/211-system-property-tomcat