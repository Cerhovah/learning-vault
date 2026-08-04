# 퀴즈 게임으로 배우는 Python 핵심 구조

> 대상 프로젝트: `python-quiz-game`  
> 핵심 파일: `main.py`, `game.py`, `quiz.py`, `state.json`

이 프로젝트는 사용자의 입력을 변수에 담고, 조건문으로 기능을 선택하고, 반복문으로 프로그램을 계속 실행하며, 함수와 클래스로 역할을 나누고, JSON 파일에 상태를 저장하는 프로그램입니다. 코드 변경 과정은 Git 커밋으로 기록합니다.

```text
main.py 실행
  ↓
QuizGame 객체 생성
  ↓
state.json 불러오기
  ↓
메뉴 표시 → 사용자 입력 → 기능 선택
  ↓
퀴즈 풀기·추가·목록·삭제·점수 확인
  ↓
변경된 상태를 state.json에 저장
```

---

## 1. 프로그램의 기본 흐름

초기 단계의 프로그램은 대체로 다음 구조로 이해할 수 있습니다.

```text
입력(Input) → 처리(Process) → 출력(Output)
```

예를 들어 메뉴 번호를 입력받는 흐름은 다음과 같습니다.

```python
user_input = input("선택: ")  # 입력: 항상 문자열
choice = int(user_input)      # 처리: 문자열을 정수로 변환
print(choice)                 # 출력
```

Python은 일반적으로 파일의 최상위 코드를 위에서 아래로 실행합니다. 다만 함수나 클래스 정의문은 그 자리에서 동작 전체를 실행하는 것이 아니라, 나중에 호출할 수 있도록 정의를 등록합니다.

---

## 2. 변수와 자료형

### 2.1 변수는 값을 가리키는 이름입니다

변수는 값을 기억하고 다시 사용하기 위해 붙이는 이름입니다.

```python
choice = 1
correct_count = 0
hints_used = 0
score = 95
```

- `choice`: 사용자가 고른 메뉴 번호
- `correct_count`: 맞힌 문제 수
- `hints_used`: 사용한 힌트 수
- `score`: 계산된 점수

변수를 단순히 “값을 담는 상자”라고 설명할 수 있지만, 더 정확히는 **특정 객체를 가리키는 이름**에 가깝습니다.

```python
score = 80
score = 95
```

두 번째 줄이 실행되면 `score`는 더 이상 80이 아니라 95를 가리킵니다.

프로젝트에서는 `game.py`의 메뉴 처리 부분에서 다음과 같이 입력 결과를 변수에 저장합니다.

```python
choice = self.get_number_input("선택: ", 1, 6)
```

### 2.2 자료형은 값의 종류입니다

값의 종류에 따라 가능한 연산과 처리 방법이 다릅니다.

| 자료형 | 뜻 | 예 | 주된 용도 |
|---|---|---|---|
| `int` | 정수 | `answer = 2`, `score = 95` | 계산, 크기 비교 |
| `str` | 문자열 | `"파이썬의 창시자는?"` | 입력·출력, 문장 저장 |
| `bool` | 참 또는 거짓 | `True`, `False` | 조건 판단 |
| `list` | 순서가 있는 여러 값 | `["add", "push", "commit"]` | 반복, 추가, 삭제 |
| `dict` | 키와 값의 대응 | `{"score": 95, "total": 3}` | 의미별 데이터 관리 |
| `None` | 값이 아직 없음을 나타냄 | `best_score = None` | 미설정 상태 표현 |

#### `int`

```python
score = round(correct_count / count * 100)
```

점수처럼 계산하거나 비교할 값은 숫자 자료형으로 다룹니다.

#### `str`

```python
question = "파이썬의 창시자는?"
user_input = input("선택: ")  # "3"
number = int(user_input)      # 3
```

`input()`은 사용자가 숫자를 입력해도 문자열을 반환합니다. 계산이나 범위 비교가 필요하면 `int()` 등으로 변환해야 합니다.

#### `bool`

