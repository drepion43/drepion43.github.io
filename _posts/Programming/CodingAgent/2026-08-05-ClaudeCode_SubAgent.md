---
title:  "Claude Code SubAgent 정리"
categories: CodingAgent
tag: [CodingAgent, Claude Code, SubAgent, Anthropic]
toc: true
author_profile: false
sidebar:
    nav: "docs"
use_math: false
excerpt: Claude Code의 SubAgent가 무엇인지 개념부터, CLI에서 직접 만드는 법, 실전 예시까지 정리
comments: true
date: 2026-08-05
toc_sticky: true
---

## 들어가며

앞서 <a href="https://drepion43.github.io/codingagent/ClaudeCode_기능/" target="_blank">기본/커스텀 커맨드</a>, <a href="https://drepion43.github.io/codingagent/ClaudeMd_파일기능/" target="_blank">CLAUDE.md</a>, <a href="https://drepion43.github.io/codingagent/ClaudeCode_Skills/" target="_blank">Skills</a>, <a href="https://drepion43.github.io/codingagent/ClaudeCode_슬래시명령어/" target="_blank">슬래시 명령어</a>, <a href="https://drepion43.github.io/codingagent/ClaudeCode_Hooks/" target="_blank">Hooks</a>, <a href="https://drepion43.github.io/codingagent/MCP_정리/" target="_blank">MCP</a>, <a href="https://drepion43.github.io/codingagent/ClaudeCode_Plugins/" target="_blank">Plugins</a>를 다뤘습니다. 이번 글에서는 <a href="https://drepion43.github.io/codingagent/ClaudeCode_동작원리/" target="_blank">에이전트 루프</a> 글에서 잠깐 언급했던 서브에이전트(SubAgent)를 본격적으로 정리하겠습니다.

## 서브에이전트란 무엇인가

작업을 팀원에게 위임하듯, Claude Code도 특정 작업을 전문 서브에이전트에게 나눠줄 수 있습니다. 서브에이전트는 각각 **독립적인 컨텍스트 윈도우, 커스텀 시스템 프롬프트, 도구 접근 권한, 독립적인 권한**을 가진 별도의 AI 어시스턴트입니다.

### 왜 필요한가

- **컨텍스트 보존**: 탐색·조사 과정에서 나온 부산물(파일 내용, 로그, 검색 결과)이 메인 대화를 오염시키지 않습니다. 서브에이전트가 끝나면 최종 요약만 메인 대화로 돌아옵니다.
- **제약 강제**: 특정 도구만 쓰도록 제한할 수 있습니다. 예를 들어 리뷰 전용 에이전트는 파일을 읽기만 하고 수정은 못 하게 만들 수 있습니다.
- **재사용**: 사용자 레벨에 정의하면 여러 프로젝트에서 같은 에이전트를 재사용할 수 있습니다.
- **특화된 동작**: 도메인에 맞춘 시스템 프롬프트로 항상 같은 관점에서 작업하게 만들 수 있습니다.
- **비용 절감**: 간단한 작업은 더 빠르고 저렴한 모델로 라우팅할 수 있습니다.

### 컨텍스트 격리

서브에이전트는 메인 세션과 별도의 컨텍스트 윈도우에서 실행됩니다. 서브에이전트가 파일 10개를 읽어도, 메인 세션에는 그 결과 요약만 들어갑니다.

서브에이전트가 독립적으로 갖는 것: 자체 시스템 프롬프트, 프로젝트 CLAUDE.md, MCP 도구·Skill 접근, (설정 시) 자체 메모리 파일.

서브에이전트에 전달되지 **않는** 것: 메인 세션의 대화 이력, 메인 세션의 Auto Memory, Plan 모드 컨트롤처럼 중첩 컨텍스트에 불필요한 도구.

## 병렬 실행 방식 중 서브에이전트의 위치

Claude Code에서 작업을 병렬화하는 방법은 서브에이전트 말고도 여러 가지가 있습니다. 어떤 방식을 고를지는 **누가 조율하는가**, **작업자들이 서로 대화해야 하는가**에 따라 갈립니다.

