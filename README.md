# MARU Platform — Claude Code 플러그인 (테섭)

MARU 플랫폼 **테스트 서버**의 MCP 도구를 Claude Code 에 붙이는 플러그인 마켓플레이스다. MARU 계정으로 로그인하면 런처·업데이터 빌드와 내 조직 콘텐츠 빌드를 올리고 **test 트랙에만** 릴리스할 수 있다.
비공개 저장소라 `code-reach` 조직 접근권이 있어야 설치된다. 실섭용은 별도 저장소(`code-reach/platform.claude-plugin`)다.

## 설치

Claude Code v2.1.231 이상.

```
/plugin marketplace add code-reach/platform.claude-plugin-test
/plugin install maru-platform-test@maru-platform-test
```

설치하면 MCP 서버 `maru-platform-test` 가 자동으로 뜬다. 처음 한 번 로그인한다.

```
/mcp  →  maru-platform-test  →  Authenticate
```

브라우저에서 MARU 계정으로 로그인하고 동의하면 끝이다. 로그인은 한 번 하면 오래 유지된다(쓰는 동안 계속 연장, 최대 1년). 비대화형(`claude -p`)은 로그인 흐름을 돌릴 수 없으니 대화형 세션에서 먼저 로그인한다.

## 들어 있는 것

| 구성 | 내용 |
|---|---|
| `.mcp.json` | MCP 서버 `maru-platform-test` → `https://test-mcp.maru-platform.com/mcp` · MCP 서버 `launcher` → `launcher-relay.ps1` |
| `launcher-relay.ps1` | 이 PC 에서 실행 중인 런처의 MCP 파이프를 찾아 stdio 와 잇는다(Windows 기본 PowerShell 로 실행, 설치할 것 없음) |
| `skills/release-program` | 런처·업데이터: 빌드 → 해시 → 업로드 → 등록 → test 릴리스 → 확인 절차 |
| `skills/release-content` | 콘텐츠: 버전 생성 → 해시 → 업로드 스크립트 → 확정 → test 릴리스 → 상태 확인 절차 |
| `skills/launcher-qa` | 이 PC 의 실행 중 런처를 `launcher` MCP 도구로 확인·시험하는 요령 — 조작 순서, 저장 동작 금지, `eval_js` 로 클릭·입력·측정, 증상별 원인(옛 exe·HMR 깨짐·포커스) |

도구는 계정 권한에 따라 보이는 것이 다르다.

| 대상 | 도구 | 보이는 계정 |
|---|---|---|
| 런처·업데이터 | `list_programs` · `list_program_versions` · `prepare_program_upload` · `register_program_version` · `release_program_test` · `verify_program_test_release` · `list_updater_versions` · `prepare_updater_upload` · `register_updater_version` · `release_updater_test` | 플랫폼 관리자 |
| 콘텐츠 | `list_my_contents` · `list_content_versions` · `get_content_release_status` · `create_content_version` · `prepare_content_upload` · `complete_content_upload` · `abort_content_upload` · `release_content_test` | 벤더 조직 owner·admin, 플랫폼 관리자 |

prod 승격과 실섭 업데이터 핀은 도구가 없다 — 런처·업데이터는 admin-web, 콘텐츠는 콘솔에서 사람이 한다. 콘텐츠 노출 전환·아카이브·삭제도 같다.

## 런처 연결 (MCP 서버 `launcher`)

이 플러그인은 이 PC 에서 **실행 중인 MARU 런처**에도 붙는다. 로그인은 없다 — 같은 PC 의 같은 Windows 사용자만 붙는다. 테섭·실섭·VR 런처 중 떠 있는 것을 스스로 찾으므로, 테섭·실섭 어느 플러그인으로 붙든 같은 런처에 붙는다.

- 런처를 먼저 띄운다. 꺼져 있으면 `/mcp` 에 `launcher` 가 연결 실패로 보이고, 런처를 켠 뒤 `/mcp reconnect all` 로 붙인다. 런처가 없는 PC 에서는 늘 실패로 보이며, 다른 기능에는 영향이 없다.
- 런처가 둘 이상 떠 있으면 연결하지 않는다. 환경 변수 `MARU_LAUNCHER_PIPE` 에 `maru-launcher-test-mcp` 처럼 하나를 적는다.
- 테섭·실섭 플러그인을 둘 다 설치했다면 같은 런처에 두 번 붙어 도구가 겹쳐 보인다. 쓰지 않는 쪽의 `launcher` 는 `/mcp` 에서 끈다.
- 도구는 런처가 정한다 — 설치·실행·강제종료·화면 이동·스크린샷·이벤트·로그. 실섭 빌드 런처(prod·vr_prod)에는 테스트 보조 도구(`eval_js` 등)가 없다.
- 실행 정책을 그룹 정책으로 막은 PC 는 런처 exe 로 직접 등록한다: `claude mcp add --transport stdio maru-launcher -- "<런처 설치 폴더>\MARU.exe" --mcp`.

## 이 저장소를 고칠 때

- 스킬 원본은 여기다. 실섭 저장소의 스킬은 여기서 검증한 뒤 옮긴 사본이다. 옮기는 방법: `skills/` 를 실섭 저장소에 통째로 복사(`diff -r` 로 같은지 확인) → 비밀·IP·서버 경로 grep → 두 저장소의 `plugin.json` 버전을 같이 올린다 → `claude plugin validate --strict .`.
- 실섭 저장소는 공개다. 옮겨 갈 내용이므로 **비밀값·서버 IP·내부 호스트명·서버 경로를 넣지 않는다.** 인증 설정(`oauth`·`headers.Authorization`)도 두지 않는다 — 로그인은 서버의 OAuth 흐름이 맡는다.
- 고친 뒤 `claude plugin validate .` 로 확인한다.
- `launcher-relay.ps1` 과 `.mcp.json` 의 `launcher` 항목도 같은 방식으로 실섭 저장소에 옮긴다(두 저장소가 같아야 한다). `launcher-relay.ps1` 은 ASCII 로만 쓴다 — PowerShell 5.1 은 BOM 없는 스크립트를 시스템 코드페이지로 읽는다. 파이프 이름 목록은 런처의 `LocalDataRoot.McpPipeName` 과 맞춘다.
