# 인계 메모

claude.ai 대화에서 만든 이 저장소를 Claude Code에서 이어서 작업하기 위한 메모입니다. 작업을 시작할 때 "다음 할 일"부터 확인하고, 끝날 때 이 파일을 갱신하세요. 마지막 갱신: 2026-11-24.

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
| `attention-span`은 복사하지 않고 `git-subdir`로 가리킴 | AGPL-3.0. 수정 없이 쓸 거라 가리키기만 하는 게 간단함 |
| `dmmulroy/anti-slop`은 넣지 않음 | TS/JS용 Oxlint 규칙이고, 원작자도 `cloudflare/oxlint-config`를 먼저 권함. 사용자의 TS/JS 사용 여부를 아직 못 들음 |
| plugin.json `version`은 워크플로가 관리 | 고정 버전이면 업데이트가 설치자에게 전달되지 않음 (115dkk가 겪은 문제) |
| 커밋에 AI 흔적 금지 | 사용자의 고정 요청. `AGENTS.md` 규칙 + `.claude/settings.json`의 `attribution` 설정 |

각 스킬의 원본 커밋과 바꾼 점은 README의 "출처" 섹션에 있습니다.

## 현재 상태

- 로컬 저장소: `~/Documents/Projects/ETC/all-for-one-harness`, 브랜치 `main`
- 아직 GitHub에 올리지 않았고 remote도 없습니다.
- `claude plugin validate .` 통과 (아이콘 없음 경고 하나만 있음). 클라우드 환경에서 `zaman0806-skills`와 `attention-span` 둘 다 설치까지 확인했습니다.

## 다음 할 일

- [ ] **GitHub에 올리기.** 공개 저장소 `ZaMan0806/all-for-one-harness`를 만들고 push. 사용자 확인 후 진행.
  `gh repo create ZaMan0806/all-for-one-harness --public --source=. --push`
- [ ] **버전 워크플로 확인.** 첫 push 뒤 Actions에서 "Plugin version"이 돌고 `chore(version): 1.0.N` 커밋이 생기는지 확인. 실패하면 저장소 Settings → Actions → General → Workflow permissions가 "Read and write"인지 확인.
- [ ] **claude.ai에 등록.** Customize → Plugins → Personal plugins의 + → Add marketplace → `ZaMan0806/all-for-one-harness` (사용자가 직접)
- [ ] **attention-span 설치 확인.** claude.ai 개인 마켓플레이스가 `git-subdir` 소스를 지원하는지는 확인하지 못했습니다. 안 되면 `alexgreensh/attention-span`을 마켓플레이스로 따로 추가하도록 README를 고칩니다.
- [ ] **청소 흐름 시험.** 실제 프로젝트 하나에서 `improve-codebase-architecture` → `tech-debt-audit` → 결과를 GitHub 이슈로 → `resolve-audit` 순서로 돌려 보고, 불편한 점을 스킬에 반영.

## 열린 질문

- **TS/JS를 쓰는지:** 쓴다면 `cloudflare/oxlint-config` 또는 `anti-slop` 추가를 검토합니다.
- **글쓰기(문장) slop 스킬:** 지금은 코드 쪽과 답변 길이(attention-span)만 있습니다. "결론적으로~", 과한 불릿 같은 문장 slop을 다루는 스킬 후보를 아직 찾지 않았습니다.
- **아이콘:** validate가 `ICON_MISSING`을 경고합니다. 넣으려면 `.claude-plugin/icon.png`(정사각형, 512~2048px).

## 검토했지만 넣지 않은 후보

| 후보 | 결과 |
|---|---|
| alexgreensh/attention-span | 가리키기만 함 (AGPL) |
| multica-ai/andrej-karpathy-skills | 넣음 (수정) |
| dmmulroy/anti-slop | 보류 (TS/JS 전용) |
| obra/superpowers `verification-before-completion` | 넣음 (수정) |
| 115dkk/agent-skills 4개 | 넣음 (접두사만 변경) |
