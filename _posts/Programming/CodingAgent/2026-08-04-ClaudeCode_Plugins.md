---
title:  "Claude Code Plugins 정리"
categories: CodingAgent
tag: [CodingAgent, Claude Code, Plugins, Anthropic]
toc: true
author_profile: false
sidebar:
    nav: "docs"
use_math: false
excerpt: Claude Code의 Plugins가 무엇인지 개념부터, 만드는 법, CLI에서 설치/관리하는 법까지 정리
comments: true
date: 2026-08-04
toc_sticky: true
---

## 들어가며

앞서 <a href="https://drepion43.github.io/codingagent/ClaudeCode_기능/" target="_blank">기본/커스텀 커맨드</a>, <a href="https://drepion43.github.io/codingagent/ClaudeMd_파일기능/" target="_blank">CLAUDE.md</a>, <a href="https://drepion43.github.io/codingagent/ClaudeCode_Skills/" target="_blank">Skills</a>, <a href="https://drepion43.github.io/codingagent/ClaudeCode_슬래시명령어/" target="_blank">슬래시 명령어</a>, <a href="https://drepion43.github.io/codingagent/ClaudeCode_Hooks/" target="_blank">Hooks</a>, <a href="https://drepion43.github.io/codingagent/MCP_정리/" target="_blank">MCP</a>를 다뤘습니다. 이번 글에서는 이 모든 확장 기능을 하나로 묶어 배포하는 마지막 퍼즐 조각, **Plugins(플러그인)** 을 정리하겠습니다.

Skills, Hooks, MCP를 개인 프로젝트에서 하나둘 만들어 쓰다 보면 자연스럽게 드는 생각이 있습니다. "이거 팀원한테도 나눠주고 싶다." Plugin은 바로 이 순간을 위한 기능입니다.

## Plugin이란 무엇인가

Plugin은 Skills, Subagents, Hooks, MCP 서버 같은 확장 기능들을 **하나의 설치 가능한 단위로 묶는 패키징 레이어**입니다. 지금까지 다룬 기능들이 각각 "무엇을 할 수 있는가"에 대한 답이었다면, Plugin은 "그것들을 어떻게 배포하고 공유하는가"에 대한 답입니다.

예를 들어 팀에서 자주 쓰는 커밋 메시지 컨벤션 검사 Skill, 위험한 배포 명령을 막는 Hook, 사내 이슈 트래커에 붙는 MCP 서버를 각각 따로 공유하려면 파일을 하나하나 복사해줘야 합니다. Plugin으로 묶으면 이 전부를 한 번의 설치 명령으로 전달할 수 있습니다.

## 단독 설정 vs Plugin

Claude Code에서 커스텀 Skills, Agents, Hooks를 추가하는 방법은 두 가지입니다.

| 방식 | 호출 형태 | 적합한 경우 |
|---|---|---|
| **단독 (`.claude/` 디렉토리)** | `/hello` | 개인 워크플로우, 프로젝트 한정 커스터마이징, 실험 단계 |
| **Plugin (`.claude-plugin/plugin.json`)** | `/my-plugin:hello` | 팀·커뮤니티 공유, 버전 관리, 여러 프로젝트에서 재사용 |

Plugin으로 만들면 명령어 앞에 플러그인 이름이 네임스페이스로 붙습니다. `/hello`가 `/my-plugin:hello`가 되는 식입니다. 조금 번거로워 보일 수 있지만, 서로 다른 팀이 만든 플러그인을 동시에 설치했을 때 이름이 겹치는 걸 막아주는 합리적인 트레이드오프입니다.

실무에서는 처음부터 Plugin으로 시작하기보다, `.claude/` 디렉토리에서 단독으로 빠르게 만들어 반복 개선한 뒤, 실제로 공유할 시점이 되면 Plugin으로 전환하는 흐름을 권장합니다.

## Plugin 디렉토리 구조

Plugin은 매니페스트 파일과 여러 컴포넌트 디렉토리로 구성됩니다.

```
my-plugin/
├── .claude-plugin/
│   └── plugin.json        # 매니페스트 (필수)
├── skills/                # Skills
│   └── code-review/
│       └── SKILL.md
├── agents/                # Subagents
│   └── security-reviewer.md
├── hooks/                 # Hooks
│   └── hooks.json
├── .mcp.json              # MCP 서버
├── .lsp.json              # LSP (코드 인텔리전스)
├── monitors/              # 백그라운드 모니터
│   └── monitors.json
├── bin/                   # 실행 파일 (PATH에 추가됨)
└── settings.json          # 기본 설정
```

