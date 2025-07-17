---
layout: post
title: Claude code 설치하기
subtitle: MCP에 대해서 알아보자!
categories: AI
tags: [AI]
---

`개인적으로 공부한 내용을 포스팅하기 때문에 잘못된 내용이 있을 수 있습니다. 만약 틀린 내용이 있다면 적극적인 피드백 부탁드립니다^^`


Anthropic이 만든 Claude 서비스에서 **Claude Code**라는 서비스를 출시했다.
Claude-code의 장점에 대해서는 찾아보면 굉장히 많이? 나오기 때문에 나중에 정리할 예정이다.

우선은 내가 주로 사용하는 개발IDE는 vscode인데, vscode 안에서 Claude-code를 사용할 수 있다고 하여 한번 개발 환경을 구축해보려고 한다.

Claude-code를 windows에서 사용하려면 wsl과 ubuntu 설치가 되어 있어야 한다.

본격적으로 claude-code를 사용하기 전에 필수요소들을 설치해두자!

---
### WSL 설치 방법
---
1. Windows PowerShell을 '관리자 권한으로 실행'
---
2. 아래 명령어 실행
```bash
$ dism.exe /online /enable-feature /featurename:Microsoft-Windows-Subsystem-Linux /all /norestart
$ dism.exe /online /enable-feature /featurename:VirtualMachinePlatform /all /norestart
```
---
3. WSL --install 명령어 실행
<img width="859" height="365" alt="Image" src="https://github.com/user-attachments/assets/5616349b-f736-42fb-bc85-346768c3139b" />
---
4. 아래 명령어 실행
```bash
$ wsl.exe --install
$ wsl.exe --update
$ wsl --set-default-version 2
```
---
5. 에러 발생 시
만약 위 명령어를 실행했을 때, "오류 코드: Wsl/WSL_E_WSL_OPTIONAL_COMPONENT_REQUIRED" 문구가 보인다면 필수 구성요소가 빠져 있다는 뜻이기 때문에 아래의 명령어를 실행하자!
```bash
$ dism.exe /online /enable-feature /featurename:Microsoft-Hyper-V-All /all /norestart (Hyper-V 설치)
$ shutdown /r /t 0 (재부팅)
$ wsl --install (wsl 재설치)
```
---
6. Node.js 설치

```bash
$ # 패키지 목록 업데이트
$ sudo apt update
$ # Node.js 설치
$ curl -fsSL https://deb.nodesource.com/setup_lts.x | sudo -E bash -
$ sudo apt-get install -y nodejs
$ # 설치 확인
$ node --version
$ npm --version
```
---
7. Node.js 설치 시 에러
그리고 또 "Error: Access Denied" 에러가 뜬다면, npm이 글로벌 디렉토리(/usr/lib/node_modules)에 접근하려다 권한이 없어서 실패했을 때 발생한다.

다시 말해, 현재 나에게 /usr/lib/node_modules/에 디렉터리를 만들 권한이 없다는 뜻이다.

> -g : '글로벌 서치'
> 
> sudo : '루트 권한 부여'

#### 방법 1.
```bash
$ sudo npm install -g @anthropic-ai/cli
```

#### 방법 2.
```bash
$ mkdir ~/.npm-global
$ npm config set prefix ~/.npm-global

$ echo 'export PATH=$PATH:~/.npm-global/bin' >> ~/.bashrc
$ source ~/.bashrc

$ npm install -g @anthropic-ai/cli
```

---
#### ubuntu 설치
link : https://apps.microsoft.com/search?query=ubuntu&hl=ko-KR&gl=KR

---
#### Claude code 초기 설정
```bash
$ claude
$ claude --project-path /path/to/your/project
```





<hr>
<br>
ref : https://goddaehee.tistory.com/313