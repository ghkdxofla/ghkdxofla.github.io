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

# Create service from template

```bash
# ./project 하위에 Node.js template로부터 Service 생성
serverless create --template aws-nodejs --path project
```

# 프로젝트 구조

```bash
project
  |- serverless.yml
  |- handler.js
```

## serverless.yml

Service는 하나의 프로젝트 단위라 생각하면 된다.   
이를 구성하기 위한 파일이 serverless.yml이다.   
본 파일에 모든 Function, Resource를 기재하면 된다.   
이 yml파일을 쪼개서 Service 단위로 분할이 가능하다.   

```bash
users
  |- serverless.yml
posts
  |- serverless.yml
comments
  |- serverless.yml
```

본 파일에서 관리되는 설정은 다음과 같다.   

- Service 선언
- Service에 들어가는 Function 정의
- Provider 정의
  - AWS, GCP, Azure...
- Plugin 정의
- Event 정의
- Resource 정의
- Event에서 사용하는 Resource 자동 생성
- Variable을 활용한 설정 정의

파일의 기본 양식은 다음과 같다.   

```yml
# serverless.yml

service: users

provider:
  name: aws
  runtime: nodejs12.x
  stage: dev # Set the default stage used. Default is dev
  region: us-east-1 # Overwrite the default region used. Default is us-east-1
  stackName: my-custom-stack-name-${self:provider.stage} # Overwrite default CloudFormation stack name. Default is ${self:service}-${self:provider.stage}
  apiName: my-custom-api-gateway-name-${self:provider.stage} # Overwrite default API Gateway name. Default is ${self:provider.stage}-${self:service}
  profile: production # The default profile to use with this service
  memorySize: 512 # Overwrite the default memory size. Default is 1024
  deploymentBucket:
    name: com.serverless.${self:provider.region}.deploys # Overwrite the default deployment bucket
    serverSideEncryption: AES256 # when using server-side encryption
    tags: # Tags that will be added to each of the deployment resources
      key1: value1
      key2: value2
  deploymentPrefix: serverless # Overwrite the default S3 prefix under which deployed artifacts should be stored. Default is serverless
  versionFunctions: false # Optional function versioning
  stackTags: # Optional CF stack tags
    key: value
  stackPolicy: # Optional CF stack policy. The example below allows updates to all resources except deleting/replacing EC2 instances (use with caution!)
    - Effect: Allow
      Principal: "*"
      Action: "Update:*"
      Resource: "*"
    - Effect: Deny
      Principal: "*"
      Action:
        - Update:Replace
        - Update:Delete
      Resource: "*"
      Condition:
        StringEquals:
          ResourceType:
            - AWS::EC2::Instance

functions:
  usersCreate: # A Function
    handler: users.create
    events: # The Events that trigger this Function
      - http: post users/create
  usersDelete: # A Function
    handler: users.delete
    events: # The Events that trigger this Function
      - http: delete users/delete

# The "Resources" your "Functions" use.  Raw AWS CloudFormation goes in here.
resources:
  Resources:
    usersTable:
      Type: AWS::DynamoDB::Table
      Properties:
        TableName: usersTable
        AttributeDefinitions:
          - AttributeName: email
            AttributeType: S
        KeySchema:
          - AttributeName: email
            KeyType: HASH
        ProvisionedThroughput:
          ReadCapacityUnits: 1
          WriteCapacityUnits: 1
```

## handler.js

여러 Function에 대한 코드가 구현되어 있는 부분이다.   

Function 부분 부터는 다음 포스트에서 진행.

# 참조

[Serverless 프레임워크 빠르게 배우기 (1)](https://changhoi.github.io/posts/serverless/serverless-framework-quicklearn-(1)/)   
