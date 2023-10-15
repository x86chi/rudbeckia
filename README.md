# 연두는말안들'어' - 난해한 프로그래밍 언어

_"진짜 말 안듣는 종합게임 스트리머"[^About]_ [연두는말안드뤄](https://www.twitch.tv/rudbeckia7)님이 자주 사용하는 말을 [예약어](https://ko.wikipedia.org/wiki/예약어)로 모아 만드는 [난해한 프로그래밍 언어](https://ko.wikipedia.org/wiki/난해한_프로그래밍_언어) 명세입니다.

[^About]: [트위치 소개 탭](https://www.twitch.tv/rudbeckia7/about)의 내용 전문

## 스택

- 스택의 모든 원소는 유리수 또는 `NaN`으로 이루어져 있습니다. 분모가 1인 유리수를 편의상 정수라고 칭합니다.
  - 어떠한 연산을 하더라도 그 연산에 `NaN`이 있을 경우 그 연산의 값은 `NaN`이 됩니다.
- 0번 스택은 입력된 문자들의 유니코드 문자값들의 나열입니다. (stdin). 먼저 입력된 문자가 먼저 뽑힙니다. 기본적으로 빈 스택이며 이 스택이 빈 스택일 때 뽑기 연산이 일어날 시 프로그램에서 필요한 만큼 문자 입력을 받아 스택에 저장합니다.
- 1번 스택으로 넣어지는 값들은 출력됩니다. (stdout). 항상 빈 스택이며, 뽑기 연산이 일어날 경우 프로그램을 정상적으로 종료합니다. (exit 0) 모든 명령을 수행 완료하고 프로그램을 종료하고 싶을 때 쓰입니다.
  - 이 스택에 넣어지는 모든 0보다 크거나 같은 정수들은 그에 해당하는 유니코드 문자로 출력됩니다.
  - 이 스택에 넣어지는 모든 음의 정수들은 부호를 제외하고 0(U+0030) 이상 9(U+0039) 이하에 있는 문자들을 사용하여 10진수 숫자로 출력합니다.
  - 이 스택에 넣어지는 모든 정수가 아닌 유리수들은 자신보다 작은 정수 중 가장 큰 정수를 택하여 위의 과정을 처리합니다.
  - 이 스택에 `NaN`이 넣어지는 경우 `연바두보`로 치환하여 출력됩니다.
  - 이 스택에 넣어지는 원소는 출력됨과 동시에 스택에서 지워집니다.
- 2번 스택으로 넣어지는 값들은 에러 문자로 출력됩니다. (stderr). 출력 규칙은 1번 스택과 동일합니다. 항상 빈 스택이며, 뽑기 연산이 일어날 경우 오류 메세지로 `또 버그야?`를 출력하고 프로그램을 비정상적으로 종료합니다. (exit 1)
- 3번 스택은 특별한 기능을 하지 않는 빈 스택입니다.
- 4번 스택은 트위치 아이디 `rudbeckia7` 에서 `rudbeckia` 가 _영원한 행복이라는_ 꽃말처럼[^rudbeckia] 스택의 원소 할당을 불변으로 강제합니다. 넣을 수만 있고 뽑기가 불가능합니다.
  - 뽑기 명령 시도 시 오류 메세지로 `영복해`를 출력하고 종료합니다. (exit 1)
- 5번 스택부터는 특별한 기능을 하지 않는 일반 스택이며, 모두 기본적으로 빈 스택입니다. 프로그램 시작 시 현재 스택은 3번 스택이 됩니다. 비어 있는 일반 스택에서 뽑기 연산이 일어날 경우 `NaN` 을 제공합니다.

[^rudbeckia]: [흑역사 대방출) '영복해'에 숨은 진실... 연두 그는 꽃인가? - YouTube](https://youtu.be/AH8xKirameY?t=184)

## 해석할 때 유의점

- 중단점(`.`), 쉼표(`,`), 개행문자로 명령어를 구분합니다.
- [명령어](#명령어)로 예약된 키워드 외에는 인터프리터에서 주석으로 무시를 합니다.
  - 이를 응용해서 [연산](#연산)의 `좋` 키워드 끝에 `니?` 또는 `아해`를 붙여서 더 자연스럽게 읽히게 표현 가능합니다: `좋니?`, `좋아해`
  - 이를 응용해서 [조건문](#조건문) 키워드 끝에 물음표를 주석으로 붙여 더 자연스럽게 코드를 읽히게도 표현 가능합니다: `안뇽? 으악 쪼오오아`

## 명령어

### 연산

명령어는 아래 후술할 예약어와 뒤따라 나오는 여러 느낌표 !(U+0021)로 이루어져 있습니다. 예약어마다 저마다 가변적인 길이로 표현이 가능한데, 이 길이를 `글자 수`라고 하고, 후자의 느낌표 수를 `느낌표 갯수`라고 합니다. 이런 표현 방식으로 스택을 다루게 됩니다.

"한글 음절 문자"는 가(U+AC00) 이상 힣(U+D7A3) 이하의 유니코드 범위를 가리킵니다.

- `쪼아`, `쪼오아`, `쪼오아!!`: `쪼` 와 `아` 사이에 `오`를 갯수 제한 없이 넣을 수 있으며, `(글자수 - 1)` 와 느낌표 갯수를 곱한 값을 스택에 집어넣습니다.

  - 숫자 `0`을 넣어야 하는 경우 `좋`이라는 별개의 명령어를 사용합니다.
  - `쪼아` 는 `1`을 집어넣고, `쪼오아` 의 경우 `2`를 집어넣습니다.
  - `쪼오아!!` 는 글자 수 `2` 와 느낌표 갯수 `2`를 곱한 `4`를 집어넣습니다.
  - `쪼`와 `아` 사이에 `오` 이외에 한글 음절 문자를 추가적으로 넣는 방식으로 글자 수를 늘릴 수 있습니다.
    - 예를 들어 `쪼오아` 와 `쪼아아`는 같은 명령어입니다.

- `싫어`: 두개를 뽑고 나누어서 다시 집어넣기
- `죽어`, `죽어어`, `죽어어!!`: 스택의 위쪽에서 글자 수 만큼 뽑아 부호를 바꾼 후 그 합을 느낌표 갯수 만큼의 스택으로 이동하고 넣습니다.

  - `죽`와 `어` 사이에 `어` 이외에 한글 음절 문자를 추가적으로 넣는 방식으로 글자 수를 늘릴 수 있습니다.
    - 예를 들어 `죽어어` 와 `죽으어`는 같은 명령어입니다.

- `으악`, `으아악`, `으아악!!`: `으`와 `악` 사이에 `아`를 갯수 제한 없이 넣을 수 있으며, 스택의 위쪽에서 글자 수 만큼 뽑아 모두 곱하여 느낌표 갯수 만큼의 스택으로 이동하고 넣습니다.

  - 예를 들어 `으악!!` 은 원소 2개를 뽑아 모두 곱한 다음 그 값을 2번 스택에 넣습니다.
  - `으`와 `악` 사이에 `아` 이외에 한글 음절 문자를 추가적으로 넣는 방식으로 글자 수를 늘릴 수 있습니다.
    - 예를 들어 `으아악` 와 `으악악`, `으어악`은 같은 명령어입니다.

- `쒸익`, `쒸이익`, `쒸이익!!`: 스택의 위쪽에서 글자 수 만큼 뽑아 느낌표 갯수 만큼의 스택에 이동하고 넣습니다.

  - 현재 0번 스택에 위치해 있고 비어 있는 경우, 이동할 스택에 넣을 원소가 없기 때문에 뽑기 과정만 수행을 합니다.
    - 예를 들어 `쒸익!` 명령으로 `B` (U+0042)가 입력이 되면 원소 `42` 이 현재 스택에 저장을 하고 1번 스택으로 이동합니다.

- 글자 수 없이 느낌표만 있는 경우 `(느낌표 갯수 - 1)` 만큼의 스택으로 이동만 합니다. 0번째 스택으로 이동을 지원하기 위해 오프셋을 `-1`로 설계를 했습니다.

- `난 트위치 최고 간땅이의 담력과 귀여움과 애교를 가진 연두라고 해`[^B대면데이트]: 현재 스택의 모든 값을 버림
- `어디서 근육질 남자 좀 떨어졌으면 좋겠다`: 현재 스택의 모든 값을 뽑고 다 더하여 다시 넣기

[^B대면데이트]: [어느날 당신에게 걸려온 영상통화... 받으시겠습니까? [연두톱15] - YouTube](https://youtu.be/O88DBUI3RMQ?t=325)

### 조건문

- `안뇽 <명령어1> <명령어2>`: 현재 위치한 스택에 값 두개를 뽑아 비교합니다. 먼저 뽑아낸 값이 더 크거나 같으면 `<명령어1>` 을 실행하고 아니면 `<명령어2>`를 실행합니다.
  - `안뇽 으악 쪼오오아`: 먼저 뽑아낸 값이 크면 `으악`을 명령을 실행하고 아니라면 `쪼오오아`를 실행합니다.

### 반복하기

- `빵떡아`, `빵떡아!`, `빵떡아!!`: 느낌표 갯수가 없으면 현재 위치한 스택의 원소가 다 비워질 때까지, 있으면 느낌표 갯수만큼 `글글글글 글러먹은 글러먹은 스트리머` 명령과 `빵떡아` 사이를 다시 반복합니다.
  - `으어엌악 쪼오오아!!! 빵떡아!`: `으어엌악` 및 `쪼오오아!!!` 명령을 두번 실행합니다.
  - `글글글글 글러먹은 글러먹은 스트리머`가 이전에 명령되지 않은 상황에는 코드 맨 위줄을 반복 시작 줄로 자동 설정합니다.

## 예시 코드

### 입력한 문자를 출력하기 (단순 명령어 나열 버전)

TODO

### 입력한 문자를 출력하기 (주석을 추가하여 줄글로 읽히는 버전)

TODO

<!-- ## 튜링 완전한가? -->

<!-- TODO: 튜링 완전한 방향으로 설계하기 -->

## 확장자

파일 확장자로 `.totem` 를 사용합니다.

## 참고한 다른 난해한 언어

[난해한 혀엉... 언어]의 글자수 갯수에 따라 수를 연산하는 체계와 스택 기반의 입출력 체계에 영향을 받았습니다.

[아희]에서 쓰이는 스택 기반의 연산 체계를 혀엉 언어와 함께 참고하여 초기 설계를 했습니다.

[아희]: https://aheui.readthedocs.io/ko/latest/specs.html
[난해한 혀엉... 언어]: https://gist.github.com/xnuk/d9f883ede568d97caa158255e4b4d069

## 설계 진행 레벨

[TC39](https://tc39.es/)의 명세 [제안 과정](https://tc39.es/process-document/)을 차용해서 언어 설계의 진행도를 4가지로 구분합니다.

| 단계 | 진입 기준                      | 진입 시 명세의 품질                                           | 예상되는 구현 유형                                      |
| :--: | ------------------------------ | ------------------------------------------------------------- | ------------------------------------------------------- |
|  0   | 첫 단계                        | 미완성                                                        |                                                         |
|  1   | 초안 완성                      | 다양한 예제                                                   |                                                         |
|  2   | 연두님이 이 언어의 존재를 인지 | [커뮤니티](https://cafe.naver.com/rudbeckia7) 피드백이 반영됨 |                                                         |
|  3   | 미정                           | 튜링 완전                                                     | 실행 가능한 인터프리터 구현체 및 웹 플레이그라운드 제공 |

현재 단계: 0

![귀여운것](./귀여운것.jpeg)
