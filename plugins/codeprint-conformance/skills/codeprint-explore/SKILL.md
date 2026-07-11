---
name: codeprint-explore
description: 코드베이스 탐색 시 Glob/Grep/Read를 반복하는 대신 저장소 지도(파일별 역할 주석 + 함수 목록)를 한 번에 얻어 토큰을 절감한다. 새 프로젝트 구조 파악, 특정 함수/파일 위치 검색, 호출관계(이웃) 확인에 사용.
argument-hint: "[repoMap|find|neighbors] [검색어(선택)]"
allowed-tools: Bash(${CLAUDE_PLUGIN_ROOT}/bin/codeprint-explore *) Read
---

Java(Java 17+ 런타임 필요)로 빌드된 AST 기반 정적 분석 도구를 실행해 현재 프로젝트(`${CLAUDE_PROJECT_DIR}`)의 구조를 요약한다. Java/Kotlin/TypeScript/TSX를 지원한다.

## 사용법

세 가지 모드를 지원한다.

1. **repoMap**(기본) — 프로젝트 전체 구조를 파일별 역할 주석 + 함수 목록 트리로 출력. 새 코드베이스를 처음 파악할 때 Glob으로 파일을 나열하고 각 파일을 Read로 열어보는 대신 이걸 먼저 실행한다.
2. **find** — 이름/경로로 노드(파일·함수) 검색.
3. **neighbors** — 특정 함수/파일의 호출 관계(인바운드·아웃바운드) 조회.

## 실행

```
"${CLAUDE_PLUGIN_ROOT}/bin/codeprint-explore" <mode> "<검색어>" "${CLAUDE_PROJECT_DIR}"
```

- `<mode>`: `repoMap`(기본값, 검색어 생략 가능) | `find` | `neighbors`
- `<검색어>`: find/neighbors 모드에서 이름 또는 경로 일부. repoMap에서는 빈 문자열(`""`).

결과는 콘솔이 아니라 `${CLAUDE_PROJECT_DIR}/build/codeprint-local/`(repoMap.md 또는 find.json/neighbors.json) 파일로 저장된다 — 실행 직후 Read 도구로 해당 파일을 열어 사용자에게 요약해 전달한다.

## 주의

- 이 스킬은 정보 조회만 수행한다(요청 시 1회 실행) — 파일 변경 감지·자동 재실행 같은 자동화는 지원하지 않는다.
- 대규모 저장소는 최대 500파일까지만 스캔한다(절단 시 안내 출력).
