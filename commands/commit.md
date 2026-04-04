---
description: 스테이징된 변경 내용을 분석해서 TR-Works 커밋 메시지를 자동 생성합니다
argument-hint: "[추가 설명 (선택)]"
allowed-tools: Bash(git diff --staged), Bash(git status)
---

스테이징된 변경 파일과 diff를 확인하고 TR-Works 팀 규칙에 맞는 커밋 메시지를 생성합니다.

## 규칙

커밋 메시지 형식: `타입: 변경 내용 설명`

타입 선택 기준:
- `feat:` — 새로운 기능, 설정값 추가, 새 노드/패키지
- `fix:` — 버그 수정, QoS 불일치 수정, 오류 해결
- `docs:` — README, 주석, 문서 수정
- `refactor:` — 코드 구조 개선 (기능 변화 없음)
- `test:` — 테스트 추가/수정
- `chore:` — 기타 설정 변경
- `build:` — CMakeLists.txt, package.xml, setup.py, launch 설정

## 실행

1. `git diff --staged` 로 변경 내용 확인
2. 변경 타입과 핵심 내용 파악
3. 한국어로 간결하게 설명하는 커밋 메시지 3가지 제안
4. 가장 적합한 것을 추천하고 이유 설명

사용자가 추가 설명을 입력했다면 그 내용을 반영합니다.

## ROS2 프로젝트 특화

- 패키지명이 명확하면 커밋 메시지에 포함: `feat(dual_laser_merger): ...`
- launch/config 변경은 `build:` 또는 `feat:` 중 적절한 것 선택
- QoS 관련 수정은 `fix:` 사용
