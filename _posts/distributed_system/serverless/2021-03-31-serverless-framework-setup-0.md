---
layout: post
title:  "Serverless framework를 활용한 서비스 개발 (0)"
categories: [Distributed System, Serverless, Cloud]
shortinfo: "방법론적인 Serverless 말고. Serverless framework를 통해 Cloud provider에 묶이지 않고 App 을 배포해보자."
tags: [Distributed System, Serverless, Framework, Cloud]
comments: true
---

# 설명

공식 페이지의 설명은 다음과 같다.   
Develop, deploy, troubleshoot and secure your serverless applications   
with radically less overhead and cost by using the Serverless Framework.   
대략적으로, **Serverless Architecture** 를 개발, 배포, 운영하는걸 편하게 해주는   
일종의 추상화된 Framework라고 생각된다.

> Serverless는 serverless 환경을 배포하고 운영하기 위한 도구다.

# 중요 개념

## Functions

AWS Lambda, Azure Function 처럼, 각 Cloud provider에서 제공하는 Serverless function을 의미한다.

## Events

위의 Functions을 실행하는 Trigger 역할을 한다.   
Serverless Framework에서 이벤트 정의 시, AWS 기준으로 AWS에 이벤트를 자동 생성 및 Lambda와 연결한다.
AWS 기준으로는 다음이 해당한다.

> API Gateway, HTTP API, Websocket
> S3, DynamoDB, SQS
> Alexa, IoT
> Cloudwatch, Cloudfront, Cognito

이 외에도 각 Provider 마다 Events 역할을 하는 서비스는 다양하다.

## Resources

Lambda가 사용하는 AWS의 Infra 영역이다.   
AWS 기준, CloudFormation에서 정의 가능한 영역 모두가 해당한다.

> All Resources are other AWS infrastructure resources which the AWS Lambda functions in your Service depend on, like AWS DynamoDB or AWS S3.

## Services

Function의 모음.   
Functions = Resources + Events   
Services = Functions + Functions + ...

> A service is like a project. It's where you define your AWS Lambda Functions, the events that trigger them and any AWS infrastructure resources they require, all in a file called serverless.yml.


## Plugins

Serverless의 기능 확장을 할 때 사용한다.

> A Plugin is custom Javascript code that creates new or extends existing commands within the Serverless Framework.

# Installation

```bash
# Using npm
npm i -g serverless

# Using yarn
yarn add -g serverless
```

설치 이후의 App 배포 및 설정 변경은 추후 기재 예정