여기서 가장 흔히 하는 실수가 있습니다. `commands/`, `agents/`, `skills/`, `hooks/`를 `.claude-plugin/` **안에** 넣는 것입니다. 실제로는 `plugin.json` 딱 하나만 `.claude-plugin/` 안에 들어가고, 나머지 컴포넌트 디렉토리는 전부 플러그인 **루트 레벨**에 있어야 합니다.

### plugin.json 매니페스트

```json
{
  "name": "my-first-plugin",
  "description": "기초를 배우기 위한 인사 plugin",
  "version": "1.0.0",
  "author": {
    "name": "Your Name"
  }
}
```

`name`은 플러그인의 식별자이자 그 안에 담긴 Skill들의 네임스페이스 역할을 합니다. `hello/SKILL.md`가 들어있는 플러그인의 이름이 `my-first-plugin`이면, 실제 호출은 `/my-first-plugin:hello`가 됩니다.

## 5분 만에 첫 Plugin 만들기

커밋 메시지 초안을 자동으로 작성해주는 간단한 Plugin을 예로 만들어 보겠습니다.

### 1. 디렉토리 생성

```bash
mkdir -p commit-helper/.claude-plugin
mkdir -p commit-helper/skills/draft-commit
```

### 2. 매니페스트 작성

`commit-helper/.claude-plugin/plugin.json`:

```json
{
  "name": "commit-helper",
  "description": "변경사항을 분석해 커밋 메시지 초안을 작성하는 plugin",
  "version": "1.0.0"
}
```

### 3. Skill 추가

`commit-helper/skills/draft-commit/SKILL.md`:

```yaml
---
description: 스테이징된 변경사항을 분석해 컨벤션에 맞는 커밋 메시지 초안을 작성한다
---

`git diff --staged`로 스테이징된 변경사항을 확인한다.
변경 내용을 "type: 설명" 형식(feat, fix, chore, docs, refactor 중 하나)으로
요약하고, 왜 이 변경이 필요했는지 한 줄을 덧붙인다.
```

### 4. 로컬에서 테스트

```bash
claude --plugin-dir ./commit-helper
```

세션이 시작되면 `/commit-helper:draft-commit`으로 바로 호출해 동작을 확인할 수 있습니다.

### 변경사항 즉시 반영

Plugin 파일을 수정한 뒤 세션을 다시 시작할 필요는 없습니다. `/reload-plugins`를 실행하면 Skills, Agents, Hooks, MCP 서버, LSP 서버가 모두 재시작 없이 다시 로드됩니다.

## Claude Code CLI에서 Plugin 사용법

### 설치·관리

Plugin은 `/plugin` 명령어로 탐색(Discover), 설치(Install), 삭제(Uninstall), 활성화/비활성화(Enable/Disable), 업데이트(Update)를 한 곳에서 처리합니다.

```bash
# 대화형 관리 화면 열기 (설치된 플러그인 목록, 탐색, 활성화/비활성화)
/plugin

# 설치된 플러그인 목록만 텍스트로 확인
/plugin list

# 활성화된 것만 필터링
/plugin list --enabled
```

특정 플러그인을 마켓플레이스에서 바로 설치하려면 `/plugin install <플러그인이름>@<마켓플레이스이름>` 형식을 씁니다.

```bash
# 공식 마켓플레이스에서 mcp-server-dev 플러그인 설치
/plugin install mcp-server-dev@claude-plugins-official

# 설치 후 현재 세션에 바로 반영
/reload-plugins
```

만약 아직 등록되지 않은 마켓플레이스라면 먼저 추가해야 합니다.

```bash
# GitHub 저장소를 마켓플레이스로 추가
/plugin marketplace add anthropics/claude-plugins-official

# 마켓플레이스 카탈로그가 오래된 경우 갱신
/plugin marketplace update claude-plugins-official
```

### 배포 방법

직접 만든 Plugin을 공유하려면 다음 순서를 따르는 것이 좋습니다.

1. **README.md 추가**: 설치 방법과 사용법을 안내
2. **버전 전략 결정**: `plugin.json`의 `version` 필드를 명시적으로 관리하거나, git 커밋 SHA에 의존하는 방식 중 선택
3. **마켓플레이스 생성·참여**: 팀 전용 마켓플레이스를 만들거나 기존 마켓플레이스에 참여해 배포
4. **팀 내 테스트**: 광범위하게 배포하기 전에 팀원과 먼저 검증

### 공식 마켓플레이스

- **`claude-plugins-official`**: Anthropic이 직접 큐레이션한 플러그인 모음으로, 모든 Claude Code 설치에 자동으로 제공됩니다.
- **`claude-community`**: 커뮤니티가 제출한 플러그인이 검토를 거쳐 등록되는 공개 마켓입니다.

## 실전 설치 예시: mcp-server-dev

