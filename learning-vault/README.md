# Learning Vault

이 저장소는 학습 자료를 많이 모으는 창고가 아니라, **다시 찾을 수 있는 지식·프로젝트 판단·이해도 검증 기록**을 관리하는 개인 학습 저장소입니다.

## 포함된 정리 문서

- `10_CONCEPTS/01_python_quiz_game_core.md`  
  Python 기초, 클래스·객체, JSON, 예외 처리, 퀴즈 게임 전체 실행 흐름

- `20_PROJECTS/python-quiz-game/02_git_mission2_learning_log.md`  
  Git 명령, 18개 커밋 계획, 미션 2 검증표, 인터뷰 질문, 다음 학습 단계

두 문서는 업로드된 메모 두 개를 바탕으로 중복, UI 흔적, 끊어진 문장을 정리하고 잘못되거나 모호한 설명을 보강한 결과입니다.

---

## 1. 폴더 구조

```text
learning-vault/
├── README.md
├── 00_INBOX.md
├── 10_CONCEPTS/
│   └── 01_python_quiz_game_core.md
├── 20_PROJECTS/
│   └── python-quiz-game/
│       └── 02_git_mission2_learning_log.md
├── 30_DESIGNS/
├── 40_QUESTIONS/
├── 50_TESTS/
│   └── TEMPLATE.md
├── 60_DAILY_LOGS/
│   └── TEMPLATE.md
├── 90_CHAT_INDEX/
│   └── TEMPLATE.md
└── .gitignore
```

### 각 폴더의 책임

- `00_INBOX.md`: 수업 중 빠르게 적는 임시 메모. 여기서는 정리하지 않습니다.
- `10_CONCEPTS`: 여러 프로젝트에 다시 쓸 수 있는 개념
- `20_PROJECTS`: 특정 프로젝트의 구조, 명령, 이력, 검증
- `30_DESIGNS`: 구현 전에 작성한 설계와 선택 이유
- `40_QUESTIONS`: 아직 해결하지 못한 질문과 해결 결과
- `50_TESTS`: 자료를 닫고 설명·재구현·변형한 결과
- `60_DAILY_LOGS`: 그날 한 작업과 다음 시작점
- `90_CHAT_INDEX`: 중요한 AI 대화의 결론과 검색어만 저장

프로젝트 실제 소스코드는 기존 프로젝트 저장소에 두고, Learning Vault에는 **학습 결과와 판단 기록**을 남기는 편이 중복 관리가 적습니다.

---

## 2. 집에서 비공개 저장소 처음 만들기

### 2.1 GitHub에서 빈 비공개 저장소 생성

1. GitHub에 로그인합니다.
2. 오른쪽 위 `+`를 누릅니다.
3. `New repository`를 선택합니다.
4. 저장소 이름을 `learning-vault`로 입력합니다.
5. 공개 범위를 `Private`로 선택합니다.
6. 이 안내의 명령을 그대로 쓸 경우 `Add a README file`, `.gitignore`, `license`는 선택하지 않고 **빈 저장소**로 만듭니다.
7. `Create repository`를 누릅니다.

### 2.2 받은 압축 파일 풀기

이 패키지의 압축을 풀면 `learning-vault` 폴더가 생깁니다. Finder에서 폴더 위치를 확인한 뒤 VS Code로 엽니다.

예를 들어 다운로드 폴더에 있다면:

```bash
cd ~/Downloads/learning-vault
```

경로가 다르면 Finder에서 폴더를 터미널 창으로 끌어다 놓으면 정확한 경로가 입력됩니다.

### 2.3 로컬 Git 저장소 만들기

```bash
git init
git branch -M main
git config user.name "본인이름"
git config user.email "GitHub에 등록한 이메일"
git status
```

### 2.4 첫 커밋 만들기

```bash
git add .
git status
git commit -m "Docs: Learning Vault 초기 구조 생성"
```

`git status`에서 예상한 Markdown 파일만 스테이징됐는지 확인한 뒤 커밋합니다.

### 2.5 GitHub 비공개 저장소 연결

아래에서 `내아이디`를 실제 GitHub 사용자 이름으로 바꿉니다.

```bash
git remote add origin https://github.com/내아이디/learning-vault.git
git remote -v
git push -u origin main
```

