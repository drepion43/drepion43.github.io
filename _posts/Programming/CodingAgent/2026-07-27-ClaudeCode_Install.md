---
title:  "Claude Code 설치 방법(Windows/Linux) 정리"
categories: CodingAgent
tag: [CodingAgent, Claude Code, Anthropic, Windows, Linux, 설치]
toc: true
author_profile: false
sidebar:
    nav: "docs"
use_math: false
excerpt: Windows와 Linux 환경에서 Claude Code CLI를 설치하고 초기 설정하는 방법 정리
comments: true
date: 2026-07-27
toc_sticky: true
---

## Claude Code란

Claude Code는 Anthropic에서 제공하는 터미널 기반 AI 코딩 에이전트입니다. 터미널에서 자연어로 요청을 하면 코드 탐색, 파일 수정, 테스트 실행, git 명령 등을 직접 수행해줍니다. 이번 글에서는 Windows와 Linux 환경에서 Claude Code를 설치하고 기본 설정을 마치는 과정을 정리하겠습니다.

## 사전 요구사항

Claude Code는 Node.js 기반 CLI이기 때문에 설치 전에 Node.js가 준비되어 있어야 합니다.

- Node.js 18 이상
- npm (Node.js 설치 시 함께 설치됨)
- Anthropic 계정 (Claude.ai 계정 또는 Anthropic Console API 키)

Node.js 설치 여부는 아래 명령어로 확인할 수 있습니다.

```bash
node -v
npm -v
```

버전이 출력되지 않는다면 [Node.js 공식 홈페이지](https://nodejs.org/)에서 LTS 버전을 먼저 설치해야 합니다.

## Windows에서 설치

Windows는 PowerShell을 이용해 npm으로 전역 설치하는 방식이 가장 간단합니다.

### 1. PowerShell에서 설치

```powershell
npm install -g @anthropic-ai/claude-code
```

전역(-g) 옵션으로 설치하면 어느 디렉터리에서든 `claude` 명령어를 사용할 수 있습니다.

### 2. 설치 확인

```powershell
claude --version
```

버전이 정상적으로 출력되면 설치가 완료된 것입니다.

### 3. WSL 사용 시 주의사항

Windows에서 WSL(Windows Subsystem for Linux)을 사용 중이라면, Windows용 npm과 WSL 내부 npm이 서로 분리되어 있다는 점에 유의해야 합니다. WSL 터미널에서 작업할 계획이라면 아래 Linux 설치 방법을 따르는 것이 더 안정적입니다. 두 환경에 각각 설치해서 혼용하면 PATH 충돌이나 버전 불일치 문제가 발생할 수 있습니다.

## Linux에서 설치

Linux(Ubuntu 등)에서도 동일하게 npm을 이용해 설치합니다.

### 1. Node.js 설치 (없는 경우)

```bash
curl -fsSL https://deb.nodesource.com/setup_20.x | sudo -E bash -
sudo apt-get install -y nodejs
```

### 2. Claude Code 설치

```bash
npm install -g @anthropic-ai/claude-code
```

권한 문제(EACCES)로 전역 설치가 실패하는 경우, sudo로 강제 설치하기보다는 npm의 전역 설치 경로를 사용자 홈 디렉터리 하위로 변경하는 방법을 권장합니다.

```bash
mkdir -p ~/.npm-global
npm config set prefix '~/.npm-global'
echo 'export PATH=~/.npm-global/bin:$PATH' >> ~/.bashrc
source ~/.bashrc

npm install -g @anthropic-ai/claude-code
```

### 3. 설치 확인

```bash
claude --version
```

## 최초 실행 및 로그인

설치가 끝나면 프로젝트 디렉터리로 이동한 뒤 `claude` 명령어로 실행합니다.

```bash
cd my-project
claude
```

최초 실행 시 브라우저가 열리며 Anthropic 계정 로그인을 요청합니다. Claude.ai 구독 플랜(Pro/Max) 또는 Anthropic Console API 키 중 하나로 인증할 수 있습니다.

- Claude.ai 계정으로 로그인하면 구독 플랜에 포함된 사용량으로 이용
- Console API 키로 로그인하면 API 사용량 기준 과금

로그인이 완료되면 터미널에 프롬프트가 나타나고, 이후부터는 자연어로 작업을 요청할 수 있습니다.

## 기본 사용법

```bash
# 대화형 세션 시작
claude

# 특정 요청을 한 줄로 실행
claude "이 프로젝트의 테스트를 실행해줘"

# 이전 세션 이어가기
claude --continue
```

세션 안에서는 `/help`, `/clear`, `/model` 등의 슬래시 명령어를 사용해 도움말 조회, 대화 초기화, 모델 변경 등을 수행할 수 있습니다.

## 자주 겪는 문제

### command not found: claude

전역 설치 경로가 PATH에 잡혀있지 않을 때 발생합니다. Linux에서는 `npm config get prefix`로 설치 경로를 확인한 뒤 해당 경로의 `bin` 디렉터리가 PATH에 포함되어 있는지 확인해야 합니다. Windows는 npm 전역 설치 시 PATH가 자동으로 등록되므로, 등록되지 않았다면 터미널을 재시작해보는 것이 우선입니다.

### 권한 오류(EACCES)로 설치 실패

앞서 설명한 대로 sudo를 사용하기보다 npm 전역 설치 경로를 사용자 디렉터리로 바꾸는 방식이 더 안전합니다. sudo로 설치하면 이후 업데이트 시에도 계속 권한 문제가 반복될 수 있습니다.

### 버전 업데이트

```bash
npm update -g @anthropic-ai/claude-code
```

## 정리

Claude Code는 Windows, Linux 모두 npm 전역 설치 한 줄로 설치를 마칠 수 있습니다. Windows에서 WSL을 함께 쓰는 경우 PATH 충돌에 주의해야 하고, Linux에서는 권한 문제를 피하기 위해 npm 전역 경로를 사용자 홈 디렉터리로 옮기는 방식을 권장합니다. 설치 이후에는 프로젝트 디렉터리에서 `claude` 명령어로 바로 대화형 세션을 시작할 수 있습니다.
