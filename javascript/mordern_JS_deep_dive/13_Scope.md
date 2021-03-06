![](https://i.imgur.com/LOqoalW.png)

본 게시글은 "모던 자바스크립트 Deep Dive"를 학습하며, 내용 요약 또는 몰랐던 부분을 정리하는 글 입니다.

# 스코프(유효범위)

## 스코프란?

스코프는 식별자가 유효한 범위를 말한다.
자바스크립트의 스코프는 다른 언어의 스코프와 구별되는 특징이 있으므로 주의가 필요하다.
**var, let, const로 선언한 변수의 스코프는 각 다르게 동작한다.**(15장에서 자세히 알아보자)

- var: 함수 레벨 스코프
- let/const: 블록 레벨 스코프

변수는 자신이 선언된 위치에 의해 자신이 유효한 범위(다른 코드가 변수 자신을 참조할 수 있는 범위)가 결정된다.
**모든 식별자는 자신이 선언된 위치에 의해 다른 코드가 식별자 자신을 참조할 수 있는 유효 범위가 결정된다.**

```javascript
var var1 = 1; // 코드의 가장 바깥 영역에서 선언한 변수

if (true) {
  var var2 = 2; // 코드 블록 내에서 선언한 변수
  if (true) {
    var var3 = 3; // 중첩된 코드 블록 내에서 선언한 변수
  }
}
console.log(var1); // 1
console.log(var2); // 2
console.log(var3); // 3
```

자바스크립트는 if문의 코드 블럭 안에서 변수를 선언하더라도 외부 코드 블록의 유효범위를 가진다.

```javascript
var x = "global";

function foo() {
  var x = "local";
  console.log(x); // ①
}

foo();

console.log(x); // ②
```

위 코드에서, 자바스크립트 엔진은 이름이 같은 두 개의 변수 중에서 어떤 변수를 참조해야 할 것인지를 결정해야한다.
이를 **식별자 결정(identifier resolution)**이라 한다.
따라서 스코프란 자바스크립트 엔진이 식별자를 검색할 때 사용하는 규칙이라고 할 수 있다.

파일 이름은 하나의 파일을 구별하여 식별할 수 있는 식별자다.
우리는 컴퓨터를 사용할 때 하나의 파일 이름만 사용하지 않는다.
식별자인 파일 이름을 중복해서 사용할 수 있는 이유는 폴더(디렉터리)라는 개념이 있기 때문이다.

이와 마찬가지로 프로그래밍 언어는 스코프를 통해 식별자인 변수 이름의 충돌을 방지하여 같은 이름의 변수를 사용할 수 있게 한다.
즉, 스코프는 **네임스페이스**다.

tip1) 코드의 문맥과 환경
**렉시컬 환경(lexical environment)**은 **코드가 어디서 실행되며 주변에 어떤 코드가 있는지**를 뜻한다.
즉, 코드의 문맥은 렉시컬 환경으로 이뤄진다. 이를 구현한 것이 **실핼 컨텍스트**이며, 모든 코드는 실행 컨텍스트에서 평가되고 실행된다.
스코프는 실행 컨텍스트와 깊은 관련이 있다.

tip2) var와 let, const
var 키워드로 선언된 변수는 같은 스코프 내에서 중복 선언이 허용된다.(의도치 않게 변수값이 재할당되어 변경되는 부작용 발생)
let, const 키워드로 선언된 변수는 같은 스코프 내에서 중복 선언을 허용하지 않는다.

## 스코프의 종류

전역 스코프: 전역 변수의 스코프로 **어디서든지 참조할 수 있다.**
지역 스코프: 지역 변수의 스코프로 **자신의 지역 스코프와 하위 지역 스코프에서 유효하다.**

## 스코프 체인

함수는 중첩될 수 있으므로 함수의 지역 스코프도 중첩될 수 있다.
이는 \*\*스코프가 함수의 중첩에 의해 계층적 구조를 갖는다는 것을 의미한다.

모든 스코프는 하나의 계층적 구조로 연결되며, 모든 지역 스코프의 최상위 스코프는 전역 스코프다.,
이렇게 스코프가 계층적으로 연결된 것을 **스코프 체인**이라 한다.

**변수를 참조할 때 자바스크립트 엔진은 스코프 체인을 통해 변수를 참조하는 코드의 스코프에서 시작하여 상위 스코프 방향으로 이동하며 선언된 변수를 검색한다.**
이를 통해 상위 스코프에서 선언한 변수를 하위 스코프에서도 참조할 수 있는 것이다. 절대 하위 스코프로 내려가면서 식별자를 검색하는 일은 없다.
**이는 상위 스코프에서 유효한 변수는 하위 스코프에서 자유롭게 참조할 수 있지만 하위 스코프에서 유효한 변수를 상위 스코프에서 참조할 수 없다**는 것을 의미한다.

tip) 스코프 체인은 실행 컨텍스트의 렉시컬 환경을 단방향으로 연결한 것이다. 전역 렉시컬 환경은 코드가 로드되면 곧바로 생성되고 함수의 렉시컬 환경은 함수가 호출되면 곧바로 생성된다.

### 스코프 체인에 의한 함수 검색

```javascript
// 전역 함수
function foo() {
  console.log("global function foo");
}

function bar() {
  // 중첩 함수
  function foo() {
    console.log("local function foo");
  }

  foo(); // ①
}

bar();
```

