---
layout: post
title: Claude code 설치하기
subtitle: MCP에 대해서 알아보자!
categories: AI
tags: [AI]
---

`개인적으로 공부한 내용을 포스팅하기 때문에 잘못된 내용이 있을 수 있습니다. 만약 틀린 내용이 있다면 적극적인 피드백 부탁드립니다^^`


### WSL 설치 방법
1. Windows PowerShell을 '관리자 권한으로 실행'
2. 아래 명령어 실행
- dism.exe /online /enable-feature /featurename:Microsoft-Windows-Subsystem-Linux /all /norestart
- dism.exe /online /enable-feature /featurename:VirtualMachinePlatform /all /norestart

3. WSL --install 명령어 실행
<br>
<img width="859" height="365" alt="Image" src="https://github.com/user-attachments/assets/5616349b-f736-42fb-bc85-346768c3139b" />
<br>

4. ubuntu 설치
link : https://apps.microsoft.com/search?query=ubuntu&hl=ko-KR&gl=KR
<br>

3. Node.js 설치
<br>
<hr>
- 패키지 목록 업데이트
sudo apt update

- Node.js 설치
curl -fsSL https://deb.nodesource.com/setup_lts.x | sudo -E bash -
sudo apt-get install -y nodejs

- 설치 확인
node --version
npm --version
<hr>






<hr>
<br>
ref : https://goddaehee.tistory.com/313