GitHub 계정 비밀번호는 Git 명령 인증에 사용할 수 없습니다. HTTPS 방식에서는 개인용 액세스 토큰(PAT)이나 GitHub가 지원하는 인증 도구가 필요합니다. 개인 컴퓨터라면 Git Credential Manager나 GitHub Desktop을 사용하는 쪽이 편합니다.

---

## 3. 이미 만든 GitHub 저장소에 파일만 넣는 방법

GitHub에서 `learning-vault`를 이미 README와 함께 만들었다면, 새 폴더에서 먼저 복제합니다.

```bash
cd ~/Desktop
git clone https://github.com/내아이디/learning-vault.git
cd learning-vault
```

그다음 이 패키지 안의 파일과 폴더를 복제한 `learning-vault` 폴더 안으로 복사합니다. 같은 이름의 `README.md`가 있다면 이 패키지의 README로 교체하거나 두 내용을 합칩니다.

```bash
git status
git add .
git status
git commit -m "Docs: Python 및 Git 학습 기록 정리"
git push
```

---

## 4. 공용 아이맥에서 안전하게 사용하기

공용 컴퓨터에서는 편의보다 **인증 정보가 남지 않는 것**이 우선입니다.

### 4.1 처음 복제

```bash
cd ~/Desktop
git -c credential.helper= clone https://github.com/내아이디/learning-vault.git
cd learning-vault
git config --local user.name "본인이름"
git config --local user.email "GitHub에 등록한 이메일"
git config --local credential.helper ""
```

`credential.helper`를 빈 값으로 설정하면 상위 설정에 있는 credential helper 목록을 현재 저장소에서 비우는 데 사용할 수 있습니다.

HTTPS 인증 화면이 나오면:

- Username: GitHub 사용자 이름
- Password: GitHub 계정 비밀번호가 아니라 PAT

공용 컴퓨터용 토큰은 가능하면 다음처럼 제한합니다.

- Fine-grained personal access token
- 접근 저장소: `learning-vault` 하나만
- 권한: 저장소 Contents 읽기·쓰기 등 실제 필요한 최소 권한
- 만료일: 짧게 설정
- 토큰을 메모 앱, 이메일 본문, 저장소 파일에 보관하지 않음

이미 사용 중인 토큰이 있다면 그 토큰이 `learning-vault`에 접근할 권한이 있는지 확인해야 합니다.

### 4.2 작업 시작

```bash
cd ~/Desktop/learning-vault
git status
git pull
```

로컬 변경이 남아 있지 않은지 확인한 뒤 최신 원격 변경을 받습니다.

### 4.3 작업 종료

```bash
git status
git add 수정한파일.md
git status
git commit -m "Learn: 오늘 학습한 핵심 정리"
git -c credential.helper= push
```

`git add .`보다 수정한 파일을 직접 지정하면 실수로 불필요한 파일을 올릴 가능성이 낮습니다.

마지막으로 GitHub 웹에서 새 커밋이 올라갔는지 확인하고 다음을 수행합니다.

1. GitHub 웹에서 로그아웃
2. VS Code의 GitHub 계정도 로그인했다면 계정 메뉴에서 로그아웃
3. 브라우저 다운로드 목록과 클립보드에 토큰이 남지 않았는지 확인
4. 실수로 세션을 남겼다면 개인 기기에서 GitHub `Settings → Sessions`에서 해당 세션 철회
5. 토큰 노출이 의심되면 해당 PAT 즉시 폐기

공용 아이맥의 로컬 폴더 자체를 남기고 싶지 않다면 원격 반영을 확인한 뒤 Finder에서 `learning-vault` 폴더를 휴지통으로 이동합니다. 다음 방문 때 다시 `clone`하면 됩니다.

---

## 5. 매일 사용하는 최소 운영 절차

### 수업 중

정리하려고 멈추지 말고 `00_INBOX.md`에 계속 추가합니다.

```markdown
## 2026-08-04

- `enumerate()`가 인덱스와 값을 동시에 주는 이유 확인
- `return`과 `break` 범위 차이
- state.json 손상 시 원본 백업 방식 질문
```

### 학습 종료 전 10분

1. 다시 쓸 개념 → `10_CONCEPTS`
2. 특정 프로젝트 판단 → `20_PROJECTS`
3. 미해결 질문 → `40_QUESTIONS`
4. 자료 없이 시험한 결과 → `50_TESTS`
5. 다음 시작점 → `60_DAILY_LOGS`
6. 중요한 AI 대화 → `90_CHAT_INDEX`

