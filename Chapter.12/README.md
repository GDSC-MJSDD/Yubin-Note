# 12장. 함수

---

### 12.1  함수란?

```jsx
// f(x, y) = x + y
function add(x, y) { 
  return x + y;  
}

// f(2, 5) = 7
add(2, 5); // 7
```

***→ 일련의 과정을 문으로 구현하고 코드 블록으로 감싸 하나의 실행 단위로 정의***

function add(x, y) { 
  return x + y;             → 함수 정의

add(2, 5);                    → 함수 호출

### 12.2 함수를 사용하는 이유

> 함수는 필요할 때 **여러 번 호출** 가능 - 코드의 재사용
> 

> 동일한 작업을 **반복적으로 수행**할 때, 함수 이용 → 효율적
> 

> 유지보수의 편의성 ⬆️,  코드의 신뢰성 ⬆️
> 

➕ 함수 = 객체 타입의 값 → 이름(식별자) 붙일 수 O

     함수 이름만 봐도 함수의 역할 파악할 수 있게  → 코드의 가독성 ⬆️

### 12.3 함수 리터럴

```jsx
var f = function add(x, y) {
  return x + y;
}; // 변수 f에 함수 리터럴 할당
```

**함수 리터럴의 구성요소**

- 함수 이름
    - 함수 이름 = 식별자  → 식별자 네이밍 규칙 준수
    - 함수 몸체 내에서만 참조 가능
    - 생략 가능
- 매개변수 목록
    - (매개변수1, 매개변수2, …)
    - 매개변수 목록은 순서대로 인수가 할당
    - 함수 몸체 내에서 변수와 동일하게 취급됨 → 식별자 네이밍 규칙 준수
- 함수 몸체
    - 함수 호출시 실행될 문들을 하나의 실행 단위로 정의한 코드 블록
    - 함수 호출에 의해 실행
    
     
    

*함수 리터럴 → 값 생성 → 값 → 객체* 

> **함수는 객체다**
> 

→ 일반 객체는 호출 X, 함수 객체는 호출 O

→ 고유한 프로퍼티 가짐

### 12.4 함수 정의

**함수 정의 방식**

- 함수 선언문
    
    ```
    function add(x, y) {
      return x + y;
    }
    ```
    
- 함수 표현식
    
    ```jsx
    var add = function(x, y) {
      return x + y;
    };
    ```
    
- Function 생성자 함
    
    ```jsx
    var add = new Function('x', 'y', 'return x + y')
    ```
    
- 화살표 함수
    
    ```jsx
    var add = (x, y) => x + y;
    ```
    
    **12.4.1 함수 선언문** 
    
    ```jsx
    // 함수 선언문
    function add(x, y) {
      return x + y;
    }
    
    console.log(add(2, 5));
    ```
    
    - 함수 선언문은 함수 이름을 생략 불가
    - 함수 선언문은 표현식이 아닌 문 → undefined 출력, 변수 할당 불가
    - 함수 선언문이 변수에 할당되는 것처럼 보이는 것
        
        ```jsx
        var add = function(x, y) {
          return x + y;
        };
        
        console.log(add(2, 5));
        ```
        
    
    → ***JS 엔진은 생성된 함수를 호출하기 위해 함수 이름과 동일한 이름의 식별자를 암묵적으로 생성하고 거기에 함수 객체를 할당함.***
    
    ***→ 함수는 함수 이름으로 호출하는 것이 아닌 함수 객체를 가리키는 식별자로 호출함.***
    
    ```jsx
    var add = function add(x, y) {  // add 1번, 2번
      return x + y;
    }
    
    console.log(add(2, 5));  // add 3번
    ```
    
    (add 1번, 3번 → 식별자 / 2번 → 함수 이름)
    
    **12.4.2 함수 표현식**
    
    **함수 = 일급 객체** 
    
    : JS의 함수는 값처럼 **변수에 할당**/ **프로퍼티 값**/ **배열의 요소**가 될 수 있음
    
    → 함수 리터럴로 생성한 함수 객체를 변수에 할당 가능 = 함수 표현식
    
    ```jsx
    var add = function foo(x, y) { 
      return x + y;
    }
    
    console.log(add(2, 5));  // 7
    console.log(foo(2, 5));  // ReferenceError: foo is not defined
    ```
    
    **12.4.3 함수 생성 시점과 함수 호이스팅**
    
    *함수 선언문으로 정의한 함수는 함수 선언문 이전에 호출할 수 있음*
    
    → 함수 선언문으로 정의한 함수는 함수 표현식으로 정의한  함수의 생성 시점이 다르기 때문
    
    > **함수 호이스팅** = 함수 선언문이 코드의 선두로 끌어 올려진 것 같은 특징
    > 
    
    **12.4.4 Function 생성자 함수 → 바람직X**
    
    : 생성자 함수 = 객체를 생성하는 함수
    
    **12.4.5 화살표 함수**
    
    : function 키워드 대신 **화살표⇒** 사용
    

### 12.5 함수 호출

**12.5.1 매개변수와 인수**

*매개변수를 통해 인수 전달, 인수는 값으로 평가될 수 있는 표현식이어야 함*

- 인수 할당되지 않은 매개변수 값 = undefined
- 초과된 인수 = 암묵적으로 객체의 프로퍼티로 보관됨

**12.5.2 인수 확인**

객체를 통해 인수 개수를 확인할 수 있음

**12.5.3 매개변수의 최대 개수**

: 매개변수의 개수는 적을수록 좋음

: 함수는 한 가지 일만 해야 하므로 가급적 작게 만들어야 함

**12.5.4 반환문**

: return키워드와 표현식으로 이루어짐

→ return 키워드를 이용하여 사용 가능한 모든 값 반환

→ 반환문 이후의 다른 문은 실행되지 않고 무시

→ return 키워드 뒤에 반환값으로 이용할 표현식을 지정하지 않으면 undefined가 반환됨

### 12.6 참조에 의한 전달과 외부 상태의 변경

 : 함수가 외부 상태를 변경하면 상태 변화를 추적하기 어려워짐

→ **객체를 불변 객체로 만들어 사용하는 방법** 

     : 객체의 상태 변경을 막음  

→ **순수 함수**

     : 외부 상태 변경 X, 의존 X 함수

### 12.7 다양한 함수 형태

**12.7.1 즉시 실행 함수**

: 단 한번만 호출

```jsx
(function () {
  var a = 3;
  var b = 5;
  return a * b;
}())  // 익명 즉시 실행 함수 
```

```jsx
(function foo() {
  var a = 3;
  var b = 5;
  return a * b;
}())

foo();  // 기명 즉시 실행 함수
```

→ 일반적으로 익명 함수를 사용하지만, 기명 함수도 사용할 수 있음(함수 리터럴로 평가됨, 다시 호출X)

**12.7.2 재귀 함수**

: 함수가 자기 자신을 호출하는 함수

: 자신을 무한 재귀 호출함 → 탈출 조건 필수 

```jsx
function countdown(n) {
  for (var i = n; i >= 0; i--) console.log(i);
}

countdown(10);
```

**12.7.3 중첩 함수(내부 함수)**

: 함수 내부에 정의된 함수 , 헬퍼 함수

**12.7.4 콜백 함수**

: repeat 함수는 매개변수를 통해 전달받은 숫자만큼 반복하여 호출함, 한가지의 일만 할 수 있음

: 매개 변수를 통해 함수의 외부에서 콜백 함수를 전달받은 함수 = 고차 함수, 고차 함수는 콜백 함수를 자신의 일부분으로 합성함, 콜백 함수의 호출 시점을 결정해서 호출함

**12.7.5 순수 함수와 비순수 함수**

순수 함수 : 부수 효과가 없는 함수 → 인수에게만 의존해 값을 생, 인수의 불변성 유지, 함수의 외부 상태 변경X

비순수 함수 : 부수 효과가 있는 함수 → 외부 상태를 변경하는 부수 효과O
