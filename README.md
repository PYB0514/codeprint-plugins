# codeprint-plugins

[Codeprint](https://github.com/PYB0514/codeprint)의 아키텍처 conformance 검사 엔진을 Claude Code Skill로 감싼 마켓플레이스입니다. 코드베이스를 클론·컴파일하지 않고, 번들된 fat jar(Java 17+ 필요)로 바로 실행합니다.

## 제공 스킬

- **codeprint-explore** — 저장소 지도(파일별 역할 + 함수 목록)를 한 번에 생성해 Glob/Grep/Read 반복 탐색 대비 토큰을 절감합니다. `repoMap`/`find`/`neighbors` 3가지 모드를 지원합니다.
- **codeprint-analyze** — DDD 구조 위반(계층 경계 침범, 순환 의존, 과도한 함수 fan-out 등)을 무설정으로 탐지합니다.

두 스킬 모두 Java/Kotlin/TypeScript/TSX를 지원하며, 요청 시 1회 실행되는 정보 조회 도구입니다(파일 변경 감지·자동 재실행 같은 자동화 기능은 포함하지 않습니다).

## 설치

Claude Code에서:

```
/plugin marketplace add PYB0514/codeprint-plugins
/plugin install codeprint-conformance
```

설치 후 `Java 17+`가 로컬에 설치돼 있어야 합니다(`java -version`으로 확인).

## 사용

Claude Code 대화 중 자연어로 요청하면 됩니다.

- "이 프로젝트 구조 좀 파악해줘" → `codeprint-explore` 자동 트리거
- "이 코드 구조 DDD 원칙에 맞는지 봐줘" → `codeprint-analyze` 자동 트리거

또는 `/codeprint-explore`, `/codeprint-analyze` 슬래시 커맨드로 직접 호출할 수 있습니다.

## 라이선스

MIT — [plugin.json](plugins/codeprint-conformance/.claude-plugin/plugin.json) 참조.