공식 마켓플레이스에 있는 대표적인 플러그인 중 하나가 `mcp-server-dev`입니다. 이 플러그인은 사용 사례를 몇 가지 질문으로 물어본 뒤, 원격 HTTP 또는 로컬 stdio 방식의 MCP 서버 스캐폴딩을 자동으로 생성해줍니다.

```bash
# 1. 마켓플레이스를 찾을 수 없다는 메시지가 뜨면 먼저 등록
/plugin marketplace add anthropics/claude-plugins-official

# 2. 플러그인 설치
/plugin install mcp-server-dev@claude-plugins-official

# 3. 현재 세션에 즉시 반영
/reload-plugins

# 4. 설치된 스킬 호출
/mcp-server-dev:build-mcp-server
```

이처럼 "마켓플레이스 등록 → 설치 → 리로드 → 사용" 네 단계가 Plugin을 설치하는 기본 흐름입니다.

## 어떤 Plugin을 많이 쓰나

Plugin 생태계는 마켓플레이스 카탈로그가 계속 갱신되는 영역이라, 특정 이름을 나열하는 것보다는 어떤 **카테고리**의 플러그인이 자주 쓰이는지를 아는 편이 더 오래 유효합니다.

- **MCP 서버 스캐폴딩**: 위에서 다룬 `mcp-server-dev`처럼, 새 MCP 서버를 빠르게 만들어주는 개발 도구형 플러그인
- **코드 리뷰/품질 자동화**: 팀의 코드 스타일이나 보안 체크리스트를 Skill·Hook으로 묶어 배포하는 플러그인
- **팀 온보딩**: 신규 합류자가 프로젝트 구조와 워크플로우를 빠르게 익히도록 돕는 Skill 번들
- **도메인 특화 도구 모음**: 특정 프레임워크나 사내 인프라에 맞춘 커스텀 커맨드·Hook 묶음

가장 정확한 방법은 세션에서 `/plugin`을 실행해 현재 등록된 마켓플레이스의 카탈로그를 직접 탐색하는 것입니다. 마켓플레이스는 계속 늘어나므로, 이 글의 특정 시점 스냅샷보다 실제 `/plugin` 화면이 항상 더 최신입니다.

## 기존 설정을 Plugin으로 전환하기

이미 `.claude/`에 Skills나 Hooks를 만들어 쓰고 있다면, 그대로 Plugin 구조로 옮길 수 있습니다.

```bash
# Plugin 구조 생성
mkdir -p my-plugin/.claude-plugin

# 매니페스트 작성
cat > my-plugin/.claude-plugin/plugin.json <<EOF
{
  "name": "my-plugin",
  "description": "단독 설정에서 이전",
  "version": "1.0.0"
}
EOF

# 기존 파일 복사
cp -r .claude/skills my-plugin/
cp -r .claude/agents my-plugin/

# 테스트
claude --plugin-dir ./my-plugin
```

핵심은 기존 `skills/`, `agents/`, `hooks/` 디렉토리를 그대로 가져와 Plugin 루트에 두고, 매니페스트만 새로 추가하면 된다는 점입니다. 파일 내용 자체를 다시 쓸 필요는 없습니다.

## MCP와 Plugin의 차이

<a href="https://drepion43.github.io/codingagent/MCP_정리/" target="_blank">앞선 글</a>에서 다룬 MCP와 이번 글의 Plugin은 이름이 비슷해서 헷갈리기 쉽지만, 사실 같은 층위에서 경쟁하는 개념이 아닙니다.

**MCP는 "외부 서비스와 통신하는 프로토콜" 그 자체**입니다. 서버 하나를 연결하면 도구(tool)·프롬프트·리소스를 Claude에게 제공하는, 딱 그 역할 하나에 집중합니다. **Plugin은 그보다 상위의 패키징 개념**입니다. Plugin 디렉토리 구조를 다시 떠올려보면, `.mcp.json`이 그 안의 컴포넌트 중 하나였습니다 — 즉 **Plugin은 MCP 서버를 내부에 번들로 포함시킬 수 있습니다**. 둘은 대체재가 아니라, Plugin이 MCP를 감싸는 컨테이너가 될 수 있는 포함 관계에 가깝습니다.

| 구분 | MCP (직접 연결) | Plugin |
|---|---|---|
| 제공하는 것 | 도구/프롬프트/리소스 연결 하나 | Skills + Agents + Hooks + MCP + LSP + settings 묶음 |
| 설치 방법 | `claude mcp add` | `/plugin install <name>@<marketplace>` |
| 공유 단위 | 서버 연결 정보(`.mcp.json`) | 연결 정보 + 사용 가이드 + 안전장치까지 통째로 |
| 스코프 제어 | `local`/`project`/`user`로 세밀하게 지정 | 마켓플레이스 단위로 설치/제거 |
| 버전 관리 | 서버 자체의 API 버전에 그대로 의존 | `plugin.json`의 `version`으로 번들 전체를 관리 |

