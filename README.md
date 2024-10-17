# javascript-calculator-precourse

## 기능 요구 사항

입력한 문자열에서 숫자를 추출하여 더하는 계산기를 구현한다.

- 쉼표(,) 또는 콜론(:)을 구분자로 가지는 문자열을 전달하는 경우 구분자를 기준으로 분리한 각 숫자의 합을 반환한다.
  - 예: "" => 0, "1,2" => 3, "1,2,3" => 6, "1,2:3" => 6
- 앞의 기본 구분자(쉼표, 콜론) 외에 커스텀 구분자를 지정할 수 있다. 커스텀 구분자는 문자열 앞부분의 "//"와 "\n" 사이에 위치하는 문자를 커스텀 구분자로 사용한다.
  - 예를 들어 "//;\n1;2;3"과 같이 값을 입력할 경우 커스텀 구분자는 세미콜론(;)이며, 결과 값은 6이 반환되어야 한다.
- 사용자가 잘못된 값을 입력할 경우 "[ERROR]"로 시작하는 메시지와 함께 Error를 발생시킨 후 애플리케이션은 종료되어야 한다.

## 입출력 요구 사항

### 입력

- 구분자와 양수로 구성된 문자열

### 출력

- 덧셈 결과

```javascript
결과: 6;
```

### 실행 결과 예시

```javascript
덧셈할 문자열을 입력해 주세요.
1,2:3
결과 : 6
```

## 요구 사항 분석

- 기본 구분자는 쉼표(,) 또는 콜론(:)으로 구성되어있다.
- 기본 구분자 외에 커스텀 구분자를 지정할 수 있다.
  - 커스텀 구분자를 생성할때는 문자열 맨 앞부분에 "//{구분자}\n"을 추가하는 것으로 생성할 수 있다.
- 잘못된 값이 입력될 경우 "[ERROR]{메시지}"를 출력하면서 Error를 발생킨 후 종료되어야 한다.
- 정상적인 값이 입력될 경우 구분자를 기준으로 분리한 숫자의 합을 반환한다.

## 구현할 기능 목록

- 입력된 값을 구분자를 통해 분리하는 기능
- 입력값을 검증하여 올바른 입력이 들어왔는지 검증하는 기능
- 분리된 문자를 숫자로 변경하는 기능
- 잘못된 값이 입력되었을 경우 Error를 발생시키는 기능
- 커스텀 구분자를 생성하는 기능
- 정상적인 값이 입력되었을 경우 분리한 숫자의 합을 반환하는 기능

## 기능 요구 사항에 기재되지 않은 내용에 대한 판단

- 요구 사항에 따르면 커스텀 구분자는 문자열 맨 앞부분에 "//{구분자}\n"을 추가하는 것으로 생성할 수 있다.

  - 커스텀 구분자는 한개만 올 수 있는가?
  - 커스텀 구분자가 여러 개 올 수 있는가?
  - 커스텀 구분자가 여러 개 올 수 있을 경우 어떻게 구분하는가?

  위와 같은 상황은 다음의 3가지 경우로 나눌 수 있다.

  1. 커스텀 구분자가 한개만 가능한 경우는 문자열 맨 앞부분에 "//{구분자}\n"이 있는지 확인 후 구분자 추출, 이때 구분자의 길이는 상관 없음
  2. 각 구분자 별로 "//{구분자}\n"를 사용하여 구분자 추출, 이 경우 구분자의 길이는 상관 없음 예) //.\n//;[\n1.2,3;[4 -> 구분자는 기본구분자에 추가로 .과 ;[가 추가됨
  3. 모든 구분자를 하나의 "//{구분자}\n"를 사용하여 구분자 추출, 이 경우 하나의 구분자의 길이는 1로 고정 예) //.;[\n -> 구분자는 기본구분자에 추가로 온점(.)과 세미콜론(;), 여는대괄호([)가 추가된다

  여기서는 "//{구분자}\n"로 구분자를 새로 만들어주는 것에 맞게 2번 방식을 사용하려 한다.
  왜냐하면 구분자의 개수에 대한 제한은 문제에 걸려있지 않으며, 3번 방식의 경우 구분자를 2자리 이상의 문자열로 하고싶을 경우에 대한 처리를 할 수 없기 때문이다.
  또한 1,2번 방법 모두 구분자를 생성할 때 같은 코드를 사용할 수 있으며, 만약 구분자를 한 개만 받는다는 제약을 걸때는 입력값에 대한 검증 부분에서 에러를 발생하도록 처리할 수 있기 때문이다.

