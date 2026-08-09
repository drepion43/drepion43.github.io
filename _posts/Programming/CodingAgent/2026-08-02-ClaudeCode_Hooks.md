---
title:  "Claude Code Hooks(훅) 기능 정리"
categories: CodingAgent
tag: [CodingAgent, Claude Code, Hooks, Anthropic]
toc: true
author_profile: false
sidebar:
    nav: "docs"
use_math: false
excerpt: Claude Code의 Hooks(훅) 기능이 무엇이고, 이벤트/훅 타입/종료 코드는 어떻게 동작하는지, 실전 예제와 함께 정리
comments: true
date: 2026-08-02
toc_sticky: true
---

## 들어가며

앞서 <a href="https://drepion43.github.io/codingagent/ClaudeCode_기능/" target="_blank">기본/커스텀 커맨드</a>, <a href="https://drepion43.github.io/codingagent/ClaudeMd_파일기능/" target="_blank">CLAUDE.md</a>, <a href="https://drepion43.github.io/codingagent/ClaudeCode_Skills/" target="_blank">Skills</a>, <a href="https://drepion43.github.io/codingagent/ClaudeCode_슬래시명령어/" target="_blank">슬래시 명령어</a>를 다뤘습니다. 이번 글에서는 이 시리즈에서 다룬 것들과 성격이 완전히 다른 **Hooks(훅)** 기능을 정리하겠습니다. 지금까지 다룬 기능들이 모두 "모델이 알아서 판단해서 쓰는 것"이었다면, 훅은 모델의 판단과 무관하게 **무조건 실행되는** 장치입니다.

## 훅이란

Git에 `pre-commit` 훅이 있다면, Claude Code에는 세션의 특정 시점마다 자동으로 끼어드는 사용자 정의 핸들러가 있습니다. 이것이 훅입니다.

여기서 핵심은 **결정론적(deterministic)** 이라는 점입니다. `CLAUDE.md`나 프롬프트로 "이런 파일은 수정하지 마세요"라고 아무리 명시해도, 그건 어디까지나 모델에게 보내는 지시일 뿐이라 상황에 따라 무시될 수 있습니다. 반면 훅으로 특정 파일 수정을 차단하면, 모델의 의지와 무관하게 **물리적으로 그 동작이 일어나지 않습니다**. CLAUDE.md가 "이렇게 해줬으면 좋겠다"는 요청이라면, 훅은 "이건 절대 이렇게는 안 된다"는 강제 규칙에 가깝습니다.

훅은 프로젝트/개인 설정 파일(`settings.json`)에 등록합니다.

```json
// ~/.claude/settings.json 또는 .claude/settings.json
{
  "hooks": {
    "PreToolUse": [
      {
        "matcher": "Bash",
        "hooks": [
          {
            "type": "command",
            "command": "bash -c 'CMD=$(cat | jq -r \".tool_input.command // empty\"); if echo \"$CMD\" | grep -q \"rm -rf /\"; then echo \"rm -rf / 는 차단됩니다\" >&2; exit 2; fi'"
          }
        ]
      }
    ]
  }
}
```

위 설정은 Claude가 `Bash` 도구를 실행하기 **직전**에 훅을 먼저 실행해서, 명령어에 `rm -rf /`가 포함되어 있으면 종료 코드 `2`를 반환해 도구 실행 자체를 막습니다.

## 훅이 발생하는 세 가지 빈도

훅 이벤트는 발생 빈도에 따라 크게 세 종류로 나눌 수 있습니다.

- **세션당 1회**: `SessionStart`, `SessionEnd` — 세션이 시작되거나 끝날 때
- **턴당 1회**: `UserPromptSubmit`, `Stop` — 사용자가 프롬프트를 제출할 때, 모델이 응답 턴을 마칠 때
- **도구 호출마다**: `PreToolUse`, `PostToolUse` — 에이전트 루프 안에서 도구를 부를 때마다

이 중 실무에서 가장 자주 쓰이는 이벤트들을 정리하면 다음과 같습니다.

