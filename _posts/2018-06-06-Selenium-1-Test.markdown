---
layout: post
title: Seleinum을 이용한 UI 자동화 연습
date: 2018-06-06 12:00:00 +0900
description: You’ll find this post in your `_posts` directory. Go ahead and edit it and re-build the site to see your changes. # Add post description (optional)
img: selenium2.jpg # Add image post (optional)
fig-caption: # Add figcaption (optional)
tags: [Selenium]
---
## Selenium의 개요
- Selenium은 웹 어플리케이션을 위한 테스팅 프레임워크로 자동화 테스트를 위한 여러가지 강력한 기능을 지원해준다.
- 다양한 브라우저들을 지원하며, 다양한 테스트 작성 언어(Java, Ruby, Groovy, Python, PHP, and Perl.)를 지원한다.
- 홈페이지 : http://seleniumhq.org/

### pom.xml maven 설정
- Maven Repository : https://mvnrepository.com/ 

{% highlight xml %}
<project xmlns="http://maven.apache.org/POM/4.0.0"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
	<modelVersion>4.0.0</modelVersion>

	<groupId>module</groupId>
	<artifactId>selenium</artifactId>
	<version>0.0.1-SNAPSHOT</version>
	<packaging>jar</packaging>

	<name>selenium</name>
	<url>http://maven.apache.org</url>

	<properties>
		<java-version>1.8</java-version>
		<project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
	</properties>

	<dependencies>
		<!-- https://mvnrepository.com/artifact/junit/junit -->
		<dependency>
			<groupId>junit</groupId>
			<artifactId>junit</artifactId>
			<version>4.11</version>
			<scope>test</scope>
		</dependency>

		<!-- https://mvnrepository.com/artifact/org.seleniumhq.selenium/selenium-java -->
		<dependency>
			<groupId>org.seleniumhq.selenium</groupId>
			<artifactId>selenium-java</artifactId>
			<version>2.45.0</version>
		</dependency>

		<!-- https://mvnrepository.com/artifact/org.seleniumhq.selenium/selenium-chrome-driver -->
		<dependency>
			<groupId>org.seleniumhq.selenium</groupId>
			<artifactId>selenium-chrome-driver</artifactId>
			<version>2.45.0</version>
		</dependency>

	</dependencies>
</project>
{% endhighlight %}


## 자바를 이용한 크롬 검색 테스트하기
- 최신 크롬 드라이버 : http://chromedriver.chromium.org/downloads

{% highlight java %}
import java.util.concurrent.TimeUnit;
import org.junit.After;
import org.junit.Before;
import org.junit.Test;
import org.openqa.selenium.By;
import org.openqa.selenium.WebDriver;
import org.openqa.selenium.WebElement;
import org.openqa.selenium.chrome.ChromeDriver;
import junit.framework.TestCase;

/**
 * @author 박성진
 * 크롬 드라이버 오류 : --ignore-certificate-errors
 * 이 오류가 생성시 크롬 드라이버를 검색 후,  최신 버전으로 다운로드 받고 경로 설정
 * {@link http://chromedriver.chromium.org/downloads}
 */
public class SeleniumTest extends TestCase {
	private WebDriver driver;
	private String baseUrl;
	private StringBuffer verificationsErrors=new StringBuffer();
	
	@Before
	public void setUp() throws Exception{
        //자신의 데스크탑에 다운로드 받은 크롬 드라이브 경로 설정
		System.setProperty("webdriver.chrome.driver", "C:\\Users\\psj\\Downloads\\chromedriver_win32\\chromedriver.exe");
		driver=new ChromeDriver();
		baseUrl="https://www.google.com";
		driver.manage().timeouts().implicitlyWait(30, TimeUnit.SECONDS);
		System.out.println("=chromedriver start=");
	}
	
	@Test
	public void testDriver() throws Exception{
		driver.get(baseUrl+"/");
		 //크롬이 실행될때 대기시간을 고려해서 스케줄러의 정확도를 위해 현재 실행중인 스레드를 지정된 밀리 초 동안 일시적으로 실행 중지 되도록 스케줄링 설정
		 Thread.sleep(2000);
		  WebElement searchBox = driver.findElement(By.name("q"));
		  searchBox.sendKeys("이글루시큐리티");
		  searchBox.submit();
	}
	
	@After
	public void tearDown() throws Exception{
		Thread.sleep(5000);
		driver.quit();
		String verificationErrorString=verificationsErrors.toString();
		if(!"".equals(verificationErrorString)) {
			fail(verificationErrorString);
		}
	}
}
{% endhighlight %}

## 셀레니움 크롬 테스트 Success!
![Chrome Success]({{site.baseurl}}/assets/img/chromeTest.png)



### 참고 및 출처
- https://nesoy.github.io/articles/2017-03/Selenium
- http://wiki.gurubee.net/pages/viewpage.action?pageId=6259762
- http://chromedriver.chromium.org/getting-started
- http://www.ulsanstar.com/blog/2014/01/21/how-to-use-selenium-webdriver/