```python
def check_answer(self, user_answer):
    return user_answer == self.answer
```

두 값이 같으면 `True`, 다르면 `False`가 반환됩니다.

#### `list`

```python
self.quizzes = []
self.quizzes.append(new_quiz)
```

리스트는 여러 값을 순서대로 관리합니다. 항목을 추가할 때 `append()`, 특정 위치의 항목을 삭제할 때 `pop()` 등을 사용할 수 있습니다.

#### `dict`

```python
record = {
    "date": "2026-08-03 20:25",
    "total": 3,
    "correct": 2,
    "score": 67,
}
```

리스트가 위치로 값을 찾는 구조라면, 딕셔너리는 키의 의미로 값을 찾는 구조입니다.

```python
record["score"]
```

#### `None`

```python
self.best_score = None
```

`0`은 실제 점수이지만, `None`은 아직 한 번도 게임을 하지 않아 최고 점수가 정해지지 않았다는 뜻으로 사용할 수 있습니다.

---

## 3. 조건문과 반복문

### 3.1 `if`·`elif`·`else`: 현재 상황에 맞는 한 갈래 선택

메뉴 입력에 따라 서로 다른 기능을 실행할 때 사용합니다.

```python
if choice == 1:
    self.play_quiz()
elif choice == 2:
    self.add_quiz()
elif choice == 3:
    self.show_quiz_list()
elif choice == 4:
    self.delete_quiz()
elif choice == 5:
    self.show_score()
elif choice == 6:
    self.save_state()
    break
```

조건은 위에서부터 검사하며, 처음 참이 된 갈래 하나만 실행합니다.

- `if`: 첫 조건
- `elif`: 앞 조건이 거짓일 때 검사할 추가 조건
- `else`: 앞의 모든 조건이 거짓일 때 실행할 기본 처리

삭제 확인처럼 두 경우만 나눌 수도 있습니다.

```python
if ok == "y":
    self.quizzes.pop(num - 1)
else:
    print("취소했습니다.")
```

숫자 변환 자체가 실패하는 경우는 예외 처리 대상이지만, 변환된 숫자가 허용 범위를 벗어난 경우는 일반 조건문으로 처리하는 편이 자연스럽습니다.

### 3.2 `for`: 준비된 대상이나 정해진 횟수를 순서대로 처리

```python
for i in range(1, 5):
    choice = self.get_text_input(f"선택지 {i}: ")
    choices.append(choice)
```

`range(1, 5)`는 `1, 2, 3, 4`를 만들어 네 번 반복합니다.

```python
for number, quiz in enumerate(selected, start=1):
    quiz.display(number)
```

`enumerate()`는 항목과 표시 번호를 함께 꺼내 줍니다.

### 3.3 `while`: 언제 끝날지 미리 알 수 없는 반복

```python
while True:
    user_input = input(prompt).strip()

    if user_input == "":
        continue

    return number
```

올바른 입력이 언제 들어올지 모르므로 입력 검증에는 `while`이 적합합니다.

- `continue`: 현재 반복의 남은 부분을 건너뛰고 다음 반복으로 이동
- `break`: 현재 반복문만 종료
- `return`: 값을 돌려주며 함수 전체를 종료. 함수 안의 반복문도 함께 끝남

선택 기준은 다음과 같습니다.

- 퀴즈 다섯 개를 하나씩 처리한다 → `for`
- 올바른 번호가 들어올 때까지 묻는다 → `while`
- 사용자가 종료를 고를 때까지 메뉴를 반복한다 → `while`

---

## 4. 함수

함수는 여러 줄의 작업에 이름을 붙인 명령 묶음입니다.

```python
def get_number_input(self, prompt, min_num, max_num):
    ...
    return number
```

- 함수 이름: `get_number_input`
- 매개변수: `prompt`, `min_num`, `max_num`
- 반환값: 검증을 통과한 정수 `number`

호출할 때 전달한 값은 매개변수와 연결됩니다.

```python
choice = self.get_number_input("선택: ", 1, 6)
```

