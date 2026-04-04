---
description: 버전 태그를 기반으로 릴리즈 노트와 CHANGELOG를 자동 생성합니다
argument-hint: "<버전 번호> (예: v1.2.0)"
allowed-tools: Bash(git log --oneline), Bash(git tag)
---

시맨틱 버저닝 규칙에 따라 릴리즈 노트를 생성합니다.

## 시맨틱 버저닝 규칙

```
v MAJOR . MINOR . PATCH

MAJOR — 하위 호환 불가 변경 (기존 기능 제거/변경)
MINOR — 하위 호환 새 기능 추가
PATCH — 버그 수정
```

## 실행 순서

1. 현재 버전 태그와 이전 태그 확인
2. 그 사이 커밋 로그 분석
3. 커밋 타입별 분류 (feat/fix/docs/build/etc)
4. 릴리즈 노트 생성:

```markdown
## v1.2.0 (YYYY-MM-DD)

### 새 기능
- LiDAR 듀얼 스캔 병합 노드 추가 (#12)

### 버그 수정
- rtabmap QoS 불일치 수정 (#18)

### 빌드/설정
- CMakeLists.txt ament_auto_package 설정 수정 (#20)

### 문서
- README 패키지 구조 설명 추가 (#22)
```

5. GitHub Release 생성 명령어 제공:
```bash
git tag -a v1.2.0 -m "Release v1.2.0"
git push origin v1.2.0
```
