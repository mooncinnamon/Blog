---
title: "Jenkins 로 Docker Deploy 해보기"
date: "2021-07-19T22:40:32.169Z"
template: "post"
draft: false
slug: "Jenkins-로-Docker-Deploy-해보기"
category: "Toy Project"
tags:
  - CI
  - Jenkins
description: "여러 CI 툴들이 존재하지만 회사에서도 사용하는 Jenkins를 사용해서 집에서 진행하는 토이프로젝트의 자동 배포를 설정해보기로 했다.
물론 Jenkins가 아닌 여러 툴이 존재하지만 설치형 CI Tool중에 마음에 드는 툴이 없어 기존에 제일 익숙한 Jenkins로 선택한 것 이다."
socialImage: "/media/image-0.jpg"
---
# Jenkins 로 Docker Deploy 해보기

### 들어가며

여러 CI 툴들이 존재하지만 회사에서도 사용하는 Jenkins를 사용해서 집에서 진행하는 토이프로젝트의 자동 배포를 설정해보기로 했다.

물론 Jenkins가 아닌 여러 툴이 존재하지만 설치형 CI Tool중에 마음에 드는 툴이 없어 기존에 제일 익숙한 Jenkins로 선택한 것 이다.

## Jenkins 설치

Home 서버로 Nas를 메인으로 사용하고 있기 때문에 설치하는 어플리케이션이 꼬일 시 귀찮은 일이 자주 발생했다. 그렇기 때문에 Docker를 사용해서 어플리케이션을 관리하고 있다 Jenkins역시 Docker를 사용하여 설치하였다

```yaml
version: '3'
services:
  jenkins:
    image: docker-jenkins:1.0
    restart: always
    ports:
        - "8080:8080"
        - "50000:50000"
    volumes:
        - '/volume1/docker/jenkins:/var/jenkins_home'
        - '/var/run/docker.sock:/var/run/docker.sock'
    environment:
        TZ: "Asia/Seoul"
```

docker 명령어로 설치해도 되지만 docker-compose 를 사용하여 관리하는가 깔끔하기 때문에 docker-compose를 사용해서 설치하였다.

## Jenkins 설정

Pipeline을 사용해서 Deploy 설정을 해보기로 한다.

PipeLine은 연속적인 전달 파이프 라인의 통합 및 구현을 지원하는 플러그인의 구현이다. 코드로 확장이 가능하기 때문에 유연하게 배포 플로우를 짤 수 있다는 장점이 있다.

```bash
node{
    stage('pull'){
        git credentialsId: 'github-id-pw', url: 'https://github.com/mooncinnamon/pipelinetest.git'
    }
    stage('Build') { 
        sh(script: 'docker-compose build app') 
    }
    stage('Tag'){
        sh(script: '''docker tag docker.cinna.dev/shutle \
        docker.cinna.dev/shutle:${BUILD_NUMBER}''')
    }
    stage('Push') { 
        sh(script: 'docker push docker.cinna.dev/shutle:${BUILD_NUMBER}') 
        sh(script: 'docker push docker.cinna.dev/shutle:latest') 
    }
    stage('Deploy'){
        sh(script: 'docker-compose up -d production')
    }
}
```

현재 docker registry server도 내부망에서 돌리고 있기 때문에 docker의 주소가 이상하지만 위에서 부터 설명해보자면

1. pull : github에서 pull로 최신코드를 받아온다. 이때 인증서를 사용해서 인증하고 싶었는데 인증서를 사용하면 인증이 되지 않는 이슈가 발생해서 지금은 id pw 기반으로 인증을 받고있다.
2. Build : 테스트용 어플리케이션은 HelloWorld를 출력해주는 docker Flask 어플리케이션이다.  역시 docker container이기 때문에 docker-compose 명령어를 사용하여 build를 해준다.
3. Tag : Tag 작업을 해서 build한 도커 이미지에 Tag를 달아준다.
4. Push : Docker 이미지를 Docker hub에 push 해준다. 나는 로컬 docker hub를 사용하고 있기 때문에 내부망 주소를 가르쳐주었다.
5. Deploy : Docker hub 기준으로 최신 이미지가 올라갔다는걸 알 수 있다면 production 을 배포해준다.

## Sample App

테스트용으로 사용한 Flask 코드는 아주 간단하다.

```python
from flask import Flask

app = Flask(__name__)

host = '0.0.0.0'

@app.route('/')
def hello_world():
    return 'Hello World!'

if __name__ == '__main__':
    app.run(host=host)
```

HelloWorld를 Return 해주는 단순한 코드이다. 

단지 외부망에서 접속을 허용하기 위해 host가 0.0.0.0 이란것 정도가 다르다

이 Flask 코드를 돌리기 위한 Docker Compose는 아래와 같다

```yaml
version: '3'

services:
  app:
    build: .
    image: docker.cinna.dev/shutle
  production:
    image: docker.cinna.dev/shutle:${BUILD_NUMBER}
    command: python app.py
    volumes:
      - /volume1/docker/pipelinetest:/code
    ports:
      - "5551:5000"
```

app과 production이 분리되어 있는걸 볼 수 있는데. 개발과, Build 과정은 app service로 테스트를 진행하고 (Local) 배포를 하는 production 코드는 분리되어있다는 걸 알 수 있다.

## 마치며

어쩌면 제일 기본적인 CI인 Jenkins를 사용해 Docker Application을 배포하는 과정을 보았다. Spring이나 node같은 다른 Docker Application을 사용한다면 Docker Application이 변경될 수 는 있지만 실행되는 Docker Application만 만든다면 Jenkins Pipeline의 설정은 크게 변경되지 않다는게 Jenkins의 장점이 아닐까 생각한다.