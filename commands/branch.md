---
description: 브랜치 생성, 전환, 목록 조회, 삭제를 TR-Works 네이밍 규칙에 맞게 수행합니다
argument-hint: "[create|switch|list|delete|rename] [인자...]"
allowed-tools: Bash(git *)
---

# /branch — 브랜치 관리

TR-Works 네이밍 규칙을 기반으로 브랜치를 관리합니다.

## TR-Works 네이밍 규칙

```
feature/이름/작업내용    — 새 기능 개발
bugfix/이름/수정내용     — 버그 수정
hotfix/이름/긴급수정     — 운영 중 긴급 패치
docs/이름/문서내용       — 문서 작업만
```

**규칙:**
- 소문자만 사용
- 단어 구분은 하이픈(`-`)
- 이름은 GitHub 계정명 또는 영문 이름

---

## 서브 커맨드

### `/branch create <이름> <작업 내용>`
새 브랜치를 네이밍 규칙에 맞게 생성하고 전환합니다.

1. 작업 내용을 분석하여 적절한 접두어 선택 (feature/bugfix/hotfix/docs)
2. 영문 소문자 + 하이픈 형식으로 변환
3. 3가지 후보를 제안하고 추천 이유 설명
4. 사용자가 선택하면 실행:
   ```bash
   git switch -c feature/이름/작업내용
   ```

### `/branch switch <브랜치명>`
기존 브랜치로 전환합니다.

1. 로컬 변경 사항이 있는지 먼저 확인:
   ```bash
   git status --short
   ```
2. 변경 사항이 있으면 사용자에게 알림:
   - stash 할지, commit 할지, 그대로 전환할지 선택
   - stash 선택 시: `git stash push -m "branch-switch auto-stash"`
3. 브랜치 전환 실행:
   ```bash
   git switch <브랜치명>
   ```
4. 원격에만 있는 브랜치인 경우 자동으로 추적 설정:
   ```bash
   git switch --track origin/<브랜치명>
   ```

### `/branch list`
로컬 및 원격 브랜치 목록을 정리해서 보여줍니다.

```bash
git branch -vv
git branch -r
git log --oneline --graph --all -15
```

### `/branch delete <브랜치명>`
브랜치를 안전하게 삭제합니다.

1. 현재 브랜치는 삭제 불가 — 먼저 다른 브랜치로 전환 안내
2. merge되지 않은 브랜치는 경고 출력 후 사용자 확인 대기
3. 로컬 삭제:
   ```bash
   git branch -d <브랜치명>        # merge된 경우
   git branch -D <브랜치명>        # 사용자가 강제 삭제 확인 시
   ```
4. 원격도 삭제할지 사용자에게 확인 후:
   ```bash
   git push origin --delete <브랜치명>
   ```

### `/branch rename <새이름>`
현재 브랜치의 이름을 변경합니다.

1. 네이밍 규칙에 맞는지 검증
2. 로컬 이름 변경:
   ```bash
   git branch -m <새이름>
   ```
3. 원격에 이미 push된 브랜치라면 안내:
   ```bash
   git push origin -u <새이름>
   git push origin --delete <기존이름>
   ```

---

## 인자 없이 `/branch` 만 실행한 경우

1. 현재 브랜치 상태를 먼저 보여줌 (`/branch list` 축약)
2. 무엇을 하고 싶은지 물어봄

---

## 안전 원칙

1. **변경 보호** — 전환 전 로컬 변경 사항 감지 및 stash 제안
2. **삭제 확인** — merge 안 된 브랜치 삭제 시 반드시 사용자 확인
3. **네이밍 검증** — 규칙에 맞지 않는 이름은 수정 제안
4. **main 보호** — main 브랜치 삭제/rename 차단
