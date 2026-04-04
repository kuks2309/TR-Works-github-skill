---
name: t-works-github
version: 1.0.0
description: >
  TR-Works 팀 GitHub 협업 전문 Skill.
  다음 상황에서 자동으로 활성화됩니다:
  - 브랜치 이름, 전략, 네이밍 규칙 질문
  - 커밋 메시지 작성 도움 요청
  - PR(Pull Request) 작성, 리뷰, Merge 관련 질문
  - Issue 등록, 연동, 관리 질문
  - GitHub 조직 설정, 프로젝트 관리
  - Git 명령어 질문 (clone, push, pull, rebase 등)
  - 충돌(Conflict) 해결 요청
  - GitHub Actions, CI/CD(Continuous Integration/Continuous Deployment) 파이프라인 설정
  - Release, 버전 관리 자동화
  - 코드 리뷰 방법, CODEOWNERS 설정
  - 일정 관리, 프로젝트 보드 운영
author: TR-Works
organization: TR-Works (T-Robotics Humanoid Division)
repository: https://github.com/TR-Works/T-Works-github-skill
license: MIT
---

# TR-Works GitHub 협업 Skill

이 Skill은 TR-Works 팀원이 GitHub을 활용한 협업 전 과정을 지원합니다.
초보자부터 고급 개발자까지 수준에 맞게 안내합니다.

---

## 조직 정보

| 항목 | 내용 |
|------|------|
| 조직 | TR-Works (T-Robotics 휴머노이드 부문) |
| 팀 규모 | 5명 이내 |
| 브랜치 전략 | GitHub Flow (소규모 팀 최적) |
| 개발 환경 | Ubuntu · VS Code · Claude Code CLI(Command Line Interface) |
| 주요 언어 | C++ (ROS2 노드, 제어, 센서, 통신) · Python (AI, GUI) |
| 빌드 시스템 | colcon (ROS2) |
| 프로젝트 관리 | GitHub Projects (칸반 보드 + 일정 관리) |

---

## 수준 자동 감지

사용자의 질문 수준에 따라 응답 깊이를 조절합니다.

| 수준 | 판단 기준 | 응답 방식 |
|------|----------|----------|
| **입문** | "어떻게 해요?", Git 기초 명령어 질문 | 단계별 설명 + 예시 명령어 + 비유 |
| **중급** | rebase, Flow 전략, 자동화 질문 | 개념 + 실무 패턴 + 선택 기준 |
| **고급** | CI/CD, Security, 아키텍처 질문 | 심화 설정 + 트레이드오프 + 레퍼런스 |

---

## Level 1 — 입문: GitHub Flow 기초 협업

> 처음 GitHub을 사용하는 팀원을 위한 기초 가이드입니다.

### TR-Works 조직 연동 (최초 1회)

```
1. github.com/signup 에서 개인 계정 생성
2. Leader에게 이메일 주소 공유
3. 초대 이메일 수신 → "Join TR-Works" 클릭
4. 개인 계정으로 로그인
5. 프로젝트 Repository 접근 확인
```

### 브랜치 전략 — GitHub Flow

main 브랜치는 항상 완성된 코드만 유지합니다.
모든 작업은 feature 브랜치에서 진행하고 PR(Pull Request)로 main에 병합합니다.

```
main (완성된 코드만)
  └── feature/이름/작업내용  (작업 공간)
  └── feature/이름/작업내용  (작업 공간)
```

### TR-Works 브랜치 네이밍 규칙

```
feature/이름/작업내용

예시:
  feature/kuks2309/lidar-driver-update
  feature/kuks2309/slam-config
  feature/hong/nav2-controller
  feature/kim/gui-calibration

규칙:
  - 소문자만 사용
  - 단어 구분은 하이픈(-) 사용
  - 이름은 GitHub 계정명 또는 영문 이름 사용
```

### 기본 Git 명령어 (일일 워크플로우)

