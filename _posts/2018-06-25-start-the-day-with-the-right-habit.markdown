---
layout: post
title:  농산물 직거래 공동구매 서비스 
date:   2020-07-25 15:01:35 +0300
image:  11.png
tags:   TeamProject
---

[GitHub] <https://github.com/88jina/team_project>

## 불필요한 유통구조를 날려버린 농산물 직거래 공동구매 서비스


+ JAVA8, Spring, MySQL, MyBatis를 이용하여 클라이언트 사이드로 렌더링한 팀 프로젝트
+ Front : vanilla js


### Spring MVC project 생성하기    


프레임워크 없이 Servlet으로 프로젝트할 때는 할 필요 없었던 환경설정이 거의 3일 동안 내 발목을 잡았다.  
프로젝트를 생성하는 것부터가 난관이었다.  
이 때는 Eclipse를 썼기 때문에 Eclipse가 가지고 있는 오류들을 떠안고 MVC 구조로 프로젝트를 만들어야 했다.    
Spring legacy project로 프로젝트를 생성했더니 jdk버전을 아무리 1.8 이상으로 설정해도 jdk버전이 1.5에서 바뀌지 않았다.  
그 다음으로는 Servlet으로 프로젝트 할 때 처럼 Web Dynamic Project로 프로젝트를 생성한 뒤 폴더구조를 mvc로 바꿔주었다.  
이 방법은 jdk 버전 설정 문제는 없었지만 mvc 구조를 잘 인식하지 못하는건지 spring 환경 설정을 어떻게 바꿔줘도 hello world도 띄우지 못했다.
마지막으로 Maven Project에서 webapp archytecture로 프로젝트를 생성했을 때 폴더구조를 따로 바꿔줄 필요 없이 스프링 설정도 먹혔다.


### Java Config로 Spring 환경설정하기

![]({{ site.baseurl }}/images/20.png)
*Servlet Configuration Code*

Spring boot가 아닌 Spring으로 환경설정을 하려고 찾아보니 온통 xml 설정 밖에 없어서 어노테이션을 이용해 spring 환경설정을 하기가 쉽지 않았다.  
개인적으로 프레임워크를 쓰는 이유 중 하나가 코드량을 줄이기 위함이라고 생각하는데, 아무리 봐도 xml 설정은 프레임워크의 효용이 극대화되지 않는다고 느껴졌다.  
Java Config로 환경설정을 찾아도 자바버전에 따라 어노테이션을 쓰는 방법이 조금씩 달라서 이번 프로젝트에서 사용하는 jdk1.8 버전에 맞는 스프링 환경을 설정하는데 어려움이 많았다.  

### 로그인

![]({{ site.baseurl }}/images/12.png)
*로그인 화면*

이전 프로젝트에서 이미 session을 이용한 로그인을 해보았기 때문에 이번에는 jwt를 이용한 로그인 기능을 구현하려고 해봤다.  
그러나 생각지도 못하게 spring 환경설정에서 시간을 너무 많이 쏟았고, 갑자기 거리두기가 2.5단계로 격상되면서 짧은 개발기간 동안 팀원을 만날 수 있는 날이 제한되었다.  
그래서 로그인 구현은 피치못하게 익숙한 세션을 이용했다.  
로그인을 유지하기 위해 처음에는 유저 아이디를 쿠키로 만들어 클라이언트에 보내주고 클라이언트에서 최초에 받은 유저 정보를 유저 아이디를 통해 계속 물고 있는 방법을 사용했었다.
그런 방식은 보안 상 치명적일 수 있다는 조언을 듣고 쿠키값 없이 클라이언트단에서 버튼을 누를 때마다 api 통신을 해서 세션에서 가지고 있는 유저 정보를 json 형태로 계속 넘겨주는 방식으로 바꾸었다.    


![]({{ site.baseurl }}/images/logincontroller.png)
*Login Controller*


### 회원가입

![]({{ site.baseurl }}/images/14.png)
*회원가입 화면*

Java mailSender 라이브러리를 이용하여 이메일 인증을 구현하였다.  
1.사용자가 이메일인증 버튼을 클릭하면 서버에서 난수를 생성하여 해당 메일 주소로 인증키를 보내준다.
2.같은 인증키를 클라이언트에 쿠키로 만들어 넣어준다.(쿠키지속시간 : 3분)
3.클라이언트단에서 쿠키값과 대조하여 인증번호를 확인한다.  

캠프를 수료할 때 이 방법이 일반적이지 않다는 피드백을 들었고, 이메일 인증을 다시 구현한다면 db에 인증키와 메일주소를 담아 놓고 인증확인을 위해 서버통신을 해야한다.

```java
    @RequestMapping(value = "/api/auth/sendMail", method = RequestMethod.GET)
    public AuthBean makeAuthKey(UserBean userBean, HttpServletResponse res) {
        AuthBean authBean = new AuthBean();

        String authKey = Integer.toString(service.authKeyMaker()); //난수로 인증키 생성
        System.out.println("인증키 생성");
        authBean.setAuthKey(authKey); //인증빈에 인증키 삽입
        System.out.println("인증키 삽입");

        String userEmail = userBean.getUserEmail();
        authBean.setUserEmail(userEmail);
        System.out.println(userEmail);
        service.sendMail(userEmail, authKey); //메일전송
        System.out.println("메일 발송");
        authBean.setSendMail(true); //메일전송여부 인증빈에 삽입

        Cookie authCookie = new Cookie("authKey", authKey); //쿠키생성
        authBean.setCookieName(authCookie.getName());
        authCookie.setValue(authKey); //쿠키에 쿠키값 삽입
        System.out.println("AuthKey : " + authKey);
        System.out.println("CookieValue: " + authCookie.getValue());
        authCookie.setMaxAge(60 * 3); //쿠키 지속시간 3분설정
        authCookie.setPath("/"); //모든 곳에서 접근 가능한 쿠키로 설정
        res.addCookie(authCookie); //클라이언트로 쿠키보냄
        System.out.println("쿠키 클라이언트로 보내줌");

        return authBean;
    }
```