```text
prompt  ← "선택: "
min_num ← 1
max_num ← 6
반환값  → choice
```

함수를 사용하는 이유는 다음과 같습니다.

1. 반복되는 코드를 한곳에 모읍니다.
2. 코드의 역할을 분리합니다.
3. 이름만 보고 의도를 파악할 수 있게 합니다.
4. 수정 범위를 줄이고 테스트하기 쉽게 만듭니다.

### `print()`와 `return`의 차이

```python
print(number)
```

화면에 값을 보여 줄 뿐, 호출한 곳에 결과를 전달하지 않습니다.

```python
return number
```

호출한 곳에 값을 돌려줍니다.

```python
choice = self.get_number_input(...)
```

---

## 5. 클래스와 객체

기능이 많아지면 서로 관련된 변수와 함수가 흩어집니다. 클래스는 관련된 데이터와 동작을 한 단위로 묶는 수단입니다.

| 클래스 | 책임 |
|---|---|
| `Quiz` | 퀴즈 한 문제의 데이터와 동작 |
| `QuizGame` | 퀴즈 목록, 메뉴, 점수, 기록, 저장 등 전체 흐름 |

### 5.1 클래스·객체·인스턴스

- **클래스**: 어떤 데이터와 동작을 가질지 정의한 설계도
- **객체**: 프로그램 안에 실제로 존재하는 하나의 값
- **인스턴스**: 특정 클래스로부터 만들어졌다는 관계를 강조한 표현

```python
quiz1 = Quiz(
    "파이썬의 창시자는?",
    ["리누스", "귀도", "제임스", "브렌던"],
    2,
    "네덜란드 출신입니다.",
)

quiz2 = Quiz(
    "리스트에 사용하는 괄호는?",
    ["( )", "[ ]", "{ }", "< >"],
    2,
)
```

`quiz1`과 `quiz2`는 같은 `Quiz` 클래스로 만들었지만 서로 다른 데이터를 가진 객체입니다. “`Quiz`의 인스턴스”라고도 부릅니다.

클래스는 사용자가 직접 만드는 새로운 자료형의 정의라고 볼 수 있습니다.

### 5.2 `__init__`: 객체의 초기 상태를 준비하는 메서드

`__init__`은 흔히 **초기화 메서드**라고 부릅니다. 객체가 생성된 직후 필요한 속성을 준비합니다.

```python
def __init__(self, question, choices, answer, hint=None):
    self.question = question
    self.choices = choices
    self.answer = answer
    self.hint = hint
```

```python
quiz = Quiz("문제", ["A", "B", "C", "D"], 2)
```

이 코드가 실행되면 Python이 `__init__`을 호출해 객체의 속성을 설정합니다. `hint=None`은 힌트를 생략했을 때 사용할 기본값입니다.

```python
game = QuizGame()
```

이때도 `QuizGame.__init__()`이 실행되어 퀴즈 목록, 최고 점수, 기록을 준비하고 저장 상태를 불러옵니다.

엄밀히 말하면 `__init__`은 객체 자체를 새로 만드는 메서드가 아니라, 이미 생성된 인스턴스의 초기 상태를 설정하는 메서드입니다.

### 5.3 `self`: 현재 메서드를 실행 중인 객체

```python
self.question = question
```

뜻은 “지금 다루는 이 객체의 `question` 속성에 전달받은 값을 저장하라”입니다.

```python
quiz1.display(1)
```

이를 단순화하면 Python은 다음과 비슷하게 처리합니다.

```python
Quiz.display(quiz1, 1)
```

같은 메서드를 호출해도 `self`가 어느 객체를 가리키는지에 따라 서로 다른 데이터를 사용합니다.

### 5.4 속성과 메서드

- **속성(attribute)**: 객체가 가진 데이터
- **메서드(method)**: 클래스 안에 정의되어 객체와 관련해 실행되는 함수