```bash
# 1. 최신 코드 받기
git pull origin main

# 2. 내 브랜치 만들기
git checkout -b feature/이름/작업내용

# 3. 작업 후 상태 확인
git status

# 4. 파일 스테이징
git add 파일명          # 특정 파일
git add .              # 전체 변경 파일

# 5. 커밋
git commit -m "feat: 변경 내용 설명"

# 6. Push
git push origin feature/이름/작업내용

# 7. 브랜치 확인
git branch             # 로컬
git branch -a          # 로컬 + 원격
```

### 커밋 메시지 규칙

```
타입: 변경 내용 (한글 가능)

타입 종류:
  feat:     새 기능 추가
  fix:      버그 수정
  docs:     문서 수정 (README 등)
  refactor: 코드 개선 (기능 변경 없음)
  test:     테스트 추가/수정
  chore:    기타 설정 변경
  build:    빌드 설정, CMakeLists.txt 등

예시:
  feat: LiDAR 듀얼 스캔 병합 노드 추가
  fix: rtabmap QoS(Quality of Service) 불일치로 인한 토픽 수신 실패 수정
  docs: README 패키지 구조 설명 추가
  refactor: launch 파일 파라미터 정리
  build: CMakeLists.txt ament_auto_package 설정 수정
```

### Pull Request 작성법

```
1. GitHub 프로젝트 Repository 접속
2. "Compare & pull request" 클릭
3. 제목 작성: "feat: LiDAR 듀얼 스캔 병합 노드 추가"
4. 설명 작성:
   ## 변경 내용
   dual_laser_merger 패키지에 2D LiDAR 병합 기능 추가

   ## 변경 이유
   전방/후방 LiDAR 데이터 통합 스캔 필요

   ## 확인 방법
   colcon build --packages-select dual_laser_merger
   ros2 launch dual_laser_merger dual_laser.launch.py

   ## 테스트 환경
   Ubuntu 22.04 / ROS2 Humble

5. Reviewers: Leader 지정
6. "Create pull request" 클릭
```

### Issue 연동 — 자동 Close

PR 설명란 맨 아래에 추가하면 Merge 시 Issue 자동 종료:

```
Closes #이슈번호

예: Closes #3
```

### 충돌(Conflict) 해결

충돌은 두 사람이 같은 파일의 같은 줄을 수정했을 때 발생합니다. 당황하지 마세요.

```
방법 1 — GitHub 웹 편집기 (초보 권장)
  PR 페이지 → "Resolve conflicts" 클릭
  → 남길 내용 선택 → "Mark as resolved" → "Commit merge"

방법 2 — VS Code
  충돌 파일 열기 → 초록/파란 버튼으로 선택
  → 저장 → git add → git commit

방법 3 — 명령어
  git pull origin main        # 최신 코드 가져오기
  # 충돌 파일 수동 편집
  git add .
  git commit -m "fix: 충돌 해결"
```

---

## Level 2 — 중급: 팀 자동화 + 프로젝트 관리

> GitHub Flow에 익숙해진 후 팀 효율을 높이고 싶을 때 적용합니다.

### 고급 Git 명령어

```bash
# 브랜치 최신화 (rebase)
git fetch origin
git rebase origin/main

# 특정 커밋만 가져오기 (cherry-pick)
git cherry-pick <commit-hash>

# 작업 임시 저장 (stash)
git stash              # 저장
git stash pop          # 복원
git stash list         # 목록

# 커밋 되돌리기
git reset --soft HEAD~1   # 커밋 취소 (변경 유지)
git reset --hard HEAD~1   # 커밋 취소 (변경 삭제)
git revert <hash>          # 되돌리기 커밋 생성

# 이력 조회
git log --oneline --graph  # 그래프 형태
git log --author="이름"    # 작성자별
```

### PR(Pull Request) 템플릿 설정

`.github/PULL_REQUEST_TEMPLATE.md` 파일 생성:

```markdown
## 변경 내용
<!-- 무엇을 변경했는지 설명 -->

## 변경 이유
<!-- 왜 이 변경이 필요한지 -->

## 테스트 방법
<!-- 빌드 명령어, launch 명령어, 확인 방법 -->

## 테스트 환경
- OS: Ubuntu 22.04
- ROS2: Humble
- 관련 하드웨어:

## 관련 Issue
Closes #
```

### GitHub Projects — 칸반 보드 + 일정 관리

