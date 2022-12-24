---
layout: post
title:  "Load balancer (1)"
categories: [Network, Server, Load balancer]
shortinfo: "네트워크에서 자주 거론되는 로드 밸런서에 대해 알아본다."
tags: [Network, Server, Load balancer]
comments: true
---

### 로드 밸런싱?

부하분산 또는 로드 밸런싱(load balancing)은 컴퓨터 네트워크 기술의 일종   
둘 혹은 셋 이상의 중앙처리장치 혹은 저장장치와 같은 컴퓨터 자원들에게 작업을 나누는 것을 의미

### 로드밸런서 종류

운영체제 로드밸런서
- 물리적 프로세서 간에 작업을 스케줄링

`네트워크 로드밸런서`
- 사용 가능한 백엔드에서 네트워크 작업을 스케줄링

### 네트워크 로드밸런서 종류

L2(Data Link Layer)
- Mac Address Load Balancing
- 예시 : Mac > 80–00–20–30–1C-47
- 브릿지, 허브 등
- 장점 : 구조가 간단, 신뢰성이 높다, 가격저렴, 성능이 좋다.
- 단점 : Broadcast 패킷에 의해 성능저하 발생, 라우팅 등 상위레이어 프로토콜 기반 스위칭 불가

L3(Network Layer)
- IP Address Load Balancing
- 예시 : IP > 213.12.32.123
- L2 + Routing
- Router, ICMP 프로토콜, IP
- 장점: Broadcast 트래픽으로 전체 성능 저하 방지, 트레픽체크
- 단점: 특정 프로토콜을 이용해야 스위칭 가능

L4(Transport Layer)
- Transport Layer(IP+Port) Load Balancing
- 예시: IP+Port > 213.12.32.123:80, 213.12.32.123:1024
- TCP, UDP Protocol
- 장점 : Port기반 스위칭 지원, VIP를 이용하여 여러대를 한대로 묶어 부하분산
- 주로 Round Robin 방식 사용

L7(Application Layer)
- Application Layer(사용자 Request) Load Balancing
- 예시 : IP+Port+패킷 내용 >
213.12.32.123:80, 213.12.32.123:1024 + GET/ img/aaa.jpg
- HTTP, FTP, SMTP Protocol

### 참조 링크

[로드밸런서란](https://medium.com/@pakss328/%EB%A1%9C%EB%93%9C%EB%B0%B8%EB%9F%B0%EC%84%9C%EB%9E%80-l4-l7-501fd904cf05)