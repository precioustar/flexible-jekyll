---
layout: post
title: Anti-Pattern
date: 2018-06-03 00:00:00 +0900
description: You’ll find this post in your `_posts` directory. Go ahead and edit it and re-build the site to see your changes. # Add post description (optional)
img: js-1.png # Add image post (optional)
tags: [Js, AntiPattern] # add tag
---
## 개요

<span style="color:#555555">자바스크립트로 프로그래밍을 할때 피해야 할 [안티패턴](https://github.com/nhnent/fe.javascript/wiki/안티-패턴)은 반드시 숙지한다.</span>

# 안티 패턴

실제 습관적으로 많이 사용하는 패턴이지만,
성능, 디버깅, 유지보수, 가독성 등의 측면에서 서비스에 부정적인 영향을 줄 수 있어 **사용을 지양하는 패턴**이다.
이 문서에서는 자주 사용하는 안티 패턴을 사례별로 설명하고, 개선 방법을 가이드 한다.

## 목차

* [\<script>는 문서 하단에서 include 하라](#script%EB%8A%94-%EB%AC%B8%EC%84%9C-%ED%95%98%EB%8B%A8%EC%97%90%EC%84%9C-include-%ED%95%98%EB%9D%BC)
* [jQuery 같은 외부 소스는 다운로드해서 사용하라](#jquery-%EA%B0%99%EC%9D%80-%EC%99%B8%EB%B6%80-%EC%86%8C%EC%8A%A4%EB%8A%94-%EB%8B%A4%EC%9A%B4%EB%A1%9C%EB%93%9C%ED%95%B4%EC%84%9C-%EC%82%AC%EC%9A%A9%ED%95%98%EB%9D%BC)
* [전역변수 대신 네임스페이스를 사용하라](#%EC%A0%84%EC%97%AD%EB%B3%80%EC%88%98-%EB%8C%80%EC%8B%A0-%EB%84%A4%EC%9E%84%EC%8A%A4%ED%8E%98%EC%9D%B4%EC%8A%A4%EB%A5%BC-%EC%82%AC%EC%9A%A9%ED%95%98%EB%9D%BC)
* [변수 선언시 반드시 "var" 키워드를 사용하라](#%EB%B3%80%EC%88%98-%EC%84%A0%EC%96%B8%EC%8B%9C-%EB%B0%98%EB%93%9C%EC%8B%9C-var-%ED%82%A4%EC%9B%8C%EB%93%9C%EB%A5%BC-%EC%82%AC%EC%9A%A9%ED%95%98%EB%9D%BC-es5) `ES5`
* [배열과 객체는 리터럴 표기법을 사용하라](#%EB%B0%B0%EC%97%B4%EA%B3%BC-%EA%B0%9D%EC%B2%B4%EB%8A%94-%EB%A6%AC%ED%84%B0%EB%9F%B4-%ED%91%9C%EA%B8%B0%EB%B2%95%EC%9D%84-%EC%82%AC%EC%9A%A9%ED%95%98%EB%9D%BC)
* [변수와 함수는 사용하기 전에 선언하라](#%EB%B3%80%EC%88%98%EC%99%80-%ED%95%A8%EC%88%98%EB%8A%94-%EC%82%AC%EC%9A%A9%ED%95%98%EA%B8%B0-%EC%A0%84%EC%97%90-%EC%84%A0%EC%96%B8%ED%95%98%EB%9D%BC)
* [문장의 끝은 세미콜론(;)을 사용하라](#%EB%AC%B8%EC%9E%A5%EC%9D%98-%EB%81%9D%EC%9D%80-%EC%84%B8%EB%AF%B8%EC%BD%9C%EB%A1%A0%EC%9D%84-%EC%82%AC%EC%9A%A9%ED%95%98%EB%9D%BC)
* ["==" 대신 "==="을 사용하라](#-%EB%8C%80%EC%8B%A0-%EC%9D%84-%EC%82%AC%EC%9A%A9%ED%95%98%EB%9D%BC)
* [{}를 생략하지 마라](#%EB%A5%BC-%EC%83%9D%EB%9E%B5%ED%95%98%EC%A7%80-%EB%A7%88%EB%9D%BC)
* [parseInt는 두 번째 매개변수인 기수를 생략하지 마라](#parseint%EB%8A%94-%EB%91%90-%EB%B2%88%EC%A7%B8-%EB%A7%A4%EA%B0%9C%EB%B3%80%EC%88%98%EC%9D%B8-%EA%B8%B0%EC%88%98%EB%A5%BC-%EC%83%9D%EB%9E%B5%ED%95%98%EC%A7%80-%EB%A7%88%EB%9D%BC)
* [switch문에서 break를 생략하지 마라](#switch%EB%AC%B8%EC%97%90%EC%84%9C-break%EB%A5%BC-%EC%83%9D%EB%9E%B5%ED%95%98%EC%A7%80-%EB%A7%88%EB%9D%BC)
* [다양한 환경을 지원해야 한다면 switch문을 사용하지 마라](#%EB%8B%A4%EC%96%91%ED%95%9C-%ED%99%98%EA%B2%BD%EC%9D%84-%EC%A7%80%EC%9B%90%ED%95%B4%EC%95%BC-%ED%95%9C%EB%8B%A4%EB%A9%B4-switch%EB%AC%B8%EC%9D%84-%EC%82%AC%EC%9A%A9%ED%95%98%EC%A7%80-%EB%A7%88%EB%9D%BC)
* [배열의 순회는 for-in을 사용하지 마라](#%EB%B0%B0%EC%97%B4%EC%9D%98-%EC%88%9C%ED%9A%8C%EB%8A%94-for-in%EC%9D%84-%EC%82%AC%EC%9A%A9%ED%95%98%EC%A7%80-%EB%A7%88%EB%9D%BC)
* [배열 순회시 array.length는 캐시해놓고 사용하라](#%EB%B0%B0%EC%97%B4-%EC%88%9C%ED%9A%8C%EC%8B%9C-arraylength%EB%8A%94-%EC%BA%90%EC%8B%9C%ED%95%B4%EB%86%93%EA%B3%A0-%EC%82%AC%EC%9A%A9%ED%95%98%EB%9D%BC)
* [순회와 관련 없는 작업들은 순환문 밖에서 처리하라](#%EC%88%9C%ED%9A%8C%EC%99%80-%EA%B4%80%EB%A0%A8-%EC%97%86%EB%8A%94-%EC%9E%91%EC%97%85%EB%93%A4%EC%9D%80-%EC%88%9C%ED%99%98%EB%AC%B8-%EB%B0%96%EC%97%90%EC%84%9C-%EC%B2%98%EB%A6%AC%ED%95%98%EB%9D%BC)
* [순환문에서 continue를 사용하지 마라](#%EC%88%9C%ED%99%98%EB%AC%B8%EC%97%90%EC%84%9C-continue%EB%A5%BC-%EC%82%AC%EC%9A%A9%ED%95%98%EC%A7%80-%EB%A7%88%EB%9D%BC)
* [try-catch는 순환문 안에서 사용하지 마라](#try-catch%EB%8A%94-%EC%88%9C%ED%99%98%EB%AC%B8-%EC%95%88%EC%97%90%EC%84%9C-%EC%82%AC%EC%9A%A9%ED%95%98%EC%A7%80-%EB%A7%88%EB%9D%BC)
* [배열의 요소를 삭제할 때 delete를 사용지 마라](#%EB%B0%B0%EC%97%B4%EC%9D%98-%EC%9A%94%EC%86%8C%EB%A5%BC-%EC%82%AD%EC%A0%9C%ED%95%A0-%EB%95%8C-delete%EB%A5%BC-%EC%82%AC%EC%9A%A9%EC%A7%80-%EB%A7%88%EB%9D%BC)
* [두 번 이상 사용되는 DOM 요소는 캐시를 사용하라](#%EB%91%90-%EB%B2%88-%EC%9D%B4%EC%83%81-%EC%82%AC%EC%9A%A9%EB%90%98%EB%8A%94-dom-%EC%9A%94%EC%86%8C%EB%8A%94-%EC%BA%90%EC%8B%9C%EB%A5%BC-%EC%82%AC%EC%9A%A9%ED%95%98%EB%9D%BC)
* [스타일 변경은 한 번에 몰아서 처리하라](#%EC%8A%A4%ED%83%80%EC%9D%BC-%EB%B3%80%EA%B2%BD%EC%9D%80-%ED%95%9C-%EB%B2%88%EC%97%90-%EB%AA%B0%EC%95%84%EC%84%9C-%EC%B2%98%EB%A6%AC%ED%95%98%EB%9D%BC)
* [이벤트는 inline 방식을 사용하지 마라](#%EC%9D%B4%EB%B2%A4%ED%8A%B8%EB%8A%94-inline-%EB%B0%A9%EC%8B%9D%EC%9D%84-%EC%82%AC%EC%9A%A9%ED%95%98%EC%A7%80-%EB%A7%88%EB%9D%BC)
* [eval()은 사용하지 마라](#eval%EC%9D%80-%EC%82%AC%EC%9A%A9%ED%95%98%EC%A7%80-%EB%A7%88%EB%9D%BC)
* [setTimeout, setInterval시 콜백 함수는 문자열로 전달하지 마라](#settimeout-setinterval%EC%8B%9C-%EC%BD%9C%EB%B0%B1-%ED%95%A8%EC%88%98%EB%8A%94-%EB%AC%B8%EC%9E%90%EC%97%B4%EB%A1%9C-%EC%A0%84%EB%8B%AC%ED%95%98%EC%A7%80-%EB%A7%88%EB%9D%BC)
* [함수 생성자 new Function()은 사용하지 마라](#%ED%95%A8%EC%88%98-%EC%83%9D%EC%84%B1%EC%9E%90-new-function%EC%9D%80-%EC%82%AC%EC%9A%A9%ED%95%98%EC%A7%80-%EB%A7%88%EB%9D%BC)
* [with()는 사용하지 마라](#with%EB%8A%94-%EC%82%AC%EC%9A%A9%ED%95%98%EC%A7%80-%EB%A7%88%EB%9D%BC)
* [네이티브 객체는 확장하거나 오버라이드하지 마라](#%EB%84%A4%EC%9D%B4%ED%8B%B0%EB%B8%8C-%EA%B0%9D%EC%B2%B4%EB%8A%94-%ED%99%95%EC%9E%A5%ED%95%98%EA%B1%B0%EB%82%98-%EC%98%A4%EB%B2%84%EB%9D%BC%EC%9D%B4%EB%93%9C%ED%95%98%EC%A7%80-%EB%A7%88%EB%9D%BC)
* [단항 연산자를 사용하지 마라](#%EB%8B%A8%ED%95%AD-%EC%97%B0%EC%82%B0%EC%9E%90%EB%A5%BC-%EC%82%AC%EC%9A%A9%ED%95%98%EC%A7%80-%EB%A7%88%EB%9D%BC)
* [this에 대한 레퍼런스를 저장하지 마라](#this%EC%97%90-%EB%8C%80%ED%95%9C-%EB%A0%88%ED%8D%BC%EB%9F%B0%EC%8A%A4%EB%A5%BC-%EC%A0%80%EC%9E%A5%ED%95%98%EC%A7%80-%EB%A7%88%EB%9D%BC)
* [변수 선언 시 "const", "let" 키워드를 사용하라](#%EB%B3%80%EC%88%98-%EC%84%A0%EC%96%B8-%EC%8B%9C-const-let-%ED%82%A4%EC%9B%8C%EB%93%9C%EB%A5%BC-%EC%82%AC%EC%9A%A9%ED%95%98%EB%9D%BC-es6) `ES6`
* ["require", "module.exports" 대신 "import", "export"를 사용하라](#require-moduleexports-%EB%8C%80%EC%8B%A0-import-export%EB%A5%BC-%EC%82%AC%EC%9A%A9%ED%95%98%EB%9D%BC-es6) `ES6`
* [import문은 파일의 맨 위에 선언하라](#import%EB%AC%B8%EC%9D%80-%ED%8C%8C%EC%9D%BC%EC%9D%98-%EB%A7%A8-%EC%9C%84%EC%97%90-%EC%84%A0%EC%96%B8%ED%95%98%EB%9D%BC-es6) `ES6`
* [제너레이터를 사용할 때 *의 위치에 주의하라](#%EC%A0%9C%EB%84%88%EB%A0%88%EC%9D%B4%ED%84%B0%EB%A5%BC-%EC%82%AC%EC%9A%A9%ED%95%A0-%EB%95%8C-%EC%9D%98-%EC%9C%84%EC%B9%98%EC%97%90-%EC%A3%BC%EC%9D%98%ED%95%98%EB%9D%BC-es6) `ES6`

- - -
**참고**: 이 글에서는 ES3 이상의 컨벤션을 다룬다.
만약, 일부 버전에서 지켜야할 컨벤션이라면 태그를 달아 표시한다. 
모든 버전에서 지켜야할 컨벤션은 태그를 달지 않았으며, 이 경우 예제코드는 ES3-5 기준을 따른다.

* `ES5` - ES3-5
* `ES6` - ES6+

- - -

<br>
<br>


## \<script>는 문서 하단에서 include 하라

전통적으로 \<script>요소는 \<head>요소 안에 쓰는 것이 일반적이었다.  
이런 형식을 취한 목적은 CSS나 자바스크립트와 같은 외부 파일 참조를 한 곳에서 처리하기 위함이었다.  
하지만 이런 형식은 CSS와 자바스크립트를 전부 다운로드하고, 파싱하고, 컴파일 할 때까지 페이지의 렌더링을 멈추게 한다.  
브라우저는 \<body> 요소를 만나게 되면서 렌더링이 시작되기 때문이다.  
특히 자바스크립트를 많이 사용하는 페이지일수록 이런 지연은 눈에 띌 정도로 커진다.  

> Solution:  
> JavaScript 코드 모두를 \<body> 요소 안, 맨 마지막에 쓴다.

[Bad]

``` html
<!DOCTYPE html>
<html>
    <head>
        <title>HTML Page</title>
        <script src="../js/jquery-1.8.3.min.js"></script>
        <script src="../js/common.js"></script>
        <script src="../js/applicationMain.js"></script>
    </head>
    ...
```

[Good]

``` html
<!DOCTYPE html>
<html>
    <head>
        <title>HTML Page</title>
    </head>
    <body>
        ...
        <!-- body 요소 안, 맨 마지막에 씀 -->
        <script src="../js/jquery-1.8.3.min.js"></script>
        <script src="../js/common.js"></script>
        <script src="../js/applicationMain.js"></script>
    </body>
</html>
```

<br>
<br>


## jQuery 같은 외부 소스는 다운로드해서 사용하라

jQuery 같은 오픈 소스들은 어디서나 직접 접근이 가능한 url을 제공하는데,  
이런 외부 url 대신 프로젝트 저장소에 소스 코드를 직접 다운로드해서 사용해야 한다.  
외부 사정에 의해 url이 변경되거나 장애가 발생할 경우,  
그 영향이 해당 서비스에 그대로 반영되어 장애로까지 이어질 수 있기 때문이다.  

> Solution:  
> 외부 url을 사용하지 말고, 프로젝트 저장소에 직접 다운로드해서 사용한다.  
> 추가로, 개발자 버전이 아닌 압축 버전을 사용한다.  
> (압축 버전은 파일명에서 "min" 키워드로 확인할 수 있다.)  

[Bad]

``` html
<!DOCTYPE html>
<html>
    <head>
        <title>HTML Page</title>
        <script src="http://code.jquery.com/jquery-1.8.3.min.js"></script>
            ...
```

[Good]

``` html
<!DOCTYPE html>
<html>
    <head>
        <title>HTML Page</title>
        <script src="../js/jquery-1.8.3.min.js"></script>
        ...
```

<br>
<br>


## 전역변수 대신 네임스페이스를 사용하라

자바스크립트는 전역변수에 기반을 두고 있다.  
즉, 모든 컴파일 단위는 하나의 공용 전역 객체(window)에 로딩된다.  
전역변수는 언제든지 프로그램의 모든 부분에서 접근할 수 있기 때문에 편하지만,  
바꿔 말하면 프로그램의 모든 부분에서 변경될 수 있고, 그로 인해 프로그램에 치명적인 오류를 발생시킬 수 있다.  
전역변수 사용은 하위 모듈들이 독립적으로 실행되는 것을 어렵게하고, 프로그램의 신뢰도를 현격히 떨어뜨린다.  

> Solution:  
> 전역에 1개의 변수만 생성하고,  
> 그 외 모든 변수와 함수가 그 아래 존재하도록 해야 한다.  
> 이렇게 전역에 단 하나의 변수(객체)만 추가하는 방법이 **네임스페이스**라는 개념이다.  

[Bad]

``` javascript
var name = '';
function sayName() {
    alert(name);
}
```

[Good]

``` javascript
// 전역변수 global 하나만 추가
var global = {
    // 전역 변수 하위에 객체와 함수가 존재
    name: '',
    sayName: function() {
        alert(this.name);
    }
};
```

[Guide]

``` javascript
// NHN Ent.의 네임스페이스로 ne를 사용
var ne = window.ne || {};

// 그 하위에 서비스명을 2차 네임스페이스로 사용
ne.serviceName = ne.serviceName || {};

// 페이지별 또는 기능별 모듈명을 3차 네임스페이스로 사용
ne.serviceName.util = {...};
ne.serviceName.component = {...};
ne.serviceName.model = {...};

// 필요에 따라 4차, 5차 네임스페이스로 확장하여 사용
ne.serviceName.view.layer = {...};
ne.serviceName.view.painter = {...};
```

<br>
<br>


## 변수 선언시 반드시 "var" 키워드를 사용하라 `ES5`

**ES6 이상**인 경우 [변수 선언 시 "const", "let" 키워드를 사용하라](#%EB%B3%80%EC%88%98-%EC%84%A0%EC%96%B8-%EC%8B%9C-const-let-%ED%82%A4%EC%9B%8C%EB%93%9C%EB%A5%BC-%EC%82%AC%EC%9A%A9%ED%95%98%EB%9D%BC-es6)를 보세요.  

"var" 키워드 없이 선언된 변수는 전역으로 처리된다.  
실수로 "var"를 사용하지 않았더라도 에러 없이 변수를 사용할 수 있지만,  
이로 인해 전역 환경이 오염되고, 때때로 매우 찾아내기 어려운 버그를 만들어 낸다.  

> Solution:  
> 변수 선언 시 "var"를 반드시 사용한다.  
> 추가로, 호이스팅되지 않도록 필요한 변수는 함수 상단에서 한번에 선언한다.

[Bad]

``` javascript
foo = value;
```

[Good]

``` javascript
var foo = value;
```

<br>
<br>


## 배열과 객체는 리터럴 표기법을 사용하라

배열이나 객체를 선언할 때 생성자를 사용할 수도 있지만,  
그보다는 리터럴 표기법을 사용하는 것이 가독성과 속도 면에서 좋다.  
리터럴 표기법은 간결하고 직관적이며, 실제로 자바스크립트 엔진은 리터럴 표기법에 맞게 최적화되어있다.  
뿐만 아니라 배열 생성자는 숫자 한 개를 파라메터로 넘길 경우 다르게 동작하여 실수하기 쉽다.  

> Solution:  
> 배열과 객체는 리터럴로 선언한다.

[Bad]

``` javascript
var emptyArr = new Array();
var emptyObj = new Object();

var arr = new Array(1, 2, 3, 4, 5);
var obj = new Object();
```

[Good]

``` javascript
var emptyArr = [];
var emptyObj = {};

var arr = [1, 2, 3, 4, 5];
var obj = {
    pro1: 'val1',
    pro2: 'val2'
};
```

<br>
<br>


## 변수와 함수는 사용하기 전에 선언하라

자바스크립트는 블록 구문을 사용하기는 하지만, 블록 유효범위를 제공하지는 않는다.  
즉, 블록 내에서 선언되기만 하면, 선언된 위치에 상관없이 블록 내 어느 곳에서든 사용이 가능하다.  
(10라인에서 선언된 변수(or 함수)를 2라인에서 먼저 사용할 수 있다.)  
이것은 자바스크립트가 컴파일 될 때 내부적으로 호이스팅(끌어올림)이 수행되기 때문인데,  
이로 인해 코드 가독성이 떨어지고 오류를 찾기 어려우며, 컴파일 시 호이스팅 비용이 발생한다.

> Solution:  
> 함수는 반드시 사용하기 전에 선언하고,  
> 변수는 "var" 키워드와 함께 함수 상단에서 선언한다.

[Bad]

``` javascript
doSomething();

function doSomething() {
    foo1 = foo2;
    ...
    var foo1 = 'value1';
    foo3 = foo4;
    ...
    var foo4 = 'value4';
    var foo2;
}
```

[Good]

``` javascript
// 함수를 사용하기 전에 선언
function avoidHoisting() {
    // 함수내에서 사용하는 모든 변수를 함수 상단에서 한번에 선언
    var foo1 = 'value1';
    var foo4 = 'value4';
    var foo3 = foo4;
    var foo2;
    ...
}

avoidHoisting();
```

<br>
<br>


## 문장의 끝은 세미콜론(;)을 사용하라

자바스크립트에서 문장은 세미콜론(;)으로 끝나야 하지만, 문법적으로 이를 강제하지 않기 때문에 생략할 수 있다.  
자바스크립트 파서가 이를 자동으로 삽입해주기 때문이다.  
하지만 이런 세미콜론(;) 자동 삽입은 종종 의도하지 않았던 코드(전혀 다른 코드)로 해석되어  
생각하지 못한 오류를 만들고 디버깅을 어렵게 한다.  
또한 자바스크립트 파서가 세미콜론(;)의 위치를 계산하고 삽입하는데 추가적인 비용이 발생한다.  

> Solution:  
> 문장의 끝은 반드시 세미콜론(;)을 사용한다.

[Bad]

``` javascript
function parserTest(options) {
    console.log('test start!!!')
    (options || []).forEach(function(i) {
    ...
    })
}

/**
 * parser는 아래와 같이 해석하고, 세미콜론을 삽입
 * function parserTest(options) {
 *     console.log('test start!!!')(options || []).forEach(function(i) {...});
 * }
 */
```

[Good]

``` javascript
function parserTest(options) {
    console.log('test start!!!');
    (options || []).forEach(function(i) {
    ...
    });
}
```

<br>
<br>


## "==" 대신 "==="을 사용하라

자바스크립트는 두 값을 비교 또는 산술하기 전에 내부적으로 암묵적인 강제 형 변환이 실행된다.  
때문에 이형 데이터 타입간의 비교와 산술이 가능한데, 이는 코드 전체에 데이터 관리를 어렵게 만들고,  
때로는 연산 과정에서 발생하는 데이터 타입의 오류를 덮어버리기도 한다.  
이중 가장 혼란을 일으키는 것이 비교 연산이다.  
이는 코드를 작성하는 사람과 읽는 사람 모두 강제 형 변환 규칙을 이해하고, 형 변환이 발생할 수 있는 모든 케이스를 고려해야 하는 어려움을 만든다.

> Solution:  
> 암묵적인 강제 형 변환이 일어나지 않도록 삼중 등호 연산자("===" 또는 "!==")를 사용한다.  
> 만약 이형 데이터간 비교가 필요하다면 명시적으로 강제 형 변환 후 삼중 등호 연산자("===" 또는 "!==")를 사용한다.

[Bad]

``` javascript
var today = new Date();
if (form.month.value == (today.getMonth() + 1) && form.day.value == today.getDate()) {
    // 생일인 경우
    ...
} else {
    // 생일이 아닌 경우
    ...
}
```

[Good]

``` javascript
var month = parseInt(form.month.value, 10);
var date = parseInt(form.month.value, 10);

if (month === (today.getMonth() + 1) && date === today.getDate()) {
    // 생일인 경우
    ...
} else {
    // 생일이 아닌 경우
    ...
}
```

[Tip]

``` javascript
// 스트링으로 변환하기
String(10) === '10';         // true
10 + '' === '10';            // true

// 숫자로 변환하기
parseInt('10', 10) === 10;   // true
Number('10') === 10;         // true
+'10' === 10;                // true

// 불린으로 변환하기
!!'foo';                     // true
!!'';                        // false
!!'0';                       // true
!!'1';                       // true
!!'-1'                       // true
!!{};                        // true
!!true;                      // true
```

<br>
<br>


## {}를 생략하지 마라

if/while/do/for 문은 한 줄짜리 블록 일 경우 {}를 생략할 수 있다.  
하지만 이런 패턴은 코드 구조를 애매하게 만들어 가독성이 떨어지고, 문법적 오류가 아니기 때문에 디버깅이 어렵다.  
이런 코드는 이후 오류 발생 확률이 높은 잠재적 위험 요소가 된다.  

> Solution:  
> 반드시 {}로 블록을 명확하게 만든다.

[Bad]

``` javascript
if (true)
    doSomething();
else
    doNotAnything();
```

[Good]

``` javascript
if (true) {
    doSomething();
} else {
    doNotAnything();
}
```

<br>
<br>


## parseInt는 두 번째 매개변수인 기수를 생략하지 마라

parseInt는 문자열을 정수로 바꿔주는 함수이다.  
이 때 두 번째 인자로 기수(진법)를 넘기는데, 생략될 경우 변환될 숫자 형식은 브라우저에서 판단하도록 일임된다.  
이는 날짜나 시간을 파싱할 때 문제를 일으키는데, 브라우저는 첫 번째 문자열이 0일 경우 문자열을 8진수로 간주하기 때문이다.  

> Solution:  
> 항상 두 번째 매개변수를 명시하여 오류를 사전에 예방한다.  
> 10진수로 변환이 필요한 경우라면 Number()를 사용하는 것이 속도 측면에서 이득이다.  

[Bad]

``` javascript
var month = parseInt('08');          // 8
var day = parseInt('09');            // 9
```

[Good]

``` javascript
var month = parseInt('08', 10);      // 8
var day = parseInt('09', 10);        // 9
```

[Tip]

``` javascript
// 10진수 변환하는 경우라면 Number()를 사용하거나 +연산자를 붙이는 것이 더 빠르다.
var month = Number('08');           // 8
var day = +'09';                    // 9
```

<br>
<br>


## switch문에서 break를 생략하지 마라

switch의 각 case절은 break 키워드를 이용하여 명시적으로 해당 절을 벗어나게 하지 않으면 다음 case절까지 계속해서 실행된다.  
이를 switch fall through라 하는데, 이는 break 생략이 의도된 것인지, 실수에 의한 누락인지 알 수 없는 모호한 코드를 만든다.  
이런 코드는 문법적 오류가 아니기 때문에 디버깅이 어렵고, 이후 오류 발생 확률이 높은 잠재적 위험 요소가 된다.  

> Solution:  
> break를 생략하지 않는다.  
> 꼭 생략하고 싶다면 어떤 이유로 생략된 케이스인지 반드시 주석으로 그 내용을 상세히 명시한다.

[Bad]

``` javascript
function getGroup(part) {
    var group;
    switch (part) {
        case 'A':
        case 'B':
        case 'C':
            group = 'RED';
            break;
        case 'D':
        case 'E' :
            group = 'BLUE';
            break;
        case 'F':
            group = 'GREEN';
            break;
    }
    return group;
}
```

[Good]

``` javascript
function getGroup(part) {
    var group;
    switch (part) {
        case 'A':
            group = 'RED';
            break;
        case 'B':
            group = 'RED';
            break;
        case 'C':
            group = 'RED';
            break;
        case 'D':
            group = 'BLUE';
            break;
        case 'E' :
            group = 'BLUE';
            break;
        case 'F':
            group = 'GREEN';
            break;
        // no default
    }
    return group;
}
```

또는

``` javascript
function getGroup(part) {
    var group;
    switch (part) {
        case 'A':
            // A, B, C는 같은 그룹으로 처리하기 위해 break 생략
        case 'B':
            // A, B, C는 같은 그룹으로 처리하기 위해 break 생략
        case 'C':
            group = 'RED';
            break;
        case 'D':
            // D, E는 같은 그룹으로 처리하기 위해 break 생략
        case 'E' :
            group = 'BLUE';
            break;
        case 'F':
            group = 'GREEN';
            break;
        // no default
    }
    return group;
}
```

<br>
<br>


## 다양한 환경을 지원해야 한다면 switch문을 사용하지 마라

switch는 if에 비해 간결한 코드를 작성할 수 있지만, 비교적 많은 메모리를 사용하고 저버전 IE에서는 처리 성능도 좋지 않다.  
최신 버전 IE와 크롬과 같은 고성능 브라우저만 지원하는 서비스라면 이런 부분을 크게 고민하지 않아도 되겠지만,  
저버전 IE를 포함한 다양한 환경을 지원해야 하는 서비스라면 switch문의 득과 실에 대해 따져볼 필요가 있다.  

* if문은 원하는 조건이 나올 때까지 순차적으로 모든 비교문을 순회하면서 비교
* switch문은 jump-table을 사용하여 한 번에 원하는 곳으로 이동

때문에

* if문은 조건 문의 개수만큼 O(n)의 시간 복잡도를 갖게 되어 성능의 단점
* switch문은 case의 개수만큼 jump-table을 차지하므로 메모리의 단점
    * 저버전 IE에서는 성능에도 문제가 있음
    * case의 조건이 정수형이 아닌 경우(문자열 또는 연산)라면 jump-table을 쓸 수 없어 성능에도 이득 없음

> Solution:

> * 3개 이하의 조건문에는 if문을 사용한다.  
> 확률이 높은 조건을 먼저 비교한다.  
> * 4개 이상 10개 이하의 조건문은 switch문을 사용한다.  
> 고성능 브라우저 지원 환경일 경우에만 해당한다.  
> 확률이 높은 조건을 먼저 비교하고, 정수형 조건을 사용한다.  
> * 저버전 IE를 지원해야 하거나 10개 이상의 조건문이라면 객체 리터럴이나 모듈 방식을 사용한다.  

[Bad]

``` javascript
switch(foo) {
    case 'alpha':
        alpha();
        break;
    case 'beta':
        beta();
        break;
    default:
        ...
        break;
}
```

[Good]

``` javascript
// 객체 리터럴 방식
var switchObj = {
    alpha: function() {
        ...
    },
    beta: function() {
        ...
    },
    _default: function() {
        ...
    }
};
switchObj[foo](args);
```

또는

``` javascript
// 모듈 방식
var switchModule = (function() {
    return {
        alpha: function() {
            ...
        },
        beta: function() {
            ...
        },
        _default: function() {
            ...
        }
    };
})();
switchModule[foo](args);
```

<br>
<br>


## 배열의 순회는 for-in을 사용하지 마라

보통 객체 순회에는 for-in을 사용한다.  
자바스크립트에서는 배열(Array)도 객체(Object)이지만 배열을 순회할 때 for-in을 사용해서는 안된다.  
for-in은 프로토타입 체인에 있는 모든 프로퍼티를 순회하기 때문에 for를 사용할 때보다 보통 10배 이상 느리고, 
index 순서대로 배열을 순회한다고 보장할 수 없다.  
순회 순서는 브라우저에 따라 다를 수 있다.  

> Solution:  
> 배열을 순회할 때는 for문을 사용한다.

[Bad]

``` javascript
var scores = [70, 75, 80, 61, 89, 56, 77, 83, 90, 93, 66];
var total = 0;
var score;

for (score in scores) {
    total += scores[score];
}
```

[Good]

``` javascript
var scores = [70, 75, 80, 61, 89, 56, 77, 83, 90, 93, 66];
var total = 0;
var i = 0;
var len = scores.length;

for (; i < len; i += 1) {
    total += scores[i];
}
```

<br>
<br>


## 배열 순회시 array.length는 캐시해놓고 사용하라

for문은 주어진 조건 표현식이 true로 평가되는 동안 실행을 반복한다.  
때문에 매 순회 시 조건 표현식을 다시 평가하는데, 100번의 순회가 있었다면 100번의 평가가 수행된다는 뜻이다.  
결국 프로그램의 성능을 높이기 위해서는 불필요한 계산이 반복되지 않도록 조건 표현식을 최적화해야 하는데,  
대표적인 예가 배열의 길이를 한 번만 계산하도록 캐싱하는 것이다.  
실제로 캐시를 사용하면 그렇지 않은 경우보다 성능이 2배 정도 빨라진다.  

> Solution:  
> for문에서 배열의 길이는 항상 캐시로 처리한다.

[Bad]

``` javascript
var scores = [70, 75, 80, 61, 89, 56, 77, 83, 90, 93, 66];
var total = 0;
var i = 0;

for (; i < scores.length; i += 1) {
    total += scores[i];
}
```

[Good]

``` javascript
var scores = [70, 75, 80, 61, 89, 56, 77, 83, 90, 93, 66];
var total = 0;
var i = 0;
var len = scores.length;
for (; i < len; i += 1) {
    total += scores[i];
}
```

<br>
<br>


## 순회와 관련 없는 작업들은 순환문 밖에서 처리하라

순환문은 주어진 조건 표현식이 true로 평가되는 동안 실행을 반복하기 때문에 프로그램의 성능에 큰 영향을 미친다.  
코드를 리팩토링할 때 첫 번째로 수행하는 작업이 이런 순환문의 최적화 작업이고,  
대부분의 리팩토링 작업에서 가장 큰 이득을 보는 부분이기도 하다.  
변수 선언이나, 동일한 값의 반복 할당 또는 변치 않는 값의 반복 계산 등 순회와 상관없는 작업들이 순회문 안에서 이뤄지지는 않도록 항상 주의해야 한다.

> Solution:  
> 순회와 관련된 작업만 수행하도록 순환문을 최적화한다.

[Bad]

``` javascript
for (var i = 0; i < days.length; i += 1) {
    var today = new Date().getDate();
    var element = getElement(i);
    if (today === days[i]) {
        element.className = 'today';
    }
}
```

[Good]

``` javascript
var today = new Date().getDate();
var i = 0;
var len = days.length;
var element;

for (; i < len; i += 1) {
    if (today === days[i]) {
        element = getElement(i);
        element.className = 'today';
        break;
    }
}
```

<br>
<br>


## 순환문에서 continue를 사용하지 마라

더글라스 클락포드는 "리팩토링을 통해 continue를 제거했을 때 성능이 향상되지 않은 경우를 본 적이 없다"고 말했다.  
continue를 사용하면 JavaScript 엔진에서 별도의 실행 컨텍스트를 만들어 관리하므로 성능 문제가 발생할 수 있고, 순환문의 성능이 전체 성능에 많은 영향을 주기 때문에 사용하지 않는 것이 좋다.  
또한 continue 문을 잘 사용하면 코드를 간결하게 작성 할 수 있지만, 과용할 경우 디버깅 시 개발자의 의도를 파악하기 어렵게 만들어 유지 보수에 문제가 생길 수 있다.  

> Solution:  
> 순환문 내부에서 특정 코드의 실행을 건너뛸때는 조건문을 사용한다.

[Bad]

``` javascript
var loopCount = 0;
var i = 1;
for (; i < 10; i += 1) {
    if (i >= 5) {
        continue;
    }
    loopCount += 1;
}
```

[Good]

``` javascript
var loopCount = 0;
var i = 1;
for (; i < 10; i += 1) {
    if (i < 5) {
        loopCount += 1;
    }
}
```

<br>
<br>


## try-catch는 순환문 안에서 사용하지 마라

흔히 예외 처리를 위해 try-catch를 사용하는데, catch문이 실행될 때 내부적으로 예외 객체를 변수에 할당하게 된다.  
때문에 try-catch가 순환문 안에서 사용될 경우, 순회가 반복될 때마다 런타임의 현재 스코프에서 예외 객체 할당을 위한 새로운 변수가 생성된다.  

> Solution:  
> try-catch를 감싼 함수를 만들고, 순환문 내부에서 함수를 호출하라.

[Bad]

``` javascript
var i = 0;
var len = array.length;

for (; i < len; i += 1) {
    try {
        // ...
    } catch (error) {
        // ...
    }
}
```

[Good]

``` javascript
var i = 0;
var len = array.length;

function doSomething() {
    try {
        ...
    } catch (error) {
        ...
    }
}

for (; i < len; i += 1) {
    doSomething();
}
```

<br>
<br>


## 배열의 요소를 삭제할 때 delete를 사용지 마라

보통 객체의 프로퍼티를 삭제할 때 delete를 사용한다.  
단순히 undefined로 설정되는 것이 아니라 프로퍼티 자체가 완전히 삭제되어 더 이상 존재하지 않게 된다.  
자바스크립트에서는 배열(Array)도 객체(Object)이지만 배열의 요소 삭제는 객체의 프로퍼티 삭제와 조금 다르게 동작한다.  
배열의 요소에 delete를 사용할 때 사실은 배열의 길이가 줄어들거나 삭제된 요소를 기준으로 shift된 효과를 기대하겠지만, 결과적으로는 해당 요소의 위치에 구멍이 생길 뿐 변하는 것은 아무것도 없다.  
삭제된 요소 외에 다른 어떤 요소도 영향을 받지 않으며, 배열의 길이 또한 변하지 않는다.  

> Solution:  
> 배열의 요소를 삭제할 때는 splice 또는 length를 사용한다.

[Bad]

``` javascript
var numbers = ['zero', 'one', 'two', 'three', 'four', 'five'];
delete numbers[2];                   // ['zero', 'one', undefined, 'three', 'four', 'five'];
```

[Good]

``` javascript
var numbers = ['zero', 'one', 'two', 'three', 'four', 'five'];
numbers.splice(2, 1);                // ['zero', 'one', 'three', 'four', 'five'];
```

[Tip]

``` javascript
// 배열의 사이즈를 줄이고 싶다면 length를 사용
var numbers = ['zero', 'one', 'two', 'three', 'four', 'five'];
numbers.length = 4;                 // ['zero', 'one', 'two', 'three'];
```

<br>
<br>


## 두 번 이상 사용되는 DOM 요소는 캐시를 사용하라

비단 DOM 요소에만 국한되는 내용은 아니지만 DOM 요소에서 더 두드러지게 성능 저하가 나타난다.  
우리는 인지하지 못하지만 코드가 실행되면 DOM 요소나 객체, 배열 등 특정 값을 읽고 쓸 때에도 비용이 발생하는데, 특히 DOM과 객체 같이 Depth가 있는 트리 구조 데이터의 경우 Depth가 깊어질수록 해당 값까지 찾아가는데 많은 비용이 발생한다.  
때문에 자주 사용되는 값에 캐시를 사용하면, 탐색 비용이 줄어들어 그 만큼 성능적 이득을 볼 수 있다.  

> Solution:  
> 탐색비용을 절약할 수 있도록 캐시를 사용한다.

[Bad]

``` javascript
var padding = document.getElementById('result').style.padding;
var margin = document.getElementById('result').style.margin;
var position = document.getElementById('result').style.position;
```

[Good]

``` javascript
var style = document.getElementById('result').style;
var padding = style.padding;
var margin = style.margin;
var position = style.position;
```

<br>
<br>


## 스타일 변경은 한 번에 몰아서 처리하라

브라우저가 렌더링 되는 과정을 간단하게 설명하면 아래와 같다.  

```
Step1          HTML과 CSS를 파싱하여 DOM Tree와 Style 문맥을 생성한다.
Step2          DOM Tree와 Style 문맥을 기반으로 엘리먼트의 색상, 면적 등의 정보를 가진 Render Tree를 생성한다.
Step3          Render Tree를 기반으로 각 노드가 화면의 정확한 비치에 표시되도록 배치한다.
Step4          배치된 노드들의 visibility, outline, color 등의 정보를 시각적으로 표현한다.
```

Step3의 과정을 Reflow, Step4의 과정을 Repaint라고 한다.  
자바스크립트에서 DOM을 조작하면 브라우저 내부적으로 Reflow와 Repaint가 반복적으로 수행되는데, 이 두 과정이 서비스 성능 비용의 대부분을 차지한다.  
결과적으로 이 두 과정에서 발생되는 비용을 얼마나 절감할 수 있는냐가 서비스의 성능을 결정한다고 해도 과언이 아니다.  

> Solution:  
> Reflow와 Repaint가 최소한으로 발생하도록 몰아서 처리한다.

[Bad]

``` html
<div id="container">
    <div id="sample" style="position:absolute;background:red; width:150px;height:50px">
        Sample BOX
    </div>
</div>
```

``` javascript
function changeDivStyle() {
    var sampleEl = document.getElementById('sample');
    sampleEl.style.left = '200px';               // reflow 1회, repaint 1회 발생
    sampleEl.style.width = '200px';              // reflow 1회, repaint 1회 발생
    sampleEl.style.backgroundColor = 'blue';     // repaint 1회 발생
    }

// 총 reflow 2회, repaint 3회 발생
changeDivStyle();
```

[Good]

``` html
<div id="container">
    <div id="sample" style="position:absolute;background:red; width:150px;height:50px">
        Sample BOX
    </div>
</div>
```

``` javascript
// cloneNode를 사용하는 방법
function changeDivStyle() {
    var sampleEl = document.getElementById('sample');
    var clone = sampleEl.cloneNode(true);
    clone.style.left = '200px';
    clone.style.width = '200px';
    clone.style.backgroundColor = 'blue';
    document.getElementById('container').replaceChild(clone, sampleEl);    // reflow 1회, repaint 1회 발생
}
changeDivStyle();
```

``` javascript
// cssText를 사용하는 방법
function changeDivStyle() {
    var sampleEl = document.getElementById('sample');
    sampleEl.style.cssText = 'position:absolute;background:blue; width:200px;height:50p;left:200px;';    // reflow 1회, repaint 1회 발생
}
changeDivStyle();
```

[Tip] - 스타일 객체는 캐싱하여 사용하라
```javascript
// cloneNode를 사용하는 방법
function changeDivStyle() {
    var sampleEl = document.getElementById('sample');
    var clone = sampleEl.cloneNode(true);
    var style = clone.style;
    style.left = '200px';
    style.width = '200px';
    style.backgroundColor = 'blue';
    document.getElementById('container').replaceChild(clone, sampleEl);    // reflow 1회, repaint 1회 발생
}
changeDivStyle();
```


> 참고  
> 위의 코드는 이해를 돕기 위한 예시일 뿐, 실제 개발에서는 **스크립트 코드 내에서 CSS를 직접 수정하지 말고, className을 수정**하는 방법을 사용해야 한다.
 
<br>
<br>


## 이벤트는 inline 방식을 사용하지 마라

자바스크립트 개발 초기에 많이 사용되던 방법이다.  
보통 doSomething()은 외부 자바스크립트 파일에 정의하는데, 만약 doSomething의 함수명을 변경하거나 버튼을 클릭했을 때 호출할 함수를 바꾸려면 자바스크립트와 HTML을 모두 변경해야 한다.  
이렇게 inline 방식으로 사용할 경우, HTML로 자바스크립트가 서로 의존 관계를 만들어 작은 변경에도 수정 범위가 커지고 유지보수 및 디버깅이 어려워진다.  
또한 inline 방식은 이벤트 핸들러를 1개밖에 사용할 수 없는 단점도 함께 가지고 있다.

> Solution:  
> inline 자바스크립트 코드는 HTML에 분리해서 사용한다.

[Bad]

``` html
<button onclick="doSomething()" id="action-btn">Click Me</button>
```

[Good]

``` html
<button id="action-btn">Click Me</button>
```

``` javascript
// jQuery를 사용하는 경우
var btn;
function doSomething() {
    ...
}

btn = $('#action-btn').on('click', $.proxy(doSomething, this));
```

``` javascript
// jQuery를 사용하지 않는 경우
var btn;

function doSomething() {
    ...
}

function attachEvent(target, event, handler) {
    if (target.addEventListener) {
        target.addEventlistener(event, handler, false);
    } else if (target.attachEvent) {
        target.attachEvent('on' + event, handler);
    } else {
        target['on' + event] = handler;
    }
}

btn = document.getElementById('action-btn');
attachEvent(btn, 'click', doSomething);
```

<br>
<br>


## eval()은 사용하지 마라

eval은 매개변수로 받은 문자열을 호출자의 지역 scope에서 즉시 실행하는 강력한 함수이다.  
이 기능은 너무 강력해서 자바스크립트에서 가장 오용되고 있는 부분이기도 하다.  
eval에서 정의한 변수나 함수는 코드 파싱 단계에서는 문자열일 뿐, 실제 변수나 함수로 정의되는 단계는 실행 시점이다.  
즉, 프로그램 실행 중 파서가 새로 기동되어야 하는데, 이는 상당한 부하를 만들어 프로그램 실행 속도를 현저히 느리게 한다.  
뿐만 아니라, 사용자 입력 또는 네트워크로 들어온 문자열을 eval로 수행할 경우 서비스 전체에 심각한 보안 문제를 일으킬 수 있다.  

> Solution:  
> eval은 절대 사용하지 말아야 한다.  
> eval을 사용하는 거의 대부분은 사실 eval 없이도 만들 수 있다.

<br>
<br>


## setTimeout, setInterval시 콜백 함수는 문자열로 전달하지 마라

setTimeout, setInterval는 일정 시간 후에 첫 번째 파라메터로 받은 콜백 함수를 실행한다.  
이때 콜백 함수인 첫 번째 파라메터는 문자열로도 전달 가능하지만,  
문자열로 전달할 경우 내부적으로 eval로 처리되어 실행 속도가 느려진다.  

> Solution:  
> setTimeout, setInterval시 콜백 함수는 함수의 참조 또는 함수로 전달하라.

[Bad]

``` javascript
function callback() {
    ...
}
setTimeout('callback()', 1000);
```

[Good]

``` javascript
function callback() {
    ...
}
setTimeout(callback, 1000);
```

또는

``` javascript
setTimeout(function() {
    ...
}, 1000);
```

<br>
<br>


## 함수 생성자 new Function()은 사용하지 마라

많이 사용되는 방법은 아니지만 함수 생성자를 이용해서 함수를 선언할 수 있다.  
하지만 이 경우, 문자열로 전달되는 파라메터가 수행시점에 eval로 처리되어 실행 속도가 느려진다.  

> Solution:  
> 함수 선언 시 함수 선언식 또는 함수 표현식을 사용하라.

[Bad]

``` javascript
var doSomething = new Function('param1', 'param2', 'return param1 + param2;');
```

[Good]

``` javascript
// 함수 선언식
function doSomething(param1, param2) {
    return param1 + param2;
}
```

또는

``` javascript
// 함수 표현식
var doSomething = function(param1, param2) {
    return param1 + param2;
};
```

<br>
<br>


## with()는 사용하지 마라

with문은 특정 객체를 반복적으로 접근할 때 간편함을 제공할 목적으로 만들어졌지만,  
이런 의도와는 다르게 많은 문제점을 낳고 있어 사용하지 않는 것이 좋다.  
다음과 같은 코드에서 value가 있는 구문은

``` javascript
function doSomething(value, obj) {
    ...
    with(obj) {
        value = "which scope is this?";
    }
}
```

아래 코드 중 하나와 같게 된다.

``` javascript
value = "which scope is this?";
obj.value = "which scope is this?";
```

어떤 코드로 실행될 지는 코드만 봐서는 알 수가 없다.  
또한 코드가 실행될 때마다 다르게 실행될 수 있고, 심지어 프로그램이 실행되는 동안에도 달라질 수 있다.  
의도하는 바가 무엇인지 명확히 알 수 없고, 어떻게 실행될 지 예측할 수 없게 된다.  
즉, 프로그램이 원하는 방향으로 제대로 실행될 것이라고 확신할 수 없게 된다.  
이 외에도 with문은 실행될 때마다 추가적인 Scope을 생성하여 추가적인 자원을 소모하고,  
자바스크립트 내부적으로 수행되는 변수 탐색 최적화를 방해하여 실행 속도를 현저히 느리게 한다.  

> Solution:  
> 특정 객체를 반복적으로 접근해야 한다면 새로운 변수에 캐싱하여 사용한다.

[Bad]

``` javascript
with(document.getElementById('myDiv').style) {
    background = "yellow";
    color = "red";
    border = "1px solid black";
}
```

[Good]

``` javascript
var style = document.getElementById('myDiv').style;
style.background = 'yellow';
style.color = 'red';
style.border = '1px solid black';
```

<br>
<br>


## 네이티브 객체는 확장하거나 오버라이드하지 마라

자바스크립트는 동적인 특징이 있어, 언제든 무엇이든 수정할 수 있다.  
다른 언어에서는 소스코드를 수정하지 않는 한 객체와 클래스를 수정할 수 없지만,  
자바스크립트에서는 언제든 어떤 객체든 수정이 가능하므로 객체의 기본 동작이 예기치 않게 바뀔 수 있다.  
이로 인해 네이티브 객체의 기본 동작을 기대한 개발자에게 혼란과 어려움을 줄 수 있으며, 이는 코드 내 예측할 수 없었던 오류를 만들 수 있다.  
이런 동적인 특징은 언어 자체에서 강제할 수 있는 부분이 아니기 때문에, 개발자 스스로가 규칙을 세우고 따라야 하는 매우 중요한 부분이다.  
설령 협업 개발자간 합의된 변경이라 하더라도 브라우저 제조사가 이들 객체를 예고 없이 변경할 수 있고,  
이런 변경이 기존 코드에 어떤 영향을 미칠지 아무도 예측할 수 없기 때문에, 합의된 변경 조차도 그 결과와 책임에서 절대 자유로울 수 없다.  

> Solution:  
> 네이티브 객체는 절대 수정하지 않는다.  
> 만약 필요한 메서드가 있다면 네이티브 객체의 prototype에 작성하는 대신 함수로 만들거나 새로운 객체를 만들고 네이티브 객체와 상호작용하게 한다.

[Bad]

``` javascript
Object.prototype.getKeys = function() {
    var keys = [];
    var key;
    for (key in this) {
        keys.push(key);
    }
    return keys;
};
```

[Good]

``` javascript
function getKeys(obj) {
    var keys = [];
    var key;
    for (key in obj) {
        keys.push(key);
    }
    return keys;
}
```

> 몽키패칭(monkey-patching):  
> 네이티브 객체나 함수를 프로그램 실행 시 다른 객체나 함수로 확장하는 것을 몽키패칭이라 한다.  
> 자바스크립트를 포함한 일부 라이브러리나 프레임워크에서 이런 식의 확장을 사용하고는 있다.  
> 하지만 이는 캡슐화를 망치고 표준이 아닌 기능을 추가해 네이티브 객체를 오염시키므로 사용하지 말아야 한다.  
> 이런 위험에도 불구하고, 신뢰성 있고 매우 중요한 몽키패칭의 특별한 한가지 사용법이 있는데, 바로 폴리필(polyfill)이다.
> 폴리필은 Array.prototype.map과 같이 자바스크립트 엔진에 새롭게 추가된 기능이 없는 경우, 비슷한 동작을 하는 다른 함수로 대체하는 것을 말한다.  
> 폴리필과 같이 자바스크립트 기능의 호환성 유지 목적을 제외하고는 어떤 경우에도 네이티브 객체의 확장은 옳지 않다.

[Example]

``` javascript
if (typeof Array.prototype.map !== 'function') {
    Array.prototype.map = function(f, thisArg) {
        var result = [];
        var i = 0;
        var n = this.length;
        for (; i < n; i += 1) {
            result[i] = f.call(thisArg, this[i], i);
        }
        return result;
    };
}
```

## 단항 연산자를 사용하지 마라

단항 연산자가 쓰인 연산의 결과를 한 눈에 파악하기 어렵다. 연산이 먼저인 지, 값 할당이 먼저인 지 고민해야 한다.  
코드를 한 줄 더 줄이는 것보다 읽기 쉬운 코드를 작성하는 것이 더 낫다.

[Bad]

``` javascript
var i = 0;
var num;

for (; i < 10; i++) {

}

num = ( ++i ) * 10;
```

[Good]

``` javascript
var i = 0;
var num;

for (; i < 10; i += 1) {
    ...
}

i += 1;
num = i * 10;
```

<br>
<br>


## this에 대한 레퍼런스를 저장하지 마라

this는 함수 실행 시점에 결정된다. 어떤 함수 내부에서 또 다른 함수를 호출하면, 그 함수의 this는 상위 함수의 this와 같지 않다. 프로그래밍을 하다보면, 상위함수의 this에 대한 레퍼런스를 전달해야할 때가 있다.  
비슷한 이름의 레퍼런스 변수(that, self, me 등)를 만들고, 내부 함수의 클로저로 사용하면, 상위함수의 this를 내부 함수에 전달할 수 있다. 이는 개발자에게 혼란을 줄 수 있는 방법이다. this를 결정하는 명확한 방식이 있으므로 그 방법을 사용해야 한다.  

> Solution:  
> Function.prototype.bind 함수나 화살표 함수를 사용한다.

[Bad]

``` javascript
function() {
    var self = this;
    return function () {
        console.log(self);
    };
}

function() {
    var that = this;
    return function () {
        console.log(that);
    };
}

function () {
    var _this = this;
    return function () {
        console.log(_this);
    };
}
```

[Good]

``` javascript
function printContext() {
    return function() {
        console.log(this);
    }.bind(this);
}

function printContext() {
    return () => console.log(this);
}
```

# ES6+

## 변수 선언 시 "const", "let" 키워드를 사용하라 `ES6`

"var"는 함수 스코프를 가진다. 코드 실행 전에 해당 변수가 hoist되므로 선언부보다 위에서 변수를 호출하여도 에러가 발생하지 않는다.  
블록 스코프에서 "var"를 사용해도 블록 스코프 밖의 함수 스코프에 변수가 hoist되어 블록 스코프 밖에서 접근이 가능하다.  
이는 Undefined된 변수의 프로퍼티에 접근할 때, 에러가 발생하는 잠재적인 원인이 된다.  
반면에, "const"와 "let"은 개발자가 예상한대로 동작한다.  
이 키워드를 통해 선언한 변수는 블록 스코프를 가진다. TDZ의 혜택을 받아 같은 스코프 내에서 선언 전에 호출하면 에러가 발생한다.  

> Solution:  
> "const"를 사용해 변수를 선언하되, 할당 후 값이 변하면 "let"을 사용한다.

[Bad]

``` javascript
var foo = 'foo';
var bar = 'bar';
...
bar = 'var';
```

[Good]

``` javascript
const foo = 'foo';
let bar = 'bar';
...
bar = 'var';
```

<br>
<br>


## "require", "module.exports"보다는 "import", "export"를 사용하라 `ES6`

자바스크립트에서의 모듈 패턴은 전역에서 특정 변수 영역을 보호하기 위해 사용된다. AMD, CommonJS 등의 모듈러가 보편화됨에 따라, ES6에서는 모듈 패턴을 지원하는 키워드인 import, export를 도입했다. 다만, 이 기능은 지원하는 브라우저의 점유율이 매우 낮기에 ES6 문법을 사용하기 위해서는 트랜스파일러를 사용해야 한다. import, export를 사용할 수 있는 환경이라면, require, module.exports 보다는 import, export를 사용하라. 사용할 수 없다면, "require", "module.exports"를 사용하라.
 
[Bad]

``` javascript
/* Top of file */
const StyleGuide = require('./StyleGuide');
module.exports = StyleGuide.es6;
```

[Good]

``` javascript
/* Top of file */
import StyleGuide from './StyleGuide';
export default StyleGuide.es6;

/* Top of file */
import { es6 } from './StyleGuide';
export default es6;
```

<br>
<br>


## import문은 파일의 맨 위에 선언하라 `ES6`

import문은 hoist 비용을 고려하여 코드의 맨 위로 작성한다.  
[Bad]

``` javascript
import foo from 'foo';
foo.init();

import bar from 'bar';
```

[Good]

``` javascript
/* Top of file */
import foo from 'foo';
import bar from 'bar';

foo.init();
```

<br>
<br>


## 제너레이터를 사용할 때 *의 위치에 주의하라 `ES6`

제너레이터의 *의 위치가 바르지 않으면 트랜스파일링 되지 않는다.  
따라서 가급적 제너레이터를 사용하지 않아야 한다.  
꼭 사용해야 한다면 *의 위치에 주의한다. 반드시 function 바로 뒤에 써야 한다.  

[Bad]

``` javascript
function * foo() {
    ...
}

const bar = function * () {
    ...
};

const baz = function *() {
    ...
};

const quux = function*() {
    ...
};

function*foo() {
    ...
}

function *foo() {
    ...
}

// very bad
function
*
foo() {
    ...
}

// very bad
const wat = function
*
() {
    ...
};
```

[Good]

``` javascript
function* foo() {
    ...
}

const foo = function* () {
    ...
};
```

<br>
<br>

# 작성하면서 참고한 페이지들
* [Airbnb JavaScript Style Guide](https://github.com/airbnb/javascript)
* [Airbnb JavaScript Style Guide - ES5(Deprecated)](https://github.com/airbnb/javascript/tree/es5-deprecated/es5)
* [ESLint Coding Convention](http://eslint.org/docs/developer-guide/code-conventions)
* [Google JavaScript Style Guide](https://google.github.io/styleguide/jsguide.html)


# 출처
* https://github.com/nhnent/fe.javascript/wiki/%EC%95%88%ED%8B%B0-%ED%8C%A8%ED%84%B4