### 같은 서비스가 MCP와 Plugin 양쪽으로 제공될 때

사용자가 예로 든 Firecrawl처럼, 하나의 서비스가 MCP 서버로도 Plugin으로도 제공되는 경우가 있습니다. 이럴 때 둘의 실질적인 차이는 위 표의 원리를 그대로 따라갑니다.

- **MCP로 직접 연결**하면 (`claude mcp add --transport http firecrawl ...`), 그 서비스가 제공하는 도구 호출 기능만 그대로 붙습니다. 인증 헤더나 스코프는 직접 설정해야 하고, "이 도구를 언제 어떻게 쓰면 좋은지"는 스스로 CLAUDE.md 등에 적어둬야 합니다. 대신 구성이 가볍고, 서버 쪽에 새 도구가 추가되면 별도 업데이트 없이 바로 사용할 수 있습니다.
- **Plugin으로 설치**하면 (`/plugin install firecrawl@...`), 동일한 MCP 연결에 더해 그 서비스를 잘 쓰기 위한 Skill(사용 가이드), Hook(예: 민감한 URL 스크래핑을 막는 안전장치), 커스텀 커맨드 같은 것들이 함께 번들되어 한 번에 설치됩니다. 팀 전체가 똑같은 "사용 경험"을 표준화해서 나눠 갖고 싶을 때 유리합니다.

다만 실제로 어떤 서비스의 Plugin 버전에 정확히 무엇이 번들되어 있는지는 그 Plugin을 만든 쪽의 `plugin.json` 구성에 달려 있으므로, 설치 전에 `/plugin`에서 상세 설명을 확인하는 것이 정확합니다.

### 장단점 정리

**MCP 직접 연결**

- 장점: 구성이 가볍고 최소한, `local`/`project`/`user` 스코프로 세밀하게 범위를 제어할 수 있음, 서버 쪽 업데이트를 지연 없이 바로 받음
- 단점: 인증·헤더·환경변수를 직접 설정해야 함, 사용 가이드나 안전장치는 스스로 마련해야 함

**Plugin**

- 장점: 한 번의 설치로 MCP 연결과 사용 가이드·안전장치까지 전체 경험을 팀에 표준 배포할 수 있음, 마켓플레이스를 통해 번들 전체를 버전 관리하고 일괄 업데이트할 수 있음
- 단점: 제작자가 번들한 구성 요소를 그대로 받으므로 불필요한 것까지 딸려올 수 있음, 명령어에 네임스페이스(`/plugin이름:명령어`)가 붙음, 마켓플레이스 갱신 주기에 따라 최신 변경사항 반영이 원본 서버보다 늦어질 수 있음

### 언제 어떤 것을 쓰면 좋은가

- **개인적으로 도구 하나만 빠르게 붙이고 싶다** → MCP를 직접 연결하는 편이 간단합니다.
- **팀 전체에 표준화된 사용 경험(가이드·안전장치 포함)을 배포하고 싶다** → Plugin으로 묶어서 배포하는 편이 낫습니다.
- **같은 서비스가 MCP와 Plugin 둘 다로 제공된다면**, 최신 기능을 가장 빠르게 쓰고 싶은 개인 사용자는 MCP 직접 연결을, 온보딩 편의와 팀 내 일관성이 더 중요한 상황이라면 Plugin 쪽을 선택하는 것이 자연스럽습니다.

## 정리

Plugin은 Skills, Subagents, Hooks, MCP 서버처럼 개별로 만들던 확장 기능들을 하나의 설치 가능한 단위로 묶는 패키징 레이어입니다. `.claude-plugin/plugin.json` 매니페스트 하나와, 나머지 컴포넌트 디렉토리를 플러그인 루트에 두는 구조만 지키면 되고, `claude --plugin-dir`로 로컬에서 바로 테스트할 수 있습니다. 설치는 `/plugin marketplace add`로 마켓플레이스를 등록한 뒤 `/plugin install <이름>@<마켓플레이스>`로 진행하며, `/reload-plugins`로 재시작 없이 반영됩니다. 개인 실험 단계에서는 `.claude/` 디렉토리로 빠르게 만들고, 팀과 공유할 시점이 되면 Plugin으로 전환하는 흐름이 가장 실용적입니다.

## 참고

- [Claude Code 하네스 엔지니어링 완벽 가이드 - 09_Plugins - 확장 기능 패키징](https://wikidocs.net/365039)