| 방식 | 제공하는 것 | 적합한 상황 |
|---|---|---|
| **Subagents** | 위임된 작업자가 자체 컨텍스트에서 곁가지 작업 수행 후 요약만 반환 | 다시 참조하지 않을 자료(검색 결과, 로그)가 메인 대화를 오염시킬 때 |
| **Agent view** (`claude agents`) | 백그라운드 세션을 한 화면에서 디스패치·모니터링 | 여러 독립 작업을 맡기고 상태만 훑어보고 싶을 때 |
| **Agent teams** | 공유 태스크 리스트로 협업하는 다수 세션 (실험적) | 여러 관점이 서로 논의하며 진행해야 할 때 |
| **Forked subagents** | 메인 대화 컨텍스트를 그대로 상속 | 배경 설명이 너무 많이 필요하거나, 같은 출발점에서 여러 접근을 동시에 시도할 때 |

이 글에서는 가장 기본이 되는 **Subagents(명명된 서브에이전트)** 를 중심으로 다룹니다. 핵심 판단 기준은 "작업자들이 서로 대화해야 하는가"입니다. 서로 대화할 필요 없이 결과만 취합하면 되는 작업은 서브에이전트로 충분하고, 여러 관점이 서로 반박하며 논의해야 하는 작업이라면 에이전트 팀이 더 적합합니다.

## 내장 에이전트 3종

커스텀 에이전트를 만들기 전에, Claude Code에 이미 내장된 에이전트부터 알아두면 좋습니다.

| 에이전트 | 도구 | 용도 |
|---|---|---|
| **Explore** | 읽기 전용 | 파일 탐색, 코드 검색, 코드베이스 분석 |
| **Plan** | 읽기 전용 | Plan 모드에서의 코드베이스 조사 |
| **General-purpose** | 전체 | 복잡한 멀티스텝 작업 |

Explore 에이전트는 호출 시 속도(탐색 범위)를 지정할 수 있습니다.

| 레벨 | 용도 | 예시 |
|---|---|---|
| quick | 특정 파일/함수를 찾을 때 | "getUserById 함수가 어디 있는지 찾아줘" |
| medium | 관련 코드를 폭넓게 파악할 때 | "인증 관련 코드를 살펴봐줘" |
| very thorough | 아키텍처 전체를 이해할 때 | "이 프로젝트의 데이터 흐름을 분석해줘" |

## 서브에이전트를 호출하는 세 가지 방법

Claude는 보통 요청 내용과 서브에이전트의 `description`을 보고 자동으로 위임 여부를 판단합니다. 자동 위임이 충분하지 않을 때는 강제력이 점점 커지는 세 가지 방법으로 직접 지정할 수 있습니다.

1. **자연어**: 프롬프트에 이름을 언급하면 Claude가 위임 여부를 판단합니다.
   ```text
   test-runner 서브에이전트로 실패한 테스트를 고쳐줘
   ```
2. **`@`-멘션**: 반드시 해당 서브에이전트가 실행되도록 지정합니다.
   ```text
   @agent-code-reviewer auth 변경을 봐줘
   ```
3. **세션 전체 적용**: 메인 세션 자체를 그 서브에이전트의 시스템 프롬프트·도구·모델로 시작합니다.
   ```bash
   claude --agent code-reviewer
   ```

## Claude Code CLI에서 커스텀 SubAgent 만드는 법

과거에는 `/agents`라는 대화형 마법사로 서브에이전트를 관리했지만, 이 명령어는 제거되었습니다. 지금은 두 가지 방법으로 만듭니다.

### 방법 1 — 파일로 직접 작성 (가장 일반적)

`.claude/agents/` 디렉토리(프로젝트 전용) 또는 `~/.claude/agents/`(모든 프로젝트 공통)에 YAML 프론트매터가 있는 마크다운 파일을 만들면 됩니다. Claude에게 "이런 서브에이전트를 만들어줘"라고 자연어로 요청해서 자동 생성하게 할 수도 있습니다.