- 커스텀 구분자를 만들 때 문자열 앞쪽에 "//{구분자}\n"를 사용하는데, 만약 "//{구분자}\n"부분이 문자열 중앙에 나올 경우 오류처리를 어떻게 할것인가?

  - "//.\n1,2,3.4//;\n5" 이런 입력이 들어올 경우 에러를 발생시켜야 함

  문자열을 //{구분자}\n와 해당 문자열이 아닌 배열로 지정 후 //{구분자}\n에 대한 index값이 연속적이지 않을 경우 에러 발생

## 상수에 관한 생각

javascript에서는 상수를 선언할때는 const를 사용한다.
상수는 프로그래밍에서 한 번 값을 할당하면 그 값을 변경할 수 없는 변수를 의미하며, 실제로 const로 선언한 값은 재할당이 불가능하다.
하지만 javascript의 객체의 경우 조금 애매해진다고 생각한다.
왜냐하면 객체나 배열의 경우 실제 값이 변수에 저장되는것이 아닌 주소값이 저장되기 때문에 객체나 배열의 내부 값은 변경이 가능하다.

이번 프리코스에서는 javascript의 네이밍 컨벤션에서 상수명은 SNAKE_CASE로 작성하도록 되어있다.
그렇다면 어디까지를 상수로 바라볼지에 대해 생각을 해봐야 할 것이다.

이런 점에서 내가 내린 결론은 SNAKE_CASE를 통해 선언하는 상수의 범위는 객체나 배열과 같이 참조값을 저장할 경우에는 내부 값도 변하지 않는것을 상수로 생각하려 한다.
예를들어 [1, 2]와 같이 배열로 선언되었지만, 해당 요소의 값을 변경하지 않고 사용할 경우를 의미한다.
이렇게 SNAKE_CASE를 통해 선언된 배열이나 객체는 Object.freeze()를 통해 불변성을 보장하도록 한 뒤 사용할 것이다.
따라서 일반적으로 내부 요소의 값이 변경될 수 있는 객체는 camelCase규칙을 따를것이다.

## 에러사항

- 커스텀 구분자로 |를 만들지 않고 |를 구분자로 사용하는 경우 에러가 발생해야하지만 정삭적으로 작동됨
- 커스텀 구분자를 만들 때 정규 표현식의 메타문자나 이스케이프 시퀀스들을 사용할 경우 에러가 발생함
- 커스텀 구분자를 만들 때 \n이 오는 경우, "//\n\n"이 경우는 //와 \n사이에 있는 문자를 커스텀 구분자로 사용하는 문제의 특성상 \n을 구분자로 사용하는 것은 불가능하도록 구현

해결방법: 이전에 입력된 값을 구분자를 통해 분리 사용했던 코드에서 `` const sepRegex = new RegExp(`[${sep.join("|")}]+`); ``부분이 잘못된것으로 생각된다.  
정규표현식을 사용할 때 OR의 역할을 하도록 |를 join해줬지만 []를 사용한 문자 클래스에서는 OR의 역할이 아닌 문자 그 자체로 인식되기에 커스텀 구분자로 |를 만들지 않아도 구분자로 사용된것으로 추정된다.  
따라서 해당 부분을 고치면서 추가적으로 정규표현식의 메타문자들과 이스케이프 시퀀스 등을 문자 자체로 바꾸는 작업을 해주어야한다.

### 해결

```javascript
const sepRegex = new RegExp(`[${sep.map((s) => s.replace(/[-/\\^$*+?.()|[\]{}]/g, "\\$&"))}]`);
```

extractInput함수에서 문자열을 구분자로 분리하기위해 정규표현식을 사용하는 부분에서 구분자로 사용할 배열을 map과 replace를 사용하여 정규표현식의 메타문자들을 문자 자체로 사용하도록 바꿔주었다.