```
설정 위치: github.com/TR-Works → Projects → New project

칸반 컬럼 구성:
  Backlog     → 할 일 목록
  In Progress → 진행 중
  Review      → PR 검토 대기
  Done        → 완료

자동화 설정:
  Issue 생성 → Backlog 자동 추가
  PR 오픈    → In Progress 자동 이동
  PR Merge   → Done 자동 이동

일정 관리:
  - Roadmap 뷰: 타임라인 기반 마일스톤 관리
  - Table 뷰: 담당자/우선순위/마감일 필터링
  - 마일스톤 설정: v1.0, v2.0 등 릴리즈 단위
```

### Issue 템플릿 설정

`.github/ISSUE_TEMPLATE/bug_report.md`:
```markdown
---
name: Bug Report
about: 버그 리포트
labels: bug
---

## 증상
<!-- 어떤 문제가 발생하는지 -->

## 재현 방법
1. 
2. 
3. 

## 기대 동작
<!-- 정상적으로는 어떻게 동작해야 하는지 -->

## 환경
- OS: Ubuntu 22.04
- ROS2: Humble
- 관련 패키지:

## 에러 로그
```
(에러 메시지 붙여넣기)
```
```

`.github/ISSUE_TEMPLATE/feature_request.md`:
```markdown
---
name: Feature Request
about: 새 기능 요청
labels: feature
---

## 기능 설명
<!-- 어떤 기능이 필요한지 -->

## 필요한 이유
<!-- 왜 이 기능이 필요한지 -->

## 구현 아이디어 (선택)
<!-- 구현 방법에 대한 아이디어가 있다면 -->
```

### Merge 전략 선택 가이드

```
Squash merge   - 커밋 하나로 합쳐서 병합. 이력 깔끔.
               - TR-Works 기본 권장
               - 사용: 대부분의 feature/bugfix PR

Merge commit   - 이력 전체 보존. 히스토리 복잡해짐.
               - 사용: 큰 기능, 추적 중요할 때

Rebase merge   - 직선 이력. 히스토리 깔끔.
               - 사용: 소규모 변경, 커밋이 이미 깔끔할 때
```

### ROS2 프로젝트 특화 규칙

```
빌드 확인 필수:
  - PR 제출 전 colcon build 성공 확인
  - 수정한 패키지: colcon build --packages-select <패키지명>
  - 의존 패키지 포함: colcon build --packages-up-to <패키지명>

QoS 호환성:
  - publisher QoS와 subscriber QoS 호환 확인
  - SensorDataQoS(BEST_EFFORT) ↔ RELIABLE = 수신 불가
  - launch 파일 작성 전 ros2 param list <node>로 파라미터명 확인

파일 구조:
  - UI 파일과 알고리즘 파일 분리 (같은 파일에 혼합 금지)
  - config는 src 폴더의 것을 사용
  - 절대 경로 금지, 상대 경로 또는 패키지 자동 탐색 사용
```

---

## Level 3 — 고급: CI/CD(Continuous Integration/Continuous Deployment) 자동화

> 팀이 성숙해지고 자동화가 필요할 때 적용합니다.

### GitHub Actions — ROS2 빌드 검증

```yaml
# .github/workflows/ci.yml
name: ROS2 Build Check

on:
  pull_request:
    branches: [ main ]
  push:
    branches: [ main ]

jobs:
  build:
    runs-on: ubuntu-22.04
    container:
      image: ros:humble
    steps:
      - uses: actions/checkout@v4

      - name: Install dependencies
        run: |
          apt-get update
          rosdep update
          rosdep install --from-paths src --ignore-src -r -y

      - name: Build
        run: |
          source /opt/ros/humble/setup.bash
          colcon build --cmake-args -DCMAKE_BUILD_TYPE=Release

      - name: Test
        run: |
          source /opt/ros/humble/setup.bash
          source install/setup.bash
          colcon test
          colcon test-result --verbose
```

### GitHub Actions — Python Lint

```yaml
# .github/workflows/lint.yml
name: Python Lint

on:
  pull_request:
    paths: ['**/*.py']

jobs:
  lint:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-python@v5
        with:
          python-version: '3.10'
      - run: pip install flake8
      - run: flake8 --max-line-length=120 src/
```