| 구분 | `Quiz` 예 | `QuizGame` 예 |
|---|---|---|
| 속성 | `question`, `choices`, `answer`, `hint` | `quizzes`, `best_score`, `history` |
| 메서드 | `display()`, `check_answer()` | `play_quiz()`, `add_quiz()`, `save_state()` |

```python
quiz.question    # 속성 읽기
quiz.display(1)  # 메서드 실행
```

---

## 6. 메모리, 파일 입출력, JSON

### 6.1 저장이 필요한 이유

실행 중인 변수와 객체는 기본적으로 메모리에 존재합니다. 프로그램이 종료되면 그 상태는 사라지므로, 다음 실행에서도 유지해야 할 데이터는 디스크의 파일에 기록해야 합니다.

이 프로젝트의 `state.json`에는 다음을 저장합니다.

- 퀴즈 목록
- 최고 점수
- 게임 기록

### 6.2 파일 입출력의 기본 형태

```python
with open("memo.txt", "w", encoding="utf-8") as file:
    file.write("안녕하세요")
```

```python
with open("memo.txt", "r", encoding="utf-8") as file:
    text = file.read()
```

- `"w"`: 파일 쓰기. 기존 내용이 있으면 덮어씀
- `"r"`: 파일 읽기
- `encoding="utf-8"`: 한글을 포함한 문자를 일관된 방식으로 인코딩·디코딩
- `with`: 작업이 끝나거나 오류가 발생해도 파일을 자동으로 닫음

### 6.3 JSON의 역할

JSON은 구조화된 데이터를 텍스트로 표현하는 형식입니다. 프로그램 내부 데이터를 파일에 저장하거나 다른 시스템과 교환할 때 널리 사용됩니다.

| Python | JSON |
|---|---|
| `dict` | 객체 `{}` |
| `list` | 배열 `[]` |
| `str` | 문자열 |
| `int`, `float` | 숫자 |
| `bool` | `true`, `false` |
| `None` | `null` |

예시는 다음과 같습니다.

```json
{
  "best_score": 100,
  "history": [
    {
      "date": "2026-08-03 20:25",
      "total": 3,
      "correct": 2,
      "score": 67
    }
  ]
}
```

- `"best_score"`: 키
- `100`: 값
- `:`: 키와 값을 연결
- `,`: 다음 항목이 이어짐
- 가장 바깥의 `{}`: JSON 객체

Python 객체를 저장 가능한 기본 자료형으로 바꾸는 과정은 넓은 의미에서 **직렬화(serialization)**, 반대로 저장된 표현을 다시 프로그램의 자료형으로 읽는 과정은 **역직렬화(deserialization)**라고 합니다.

### 6.4 객체를 JSON에 저장하는 흐름

JSON은 `Quiz` 같은 사용자 정의 클래스를 직접 이해하지 못합니다. 따라서 저장 전에는 객체를 딕셔너리로 바꿔야 합니다.

```text
Quiz 객체
  ↓ to_dict()
Python 딕셔너리
  ↓ json.dump()
state.json의 JSON 텍스트
```

```python
data = {
    "quizzes": [quiz.to_dict() for quiz in self.quizzes],
    "best_score": self.best_score,
    "history": self.history,
}

with open(DATA_FILE, "w", encoding="utf-8") as file:
    json.dump(data, file, ensure_ascii=False, indent=2)
```

- `ensure_ascii=False`: 한글을 `\uXXXX` 형식이 아니라 실제 한글로 기록
- `indent=2`: 사람이 읽기 쉽도록 들여쓰기

### 6.5 JSON에서 객체를 복원하는 흐름

```text
state.json
  ↓ json.load()
Python 딕셔너리와 리스트
  ↓ Quiz.from_dict()
Quiz 객체
```

```python
data = json.load(file)
self.quizzes = [Quiz.from_dict(item) for item in data["quizzes"]]
```

`to_dict()`와 `from_dict()`는 저장 형식과 객체 구조 사이를 연결하는 변환 지점입니다.

---

## 7. 예외 처리

`try/except`는 예상 가능한 실패가 발생했을 때 프로그램이 어떻게 대응할지 정하는 문법입니다.

