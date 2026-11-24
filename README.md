# all-for-one-harness

AI가 만든 코드와 답변에서 AI-Slop을 줄이기 위한 스킬 모음입니다. Claude Code 플러그인 마켓플레이스 형식이라, 저장소를 한 번 등록하면 claude.ai, 데스크톱 앱, Claude Code에서 같이 쓸 수 있습니다.

## 들어 있는 것

### 예방: 새로 만들 때 slop이 안 생기게

| 스킬 | 하는 일 | 작동 |
|---|---|---|
| `karpathy-guidelines` | 요청 안 한 기능, 쓸데없는 추상화, 요청 범위 밖 수정을 막고 가정을 먼저 말하게 합니다. | 코드 작성·리뷰·리팩터링 때 자동 |
| `verification-before-completion` | 테스트·빌드를 실제로 돌려 보기 전에는 "완료", "통과"라고 말하지 못하게 합니다. | 완료를 주장하기 직전 자동 |

### 청소: 이미 쌓인 slop을 찾아서 치우기

| 스킬 | 하는 일 |
|---|---|
| `improve-codebase-architecture` | 중복 구현과 얕은 모듈을 찾아 더 작은 인터페이스 뒤로 묶을 후보(deepening)를 도식과 함께 보고합니다. 결과: `ARCHITECTURE_AUDIT.md` |
| `tech-debt-audit` | 저장소 전체를 9개 관점(구조 부패, 일관성, 타입, 테스트, 의존성, 성능, 오류 처리, 보안, 문서)으로 감사합니다. 모든 발견에 `파일:줄`, 심각도, 수고가 붙습니다. 결과: `TECH_DEBT_AUDIT.md` |
| `resolve-audit` | 위 두 감사 결과(GitHub 이슈)를 PR 하나로 해결합니다. 발견 하나에 커밋 하나, 커밋 메시지에 감사 ID가 들어갑니다. |
| `tdd` | `resolve-audit`에서 동작이 바뀌는 수정이 나오면 테스트부터 쓰게 합니다(red → green). |

### 말투 (선택)

| 플러그인 | 하는 일 |
|---|---|
| `attention-span` | 답변을 짧고 훑어보기 좋게 바꾸는 스타일. `/attention-span:spartan`, `rundown`, `attention-kind`, `tldr`. 직접 불러야만 작동합니다. |

`attention-span`은 AGPL-3.0이라 복사하지 않고 원본 저장소를 가리키기만 합니다(`marketplace.json`의 `git-subdir`). 원본이 업데이트되면 그대로 따라갑니다.

## 설치

**claude.ai / 데스크톱 앱**: Customize → Plugins → Personal plugins의 **+** → **Add marketplace** → `ZaMan0806/all-for-one-harness` 입력 → 원하는 플러그인 설치

**Claude Code**:

```
/plugin marketplace add ZaMan0806/all-for-one-harness
/plugin install zaman0806-skills@zaman0806
/plugin install attention-span@zaman0806
```

같은 계정으로 로그인했다면 claude.ai에서 켠 플러그인은 Claude Code에도 들어옵니다.

설치 후 스킬 이름은 `/zaman0806-skills:tech-debt-audit`처럼 플러그인 이름이 앞에 붙습니다.

> **주의:** `115dkk/agent-skills`나 `obra/superpowers` 마켓플레이스를 이미 설치했다면 같은 스킬이 두 번 들어옵니다. 이 저장소로 옮겼다면 원본 쪽은 꺼 두세요.

## 쓰는 법

예방 스킬은 따로 부를 필요 없이 코드 작업 중에 자동으로 켜집니다.

청소는 이 순서로 돌립니다.

1. "이 저장소 아키텍처 리뷰해 줘" → `improve-codebase-architecture`
2. "기술 부채 감사해 줘" → `tech-debt-audit`
3. 두 결과를 GitHub 이슈 하나로 올린 뒤 "이슈 #N 감사 결과 해결해 줘" → `resolve-audit` (필요하면 `tdd`를 알아서 씁니다)

## 업데이트

`skills/`나 `.claude-plugin/`을 바꾸는 push마다 [버전 워크플로](.github/workflows/plugin-version.yml)가 `plugin.json`의 `version`을 `1.0.N`으로 올립니다. Claude Code는 이 문자열이 바뀌어야 새 버전을 받아 가므로, 버전은 손으로 고치지 않습니다.

- claude.ai에서 추가한 마켓플레이스: 자동 업데이트가 기본으로 켜져 있습니다.
- `/plugin marketplace add`로 추가한 경우: `/plugin` → Marketplaces → `zaman0806` → Enable auto-update, 또는 `claude plugin update zaman0806-skills@zaman0806`.

## 출처

다른 저장소에서 가져온 스킬은 아래에 원본, 가져온 커밋, 바꾼 점을 적습니다. 원본을 다시 반영할 때는 원본의 diff를 읽고 필요한 변경만 옮긴 뒤 여기 커밋을 갱신합니다.