### CODEOWNERS 설정

`.github/CODEOWNERS` 파일:

```
# 전체 파일 기본 리뷰어
*                        @TR-Works/core-team

# ROS2 제어/센서
src/Control/             @kuks2309
src/Sensor/              @kuks2309

# SLAM / Navigation
src/SLAM/                @kuks2309
src/Planner/             @kuks2309

# AI / GUI
src/AI/                  @kuks2309
src/GUI/                 @kuks2309

# Launch / Config
*.launch.py              @kuks2309
*.yaml                   @kuks2309
```

### Release 자동화

```yaml
# .github/workflows/release.yml
name: Release

on:
  push:
    tags:
      - 'v*'

jobs:
  release:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4

      - name: Create Release
        uses: actions/create-release@v1
        env:
          GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
        with:
          tag_name: ${{ github.ref }}
          release_name: Release ${{ github.ref }}
          draft: false
          prerelease: false
```

```bash
# 릴리즈 태그 생성
git tag -a v1.0.0 -m "Release version 1.0.0"
git push origin v1.0.0
```

### Branch Protection 설정

```
Settings → Branches → Add ruleset → main

필수 설정:
  - Require a pull request before merging
    - Required approvals: 1 (소규모 팀)
  - Require status checks to pass (CI 통과 필수)
  - Require branches to be up to date

권장 설정:
  - Require linear history (Squash merge만 허용 시)
  - Do not allow bypassing the above settings
```

---

## TR-Works 팀 공통 규칙

> 레벨에 관계없이 모든 팀원이 따르는 규칙입니다.

### 협업 5대 원칙

```
1. main 직접 Push 금지       — 반드시 PR 사용
2. 브랜치 네이밍 규칙 준수   — feature/이름/작업내용
3. PR 승인 1명 이상 필수     — Leader 또는 시니어
4. Merge 후 브랜치 삭제      — 목록 정리
5. 커밋 메시지 접두어 사용   — feat/fix/docs/refactor/build
```

### ROS2 프로젝트 커밋 전 체크리스트

```
1. colcon build 성공 확인
2. QoS 호환성 확인 (publisher ↔ subscriber)
3. launch 파라미터 실제 노드 파라미터와 일치 확인
4. 절대 경로 사용하지 않았는지 확인
5. UI 파일과 알고리즘 파일 분리 확인
6. git add로 필요한 파일만 스테이징
```

### 자주 하는 실수 & 해결법

```
실수 1: main에 직접 Push 시도
  오류: "protected branch"
  해결: feature 브랜치 만들고 PR 사용

실수 2: Push 전 Pull 안 함
  오류: "Updates were rejected"
  해결: git pull origin main 먼저 실행

실수 3: 브랜치 안 만들고 작업
  상황: main에서 직접 파일 수정
  해결: git stash → 브랜치 생성 → git stash pop

실수 4: colcon build 안 하고 커밋
  해결: 커밋 전 반드시 colcon build 실행

실수 5: --symlink-install로 Python 패키지 빌드
  증상: PackageNotFoundError
  해결: --symlink-install 없이 colcon build
```

---

## Claude Code 활용 프롬프트 모음

```
브랜치 이름 고민:
  "LiDAR 드라이버 업데이트 작업할 건데 브랜치 이름 추천해줘"

커밋 메시지 작성:
  "launch 파일에 QoS 파라미터 추가했어. 커밋 메시지 어떻게 써?"

PR 설명 초안:
  "dual_laser_merger에 ComposableNode 지원 추가했어.
   PR 제목이랑 설명 초안 만들어줘."

충돌 해결:
  "지금 Conflict가 생겼는데 어떻게 해결해야 해?"

프로젝트 관리:
  "이번 스프린트 Issue 목록 정리해줘"
  "마일스톤 v1.0 진행 상황 확인해줘"
```

---

## 참조 문서

- [references/t-works-conventions.md](references/t-works-conventions.md) — 팀 규칙 전체
- [references/troubleshooting.md](references/troubleshooting.md) — 에러 해결 모음