### 숫자가 아닌 입력

```python
try:
    number = int(user_input)
except ValueError:
    print("숫자를 입력하세요.")
```

`int("abc")`는 정수 변환이 불가능하므로 `ValueError`가 발생합니다. 예외 처리가 없으면 해당 오류가 처리되지 않은 채 프로그램이 종료될 수 있습니다.

### 저장 파일이 없음

```python
except FileNotFoundError:
    self.quizzes = self.get_default_quizzes()
```

첫 실행에는 저장 파일이 없을 수 있으므로 기본 퀴즈로 시작합니다.

### JSON이 손상되었거나 구조가 예상과 다름

```python
except (json.JSONDecodeError, KeyError, TypeError):
    self.quizzes = self.get_default_quizzes()
```

- `JSONDecodeError`: JSON 문법을 읽지 못함
- `KeyError`: 필요한 키가 없음
- `TypeError`: 예상한 자료형과 다름

실제 서비스라면 손상 파일을 즉시 덮어쓰기보다 백업한 뒤 복구 여부를 안내하는 방식이 더 안전할 수 있습니다.

### 파일 저장 실패

```python
except OSError:
    print("저장 중 문제가 발생했습니다.")
```

권한, 디스크, 경로 등의 문제로 파일 작업이 실패할 수 있습니다.

### 사용자가 강제로 중단함

```python
except (KeyboardInterrupt, EOFError):
    self.save_state()
```

`Ctrl+C` 또는 입력 스트림 종료가 발생했을 때 가능한 범위에서 저장 후 종료합니다. 다만 저장 자체도 실패할 수 있으므로 종료 처리 안의 예외까지 고려하면 더 안전합니다.

예외 처리는 모든 오류를 무조건 숨기는 장치가 아닙니다. 프로그램이 예상하고 의미 있게 대응할 수 있는 오류만 구체적으로 처리해야 합니다.

---

## 8. Git과 GitHub의 역할

- **Git**: 현재 컴퓨터에서 파일 변경 이력을 기록하는 버전 관리 도구
- **GitHub**: Git 저장소를 원격에 보관하고 동기화하는 서비스

```text
작업 파일 수정
  ↓ git add
스테이징 영역
  ↓ git commit
로컬 Git 이력
  ↓ git push
GitHub 원격 저장소
```

핵심 차이는 다음과 같습니다.

- `git add`: 다음 커밋에 포함할 변경 선택
- `git commit`: 선택한 변경을 로컬 이력으로 확정
- `git push`: 로컬 커밋을 GitHub에 전송
- `git pull`: GitHub의 새 변경을 현재 로컬 저장소에 가져와 통합
- `git clone`: 원격 저장소를 처음으로 새 폴더에 복제
- `git branch`: 독립된 작업 흐름을 관리
- `git merge`: 다른 브랜치의 변경을 현재 브랜치에 통합

커밋만 했다고 GitHub에 올라가는 것은 아닙니다. 원격 반영에는 `push`가 필요합니다.

세부 명령과 미션 이력은 `02_git_mission2_learning_log.md`에서 관리합니다.

---

## 9. 프로그램 전체 실행 흐름

1. `python3 main.py`로 프로그램을 시작합니다.
2. `QuizGame()` 객체를 만듭니다.
3. `QuizGame.__init__()`이 속성을 준비합니다.
4. `load_state()`가 `state.json`을 읽습니다.
5. `run()`이 `while True`로 메뉴를 반복합니다.
6. 사용자의 입력을 `choice`에 저장합니다.
7. `if/elif`로 실행할 기능을 선택합니다.
8. 퀴즈를 풀 때 `for`로 문제를 출제하고, `while`로 입력을 검증합니다.
9. `Quiz.check_answer()`가 정답 여부를 `bool`로 반환합니다.
10. 점수와 기록을 리스트·딕셔너리로 정리합니다.
11. `save_state()`가 상태를 JSON 파일에 기록합니다.
12. 코드 변경은 기능 단위 Git 커밋으로 남겨 GitHub에 올립니다.

