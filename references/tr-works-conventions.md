# TR-Works 팀 GitHub 협업 규칙

> 모든 팀원이 따르는 공식 규칙입니다.

---

## 브랜치 규칙

### 네이밍 형식
```
feature/이름/작업내용    — 새 기능 개발
bugfix/이름/수정내용     — 버그 수정
hotfix/이름/긴급수정     — 운영 중 긴급 패치
docs/이름/문서내용       — 문서 작업만
```

### 규칙
- 소문자만 사용
- 단어 구분은 하이픈(-), 언더스코어(_) 사용 금지
- 이름은 GitHub 계정명 기준
- Merge 후 브랜치 즉시 삭제

### 보호 브랜치
- `main`: 직접 Push 금지, PR + 1명 이상 승인 필수

---

## 커밋 메시지 규칙

### 형식
```
타입: 변경 내용 설명 (한글 가능, 50자 이내)
타입(패키지명): 변경 내용 설명 (패키지 명시 시)
```

### 타입 정의
| 타입 | 사용 시점 |
|------|----------|
| `feat:` | 새 기능, 새 노드/패키지 추가 |
| `fix:` | 버그, QoS(Quality of Service) 불일치, 오류 수정 |
| `docs:` | README, 주석, 문서 수정 |
| `refactor:` | 코드 구조 개선 (기능 변화 없음) |
| `test:` | 테스트 추가/수정 |
| `chore:` | 기타 설정 변경 |
| `build:` | CMakeLists.txt, package.xml, launch 설정 |
| `style:` | 포맷, 공백 등 |

### 좋은 예 / 나쁜 예
```
feat: LiDAR 듀얼 스캔 병합 노드 추가
feat(dual_laser_merger): ComposableNode 지원 추가
fix: rtabmap QoS 불일치로 인한 토픽 수신 실패 수정
build: CMakeLists.txt ament_auto_package 설정 수정
docs: README 패키지 구조 설명 추가

(나쁜 예)
수정
fix
업데이트함
```

---

## PR(Pull Request) 규칙

1. 모든 코드 변경은 PR을 통해서만 main 병합
2. 최소 1명의 승인 필수 (Leader 또는 시니어)
3. PR 제목은 커밋 메시지 형식 따름
4. 하나의 PR은 하나의 기능/수정에 집중
5. WIP(Work In Progress) PR(Pull Request)은 제목에 `[WIP]` 접두어
6. colcon build 성공 확인 필수

---

## Issue 관리 규칙

### 라벨 정의
| 라벨 | 의미 |
|------|------|
| `bug` | 버그 리포트 |
| `feature` | 새 기능 요청 |
| `docs` | 문서 개선 |
| `question` | 질문/논의 |
| `urgent` | 긴급 처리 |
| `ros2` | ROS2 관련 |
| `hardware` | 하드웨어 관련 |

---

## ROS2 프로젝트 규칙

### 빌드
- PR 제출 전 `colcon build` 성공 필수
- 수정 패키지: `colcon build --packages-select <패키지명>`
- Python 패키지: `--symlink-install` 없이 빌드

### QoS
- publisher QoS와 subscriber QoS 호환 확인
- SensorDataQoS(BEST_EFFORT) ↔ RELIABLE = 수신 불가
- launch 파일 작성 전 `ros2 param list <node>`로 파라미터명 확인

### 코드 구조
- C++: ROS2 노드, 제어, 센서, 통신
- Python: AI, GUI
- UI 파일과 알고리즘 파일 분리 (혼합 금지)
- 절대 경로 금지, 상대 경로 또는 패키지 자동 탐색 사용

---

## 버전 관리 정책

```
v MAJOR . MINOR . PATCH

MAJOR — 하위 호환 불가 변경
MINOR — 새 기능 추가 (하위 호환)
PATCH — 버그 수정
```