| 이벤트 | 발생 시점 | 주 용도 |
|---|---|---|
| `SessionStart` | 세션 시작/재개 시 | 환경 변수 설정, 프로젝트 초기화 스크립트 실행 |
| `UserPromptSubmit` | 사용자가 프롬프트를 제출할 때 | 입력 검증, 위험한 요청 사전 차단 |
| `PreToolUse` | 도구 실행 **전** | 명령어 차단, 파일 보호 같은 가드레일 |
| `PostToolUse` | 도구 실행 **후** | 자동 포맷팅, 린트 같은 후처리 |
| `Stop` | 모델의 응답 턴이 끝날 때 | 작업 완료 알림, 테스트 자동 실행 |
| `PreCompact` | 컨텍스트 압축 전 | 압축 차단, 중요 정보 보존 |

이 외에도 서브에이전트 시작/종료(`SubagentStart`/`SubagentStop`), 권한 요청(`PermissionRequest`) 등 더 세분화된 이벤트가 있지만, 실제로 대부분의 커스터마이징은 위 표의 이벤트만으로 충분합니다.

## 훅 설정 구조

훅은 `settings.json`의 `hooks` 필드 아래 **이벤트 → 매처(matcher) → 훅** 3단 구조로 작성합니다.

```json
{
  "hooks": {
    "PreToolUse": [
      {
        "matcher": "Bash",
        "hooks": [
          { "type": "command", "command": "./scripts/validate-command.sh" }
        ]
      }
    ],
    "PostToolUse": [
      {
        "matcher": "Edit|Write",
        "hooks": [
          { "type": "command", "command": "npm run lint --fix" }
        ]
      }
    ]
  }
}
```

### 매처(Matcher)

매처는 "이 이벤트 중에서도 어떤 도구/에이전트에 대해서만 훅을 실행할지"를 정합니다.

| 매처 값 | 평가 방식 | 예시 |
|---|---|---|
| `"*"`, `""`, 생략 | 모든 항목에 매칭 | 이벤트 발생 시 항상 실행 |
| 문자/숫자/`_`/`-`/공백/`,`/`\|`만 포함 | 정확한 문자열(또는 `\|`, `,`로 구분된 목록) | `Edit\|Write`는 Edit 또는 Write에만 매칭 |
| 그 외 문자 포함 | 정규식으로 평가 | `^Notebook`은 Notebook으로 시작하는 모든 도구 |

### 조건부 실행(if 필드)

매처가 "어떤 도구인가"를 본다면, `if` 필드는 "그 도구를 **어떤 인자**로 부르는가"까지 확인합니다.

```json
{
  "hooks": {
    "PreToolUse": [
      {
        "matcher": "Bash",
        "hooks": [
          {
            "type": "command",
            "if": "Bash(git push *)",
            "command": "./scripts/check-branch-protection.sh"
          }
        ]
      }
    ]
  }
}
```

이 예시는 매처 자체는 모든 `Bash` 호출에 걸리지만, 실제로 훅이 실행되는 건 `git push`로 시작하는 명령일 때뿐입니다. 이렇게 하면 관련 없는 Bash 호출마다 불필요하게 스크립트를 띄우는 걸 피할 수 있습니다.

## 훅 타입 다섯 가지

훅은 "무엇으로 판단을 내리는가"에 따라 다섯 가지 타입으로 나뉩니다.

| 타입 | 판단 주체 | 언제 쓰는가 |
|---|---|---|
| `command` | 셸 스크립트 | 규칙이 명확하고 코드로 표현 가능할 때 (가장 기본) |
| `prompt` | LLM 단일 턴 판단 | `if/else`로 딱 떨어지지 않는 애매한 판단이 필요할 때 |
| `agent` | 도구를 쓰는 서브에이전트 | 판단을 위해 파일을 읽거나 테스트를 실행해봐야 할 때 |
| `http` | 외부 서버 | 팀 전체 정책을 중앙 서버에서 관리하고 싶을 때 |
| `mcp_tool` | 이미 연결된 MCP 도구 | 별도 스크립트 없이 기존 MCP 도구를 그대로 재사용하고 싶을 때 |

