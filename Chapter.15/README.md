# 15장. let, const 키워드와 블록 레벨 스코프

---

## 15.1 var 키워드로 선언한 변수의 문제점

---

### 15.1.1 변수 중복 선언 허용

: var 키워드로 선언한 변수는 중복 선언 가능

```jsx
var x = 1;
var y = 1;

var x = 100;
var y;

console.log(x);  // 100
console.log(y);  // 1
```

: 변수 x와 y는 중복 선언됨.

💡 var 키워드로 선언한 변수를 중복 선언하면, 초기화문(선언과 동시에 초기값을 할당하는 문) 유무에 따라 다르게 동작함.

💡 초기화문 O = var 키워드가 없는 것처럼 동작
     초기화문 X = 변수 선언문 무시

: 초기화문이 있는 **var x = 100;** 은 var 키워드가 없는 것처럼 동작 + 값 변경됨.
  초기화문이 없는 **var y;** 은 무시함 + 에러 X

### 15.1.2 함수 레벨 스코프

: var 키워드로 선언한 변수 → 오로지 함수의 코드 블록만을 지역 스코프로 인정
 함수 외부에서 var 키워드로 선언한 변수 → 코드 블록 내에서 선언해도 모두 전역 변수가 됨.
: 전역 변수를 남발할 가능성 ⬆️

### 15.1.3 변수 호이스팅

: 변수 선언문 이전에 참조가능
  단, 할당문 이전에 변수를 참조하면 undefined를 반환함.

```jsx
console.log(foo);  // undefined

foo = 123;

console.log(foo);  // 123
var foo;
```

→ 변수 선언문 이전에 변수 참조하면, 가독성 ⬇️, 오류 발생 여지 O

## 15.2 let 키워드

---

var 키워드의 단점 보완 위해 → let, const 키워드 도입

### 15.2.1 변수 중복 선언 금지

- var 키워드로 중복 선언
    - 에러 발생 X
    - 중복 선언하면서 값도 할당 → 변수 값 변경되는 부작용 O
- let 키워드로 중복 선언
    - 문법 에러 발생 O
    

### 15.2.2 블록 레벨 스코프

- var 키워드
    - 함수 레벨 스코프(오로지 함수의 코드 블록만을 지역 스코프로 인정) 따름
- let 키워드
    - 블록 레벨 스코프(모든 코드 블록(함수, if문, for문, while문 등)을 지역 스코프로 인정) 따름
    
    ```jsx
    let foo = 1; // 전역 변수
    {
      let foo = 2; // 지역 변수
      let bar = 3; // 지역 변수
    }
    
    console.log(foo); // 1
    console.log(bar); // ReferenceEroor: bar is not defined
    ```
    
    : 함수도 코드 블록이므로 스코프 생성
    : 함수 내의 코드 블록은 함수 레벨 스코프에 중첩됨.
    

### 15.2.3 변수 호이스팅

- var 키워드
    - 변수 호이스팅 발생 O
    - 암묵적으로 ‘선언 단계’와 ‘초기화 단계’가 한번에 진행됨.
    - 한번에 진행되므로 변수는 할당 전에 undefined 값 가짐.
- let 키워드
    - 변수 호이스팅 발생하지 않는 것처럼 보이지만, 호이스팅 발생 O
    - ‘선언 단계’와 ‘초기화 단계’가 분리되어 진행됨.
    - 선언 단계는 먼저 실행, 초기화 단계는 변수 선언문에 도달했을 때 실행
    - 초기화 단계가 실행되기 이전에 변수에 접근 → 참조 에러
    - ***스코프의 시작 지점부터 초기화 단계 시작 지점(변수 선언문)까지*** 변수 참조 불가능 → ***일시적 사각지대***
    
    ```jsx
    console.log(foo);  // ReferenceError: foo is not defined
    
    let foo;
    console.log(foo);  // undefined, 초기화 단계 O
    
    foo = 1;
    console.log(foo);  // 1
    ```
    
    ```jsx
    let foo = 1;  // 전역 변수
    
    {
      console.log(foo);  // ReferenceError: Cannot access 'foo' before initialization
      let foo = 2;  // 지역 변수
    }
    ```
    
    : let 키워드 → 변수 호이스팅 발생 → 참조 에러 = 중복 선언 불가
    
    💡모든 선언은 호이스팅이 발생하지 않는 것처럼 보이지만, 모두 호이스팅함.
    

