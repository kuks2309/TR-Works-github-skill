# TR-Works GitHub 트러블슈팅 가이드

자주 만나는 에러와 해결법 모음입니다.

---

## Git 오류 메시지별 해결법

### `error: failed to push some refs`
```
원인: 원격 저장소에 내 로컬에 없는 변경이 있음
해결:
  git pull origin 브랜치명
  # 충돌 있으면 해결 후
  git push origin 브랜치명
```

### `rejected — protected branch`
```
원인: main 브랜치에 직접 Push 시도
해결:
  git checkout -b feature/이름/작업내용
  git push origin feature/이름/작업내용
  # 이후 PR 생성
```

### `CONFLICT (content): Merge conflict in 파일명`
```
원인: 같은 파일의 같은 줄을 두 사람이 수정
해결:
  1. git status 로 충돌 파일 확인
  2. 해당 파일 열기 (<<<<<, =====, >>>>> 마커 찾기)
  3. 남길 내용 선택 후 마커 삭제
  4. git add 파일명
  5. git commit
```

### `Your branch is behind 'origin/main'`
```
원인: 원격 main이 내 브랜치보다 앞서 있음
해결:
  git fetch origin
  git rebase origin/main
```

### `fatal: not a git repository`
```
원인: Git 저장소가 아닌 폴더에서 명령 실행
해결:
  cd ~/T-Robot_nav_ros2_ws   # 올바른 폴더로 이동
```

### `Please tell me who you are` (최초 설정)
```
원인: Git 사용자 정보 미설정
해결:
  git config --global user.email "이메일@example.com"
  git config --global user.name "이름"
```

---

## GitHub 웹 오류

### PR 생성 시 "This branch has conflicts"
```
원인: 내 브랜치가 main과 충돌
해결:
  1. 로컬에서 git pull origin main
  2. 충돌 파일 수정
  3. git add . && git commit
  4. git push
  5. PR 페이지 새로고침
```

### PR 머지 버튼이 비활성화
```
원인 1: 리뷰 승인 미완료 → Reviewer에게 승인 요청
원인 2: CI 테스트 실패   → Actions 탭에서 원인 확인
원인 3: 브랜치 뒤처짐    → "Update branch" 버튼 클릭
```

### Issue가 PR Merge 후에도 안 닫힘
```
원인: PR 설명에 키워드 오타 또는 누락
해결: PR 설명 편집 → 정확한 형식 입력
  Closes #번호
  Fixes #번호
  Resolves #번호
```

---

## ROS2 + GitHub 관련

### colcon build 실패 후 커밋하려는 경우
```
원인: 빌드 실패 상태에서 코드 커밋 시도
해결:
  1. colcon build --packages-select <패키지명> 으로 에러 확인
  2. 에러 수정
  3. 빌드 성공 후 git add → git commit
```

### QoS 관련 PR 리뷰 지적
```
확인사항:
  - publisher QoS와 subscriber QoS 호환?
  - SensorDataQoS(BEST_EFFORT) pub → RELIABLE sub = 수신 불가
  - ros2 param list <node> 로 실제 파라미터명 확인
```

### launch 파일에서 절대 경로 사용
```
문제: /home/tc/... 같은 절대 경로 사용
해결:
  - get_package_share_directory() 사용
  - 또는 상대 경로 사용
  - LIO-SAM 등에서 HOME+path 시 경로 이중화 주의
```

---

## VS Code + Claude Code 관련

### Claude Code가 응답하지 않음
```
1. VS Code 재시작
2. 터미널에서: claude --version (설치 확인)
3. claude 명령 재실행
```

### `claude: command not found`
```
해결:
  npm install -g @anthropic-ai/claude-code
  # 설치 후 터미널 재시작
```

---

## 자주 묻는 질문

**Q. 실수로 잘못된 파일을 commit했어요. Push 전이에요.**
```bash
git reset --soft HEAD~1       # 커밋 취소 (변경 내용 유지)
git restore --staged 파일명   # 특정 파일 unstage
```

**Q. 이미 Push한 커밋 메시지를 바꾸고 싶어요.**
```bash
# 주의: PR이 있다면 팀원과 상의 필요
git commit --amend -m "새 메시지"
git push --force-with-lease origin 브랜치명
```

**Q. 브랜치를 잘못된 곳에서 만들었어요.**
```bash
git log --oneline             # 커밋 해시 확인
git checkout 올바른_브랜치
git cherry-pick 커밋해시
```

**Q. 실수로 민감한 정보(API 키 등)를 커밋했어요.**
```
1. 즉시 해당 키/비밀번호 무효화 (가장 중요!)
2. Leader에게 알림
3. BFG Repo Cleaner로 이력 제거 후 force push
```

**Q. --symlink-install로 빌드했는데 PackageNotFoundError 발생**
```
원인: .egg-link 생성 → importlib.metadata 인식 불가
해결: --symlink-install 없이 colcon build
```
