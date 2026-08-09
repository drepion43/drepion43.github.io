---
title:  "Claude Code 활용한 프로젝트 구성 방법"
categories: CodingAgent
tag: [CodingAgent, Claude Code, Agile, Anthropic]
toc: true
author_profile: false
sidebar:
    nav: "docs"
use_math: false
excerpt: 지금까지 다룬 Claude Code 기능들을 새 프로젝트에 스프린트 단위로 조합해서 적용하는 실전 가이드
comments: true
date: 2026-08-06
toc_sticky: true
---

## 들어가며

이 시리즈에서 <a href="https://drepion43.github.io/codingagent/ClaudeCode_기능/" target="_blank">기본/커스텀 커맨드</a>, <a href="https://drepion43.github.io/codingagent/ClaudeMd_파일기능/" target="_blank">CLAUDE.md</a>, <a href="https://drepion43.github.io/codingagent/ClaudeCode_Skills/" target="_blank">Skills</a>, <a href="https://drepion43.github.io/codingagent/ClaudeCode_슬래시명령어/" target="_blank">슬래시 명령어</a>, <a href="https://drepion43.github.io/codingagent/ClaudeCode_Hooks/" target="_blank">Hooks</a>, <a href="https://drepion43.github.io/codingagent/MCP_정리/" target="_blank">MCP</a>, <a href="https://drepion43.github.io/codingagent/ClaudeCode_Plugins/" target="_blank">Plugins</a>, <a href="https://drepion43.github.io/codingagent/ClaudeCode_SubAgent/" target="_blank">SubAgent</a>까지 부품을 하나씩 살펴봤습니다. 이번 글에서는 이 부품들을 실제로 조립해서, 새 프로젝트를 시작할 때 어떤 순서로 어떤 기능을 붙여나가면 좋을지 정리하겠습니다.

## 예제 프로젝트: 왜 작게 시작하는가

이 글에서는 예제로 **할 일 관리 CLI 도구(Todo CLI)** 를 다룹니다. 터미널에서 `todo add`, `todo list`, `todo done` 같은 명령으로 할 일을 관리하는 개인용 도구로, 데이터는 로컬 JSON 파일 하나에 저장합니다. DB 서버나 배포 인프라가 필요 없는 최소 규모입니다.

일부러 작은 프로젝트를 골랐습니다. 스크래핑, 데이터베이스, 대시보드까지 갖춘 큰 프로젝트를 예로 들면, 정작 하고 싶은 이야기인 "Claude Code 기능을 언제 왜 쓰는가"보다 프로젝트 자체를 설명하는 데 지면이 더 들어가기 때문입니다. 규모는 작아도 핵심 요구사항은 실전 프로젝트와 다르지 않습니다.

- 할 일 추가/목록 조회/완료 처리/삭제
- 우선순위와 마감일 설정
- 데이터 파일이 깨지지 않아야 함(동시 실행, 비정상 종료 대비)
- 나중에 팀원과 공유해서 각자 변형해 쓸 수 있어야 함

## 0단계 — 프로젝트 뼈대 세팅 (Day 0)

코드를 한 줄도 쓰기 전에 먼저 해두는 게 있습니다.

1. **저장소 초기화 후 `/init` 실행**: Claude Code가 프로젝트를 스캔해서 `CLAUDE.md` 초안을 만들어줍니다. 아직 코드가 없으니 초안은 비어있다시피 하겠지만, 앞으로 채워나갈 뼈대가 생깁니다.
2. **디렉토리 컨벤션을 CLAUDE.md에 바로 기록**: "명령어는 `src/commands/`, 각 명령어 파일명은 동사형(`add.py`, `list.py`)" 같은 규칙을 정하는 즉시 적어둡니다. 나중에 기억을 더듬어 채우려면 이미 컨벤션이 어긋난 파일이 몇 개 생긴 뒤입니다.
3. **안전장치 Hook을 가장 먼저 설치**: 아직 기능이 하나도 없는 시점이지만, 데이터 파일 보호 규칙만큼은 먼저 넣어둡니다.

```json
{
  "hooks": {
    "PreToolUse": [
      {
        "matcher": "Write",
        "hooks": [
          {
            "type": "command",
            "command": "bash -c 'FILE=$(cat | jq -r \".tool_input.file_path // empty\"); if echo \"$FILE\" | grep -q \"data/todos.json\"; then echo \"데이터 파일은 add/done 커맨드를 통해서만 수정합니다\" >&2; exit 2; fi'"
          }
        ]
      }
    ]
  }
}
```

