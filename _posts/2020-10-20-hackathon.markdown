---
layout: post
title:  친환경 패션기업 소개 서비스 
date:   2020-10-25 15:01:35 +0300
image:  zero.JPG
tags:   TeamProject
---

[GitHub]  
<https://github.com/88jina/WTM_hackathon>  
<https://github.com/88jina/hackathon>

## 2020 WTM HACKATHON
- Spring boot 와 React 프레임워크를 사용한 프로젝트
- 프론트엔드 2명 백엔드 1명으로 팀 구성
- 백엔드 전반을 담당
- 모든 엔지니어들이 이전에 해보지 않았던 걸 도전해보자는 취지로 한 프로젝트라 동적 기능 없이 최소한의 정적 기능만 구현

#### react의 node.js 서버와 spring의 tomcat 서버 동시 구동 문제
- 이전 프로젝트에서는 vanilla js로 프론트를 구성해서 프론트 개별 서버가 돌지 않았는데 리액트는 자체 노드 서버가 돌아가는 문제가 있었음
- 프록시 설정을 통해 톰캣 서버가 구동될때 노드 서버가 톰캣 포트로 돌아가도록 함
