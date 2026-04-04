---
description: 현재 브랜치를 원격과 안전하게 동기화합니다 (상태 확인 → 차이 분석 → 충돌 감지 → 동기화 → 리포트)
allowed-tools: Bash(git *)
---

# /git-sync — 안전한 원격 동기화

현재 브랜치를 원격 저장소와 안전하게 동기화합니다.
**절대로 데이터를 잃지 않는 것**이 최우선 원칙입니다.

## 실행 순서

### Phase 1: 현재 브랜치 상태 확인

다음 명령어들을 **병렬로** 실행하여 현재 상태를 파악합니다:

```bash
git branch --show-current
git status --short
git stash list
git log --oneline -5
```

결과를 아래 형식으로 정리합니다:

```
현재 브랜치: <branch>
로컬 변경: staged X개 / unstaged X개 / untracked X개
Stash: X개
```

### Phase 2: 원격과 차이 분석

```bash
git fetch origin
git rev-list --count HEAD..origin/<branch>
git rev-list --count origin/<branch>..HEAD
git log --oneline origin/<branch>..HEAD
git log --oneline HEAD..origin/<branch>
```

### Phase 3: 안전한 동기화 실행

#### Case A: 이미 최신 (ahead=0, behind=0)
→ 아무 작업 불필요.

#### Case B: Pull만 필요 (ahead=0, behind>0)
- 로컬 변경이 있으면 먼저 `git stash push -m "git-sync auto-stash"` 실행
- `git pull --rebase origin <branch>` 실행
- stash를 했었다면 `git stash pop` 실행

#### Case C: Push만 필요 (ahead>0, behind=0)
- `git push origin <branch>` 실행

#### Case D: 분기됨 — diverged (ahead>0, behind>0)
- 충돌 가능성을 사전 분석
- 로컬 변경이 있으면 먼저 stash
- 충돌 파일이 없으면: `git pull --rebase` → `git push`
- 충돌 파일이 있으면: `git pull` (merge 방식) → 충돌 시 사용자 판단 대기

#### Case E: 원격 브랜치 없음 (신규 브랜치)
- `git push -u origin <branch>` 실행

### Phase 4: 결과 리포트

```
═══════════════════════════════════════
  git-sync 결과
═══════════════════════════════════════
  브랜치:    <branch>
  동기화 전: [상태]
  수행 작업: [pull --rebase / push / 없음 / ...]
  동기화 후: [최신 / Push 완료 / 사용자 판단 필요]
═══════════════════════════════════════
```

## 안전 원칙

1. **fetch만 먼저** — 원격 정보를 가져오되 로컬에 영향 없음
2. **변경 보호** — 로컬 변경이 있으면 반드시 stash 후 작업
3. **충돌 시 중단** — 충돌 파일 목록과 해결 가이드만 제공
4. **force push 절대 금지** — 어떤 상황에서도 `--force` 사용 안 함
