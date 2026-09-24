# Kanban-DB

> 과제 #3에서 사용한 GitHub Kanban 보드를 기반으로 설계·구현한
> Supabase 관계형 데이터베이스입니다.

## 프로젝트 목적

과제 #3에서 사용한 GitHub Kanban 보드의 프로젝트, 팀원, 컬럼,
마일스톤, 이슈 및 체크리스트 데이터를 관계형 데이터베이스로
모델링했습니다.

이슈의 현재 위치와 과거 컬럼 이동 이력을 분리하여
Kanban Board의 현재 상태뿐만 아니라 업무 진행 과정도
확인할 수 있도록 설계했습니다.

## 기준 서비스

- GitHub Repository:
  https://github.com/ghlee050505-web/3-Team
- GitHub Project:
  https://github.com/users/ghlee050505-web/projects/5
- Database:
  Supabase `3-Team Kanban DB`

## 데이터 모델

| 영역 | 주요 테이블 | 역할 |
|---|---|---|
| 프로젝트 | `board_projects` | Kanban 프로젝트 정보 |
| 팀원 | `board_members`, `board_project_members` | 팀원 정보 및 프로젝트 참여 관계 |
| Kanban 구조 | `board_columns`, `board_project_items` | 컬럼과 이슈의 현재 위치·순서 |
| 업무 | `board_issues`, `board_milestones` | 이슈와 마일스톤 관리 |
| 담당자 | `board_issue_assignees` | 이슈와 담당자의 N:M 관계 |
| 체크리스트 | `board_checklist_items` | 이슈별 세부 작업 및 완료 상태 |
| 이동 이력 | `board_column_movements` | 이슈의 컬럼 이동 과정 기록 |

## Conceptual ERD

![Conceptual ERD](./docs/conceptual-erd.png)

프로젝트를 중심으로 팀원, Kanban 컬럼, 마일스톤 및 이슈가
연결되며, 각 이슈에는 담당자와 체크리스트를 연결할 수 있습니다.

## Detailed ERD

![Detailed ERD](./docs/erd.png)

실제 Supabase에서는 N:M 관계와 이슈의 현재 위치 및
이동 이력을 별도의 테이블로 분리하여 관리합니다.

## 핵심 설계

- `board_projects`는 Kanban 프로젝트의 기본 정보를 관리합니다.
- `board_project_members`는 프로젝트와 팀원의 관계를 관리합니다.
- `board_issues`는 실제 GitHub Issue 정보를 저장합니다.
- `board_project_items`는 각 이슈가 현재 어느 Kanban 컬럼에
  위치하는지와 해당 컬럼 내 순서를 관리합니다.
- `board_checklist_items`는 Issue의 세부 작업과 완료 여부를
  개별 데이터로 관리합니다.
- `board_column_movements`는 이슈의 현재 위치와 별도로
  과거 컬럼 이동 이력을 저장합니다.
- `board_issue_assignees`는 이슈와 담당자의 N:M 관계를
  표현할 수 있도록 설계했습니다.

## 실제 적재 데이터

- 프로젝트: 1개
- 팀원: 3명
- Kanban 컬럼: 5개
- 마일스톤: 3개
- 이슈: 9개
- 보드 아이템: 9개
- 체크리스트: 72개
- 컬럼 이동 이력: 41개

과제 #3에서 실제 사용했던 GitHub Kanban Board의 데이터를
샘플 데이터로 활용했습니다.

현재 샘플에서는 별도의 Issue 담당자 데이터가 없어
`board_issue_assignees`는 구조만 구현되어 있습니다.

## 데이터 흐름

Issue가 생성되면 `board_issues`에 업무 정보가 저장되고,
`board_project_items`를 통해 Kanban 컬럼에 배치됩니다.

Issue 내부의 세부 작업은 `board_checklist_items`에 저장되며,
컬럼이 변경될 경우 해당 과정은 `board_column_movements`에
이력으로 기록됩니다.

## 참고 자료

- GitHub Repository:
  https://github.com/ghlee050505-web/Kanban-DB
- Supabase:
  3-Team Kanban DB