예를 들어, API 응답이 문서에 정의된 스키마와 일치하는지 검증하는 서브에이전트를 만든다고 하면 다음과 같이 작성합니다.

`.claude/agents/api-schema-checker.md`:

```markdown
---
name: api-schema-checker
description: API 응답이 OpenAPI 스펙과 일치하는지 검증합니다. API 라우트 수정 후 자동으로 사용합니다.
tools: Read, Grep, Glob, Bash
model: sonnet
---

당신은 API 계약 검증 전문가입니다.

호출되면:
1. 변경된 라우트 파일을 찾는다
2. openapi.yaml에서 해당 엔드포인트의 스키마를 확인한다
3. 응답 필드, 타입, 필수 여부가 스키마와 일치하는지 대조한다
4. 불일치가 있으면 정확히 어떤 필드가 다른지 보고한다
```

프론트매터에서 실무에서 자주 쓰는 필드는 다음과 같습니다.

| 필드 | 설명 |
|---|---|
| `name` | 고유 식별자 (소문자, 하이픈) |
| `description` | Claude가 언제 위임할지 판단하는 기준 |
| `tools` | 사용 가능 도구 (생략 시 전체 상속) |
| `model` | `sonnet`/`opus`/`haiku`/`inherit` (기본: `inherit`) |
| `permissionMode` | `default`/`acceptEdits`/`auto`/`dontAsk`/`bypassPermissions`/`plan` |
| `memory` | 영구 메모리 스코프: `user`/`project`/`local` |
| `isolation` | `worktree`로 설정하면 격리된 git worktree에서 실행 |
| `background` | `true`면 항상 백그라운드에서 실행 |

만든 뒤에는 자연어로 위임하거나, `claude --agent api-schema-checker`로 세션 전체를 이 에이전트로 시작할 수 있습니다.

### 방법 2 — CLI에서 즉석으로 정의 (파일 없이 임시로)

디스크에 저장하지 않고 그 세션에서만 쓸 임시 에이전트가 필요하다면 `--agents` 플래그에 JSON을 바로 전달할 수 있습니다. 빠른 테스트나 자동화 스크립트에 유용합니다.

```bash
claude --agents '{
  "log-triager": {
    "description": "배포 로그에서 에러 패턴을 분류합니다.",
    "prompt": "로그를 훑어보고 에러를 심각도별로 분류한 뒤 요약을 보고합니다.",
    "tools": ["Read", "Grep", "Bash"],
    "model": "haiku"
  }
}'
```

파일 기반 서브에이전트와 동일한 필드(`description`, `tools`, `model`, `permissionMode` 등)를 지원하며, `prompt` 필드가 파일의 마크다운 본문(시스템 프롬프트)에 해당합니다.

## 도구 제한과 권한

서브에이전트가 아무 도구나 쓸 수 있으면 의도치 않은 파일 수정이 발생할 수 있습니다. `tools`(허용 목록) 또는 `disallowedTools`(차단 목록)로 범위를 좁힙니다.

서브에이전트가 또 다른 서브에이전트를 만들 수 있는 범위도 제한할 수 있습니다.

```yaml
---
name: coordinator
description: 전문 에이전트들 사이의 작업 조율
tools: Agent(worker, researcher), Read, Bash
---
```

`Agent(worker, researcher)`는 이 두 에이전트만 생성 가능하다는 뜻이고, 괄호 없이 `Agent`만 쓰면 모든 서브에이전트를 생성할 수 있으며, `Agent` 자체를 생략하면 하위 에이전트를 아예 만들 수 없습니다.

특정 서브에이전트를 아예 못 쓰게 막으려면 권한 설정에서 차단합니다.

```json
{
  "permissions": {
    "deny": ["Agent(api-schema-checker)"]
  }
}
```

## 영구 메모리와 워크트리 격리

서브에이전트는 기본적으로 호출될 때마다 백지 상태에서 시작합니다. 두 가지 옵션으로 이 한계를 보완할 수 있습니다.