### 15.2.4 전역 객체와 let

```jsx
var x = 1;
y = 2;
function foo() {}
console.log(window.x);  // 1
console.log(x);  // 1

console.log(window.y);  // 2
console.log(y);  // 2

console.log(window.foo);  // f foo() {}
console.log(foo);  // f foo() {}
```

```jsx
let x = 1;

console.log(window.x);  // undefined
console.log(x);  // 1
```

- **var 키워드**로 선언한 전역 변수와 전역 함수, 선언하지 않은 변수에 값을 할당한 암묵적 전역 = 전역 객체 window의 프로퍼티
    - 전역 객체의 프로퍼티를 참조할 때, window 생략 가능
- **let 키워드**로 선언한 전역 변수 ≠ 전역 객체의 프로퍼티
    - window.foo와 같이 접근 불가
    - 보이지 않는 개념적인 블록 내에 존재

## 15.3 const 키워드

---

: const 키워드는 상수를 선언하기 위해 사용(반드시는 아님)

### 15.3.1 선언과 초기화

💡 **const 키워드로 선언한 변수는 반드시 선언과 동시에 초기화해야 함**

→ 선언과 값 할당을 동시에 해야 함.

```jsx
const foo = 1; 
const foo;  // SyntaxError
```

💡 블록 레벨 스코프 가지고, 변수 호이스팅이 발생하지 않는 것 같지만 발생함.

```jsx
{
console.log(foo);  // ReferenceError: Cannot access 'foo' before initialization
const foo = 1;
console.log(foo);  // 1
}

console.log(foo);  // ReferenceError: foo is not defined
```

### 15.3.2 재할당 금지

var 또는 let 키워드로 선언한 변수는 재할당이 자유로움
**const 키워드로 선언한 변수는 재할당이 금지됨**

```jsx
const foo = 1;
foo = 2;  // TypeError: Assignment to constant variable.
```

### 15.3.3 상수

const 키워드로 선언한 변수에 원시 값 할당 후, 변수 값 변경 불가능
원시 값 = 변경 불가능한 값 → 재할당 없이 값 변경할 수 있는 방법 없음.
→ const 키워드 이용하여 상수 표현함.

변수 ↔ 상수 = 재할당이 금지된 변수

상수도 값을 저장하기 위한 메모리 공간이 필요하므로 변수라고 할 수 O
단, 변수는 언제든지 재할당을 통해 변수 값 변경 가능
      상수는 재할당 금지

💡 상수의 이름 = 대문자로 선언해 상수임을 명확히 나타냄
                             여러 단어로 이뤄진 경우, 언더스코어(*)로 구분함(스네이크)
                             ex) TAX_RATE*

### 15.3.4 const 키워드와 객체

**💡 const 키워드로 선언된 변수에 객체를 할당한 경우, 값 변경 가능**
→ 원시 값 : 재할당 없이 값 변경 방법 X
→ 객체 : 재할당 없이도 직접 값 변경 가능

```jsx
const person = {
  name: 'Lee'
};

person.name = 'Kim';

console.log(person);  // {name: "Kim"}
```

**const 키워드는 재할당을 금지할 뿐, “불변”을 의미하지 X**
객체가 변경되더라도 변수에 할당된 참조 값은 변경되지 X

## 15.4 var vs. let vs. const

---

- var
    - ES6 사용시, var 키워드 사용X
- let
    - 재할당이 필요한 경우에 한정하여 사용
    - 변수의 스코프는 최대한 좁게 만듦
- const
    - 변수 선언에 기본적으로 사용
    - 의도치 않은 재할당 방지 가능(var, let 보다 안전)
    

객체는 의외로 재할당하는 경우가 드묾 → 변수 선언할 때, 일단 const 키워드 사용하자. → 재할당이 필요하면 let으로 변경
