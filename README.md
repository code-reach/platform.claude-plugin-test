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
| `.mcp.json` | MCP 서버 `maru-platform-test` → `https://test-mcp.maru-platform.com/mcp` |
| `skills/release-program` | 런처·업데이터: 빌드 → 해시 → 업로드 → 등록 → test 릴리스 → 확인 절차 |
| `skills/release-content` | 콘텐츠: 버전 생성 → 해시 → 업로드 스크립트 → 확정 → test 릴리스 → 상태 확인 절차 |

도구는 계정 권한에 따라 보이는 것이 다르다.

| 대상 | 도구 | 보이는 계정 |
|---|---|---|
| 런처·업데이터 | `list_programs` · `list_program_versions` · `prepare_program_upload` · `register_program_version` · `release_program_test` · `verify_program_test_release` · `list_updater_versions` · `prepare_updater_upload` · `register_updater_version` · `release_updater_test` | 플랫폼 관리자 |
| 콘텐츠 | `list_my_contents` · `list_content_versions` · `get_content_release_status` · `create_content_version` · `prepare_content_upload` · `complete_content_upload` · `abort_content_upload` · `release_content_test` | 벤더 조직 owner·admin, 플랫폼 관리자 |

prod 승격과 실섭 업데이터 핀은 도구가 없다 — 런처·업데이터는 admin-web, 콘텐츠는 콘솔에서 사람이 한다. 콘텐츠 노출 전환·아카이브·삭제도 같다.

## 이 저장소를 고칠 때

- 스킬 원본은 여기다. 실섭 저장소의 스킬은 여기서 검증한 뒤 옮긴 사본이다. 옮기는 방법: `skills/` 를 실섭 저장소에 통째로 복사(`diff -r` 로 같은지 확인) → 비밀·IP·서버 경로 grep → 두 저장소의 `plugin.json` 버전을 같이 올린다 → `claude plugin validate --strict .`.
- 실섭 저장소는 공개다. 옮겨 갈 내용이므로 **비밀값·서버 IP·내부 호스트명·서버 경로를 넣지 않는다.** 인증 설정(`oauth`·`headers.Authorization`)도 두지 않는다 — 로그인은 서버의 OAuth 흐름이 맡는다.
- 고친 뒤 `claude plugin validate .` 로 확인한다.