- **영구 메모리** (`memory: user`/`project`/`local`): 이전 호출에서 발견한 패턴이나 인사이트를 `MEMORY.md`에 남겨 다음 호출에서 이어받습니다. 예를 들어 `api-schema-checker`가 프로젝트마다 자주 틀리는 필드 타입 패턴을 기억해두면, 다음번엔 그 부분을 먼저 확인하게 만들 수 있습니다.
- **워크트리 격리** (`isolation: worktree`): 서브에이전트가 별도 git worktree에서 작업하도록 만들어, 메인 작업 중인 코드와 파일 변경이 충돌하지 않게 합니다. 대규모 리팩터링처럼 실제로 파일을 많이 건드리는 서브에이전트에 적합합니다.

## 실전 예시: 마이그레이션 검토 에이전트

여러 필드를 함께 활용하는 예시로, 데이터베이스 마이그레이션 파일을 검토하는 서브에이전트를 만들어보겠습니다. 워크트리 격리 없이 읽기 전용으로 동작하고, 발견한 위험 패턴을 프로젝트 메모리에 누적합니다.

```markdown
---
name: migration-reviewer
description: 데이터베이스 마이그레이션 파일의 안전성을 검토합니다. 마이그레이션 파일 작성/수정 후 사용합니다.
tools: Read, Grep, Glob, Bash
model: sonnet
memory: project
---

당신은 데이터베이스 마이그레이션 안전성 검토 전문가입니다.

체크리스트:
- NOT NULL 컬럼을 기본값 없이 추가하는지
- 대용량 테이블에 락을 오래 거는 작업(인덱스 동기 생성 등)이 있는지
- 롤백(down) 마이그레이션이 실제로 원상 복구가 가능한지
- 컬럼 삭제 시 아직 참조하는 코드가 남아있지 않은지

이전에 발견한 위험 패턴은 프로젝트 메모리에서 확인하고,
새로 발견한 패턴은 메모리에 추가해 다음 검토에 활용하세요.
```

## 서브에이전트 vs 에이전트 팀

서브에이전트와 비슷해 보이지만 성격이 다른 기능으로 **에이전트 팀(Agent Teams)**이 있습니다. 아직 실험적 기능이고 기본적으로 비활성화되어 있지만, 차이를 알아두면 어떤 상황에 무엇을 써야 할지 판단하기 쉽습니다.

| 항목 | 서브에이전트 | 에이전트 팀 |
|---|---|---|
| 소통 | 메인 에이전트에게만 결과 보고 | 팀원끼리 직접 메시지 교환 |
| 조율 | 메인 에이전트가 모든 작업 관리 | 공유 태스크 리스트로 자체 조율 |
| 적합한 작업 | 결과만 중요한 집중 작업 | 토론과 협업이 필요한 복잡한 작업 |
| 토큰 비용 | 낮음 (결과만 요약되어 반환) | 높음 (팀원마다 별도 인스턴스) |

"이거 조사해서 결과만 알려줘" 유형의 작업은 서브에이전트로 충분하고, 여러 관점이 서로의 의견에 반론을 제기하며 합의에 도달해야 하는 작업이라면 에이전트 팀이 더 적합합니다.

## 정리

서브에이전트는 독립된 컨텍스트 윈도우·시스템 프롬프트·도구 권한을 가진 위임 작업자로, 메인 대화를 탐색 과정의 부산물로 오염시키지 않으면서 결과만 요약해서 돌려받을 수 있게 해줍니다. Claude Code에는 Explore/Plan/General-purpose 3종이 기본 내장되어 있고, 커스텀 서브에이전트는 `.claude/agents/`에 YAML 프론트매터가 있는 마크다운 파일을 작성하거나 `claude --agents`로 세션 한정 임시 정의를 전달해서 만들 수 있습니다. `tools`로 도구 범위를 좁히고, `memory`로 검토 노하우를 누적시키고, `isolation: worktree`로 파일 충돌을 막는 식으로 필드를 조합하면, 반복되는 검토·조사 작업을 신뢰할 수 있는 전문 에이전트로 만들어 재사용할 수 있습니다.

## 참고

- [클로드 코드 가이드 - 10. 서브에이전트](https://wikidocs.net/333425)