### karpathy-guidelines

[multica-ai/andrej-karpathy-skills](https://github.com/multica-ai/andrej-karpathy-skills) (MIT, 작성자 forrestchang), 커밋 [`2c60614`](https://github.com/multica-ai/andrej-karpathy-skills/tree/2c606141936f1eeef17fa3043a72095b4765b9c2) (2026-04-20).

바꾼 점:

- **요청된 정리에 대한 예외.** 3번 "Surgical Changes"(고장 안 난 건 리팩터링하지 말고, 기존 죽은 코드는 지우지 말 것)는 사용자가 명시적으로 요청한 정리·리팩터링·감사 해결 범위에는 적용하지 않는다는 문단을 추가했습니다. 이 저장소의 청소 스킬들과 반대로 작동하지 않게 하기 위해서입니다.

### verification-before-completion

[obra/superpowers](https://github.com/obra/superpowers) (MIT, Jesse Vincent), 커밋 [`22f0b46`](https://github.com/obra/superpowers/tree/22f0b463e7a5e579904dd6211a520321035526f7/skills/verification-before-completion) (2026-11-03). 이 스킬만 가져왔고 superpowers의 다른 스킬과 세션 시작 훅은 가져오지 않았습니다.

바꾼 점:

- **범위를 좁혔습니다.** description과 Scope를 "코드·작업이 끝났다, 고쳤다, 통과했다는 주장"으로 한정하고, 작업 상태를 주장하지 않는 일반 대화에는 적용하지 않는다고 명시했습니다. 원본은 모든 만족 표현과 긍정적 발언에 적용됩니다.
- **강한 표현을 낮췄습니다.** "Iron Law", "Skip any step = lying", 대문자 ALWAYS/ANY 강조, "Violating the letter..." 문단을 평이한 문장으로 바꿨습니다. 지시를 잘 따르는 모델에서 과하게 발동되는 것을 줄이기 위해서입니다. "I'm tired"류 항목도 뺐습니다.
- **검증할 수 없는 경우를 추가했습니다.** 확인을 돌릴 수 없으면 그렇다고 말하고 주장을 "미확인"으로 보고하게 했습니다.
- 표와 예시의 내용(무엇이 증거이고 무엇이 아닌지)은 원본 그대로입니다.

### improve-codebase-architecture, tech-debt-audit, tdd, resolve-audit

[115dkk/agent-skills](https://github.com/115dkk/agent-skills) (MIT), 커밋 [`d986564`](https://github.com/115dkk/agent-skills/tree/d9865649e54fc0739baae488c1e3f0383d93a2e6) (2026-10-24). 115dkk 버전은 다시 다음을 고친 것입니다.

- `improve-codebase-architecture`, `tdd`: [mattpocock/skills](https://github.com/mattpocock/skills) (MIT)
- `tech-debt-audit`: [ksimback/tech-debt-skill](https://github.com/ksimback/tech-debt-skill) (MIT)
- `resolve-audit`: 115dkk 자체 스킬

원본 대비 115dkk의 변경 내역은 [그 저장소 README의 "Where it comes from"](https://github.com/115dkk/agent-skills/tree/d9865649e54fc0739baae488c1e3f0383d93a2e6#where-it-comes-from)에 있습니다.

이 저장소에서 바꾼 점:

- 스킬끼리 부르는 이름의 접두사 `/115dkk-skills:`를 `/zaman0806-skills:`로 바꿨습니다(`resolve-audit`의 SKILL.md와 `references/classification-guide.md`). 그 외 내용은 그대로입니다.
- 115dkk 플러그인 아이콘(`assets/`)과 Codex용 매니페스트는 가져오지 않았습니다.

### attention-span

[alexgreensh/attention-span](https://github.com/alexgreensh/attention-span) (AGPL-3.0). 복사하지 않고 `marketplace.json`에서 `plugins/attention-span` 폴더를 가리킵니다. 확인한 시점의 커밋은 [`39c6b46`](https://github.com/alexgreensh/attention-span/tree/39c6b46b24dddecc4cef4e3ab8dc36d3fbffeb42) (2026-10-30)입니다. 고쳐 쓰고 싶어지면 복사해 오되, 그 폴더는 AGPL-3.0으로 공개하고 원본 LICENSE를 같이 둬야 합니다.

### 넣지 않은 것

- [dmmulroy/anti-slop](https://github.com/dmmulroy/anti-slop): TypeScript/JavaScript용 Oxlint 규칙입니다. 원작자도 [cloudflare/oxlint-config](https://github.com/cloudflare/oxlint-config)를 먼저 권합니다. TS/JS 프로젝트에서 필요해지면 그때 추가합니다.

## 라이선스

이 저장소의 파일은 MIT입니다. [LICENSE](LICENSE)에 원작자 저작권 줄을 모두 남겼습니다. `attention-span`은 이 저장소에 포함되지 않으며 원본의 AGPL-3.0을 따릅니다.
