---
layout: post
title:  주차장 정산 시스템 
date:   2020-06-25 15:01:35 +0300
image:  16.jpg
tags:   PersonalProject
---

[GitHub] <https://github.com/88jina/mini_project>

![]({{ site.baseurl }}/images/lastparking.pdf)
## 주차장 정산 시스템 

* Java11, JSP, Servlet, JDBC, MySQL을 이용하여 서버사이드 렌더링한 개인 프로젝트
* 개인 풀스택  

![]({{ site.baseurl }}/images/parkingUml.jpg)
### 첫 MVC 패턴
- 처음으로 DAO,DTO,Controller,View를 나누어 코드 구성  

### MySQL JDBC를 통한 DB연결
- DAO를 만들 때 마다 DB연결을 위한 반복적인 코드를 작성하는 것이 매우 비효율적이라고 느껴짐    
- CRUD를 위한 쿼리를 일일히 작성해주어야 함  
- 끝없는 예외처리  

### 풀스택개발
- 모든 페이지를 하나씩 만들어주어야 하는 문제  
- 위의 문제 때문에 일관성있는 css적용을 위해 더 힘써야 했음  

### 관리자 로그인
- session을 이용한 로그인  
- 페이지마다 세션값을 확인하고 페이지를 redirect 해주어야 해서 시간 지연이 있음  