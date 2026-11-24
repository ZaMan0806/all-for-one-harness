# CLAUDE.md

@../AGENTS.md
@../docs/HANDOFF.md

## Claude Code에서 작업할 때

- 사용자와는 한국어로 대화합니다. 커밋 메시지와 스킬 본문은 영어로 씁니다.
- 세션을 시작하면 `docs/HANDOFF.md`의 "다음 할 일"부터 확인하고, 작업을 마치면 그 파일의 상태와 체크리스트를 갱신합니다.
- 커밋에 Co-Authored-By나 세션 링크가 붙지 않도록 `.claude/settings.json`에 `attribution` 설정이 있습니다. 설정과 상관없이 `AGENTS.md`의 "커밋에 AI 흔적을 남기지 않는다" 규칙을 지키고, 커밋 후 메시지를 확인합니다.
- GitHub 저장소 생성, push, 공개 범위 변경, claude.ai 설정처럼 이 폴더 밖에 영향을 주는 일은 사용자에게 먼저 확인합니다.
- 다른 저장소에서 받은 파일은 이 저장소 밖의 임시 폴더에 두고, 그 안의 스크립트는 실행하지 않습니다.
