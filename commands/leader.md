---
description: 프로젝트 리더용 — 팀원 PR(Pull Request)/브랜치 현황 확인 후 검토·머지까지 안내합니다
allowed-tools: Bash(git *), Bash(gh *)
---

# /leader — 리더 머지 관리

`/leader`를 실행하면 자동으로 다음을 순서대로 수행합니다.

## 실행 순서

### 1. 팀 현황 파악

```bash
git fetch --all --prune
gh pr list --state open --json number,title,author,headRefName,createdAt,reviewDecision
```

열린 PR과 활성 브랜치를 한눈에 보여줍니다:

```
열린 PR:
  #12  feat: LiDAR 듀얼 스캔 병합   kuks2309    리뷰 대기
  #13  fix: QoS 불일치 수정         hong        승인됨
```

### 2. 어떤 PR을 처리할지 물어봄

PR이 여러 개면 "어떤 PR을 검토할까요?" 하고 물어봅니다.
PR이 1개면 바로 검토로 넘어갑니다.

### 3. 선택한 PR 검토

변경 파일, diff, 커밋 메시지 규칙 준수 여부를 분석하고 요약합니다.
- 문제 없으면: "머지해도 될까요?" 확인
- 문제 있으면: 수정 필요 사항 안내 + 작성자에게 코멘트 남길지 확인

### 4. 머지 실행

사용자가 승인하면:
```bash
gh pr merge <번호> --squash --delete-branch
```
- 기본은 squash merge (TR-Works 권장)
- 충돌 시 작성자에게 해결 요청 코멘트 자동 작성 제안
- 머지 후 브랜치 자동 삭제

---

## 안전 원칙

- CI(Continuous Integration) 실패 시 머지 차단
- 리뷰 없으면 경고
- 머지 전 반드시 사용자 확인
- force push 절대 금지
