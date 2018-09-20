---
layout: post
title: 쿠키(Cookie)와 세션(Session)
tags: [stateless, cookie, 쿠키, session, 세션]
---
> HTTP 프르토콜에서 상태를 유지하기 위한 기술인 쿠키와 세션의 개념과 차이점을 알 수 있다.
<!--more-->

### HTTP 프로토콜의 특징
* Stateless
    * HTTP 프로토콜은 기본적으로 무상태 프로토콜이다.
    * 모든 요청 간 의존관계가 없다.
    * 즉, 현재 접속한 사용자가 이전에 접속했던 사용자와 같은 사용자인지 아닌지 알 수 있는 방법이 없다.
    * 이전 요청과 현재 요청이 같은 사용자의 요청인지 알기 위해서는 상태를 유지해야 한다.
    * HTTP 프로토콜에서 상태를 유지하기 위한 기술로 쿠키와 세션이 있다.

### 쿠키(Cookie) 란?
* 개념
    * 클라이언트 로컬에 저장되는 키와 값이 들어있는 파일이다.
    * 이름, 값, 유호시간, 경로 등을 포함하고 있다.
    * 클라이언트의 상태 정보를 브라우저에 저장하여 참조한다.
* 구성 요소
    1. 쿠키의 이름(name)
    2. 쿠키의 값(value)
    3. 쿠키의 만료시간(Expires)
    4. 쿠키를 전송할 도메인 이름(Domain)
    5. 쿠키를 전송할 패스(Path)
    6. 보안 연결 여부(Secure)
    7. HttpOnly 여부(HttpOnly)
* 동작 방식
    1. 웹브라우저가 서버에 요청
    2. 서버에서 상태를 유지하고 싶은 값을 쿠키(cookie)로 생성
    3. 서버 응답 HTTP 헤더에 쿠키를 포함해서 전송
        ```java
        Set−Cookie: id=doy
        ```
    4. 전달받은 쿠키는 웹브라우저에서 관리하고 있다가, 다음 요청 때 쿠키를 함께 전송
        ```java
        cookie: id=doy
        ```
    5. 서버에서는 쿠키 정보를 읽어 이전 상태 정보를 확인
* 쿠키 사용 예
    * 아이디, 비밀번호 저장
    * 쇼핑몰 장바구니

### 세션(Session) 이란?
* 개념
    * 일정 시간동안 같은 브라우저로부터 들어오는 요청을 하나의 상태로 보고 그 상태를 유지하는 기술이다.
    * 서버에서 클라이언트의 상태를 유지하는 수단이다.
* 동작 방식
    1. 웹브라우저가 서버에 요청
    2. 서버가 해당 웹브라우저(클라이언트)에 유일한 ID(Session ID)를 부여함
    3. 해당 ID는 응답 HTTP 헤더에 쿠키를 포함해서 전송  
    쿠키에 이 값을 JSESSIONID 로 저장
        ```java
        Set−Cookie: JSESSIONID=pi0fo9v2kdi5nuha3bcgiu8fq2
        ```
    4. 웹브라우저는 이후 웹브라우저를 닫기 까지 해당 서버 요청할 때 부여된 세션 ID를 HTTP 헤더에 넣어서 전송
        ```java
        Cookie: JSESSIONID=pi0fo9v2kdi5nuha3bcgiu8fq2
        ```
    5. 서버는 세션 ID를 확인하고, 해당 세션에 관련된 정보를 확인한 후 응답

> 세션도 쿠키를 사용하여 값을 주고받으며 클라이언트의 상태 정보를 유지한다.  
> 즉, 상태 정보를 유지하는 수단은 **쿠키** 뿐 이다.

* 세션 사용 예
    * 로그인

---
### Reference
- [http://jeong-pro.tistory.com/80](http://jeong-pro.tistory.com/80)
- [http://victorydntmd.tistory.com/34](http://victorydntmd.tistory.com/34)
- [http://www.fun-coding.org/crawl_advance1.html#6.1.-%EC%BF%A0%ED%82%A4(cookie):-%EC%83%81%ED%83%9C-%EC%A0%95%EB%B3%B4%EB%A5%BC-%ED%81%B4%EB%9D%BC%EC%9D%B4%EC%96%B8%ED%8A%B8%EC%97%90-%EC%A0%80%EC%9E%A5%ED%95%98%EB%8A%94-%EB%B0%A9%EC%8B%9D](http://www.fun-coding.org/crawl_advance1.html#6.1.-%EC%BF%A0%ED%82%A4(cookie):-%EC%83%81%ED%83%9C-%EC%A0%95%EB%B3%B4%EB%A5%BC-%ED%81%B4%EB%9D%BC%EC%9D%B4%EC%96%B8%ED%8A%B8%EC%97%90-%EC%A0%80%EC%9E%A5%ED%95%98%EB%8A%94-%EB%B0%A9%EC%8B%9D)