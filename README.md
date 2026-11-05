# MARU Platform (테스트 서버) — Claude Code · Codex 플러그인

MARU 플랫폼 **테스트 서버**에 빌드를 올리고 테스트 트랙에 릴리스하는 플러그인입니다. 개발과 검증에 쓰는 용도이며, 실제 서비스에는 [MARU Platform 플러그인](https://github.com/code-reach/platform.claude-plugin)을 써 주세요.

테스트 서버의 계정은 실제 서비스 계정과 별개입니다. 테스트 서버에 있는 MARU 계정으로 로그인합니다.

## 설치

### Claude Code

Claude Code v2.1.231 이상이 필요합니다.

```
/plugin marketplace add code-reach/platform.claude-plugin-test
/plugin install maru-platform-test@maru-platform-test
```

설치한 뒤 한 번 로그인합니다.

```
/mcp  →  maru-platform-test  →  Authenticate
```

브라우저에서 MARU 계정으로 로그인하고 동의하면 됩니다. 로그인은 대화형 세션에서 해 주세요 — `claude -p` 에서는 로그인 창을 띄울 수 없습니다.

### Codex

Codex CLI 0.166.0 기준 설치 명령입니다.

```sh
codex plugin marketplace add code-reach/platform.claude-plugin-test
codex plugin add maru-platform-test@maru-platform-test
codex mcp login maru-platform-test
```

브라우저에서 MARU 계정으로 로그인하고 동의한 뒤 새 Codex 세션을 엽니다. Codex 앱·IDE에서는 해당 클라이언트의 MCP Authenticate 기능을 씁니다. `/mcp`로 연결 상태를 확인할 수 있습니다.

예전에 같은 이름을 `codex mcp add maru-platform-test`로 수동 등록했다면 수동 서버가 플러그인보다 우선합니다. 설정을 확인·백업한 뒤 `codex mcp remove maru-platform-test`로 그 등록만 지우고 새 세션을 여세요. 다른 MCP 설정이나 인증 파일은 지우지 마세요.

## 할 수 있는 것

- 런처·업데이터 빌드를 올리고 테스트 트랙에 릴리스
- 내 조직 콘텐츠의 새 버전을 만들고, 빌드를 올리고, 테스트 트랙에 릴리스
- 버전 목록과 릴리스 상태 확인

Claude 또는 Codex에게 "새 빌드 올려서 테스트 트랙에 릴리스해 줘"처럼 말하면 플러그인에 든 절차를 따라 진행합니다. 쓸 수 있는 기능은 계정 권한에 따라 다르고, 권한이 없으면 로그인한 뒤에도 기능이 보이지 않습니다.

## 이 PC 의 런처와 연결

MARU 런처가 이 PC에서 실행 중이면 Claude 또는 Codex가 그 런처를 직접 다룰 수 있습니다. 콘텐츠를 설치하거나 실행해 보고, 화면을 캡처하고, 로그를 읽는 식입니다. 따로 로그인할 필요는 없습니다.

- 설치한 MARU 런처를 먼저 실행해 주세요. 런처가 꺼져 있으면 `launcher`가 연결 실패로 표시됩니다. 런처를 켠 뒤 Claude Code는 `/mcp reconnect all`을 실행하고, Codex는 새 세션을 열어 다시 연결합니다.
- 런처가 없는 PC 에서는 `launcher` 가 늘 연결 실패로 표시되지만, 다른 기능은 그대로 쓸 수 있습니다.
- 런처가 두 개 이상 실행 중이면 연결하지 않습니다. 하나만 남기거나, 환경 변수 `MARU_LAUNCHER_PIPE`에 `maru-launcher-test-mcp`처럼 연결할 런처를 적어 주세요.
- 이 플러그인과 실제 서비스용 플러그인을 함께 설치하면 `launcher` 도구가 겹칠 수 있습니다. 클라이언트의 MCP 설정에서 쓰지 않는 쪽을 꺼 주세요.
- PowerShell 실행이 그룹 정책으로 막힌 PC에서는 런처 실행 파일로 직접 등록합니다: `claude mcp add --transport stdio maru-launcher -- "<런처 설치 폴더>\MARU.exe" --mcp`
- Claude 또는 Codex에게 "런처에서 이 화면 확인해 줘"처럼 말하면 플러그인에 든 확인 요령을 따라 런처를 살펴봅니다. 저장하거나 지우는 동작은 요청하지 않는 한 누르지 않고, 확인이 끝나면 화면을 원래대로 돌려놓습니다.

## 알아 둘 것

- 테스트 트랙에 릴리스한 빌드는 테스트 서버에서 테스트 트랙을 쓰는 기기에 전달됩니다.
- 정식 릴리스, 공개·숨김 전환, 삭제는 이 플러그인으로 할 수 없습니다. 관리 화면에서 해 주세요.
