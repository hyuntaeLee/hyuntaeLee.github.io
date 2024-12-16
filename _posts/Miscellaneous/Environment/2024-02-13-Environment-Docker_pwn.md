---
layout: post
title: Docker Pwnable setting
subtitle: Lee_pwn
categories: Environment
tags: [Docker, vscode]
---

CTF에 나갈때 Pwnable 문제를 풀때면 불편한게 있었다.

문제를 받고 폴더 옮기고, vi 키고 끄고 하는 과정이 너무 풀편함을 느꼈다.<br>
그래서 Vscode + Docker만 사용해서 편한 환경을 만들어봤다. +(구글링을 참고해서 만들었다)

## Docker Desktop
우선 시작에 앞서 [**Docker Desktop**](https://www.docker.com/products/docker-desktop/)사이트에서 **Docker**를 다운로드해 준다.

## VScode

그리고 이제 [VScode](https://code.visualstudio.com/)를 다운로드해 준다.

## Visual Code Extension

**Visual Code** 를 설치하고 아래와 같이 확장 프로그램을 설치해주시면 됩니다.

<p align="center">
<img src ="https://user-images.githubusercontent.com/78135526/205292743-1c0bf652-4583-4c4f-90b2-27f033960618.png" width = 400>
</p>

이후 현재 디렉토리에 Dockerfile을 생성하고 사용할 버전을 맞춰 만들면 됩니다.

## Dockerfile
그리고 해당 [Github](https://github.com/hyuntaeLee/lee_pwn)에서 git clone을 하든 zip파일로 다운을 받든 알아서 다운로드를 받아준다.


## Vscode & Docker 연동

First. **터미널을 열어서 위에서 다운받은 폴더로 들어간다. 폴더를 확인하면 알겠지만, 버전 별로 폴더를 만들었는데 원하는 버전이 있는 폴더로 이동한다.**

```
docker build --tag {imagename}:{tagnum} .
```

* **.** 은 현재 위치에 있는 Dockerfile을 build한다고 생각하면 된다. imagename:tag는 말그대로 해당 Dockerfile을 구분하기 위함이기에 마음대로 적어도 된다.

Second. **위 코드로 만들어진 image를 실행하면 된다.**

```
docker run -it --rm --cap-add SYS_PTRACE --security-opt seccomp:unconfined --name 22.04 -v ${PWD}/study:/study {imagename}:{tagnum} /usr/bin/zsh
```
* 이렇게 실행함으로 공유폴더가 생성된다.
  * 공유폴더를 만드는 이유는 다운받은 문제를 편하게 갖고오기 위해서 이다.

<p align="center">
<img src ="/assets/images/data/set.png" width = 700>
</p>

그럼 이제 사진과 같이 VScode를 키고 터미널에 위에 명령어를 적으면 위에서 빌드한 이미지가 실행된다.

study라는 공유폴더를 만든 이유는 공유폴더가 없으면 다운 받은 pwnable 문제를 또 따로 어떻게 어떻게해서 실행되고 있는 도커 환경을 옮겨야하는데, 공유 폴더가 있으면 문제를 공유 폴더로 그냥 이동만 시키면 Docker환경에서 cd 명령어로 편하게 이동할수 있다.