이 순서를 맨 처음에 두는 이유는 단순합니다. 규칙을 나중에 추가하면, 이미 그 규칙을 어기는 코드가 코드베이스에 쌓인 뒤이기 때문입니다. 컨벤션과 안전장치는 "필요해지면 그때" 추가하는 것보다, 프로젝트가 비어있을 때 미리 깔아두는 편이 훨씬 쌉니다.

## 애자일 스프린트로 기능 쌓아 올리기

스프린트마다 "이번엔 무엇을 만드는가"와 "이 단계에서 왜 이 Claude Code 기능이 필요해지는가"를 짝지어 진행합니다. 핵심은 모든 기능을 처음부터 다 세팅하는 게 아니라, **실제로 그 기능이 필요해지는 스프린트에서** 하나씩 도입한다는 점입니다.

### Sprint 1 — 핵심 CRUD 명령어

**목표**: `add`, `list`, `done` 세 가지 기본 명령어 구현.

이 스프린트부터 반복이 시작됩니다. 명령어 하나를 추가할 때마다 "파서 등록 → 테스트 작성 → 도움말 문서 갱신"이라는 같은 절차를 거치게 됩니다. 이 절차를 매번 설명하는 대신 Skill로 표준화합니다.

`.claude/skills/add-command/SKILL.md`:

```yaml
---
description: Todo CLI에 새 서브커맨드를 추가한다. "delete 커맨드 추가해줘"처럼 새 명령어 구현을 요청할 때 사용한다.
---

1. `src/commands/<이름>.py`에 커맨드 함수를 추가한다.
2. `src/cli.py`의 서브파서 등록 목록에 새 커맨드를 등록한다.
3. `tests/test_<이름>.py`에 정상 케이스와 실패 케이스 테스트를 작성한다.
4. `README.md`의 명령어 목록 표에 새 커맨드를 추가한다.
5. `pytest`를 실행해 전체 테스트가 통과하는지 확인한다.
```

이렇게 해두면 "delete 커맨드 추가해줘" 한 마디로, 파서 등록부터 테스트·문서화까지 매번 같은 순서로 진행됩니다.

### Sprint 2 — 데이터 무결성과 엣지 케이스

**목표**: 우선순위·마감일 필드 추가, 데이터 파일이 깨지지 않도록 보강.

기능이 늘어나면서 엣지 케이스도 늘어납니다. 빈 파일, 깨진 JSON, 두 프로세스가 동시에 쓰기를 시도하는 경우처럼 평소엔 잘 안 짚어보는 경로를 전담으로 파고드는 SubAgent를 만듭니다.

`.claude/agents/edge-case-tester.md`:

```markdown
---
name: edge-case-tester
description: 데이터 파일 처리 로직의 엣지 케이스를 검증합니다. 파일 읽기/쓰기 로직 변경 후 사용합니다.
tools: Read, Write, Bash, Grep, Glob
model: sonnet
---

당신은 엣지 케이스 검증 전문가입니다.

다음 시나리오를 실제로 재현해서 확인하세요:
1. data/todos.json이 존재하지 않을 때 첫 실행이 정상 동작하는가
2. 파일 내용이 빈 문자열이거나 깨진 JSON일 때 에러 메시지가 명확한가
3. 두 프로세스가 거의 동시에 add를 실행했을 때 데이터 유실이 없는가
4. 마감일 형식이 잘못 입력됐을 때 적절히 거부하는가

각 시나리오를 실제로 실행해보고, 실패하면 재현 방법과 함께 보고하세요.
```

Sprint 0에서 걸어둔 Hook과 이 SubAgent는 역할이 다릅니다. Hook은 "이런 수정은 아예 못 하게 막는" 강제 규칙이고, SubAgent는 "이런 상황에서 실제로 잘 동작하는지 확인하는" 검증 작업입니다.

### Sprint 3 — 마감 임박 알림 (스트레치)

**목표**: 마감이 하루 안 남은 항목을 Slack으로 알림.

지금까지는 전부 로컬에서 끝나는 작업이었습니다. 여기서 처음으로 외부 서비스 연동이 실제로 필요해집니다. 필요해진 시점에 MCP를 도입합니다.

```bash
claude mcp add --transport http slack https://mcp.slack.com/mcp
```

연결 후 세션에서 `/mcp`로 인증을 마치면, "마감 하루 남은 할 일을 Slack #reminders 채널로 보내줘" 같은 요청을 그대로 처리할 수 있습니다. 알림이 너무 자주 울리지 않도록, 같은 항목에 대한 중복 알림을 막는 Hook도 함께 추가합니다.

