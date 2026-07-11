---
name: codeprint-analyze
description: 코드 구조가 DDD 원칙(계층 분리·순환 의존·과도한 함수 fan-out 등)을 지키는지 무설정으로 검사한다. 커밋 전 자가 점검, 아키텍처 리뷰, "이 코드 구조 괜찮은지 봐줘" 같은 요청에 사용.
argument-hint: "[분석 대상 경로(선택, 기본 프로젝트 루트)]"
allowed-tools: Bash(${CLAUDE_PLUGIN_ROOT}/bin/codeprint-analyze *)
---

Java(Java 17+ 런타임 필요)로 빌드된 AST 기반 정적 분석 도구를 실행해 현재 프로젝트(`${CLAUDE_PROJECT_DIR}`)의 구조 위반을 탐지한다. Java/Kotlin/TypeScript/TSX를 지원한다.

## 감지 항목

- `HIGH_FAN_OUT` — 단일 함수가 과도하게 많은 함수를 호출(단일 책임 원칙 위반 가능성)
- `CROSS_DOMAIN_CALL` / `DOMAIN_IMPORTS_INFRA` — 레이어·도메인 경계 위반
- `CYCLIC_IMPORT` — 순환 의존
- `DEAD_CODE` — 미호출 함수(신뢰도 게이트 적용)
- 그 외 DDD 계층 위반 패턴 다수

## 실행

```
"${CLAUDE_PLUGIN_ROOT}/bin/codeprint-analyze" "${CLAUDE_PROJECT_DIR}"
```

결과는 stdout으로 바로 출력된다(경고 목록 + 유형별 요약) — 실행 결과를 그대로 사용자에게 요약해 전달한다.

## 주의

- 이 스킬은 요청 시 1회 실행만 수행한다 — 파일 저장 시 자동 재실행(watch 모드)은 지원하지 않는다.
- 오탐 가능성이 있는 항목은 경고 메시지에 수정 가이드가 함께 출력된다 — 그대로 지시로 받아들이지 말고 코드 맥락을 함께 확인한다.
