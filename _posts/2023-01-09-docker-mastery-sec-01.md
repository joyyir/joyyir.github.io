---
title: "[Docker Mastery] Section 1. Quick Start!"
description: Section 1. Quick Start!
excerpt : Section 1. Quick Start!
date: 2023-01-09 00:00:00 -0400
categories:
  - Cloud
---

Udemy [Docker Mastery: with Kubernetes +Swarm from a Docker Captain](https://www.udemy.com/course/docker-mastery) 강의를 기반으로 정리합니다.

- 핵심 아이디어: Build -> Ship -> Run
- Docker의 세 가지 혁신
  1. Docker image
  2. Docker registry
  3. Docker container
- Docker image
  - Dockerfile & `docker build`
- Docker registry
  - Docker Hub (default), GitHub, Bitbucket, ...
- Docker container
  - 관련 Linux 기술: namespace, cgroups, veth, iptables, union mount
- [Play With Docker](https://labs.play-with-docker.com)
  - Docker client, Docker server
  - `docker run -d -p 8800:80 httpd`
- 왜 Docker를 써야하는가?
  1. Isolation
    - better isolation in a single OS
  2. Environments
    - reduced environment variances
  3. Speed (of business)
    - increase speed of change

## 생각

**왜 Docker를 써야하는가?** 이 부분이 이번 강의의 핵심인 것 같다. 컴퓨팅 자원을 격리시켜서 사용할 수 있다는 점, 다양한 환경에서 일관된 방식으로 사용할 수 있다는 점, 비즈니스 요구에 빠르게 대응할 수 있다는 점이 Docker를 사용하는 이유인 것 같다.
