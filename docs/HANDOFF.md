# 인계 메모

claude.ai 대화에서 만든 이 저장소를 Claude Code에서 이어서 작업하기 위한 메모입니다. 작업을 시작할 때 "다음 할 일"부터 확인하고, 끝날 때 이 파일을 갱신하세요. 마지막 갱신: 2026-11-26.

## 왜 만들었나

AI가 만든 코드와 답변의 AI-Slop을 줄이고 싶어서 스킬을 모으기 시작했습니다. 계기는 디시인사이드 글이었고, 요지는 이렇습니다.

- 사람이든 AI든 코드는 결국 지저분해지니, 실수를 막는 것보다 주기적으로 찾아서 치우는 체계가 중요하다.
- 좋은 스킬을 주워 와서 내 저장소에 내 것으로 고쳐 두고, 마켓플레이스로 등록해 claude.ai와 Claude Code 어디서나 쓰게 한다.
- 참고 저장소: [115dkk/agent-skills](https://github.com/115dkk/agent-skills). 이 저장소의 구조, 출처 기록 방식, 버전 워크플로는 거기서 본떴습니다.

## 지금까지 정한 것

| 결정 | 이유 |
|---|---|
| 저장소 전체를 플러그인 하나로(`source: "./"`) | 플러그인이 하나뿐이면 가장 단순함 (115dkk 방식) |
| 스킬을 예방 / 청소 두 그룹으로 나눔 | 새로 생기는 slop을 막는 것과 쌓인 slop을 치우는 것은 쓰는 시점이 다름 |
| `karpathy-guidelines` 3번 원칙에 예외 문단 추가 | "고장 안 난 건 고치지 마라"가 청소 스킬과 반대로 작동하지 않게 |
| `verification-before-completion`의 범위를 좁히고 표현을 낮춤 | 원본은 모든 만족 표현에 적용되고 표현이 매우 강해서, 일반 대화에서도 과하게 발동할 수 있음 |
| superpowers는 위 스킬 하나만 가져옴 | 전체 플러그인은 스킬 15개 + 세션 시작 훅이라 과함 |
| `dmmulroy/anti-slop`은 넣지 않음 | TS/JS용 Oxlint 규칙이고, 원작자도 `cloudflare/oxlint-config`를 먼저 권함. 사용자의 TS/JS 사용 여부를 아직 못 들음 |
| plugin.json `version`은 워크플로가 관리 | 고정 버전이면 업데이트가 설치자에게 전달되지 않음 (115dkk가 겪은 문제) |
| 커밋에 AI 흔적 금지 | 사용자의 고정 요청. `AGENTS.md` 규칙 + `.claude/settings.json`의 `attribution` 설정 |

각 스킬의 원본 커밋과 바꾼 점은 README의 "출처" 섹션에 있습니다.

## 현재 상태

- 로컬 저장소: `~/Documents/Projects/ETC/all-for-one-harness`, 브랜치 `main`
- GitHub: [ZaMan0806/all-for-one-harness](https://github.com/ZaMan0806/all-for-one-harness) (공개), `origin/main`과 같음. 첫 push는 커밋 하나(`chore: initial setup ...`)로 합쳐서 올렸습니다.
- 버전 워크플로가 첫 push에서 성공해 `plugin.json`이 `1.0.1`이 되었습니다.
- `claude plugin validate .` 통과 (아이콘 없음 경고 하나만 있음). 클라우드 환경에서 `zaman0806-skills` 설치까지 확인했습니다.

## 다음 할 일

- [x] **GitHub에 올리기.** 2026-11-24 push 완료.
- [x] **버전 워크플로 확인.** 첫 push에서 성공, `chore(version): 1.0.1` 커밋 생성.
- [ ] **claude.ai에 등록.** Customize → Plugins → Personal plugins의 + → Add marketplace → `ZaMan0806/all-for-one-harness` (사용자가 직접)
- [ ] **청소 흐름 시험.** 실제 프로젝트 하나에서 `improve-codebase-architecture` → `tech-debt-audit` → 결과를 GitHub 이슈로 → `resolve-audit` 순서로 돌려 보고, 불편한 점을 스킬에 반영.

- [ ] **리뷰 지적 반영.** (1) 가져온 스킬을 이 저장소에서 고칠 때 출처 갱신·`LICENSE` 원작자 줄 유지 규칙이 `AGENTS.md`에서 빠짐. (2) `tdd`가 청소 그룹인데 karpathy 예외 문단에 없음: 예방으로 옮길지 예외에 넣을지 결정. (3) `.claude/CLAUDE.md`가 `AGENTS.md` 규칙 두 줄을 중복.

## 열린 질문

- **TS/JS를 쓰는지:** 쓴다면 `cloudflare/oxlint-config` 또는 `anti-slop` 추가를 검토합니다.
- **글쓰기(문장) slop 스킬:** `stop-slop`을 넣었다가 pstack의 `unslop`으로 바꿨습니다(2026-11-26). 규칙이 영어 글 기준이라 한국어 글에 맞게 고칠지는 써 보고 정합니다.
- **아이콘:** validate가 `ICON_MISSING`을 경고합니다. 넣으려면 `.claude-plugin/icon.png`(정사각형, 512~2048px).

## 검토했지만 넣지 않은 후보

| 후보 | 결과 |
|---|---|
| multica-ai/andrej-karpathy-skills | 넣음 (수정) |
| dmmulroy/anti-slop | 보류 (TS/JS 전용) |
| obra/superpowers `verification-before-completion` | 넣음 (수정) |
| 115dkk/agent-skills 4개 | 넣음 (접두사만 변경) |
| hardikpandya/stop-slop | 넣었다가 `unslop`으로 교체 (절대 규칙이 많고 서식 slop을 안 다룸) |
| cursor/plugins `pstack` | `unslop`만 넣음. 나머지 스킬은 기존 것이 더 깊거나 pstack 전체 체계 안에서만 작동. `correct`, `no-comments`는 추가 후보 |
| DietrichGebert/ponytail | `ponytail-review`만 넣음 (본체는 karpathy-guidelines와, audit은 tech-debt-audit과 겹침) |