모든 메모를 완벽하게 옮길 필요는 없습니다. 다시 쓸 가치가 없는 임시 문장은 삭제합니다.

### Git 기록

```bash
git status
git add 00_INBOX.md 50_TESTS/2026-08-04_python-basics.md
git commit -m "Test: Python 기초 폐쇄형 회상 점검"
git push
```

추천 접두어:

- `Learn:` 개념 정리
- `Test:` 이해도 검증
- `Question:` 질문과 해결 기록
- `Design:` 구현 전 설계
- `Docs:` 구조·안내 문서
- `Refactor:` 내용은 유지하며 문서 구조 정리

커밋은 하루 횟수를 채우기 위한 것이 아니라, 나중에 “어떤 판단 단위로 바뀌었는가”를 찾기 위한 기록입니다.

---

## 6. 파일 이름 규칙

```text
YYYY-MM-DD_주제.md
```

예:

```text
2026-08-04_docker-run-options.md
2026-08-04_quizgame-state-flow.md
2026-08-05_git-branch-merge-test.md
```

지속적으로 갱신하는 기준 문서는 날짜 없이 번호와 주제를 사용합니다.

```text
01_python_quiz_game_core.md
02_git_mission2_learning_log.md
```

---

## 7. 채팅 이력 관리

대화 전문을 모두 복사하지 않습니다. 중요한 대화가 끝나면 `90_CHAT_INDEX`에 다음만 남깁니다.

```markdown
# [설계] QuizGame 저장 흐름

- 날짜: 2026-08-04
- 문제: `state.json`을 언제 읽고 언제 저장하는가?
- 결론: 객체 생성 시 불러오고, 상태 변경 후 또는 종료 시 저장한다.
- 수정한 오해: JSON이 `Quiz` 객체를 직접 저장하는 것은 아니다.
- 관련 파일: `game.py`, `quiz.py`, `state.json`
- 검색어: `load_state`, `save_state`, `to_dict`, `from_dict`
- 남은 질문: 저장 도중 프로그램이 종료되면 파일이 손상될 수 있는가?
```

ChatGPT 프로젝트에는 대화 맥락을 두고, GitHub Learning Vault에는 검증된 결론과 검색어를 남깁니다.

---

## 8. 이해도 검증 기록

읽은 횟수가 아니라 다음 네 단계로 판정합니다.

| 점수 | 판정 |
|---|---|
| 1 | 보면 이해하지만 자료 없이 설명하지 못함 |
| 2 | 자료 없이 개념과 프로젝트 사례를 설명함 |
| 3 | 핵심 코드나 명령 흐름을 재구성함 |
| 4 | 조건을 바꿔 수정하고 실패 원인을 설명함 |

최소 시험 절차:

1. 자료를 닫습니다.
2. 3분 동안 핵심 흐름을 씁니다.
3. 7분 동안 코드나 명령을 재구성합니다.
4. 5분 동안 조건 하나를 바꿔 해결합니다.
5. 틀린 부분만 원문과 비교해 수정합니다.
6. 다음 날과 일주일 뒤 다시 시험합니다.

결과는 `50_TESTS/TEMPLATE.md`를 복사해 기록합니다.

---

## 9. 실패 조건과 복구

### `push`가 거절됨

먼저 상태를 확인합니다.

```bash
git status
git pull
```

원격과 로컬이 모두 바뀐 경우 충돌 가능성이 있으므로 오류 메시지를 그대로 보존하고 무작정 `--force`를 사용하지 않습니다.

### 다른 저장소에서 작업함

```bash
pwd
git remote -v
git branch
```

현재 위치, 원격 주소, 브랜치를 확인합니다.

### 토큰을 파일에 적음

1. 파일에서 지운 뒤 커밋하는 것만으로는 과거 커밋에 남을 수 있습니다.
2. 토큰을 즉시 폐기합니다.
3. 새 토큰을 발급합니다.
4. 공개 또는 공유 가능성이 있으면 Git 이력 정리 절차를 별도로 수행합니다.

### 정리가 밀림

`00_INBOX.md` 전체를 완벽히 정리하려 하지 않습니다. 다음 작업에 재사용할 내용 세 개만 옮기고 나머지는 보류 또는 삭제합니다.