각 문법은 따로 존재하는 것이 아니라 하나의 사용자 경험을 만들기 위해 연결됩니다.

---

## 10. 파일을 나눈 이유

프로젝트를 `main.py`, `quiz.py`, `game.py`로 나누는 핵심 목적은 파일 수를 늘리는 것이 아니라 **변경 이유가 다른 책임을 분리하는 것**입니다.

- `main.py`: 프로그램 시작점
- `quiz.py`: 퀴즈 한 문제의 구조와 행동
- `game.py`: 메뉴, 게임 진행, 입력 검증, 점수와 저장 흐름
- `state.json`: 실행 사이에 유지할 데이터

분리의 이점은 다음과 같습니다.

1. 어느 기능을 고칠지 찾기 쉬워집니다.
2. 한 파일의 변경이 다른 책임과 뒤섞이는 것을 줄입니다.
3. 클래스별 테스트와 재사용이 쉬워집니다.
4. AI가 만든 변경 사항도 파일별 책임을 기준으로 검토할 수 있습니다.

단, 프로그램이 매우 작다면 무조건 여러 파일로 나누는 것이 더 낫지는 않습니다. 파일 경계를 설명할 책임이 분명할 때 분리해야 합니다.

---

## 11. 스스로 설명하는 답변 틀

개념을 설명할 때 다음 네 부분을 사용합니다.

```text
① 무엇인가
② 왜 사용하는가
③ 이 프로젝트 어디에서 사용했는가
④ 사용하지 않으면 무엇이 불편한가
```

예:

> 변수는 값을 가리키는 이름입니다. 값을 다시 사용하거나 변경하기 위해 씁니다. 이 프로젝트에서는 메뉴 번호를 `choice`, 점수를 `score`에 저장했습니다. 변수명이 없다면 같은 값을 여러 코드에서 알아보기 쉽게 재사용하기 어렵습니다.

---

## 12. 이해도 검증 질문

자료를 닫고 다음 질문에 자신의 말로 답합니다.

1. `choice`, `score`, `self.quizzes`는 각각 무엇을 가리키는가?
2. `input()`의 결과와 정답 번호의 자료형은 왜 다를 수 있는가?
3. 리스트와 딕셔너리는 각각 어떤 데이터에 적합한가?
4. 메뉴 처리에 `if/elif`를 사용한 이유는 무엇인가?
5. 선택지 입력에는 왜 `for`, 입력 검증에는 왜 `while`을 사용했는가?
6. `get_number_input()`의 매개변수와 반환값은 무엇인가?
7. `Quiz` 클래스와 `quiz1` 객체는 어떻게 다른가?
8. 객체와 인스턴스라는 표현은 무엇을 각각 강조하는가?
9. `__init__`은 언제 실행되고 `self`는 무엇을 가리키는가?
10. 속성과 메서드는 어떻게 다른가?
11. 프로그램을 종료해도 데이터가 남는 이유는 무엇인가?
12. `json.dump()`와 `json.load()`는 각각 무엇을 하는가?
13. `to_dict()`와 `from_dict()`가 필요한 이유는 무엇인가?
14. `try/except`가 없으면 잘못된 입력이나 손상 파일에서 어떤 일이 생기는가?
15. `main.py`, `quiz.py`, `game.py`의 책임은 어떻게 나뉘는가?
16. `add`, `commit`, `push`는 왜 별도의 단계인가?
17. `clone`, `pull`, `push`는 각각 어느 방향으로 무엇을 옮기는가?

### 통과 기준

- **1단계**: 설명을 읽으면 이해됨
- **2단계**: 자료 없이 개념과 프로젝트 사례를 설명함
- **3단계**: 핵심 코드를 빈 파일에서 재구성함
- **4단계**: 입력 조건이나 저장 구조가 바뀌어도 수정 지점을 찾고 이유를 설명함

실전 이해는 3단계부터, 변형 대응 능력은 4단계부터 확인할 수 있습니다.
