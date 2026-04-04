# T-Works GitHub Skill

TR-Works (T-Robotics 휴머노이드 부문) 팀 GitHub 협업을 위한 Claude Code CLI Skill입니다.
초보자부터 고급 개발자까지 수준에 맞는 GitHub 협업 가이드를 제공합니다.

## 특징

- **단일 Skill, 전 레벨 지원** — 입문(Lv1) / 중급(Lv2) / 고급(Lv3) 모두 포함
- **TR-Works 특화** — ROS2 프로젝트 규칙, QoS 호환성 체크, colcon 빌드 검증
- **슬래시 커맨드 7개** — `/commit`, `/pr`, `/branch`, `/review`, `/release`, `/git-sync`, `/leader`
- **트러블슈팅** — Git + ROS2 + Claude Code 에러 해결법 모음
- **프로젝트 관리** — GitHub Projects 칸반 보드 + 일정 관리 가이드

## 설치

```bash
# GitHub에서 직접 설치
/plugin install T-Works-github-skill

# 또는 로컬 클론 후 설치
git clone https://github.com/TR-Works/T-Works-github-skill.git
/plugin install ./T-Works-github-skill
```

## 슬래시 커맨드

| 커맨드 | 기능 | 대상 |
|--------|------|------|
| `/commit` | 커밋 메시지 자동 생성 (ROS2 패키지명 포함) | 전체 |
| `/pr` | PR 제목·설명 초안 생성 (빌드/테스트 방법 포함) | 전체 |
| `/branch` | 브랜치 생성·전환·목록·삭제·rename | 전체 |
| `/review` | 코드 리뷰 체크리스트 (ROS2 QoS 검증 포함) | 전체 |
| `/release [버전]` | 릴리즈 노트 생성 | 고급 |
| `/git-sync` | 원격과 안전한 동기화 (pull/push/merge) | 전체 |
| `/leader` | 팀원 PR 현황 확인·검토·머지 | 리더 |

## 구조

```
T-Works-github-skill/
├── SKILL.md                          # 메인 Skill 정의 (Lv1~Lv3 전체)
├── references/
│   ├── t-works-conventions.md        # 팀 공통 규칙
│   └── troubleshooting.md            # 에러 해결 모음
├── commands/
│   ├── commit.md                     # /commit 커맨드
│   ├── pr.md                         # /pr 커맨드
│   ├── branch.md                     # /branch 커맨드
│   ├── review.md                     # /review 커맨드
│   ├── release.md                    # /release 커맨드
│   ├── git-sync.md                   # /git-sync 커맨드
│   └── leader.md                     # /leader 커맨드
├── docs/
│   └── README.md
├── .claude-plugin/
│   └── plugin.json
├── CHANGELOG.md
└── README.md
```

## 팀 정보

| 항목 | 내용 |
|------|------|
| 조직 | TR-Works (T-Robotics 휴머노이드 부문) |
| 팀 규모 | 5명 이내 |
| 브랜치 전략 | GitHub Flow |
| 개발 환경 | Ubuntu · VS Code · Claude Code CLI |
| 주요 언어 | C++ (ROS2) · Python (AI/GUI) |
| 빌드 시스템 | colcon (ROS2 Humble) |
