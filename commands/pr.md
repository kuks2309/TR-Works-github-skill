---
description: 현재 브랜치의 변경 내용을 분석해서 PR(Pull Request) 제목과 설명 초안을 생성합니다
argument-hint: "[추가 설명 (선택)]"
allowed-tools: Bash(git log --oneline origin/main..HEAD), Bash(git diff origin/main...HEAD --stat)
---

현재 브랜치와 main의 차이를 분석해서 TR-Works 팀 PR 양식에 맞는 초안을 생성합니다.

## 실행 순서

1. `git log --oneline origin/main..HEAD` 로 커밋 목록 확인
2. `git diff origin/main...HEAD --stat` 로 변경 파일 목록 확인
3. 아래 형식으로 PR 초안 생성

## PR 양식

```
제목: [타입]: [핵심 변경 내용 요약]

## 변경 내용
(무엇을 변경했는지 구체적으로)

## 변경 이유
(왜 이 변경이 필요한지)

## 테스트 방법
(빌드 명령어, launch 명령어, 확인 방법)

## 테스트 환경
- OS: Ubuntu 22.04
- ROS2: Humble
- 관련 하드웨어:

## 관련 Issue
Closes #
```

## 주의사항

- 제목은 50자 이내로 간결하게
- ROS2 패키지 변경 시 빌드 명령어 포함
- Issue 번호가 있다면 반드시 `Closes #번호` 포함
- 사용자가 추가 설명을 입력했다면 반영
