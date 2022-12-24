---
layout: post
title:  "Serverless framework를 활용한 서비스 개발 (1)"
categories: [Distributed System, Serverless, Cloud]
shortinfo: "방법론적인 Serverless 말고. Serverless framework를 통해 Cloud provider에 묶이지 않고 App 을 배포해보자."
tags: [Distributed System, Serverless, Framework, Cloud]
comments: true
---

# Function?

`serverless.yml` 파일에는 functions 속성이 존재한다. 현재 개발 중인 프로젝트에서 일부 발췌하면 다음과 같다.   
참고로, 아래의 설정값은 `function.yml`에 기입되어 있고, 해당 파일을 serverless.yml에서 참조하는 형태로 구성하였다.

```yaml
google-calendar:
  handler: src/handlers/router.common # AWS Lambda에 세팅되는 handler
  name: ${self:custom.env.region}-${self:custom.env.stage}-agent-google-calendar # Lambda의 이름
  description: 'google calendar agent'
  # runtime: python2.7 # Default는 Provider에서 제공하는 runtime으로 설정됨
  memorySize: 512
  # timeout: 10 Default는 6초
  events:
    - httpApi:
        method: get
        path: /google-calendar/status
    - httpApi:
        method: post
        path: /google-calendar/rpc
    - httpApi:
        method: post
        path: /google-calendar/poll
    - httpApi:
        method: post
        path: /google-calendar/hook
    - httpApi:
        method: delete
        path: /google-calendar/hook
    - httpApi:
        method: '*'
        path: /google-calendar/hook/{callback_id}
    - httpApi:
        method: get
        path: /google-calendar/oauth
    - httpApi:
        method: get
        path: /google-calendar/oauth/callback
    - httpApi:
        method: get
        path: /google-calendar/oauth/token
    - httpApi:
        method: get
        path: /google-calendar/oauth/token/refresh
```

위의 설정 외에도 많이 존재하는데, 문서를 참고하자(귀찮다).   
전역 설정은 provider 속성에서 상속받는다.   

```yaml
provider:
  name: aws
  runtime: nodejs12.x
  stage: ${self:custom.env.stage}
  region: ${self:custom.env.awsRegion.${self:custom.env.region}}
  stackName: ${self:custom.env.region}-${self:custom.env.stage}-agent
  apiName: ${self:custom.env.region}-${self:custom.env.stage}-agent
  profile: ${opt:profile, 'default'}
  httpApi:
    payload: '2.0'
  environment: # 환경 변수 설정
    AWS_ACCOUNT_ID: "#{AWS::AccountId}"
    STAGE: ${self:custom.env.stage}
    REGION: ${self:custom.env.awsRegion.${self:custom.env.region}}
    GENERIC_DOMAIN: ${self:custom.env.region}-${self:custom.env.domain.generic.${self:custom.env.stage}}
    WORKFLOW_DOMAIN: ${self:custom.env.region}-${self:custom.env.domain.workflow.${self:custom.env.stage}}
    AGENT_DOMAIN: ${self:custom.env.region}-${self:custom.env.domain.agent.${self:custom.env.stage}}
    PORTAL_DOMAIN: ${self:custom.env.region}-${self:custom.env.domain.portal.${self:custom.env.stage}}
    DELETE_POLICY: ${self:custom.env.deletePolicy.${self:custom.env.stage}}
  iamRoleStatements: # Lambda function의 권한 설정
    - Effect: Allow
      Action:
        - lambda:InvokeFunction
      Resource: "*"
```

이런식으로 기본 설정에 대해 정의할 수 있다.   

## VPC 설정

현 프로젝트에서는 VPC 설정을 따로하지는 않았으나, [참조한 블로그](https://changhoi.github.io/posts/serverless/serverless-framework-quicklearn-(2)/)에서 VPC 설정에 대한 내용을 빌려 설명한다.   

- function 단위

```yaml
service: service-name
provider: aws

functions:
  hello:
    handler: handler.hello
    vpc:
      securityGroupIds:
        - securityGroupId1
        - securityGroupId2
      subnetIds:
        - subnetId1
        - subnetId2
```

- provider 단위

```yaml
service: service-name
provider:
  name: aws
  vpc:
    securityGroupIds:
      - securityGroupId1
      - securityGroupId2
    subnetIds:
      - subnetId1
      - subnetId2

functions:
  hello: # 이 함수는 상위 VPC 설정을 덮어 쓸 것임.
    handler: handler.hello
    vpc:
      securityGroupIds:
        - securityGroupId1
        - securityGroupId2
      subnetIds:
        - subnetId1
        - subnetId2
  users: # 이 함수는 그냥 위의 VPC 설정을 그대로 사용할 것임
    handler: handler.users
```

# Provider 설정

## AWS

여러 방법으로 AWS credential을 등록할 수 있으나, 가장 간단한 방법으로 기재한다.   
[나머지는 링크 참조](https://www.serverless.com/framework/docs/providers/aws/guide/credentials/)   

```bash
serverless config credentials --provider aws --key AKIAIOSFODNN7EXAMPLE --secret wJalrXUtnFEMI/K7MDENG/bPxRfiCYEXAMPLEKEY
```

# Deploy

기본 세팅이 완료됐을 경우, 다음의 명령어로 간편하게 배포가 가능하다.   

```bash
serverless deploy
```

function 단위 별로 개별 배포가 가능한데, 다음의 library를 추가 후 사용 가능하다.   

```bash
yaml add -d serverless-plugin-split-stacks
```

개별 배포 커멘드는 다음과 같다.   

```bash
serverless deploy function -f {function_name}
```

# Test

## Install test framework

Jest를 test 시 사용할 framework로 선정하였다.   
Test 관련 글 참조 시, 다음의 이유로 Jest를 추천하였다.   

> there's zero configuration needed to get started
> it includes a good test runner
> has built-in functionality for mocks, stubs, and spies
> has built-in code coverage reporting

설치는 다음의 Command로 진행한다.   

```bash
yarn add --dev jest
```

## Local test

Mocked event 파일을 만들어 진행한다.   

```bash
serverless invoke local -f testFunc -p testFunc.json
```

# 참조

[Serverless 프레임워크 빠르게 배우기 (2)](https://changhoi.github.io/posts/serverless/serverless-framework-quicklearn-(2)/)