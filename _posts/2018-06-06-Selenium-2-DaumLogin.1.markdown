---
layout: post
title: Seleinum을 이용한 Daum 로그인 테스트
date: 2018-06-06 23:00:00 +0900
description: You’ll find this post in your `_posts` directory. Go ahead and edit it and re-build the site to see your changes. # Add post description (optional)
img: selenium2.jpg # Add image post (optional)
fig-caption: # Add figcaption (optional)
tags: [Selenium]
---

## 셀레니움을 이용한 Daum 로그인 간단하게 구현하기
- 최신 크롬 드라이버 : http://chromedriver.chromium.org/downloads

{% highlight java %}
package module.selenium.doipo.test;

import java.util.concurrent.TimeUnit;
import org.junit.After;
import org.junit.Before;
import org.junit.Test;
import org.openqa.selenium.By;
import org.openqa.selenium.WebDriver;
import org.openqa.selenium.chrome.ChromeDriver;
import junit.framework.TestCase;


/**
 * @author 박성진
 * 크롬 드라이버 오류 : --ignore-certificate-errors
 * 이 오류가 생성시 크롬 드라이버를 검색 후,  최신 버전으로 다운로드 받고 경로 설정
 * {@link http://chromedriver.chromium.org/downloads}
 */
public class DaumLoginTest extends TestCase {
	private WebDriver driver;
	private String baseUrl;
	private StringBuffer verificationsErrors=new StringBuffer();

	@Before
	public void setUp() throws Exception{
		//자신의 데스크탑에 다운로드 받은 크롬 드라이브 경로 설정
		System.setProperty("webdriver.chrome.driver", "C:\\Users\\psj\\Downloads\\chromedriver_win32\\chromedriver.exe");
		driver=new ChromeDriver();
		baseUrl="http://www.daum.net";
		driver.manage().timeouts().implicitlyWait(30, TimeUnit.SECONDS);
		System.out.println("=chromedriver start=");
	}
	
	@Test
	public void testDriver() throws Exception{
		driver.get(baseUrl+"/");
		 //크롬이 실행될때 대기시간을 고려해서 스케줄러의 정확도를 위해 현재 실행중인 스레드를 지정된 밀리 초 동안 일시적으로 실행 중지 되도록 스케줄링 설정
		 Thread.sleep(3000);
		  driver.get("https://logins.daum.net/accounts/loginform.do");
		  driver.findElement(By.name("id")).sendKeys("");//아이디 넣기
		  driver.findElement(By.name("pw")).sendKeys("");//비밀번호 넣기
		  driver.findElement(By.id("loginBtn")).click();
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
