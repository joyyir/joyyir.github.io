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