가장 많이 쓰이는 건 역시 `command`입니다.

```json
{
  "type": "command",
  "command": "./scripts/check-style.sh"
}
```

반면 "이 커밋 메시지가 팀 컨벤션에 맞는가?" 같은 질문은 규칙만으로 판단하기 애매하므로 `prompt` 타입이 더 적합합니다.

```json
{
  "type": "prompt",
  "prompt": "방금 작성된 커밋 메시지가 'type: 설명' 형식(feat, fix, chore, docs 등)을 따르는지 확인하세요. 따르지 않으면 {\"ok\": false, \"reason\": \"위반 내용\"}으로 응답하세요."
}
```

`prompt` 훅은 모델이 `{"ok": true}` 또는 `{"ok": false, "reason": "..."}` 형태의 JSON으로 판단을 반환합니다. 여기서 한 걸음 더 나아가 "테스트를 실제로 돌려보고 통과했는지 확인해야" 하는 경우에는 도구 사용이 가능한 `agent` 타입이 필요합니다.

```json
{
  "type": "agent",
  "prompt": "테스트 스위트를 실행하고 모두 통과하는지 확인하세요. 실패하는 테스트가 있으면 어떤 것인지 보고하세요.",
  "timeout": 120
}
```

## 종료 코드와 차단 동작

`command` 타입 훅에서는 **종료 코드 하나**로 다음 행동이 결정됩니다. 여기서 가장 헷갈리기 쉬운 부분을 짚고 넘어가야 합니다.

- **종료 코드 `0`**: 통과. 정상적으로 다음 단계로 진행
- **종료 코드 `1`**: 에러지만 **차단하지 않음**. 경고만 표시되고 작업은 계속됨
- **종료 코드 `2`**: **차단**. 이벤트에 따라 도구 실행을 막거나(`PreToolUse`), 세션을 다시 진행시키거나(`Stop`) 하는 등 실제로 흐름을 바꿈

일반적인 유닉스 셸 스크립트에서는 `1`이 대표적인 실패 코드지만, 훅에서는 `1`이 "그냥 경고"로 취급됩니다. 강제로 무언가를 막고 싶다면 반드시 `exit 2`를 써야 한다는 점이 실무에서 가장 자주 놓치는 부분입니다.

이벤트별로 종료 코드 `2`의 의미도 조금씩 다릅니다.

| 이벤트 | 종료 코드 2의 의미 |
|---|---|
| `PreToolUse` | 도구 실행을 막고, stderr 메시지가 모델에게 피드백으로 전달됨 |
| `Stop` | 오히려 "계속 진행"을 의미 — stdout 내용이 사용자 메시지로 주입되어 모델이 작업을 이어감 |
| `PreCompact` | 컨텍스트 압축을 막음 |

즉 `PreToolUse`에서는 `2`가 "멈춰"라는 뜻이지만, `Stop`에서는 정반대로 "아직 안 끝났으니 더 해"라는 뜻이 됩니다. 이벤트마다 종료 코드의 의미가 다르다는 점을 반드시 문서에서 확인하고 적용해야 합니다.

## 실전 예제

### 1. 프로덕션 설정 파일 수정 차단

배포 환경 설정 파일은 실수로라도 수정되면 곤란하므로, 편집 자체를 훅으로 막아버릴 수 있습니다.

```json
{
  "hooks": {
    "PreToolUse": [
      {
        "matcher": "Edit|Write",
        "hooks": [
          {
            "type": "command",
            "command": "bash -c 'FILE=$(cat | jq -r \".tool_input.file_path // empty\"); if echo \"$FILE\" | grep -q \"config/production\"; then echo \"프로덕션 설정 파일은 직접 수정할 수 없습니다\" >&2; exit 2; fi'"
          }
        ]
      }
    ]
  }
}
```

### 2. 파일 저장 후 자동 포맷팅

코드를 고칠 때마다 매번 "포맷팅도 해줘"라고 말하는 대신, 파일이 수정될 때마다 자동으로 포매터를 돌립니다.