여기서 중요한 건 MCP를 "언젠가 필요할 것 같아서" 스프린트 0에 미리 깔지 않았다는 점입니다. 로컬 도구로 시작해서, 실제로 외부 연동이 필요해진 스프린트에서만 붙였습니다.

### Sprint 4 — 다른 프로젝트에서도 재사용 & 팀 공유

**목표**: 지금까지 쌓은 Skill·Hook·MCP 설정을 다른 프로젝트나 팀원에게 그대로 넘기기.

`add-command` Skill, 데이터 보호 Hook, Slack MCP 연결까지 개별적으로 공유하려면 파일을 하나하나 복사해줘야 합니다. Plugin으로 묶으면 한 번에 배포할 수 있습니다.

```
todo-cli-toolkit/
├── .claude-plugin/
│   └── plugin.json
├── skills/
│   └── add-command/
│       └── SKILL.md
├── hooks/
│   └── hooks.json
└── .mcp.json
```

```json
{
  "name": "todo-cli-toolkit",
  "description": "Todo CLI류 프로젝트에서 재사용하는 커맨드 추가 절차와 데이터 보호 규칙",
  "version": "1.0.0"
}
```

이렇게 패키징해두면 비슷한 구조의 다음 CLI 프로젝트를 시작할 때도, 처음부터 다시 만들지 않고 `/plugin install todo-cli-toolkit@<마켓플레이스>` 한 번으로 같은 작업 방식을 그대로 가져올 수 있습니다.

## 하루 작업 흐름 예시

스프린트 진행 중 실제 하루가 어떻게 흘러가는지 시나리오로 보면 이렇습니다.

1. 세션을 열고 어제 어디까지 했는지 확인 (`/context`로 컨텍스트 상태 확인, 또는 `/resume`으로 어제 세션 이어가기)
2. "priority 필드 정렬이 이상하다"는 이슈를 확인하고, `edge-case-tester` 서브에이전트에 재현을 위임 — 메인 세션은 그 사이 다른 작업(문서 정리 등)을 계속 진행
3. 서브에이전트가 재현 결과를 요약해서 돌아오면, 원인을 파악하고 직접 수정
4. 수정 후 `/code-review`로 변경사항 검토
5. 리뷰에서 지적된 부분을 반영하고 커밋 (이때 Sprint 0에서 걸어둔 "테스트 없이 커밋 방지" Hook이 자동으로 검증)

기능 하나하나를 언제 썼는지 되짚어보면, 결국 "지금 이 순간 필요한 것"을 그때그때 자연스럽게 썼을 뿐입니다.

## 완성된 CLAUDE.md 스냅샷

네 스프린트를 거치며 쌓인 `CLAUDE.md`는 대략 이런 모습이 됩니다.

```markdown
## 디렉토리 구조
- 커맨드는 `src/commands/<동사>.py`, 서브파서는 `src/cli.py`에 등록
- 데이터 파일은 `data/todos.json` 하나만 사용, 직접 편집 금지(Hook으로 차단됨)

## 개발 명령어
- 테스트: `pytest`
- 새 서브커맨드 추가: "X 커맨드 추가해줘"라고 요청하면 add-command Skill이 처리

## 제약 사항
- data/todos.json은 커맨드 함수를 통해서만 수정한다
- 커밋 전 반드시 pytest를 통과시킨다
- Slack 알림은 같은 항목에 하루 1회로 제한한다
```

길지 않습니다. 각 줄이 정확히 어느 스프린트에서, 어떤 이유로 추가됐는지 설명할 수 있다는 점이 중요합니다.

## 정리

새 프로젝트에 Claude Code 기능을 "가장 완벽하게" 세팅한다는 것은, 모든 기능을 처음부터 다 켜두는 것이 아닙니다. 오히려 반대에 가깝습니다. 프로젝트가 비어있는 시점엔 컨벤션과 최소한의 안전장치(CLAUDE.md, Hook)만 세워두고, 반복되는 패턴이 생기면 Skill로, 반복적으로 검증이 필요한 영역이 생기면 SubAgent로, 실제로 외부 연동이 필요해지면 그때 MCP로, 다른 프로젝트·팀원과 나눌 시점이 오면 Plugin으로 묶습니다. 스프린트마다 필요해지는 순간에 맞는 기능을 하나씩 얹는 것, 그것이 이 시리즈에서 다룬 여덟 가지 기능을 가장 낭비 없이 조합하는 방법입니다.
