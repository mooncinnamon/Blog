---
title: OpenJDK 11 Jenkins awt Headless error
date: '2019-11-15T00:00:00.000Z'
layout: post
draft: false
path: '/posts/issue/1'
category: 'Issue'
tags:
  - 'Jenkins'
description: 'OpenJDK 11 Jenkins awt Headless error'
---

# OpenJDK 11 Jenkins awt Headless error

## issue

OpenJDK 11을 사용하여 Ubuntu 에 Jenkins를 설치했지만 java.lang.Error: Probable fatal error:No fonts found. 가 떠서 Jenkins의 사용이 불가능했다.

## solution.

해당 이슈에 대해서는 jeknins wiki에 잘 적혀 있으니 먼저 따라하자

[jenkins wiki - Jenkins got java.awt.headless problem](https://wiki.jenkins.io/display/JENKINS/Jenkins+got+java.awt.headless+problem)

위의 issue로도 해결이 안된다면 openjdk-11-jre-headless를 설치해보자.

apt install openjdk-11-jre-headless

OpenJdk로 넘어오고 나서 더 불편해진거 같기도 하고... 망할놈의 오라클