함수 선언문으로 함수를 정의하면 런타임 이전에 함수 객체가 먼저 생성된다.(호이스팅)
자바스크립트 엔진은 함수 이름과 동일한 이름의 식별자를 암묵적으로 선언하고 생성된 함수 객체를 할당한다.

따라서 위 예제의 모든 함수는 함수 이름과 동일한 이름의 식별자에 할당된다.
(1)에서 foo 함수를 호출하면 자바스크립트 엔진은 함수를 호출하기 위해 먼저 함수를 가리키는 식별자 foo를 검색한다.

이처럼 함수도 식별자에 할당되기 때문에 스코프를 갖는다. 사실 함수는 식별자에 함수 객체가 할당된 것 외에는 일반 변수와 다를 바 없다.

## 함수 레벨 스코프

**지역은 함수 몸체 내부를 말하고 지역은 지역 스코프를 만든다.**
**이는 코드 블록이 아닌 함수에 의해서만 지역 스코프가 생성된다는 의미이다.**

C나 자바 등을 비롯한 대부분의 프로그래밍 언어는 함수 몸체만이 아니라 모든 코드 블록(if, try/catch 등)이 지역 스코프를 만든다.
이러한 특성을 **블록 레벨 스코프**라고 한다. 하지만 **var 키워드로 선언된 변수는 오로지 함수의 코드 블록(함수 몸체)만을 지역 스코프로 인정**한다.
이러한 특성을 **함수 레벨 스코프**라고 한다.

```javascript
var x = 1;

if (true) {
  // var 키워드로 선언된 변수는 함수의 코드 블록(함수 몸체)만을 지역 스코프로 인정한다.
  // 함수 밖에서 var 키워드로 선언된 변수는 코드 블록 내에서 선언되었다 할지라도 모두 전역 변수다.
  // 따라서 x는 전역 변수다. 이미 선언된 전역 변수 x가 있으므로 x 변수는 중복 선언된다.
  // 이는 의도치 않게 변수 값이 변경되는 부작용을 발생시킨다.
  var x = 10;
}

console.log(x); // 10
```

위 예제에사 앞서 말했듯이, var 키워드로 선언된 변수는 함수 레벨 스코프만 인정하기 때문에 if문 내에서(코드 블럭 내에서) 선언되었다 할지라도 모두 전역변수이다.
따라서 x는 중복 선언되고 그 결과 의도치 않은 전역 변수의 값이 재할당된다.

```javascript
var i = 10;

// for 문에서 선언한 i는 전역 변수다. 이미 선언된 전역 변수 i가 있으므로 중복 선언된다.
for (var i = 0; i < 5; i++) {
  console.log(i); // 0 1 2 3 4
}

// 의도치 않게 변수의 값이 변경되었다.
console.log(i); // 5
```

블록 레벨 스코프의 프로그래밍 언어에서는 for 문에서 반복을 위해 선언된 i 변수가 fot 문의 코드 블록 내에서만 유효한 지역변수다.
하지만 JS에서 var 키워드로 선언된 변수는 블록 레벨 스코프를 인정하지 않기 때문에 i는 전역변수이다.
따라서 변수 i는 중복 선언되고 의도치 않은 전역 변수의 값이 재할당된다.

## 렉시컬 스코프

```javascript
var x = 1;

function foo() {
  var x = 10;
  bar();
}

function bar() {
  console.log(x);
}

foo(); // ?
bar(); // ?
```

위 예제의 실행 결과는 두 가지 패턴으로 예측할 수 있다.

1. **함수를 어디서 호출했는지**에 따라 함수의 상위 스코프를 결정한다.
   동적 스코프: 함수를 정의하는 시점에는 함수가 어디서 호출될지 알 수 없다)

2. **함수를 어디서 정의했는지**에 따라 함수의 상위 스코프를 결정한다.
   정적 스코프(렉시컬 스코프): 함수 정의가 평가되는 시점에 상위 스코프가 정적으로 결정되기 때문에 정적 스코프라고 부른다. 대부분의 프로그래밍 언어는 렉시컬 스코프를 따른다.

**따라서 자바스크립트는 렉시컬 스코프를 따르므로 함수를 어디서 호출했는지는 상위 스코프 결정에 어떠한 영향도 주지 않는다.**
이처럼 함수의 상위 스코프는 함수 정의가 실행될 때 정적으로 결정된다. 함수 정의(함수 선언문 또는 함수 표현식)가 실행되어 생성된 함수 객체는 이렇게 결정된 상위 스코프를 기억한다.
함수가 호출될 때마다 함수의 상위 스코프를 참조할 필요가 있기 때문이다.

위 예제에서 bar 함수는 전역에서 정의된 함수다. 함수 선언문으로 정의된 bar함수는 전역 코드가 실행되기전에 먼저 평가되어 함수 객체를 생성한다.
이때 생성된 함수 객체는 자신이 정의된 스코프, 즉 전역 스코프를 기억한다. 호출될 때 마다 호출된 곳이 어디인지 관계없이 언제나 자신이 기억하고 있는 전역스코프를 상위 스코프로 사용한다.
따라서 위 예제를 실행하면 전역 변수 x의 값 1을 두 번 출력한다.