```json
{
  "hooks": {
    "PostToolUse": [
      {
        "matcher": "Edit|Write",
        "hooks": [
          {
            "type": "command",
            "command": "npx prettier --write $(cat | jq -r '.tool_input.file_path // empty')"
          }
        ]
      }
    ]
  }
}
```

### 3. 작업 완료 시 데스크톱 알림

긴 작업이 끝났을 때 터미널을 계속 쳐다보고 있지 않아도 되도록, 응답 턴이 끝나는 시점에 알림을 띄웁니다.

```json
{
  "hooks": {
    "Stop": [
      {
        "hooks": [
          {
            "type": "command",
            "command": "terminal-notifier -title 'Claude Code' -message '작업이 완료되었습니다'"
          }
        ]
      }
    ]
  }
}
```

### 4. 세션 시작 시 팀 공유 스크립트 자동 실행

세션이 열릴 때마다 사람이 매번 "환경변수 세팅해줘"라고 말하지 않도록, 초기화 스크립트를 자동 실행합니다.

```json
{
  "hooks": {
    "SessionStart": [
      {
        "hooks": [
          {
            "type": "command",
            "command": "./scripts/load-dev-env.sh"
          }
        ]
      }
    ]
  }
}
```

## 실행 시 알아두면 좋은 것들

- **타임아웃**: `command` 타입은 기본 10분(600초)까지 대기하며, 훅 항목에 `timeout` 필드를 넣어 조절할 수 있습니다. 다만 `UserPromptSubmit`처럼 매 프롬프트마다 실행되는 이벤트는 기본 타임아웃이 30초로 훨씬 짧습니다. 훅이 세션 전체를 지연시키는 걸 막기 위한 설계입니다.
- **경로 플레이스홀더**: 훅이 어느 디렉터리에서 실행되는지와 무관하게 스크립트 경로를 참조하려면 `${CLAUDE_PROJECT_DIR}`(프로젝트 루트)를 사용합니다. 상대 경로에 의존하면 훅이 실행되는 위치에 따라 스크립트를 못 찾는 문제가 생길 수 있습니다.
- **디버깅**: 훅이 실패하면 트랜스크립트에 에러 알림과 stderr 첫 줄이 표시되지만, 전체 내용을 보려면 `claude --debug`로 세션을 시작한 뒤 디버그 로그를 확인하는 것이 가장 확실합니다.
- **`settings.json` 위치**: 프로젝트 전체에 적용하려면 `.claude/settings.json`(팀과 공유, 커밋 대상), 개인 전역 설정에는 `~/.claude/settings.json`을 사용합니다. 여러 레벨의 훅은 서로 덮어쓰지 않고 **병합**되어 함께 실행됩니다.

## 정리

훅은 Claude Code의 라이프사이클 특정 시점(세션 시작, 도구 실행 전후, 턴 종료 등)에 자동으로 끼어드는 사용자 정의 핸들러이며, CLAUDE.md와 달리 **모델의 판단을 거치지 않고 결정론적으로 실행이 보장**된다는 점이 핵심입니다. `settings.json`에 이벤트-매처-훅 3단 구조로 등록하며, 판단 방식에 따라 command/prompt/agent/http/mcp_tool 다섯 가지 타입 중 선택할 수 있습니다. 가장 중요한 실무 포인트는 종료 코드 규칙입니다 — 대부분의 이벤트에서 오직 `2`만 차단으로 취급되고 `1`은 경고에 그치며, 같은 `2`라도 이벤트에 따라 "멈춰라"와 "계속하라"로 의미가 정반대가 될 수 있습니다. 자동 포맷팅, 위험한 명령 차단, 파일 보호, 완료 알림처럼 "사람이 매번 확인하기 귀찮고, 절대 놓치면 안 되는" 작업일수록 훅으로 옮겨두는 것이 효과적입니다.

## 참고

- [클로드 코드 가이드 - 09. 훅 (Hooks)](https://wikidocs.net/333424)
