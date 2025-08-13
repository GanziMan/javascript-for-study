## 변수 특징

- `let`과 `var`는 변수를 선언할 때 사용되지만, 중요한 차이가 있다.
  - `var`는 함수 스코프를 가지며, 선언 전에 참조해도 `undefined`가 반환되는 호이스팅(hoisting)이 발생한다.
  - `let`은 블록 스코프를 가지며, 선언 전에 참조하면 참조 오류(ReferenceError)가 발생한다.

```javascript
console.log(a); // undefined (var 호이스팅)
var a = 10;

console.log(b); // ReferenceError: Cannot access 'b' before initialization
let b = 20;
```

- `const`로 선언한 변수는 재할당이 불가능하지만, 객체나 배열 같은 참조 타입의 내부 값은 변경할 수 있다.

```javascript
const obj = { name: "Alice" };
obj.name = "Bob"; // 가능
// obj = {}; // 오류 발생

const arr = [1, 2, 3];
arr.push(4); // 가능
// arr = []; // 오류 발생
```

## null과 undefined 차이

- `undefined`는 변수가 선언되었지만 값이 할당되지 않은 상태를 의미한다.
- `null`은 의도적으로 값이 없음을 나타내는 값이다.

```javascript
let x;
console.log(x); // undefined

let y = null;
console.log(y); // null
```

## == 와 === 차이

- `==`는 타입 변환 후 값 비교를 수행하는 느슨한 동등 연산자이다.
- `===`는 타입과 값이 모두 같은지 비교하는 엄격한 동등 연산자이다.

```javascript
console.log(0 == "0"); // true
console.log(0 === "0"); // false
```

## 스코프(scope)와 스코프 체이닝

- 스코프는 변수의 유효 범위를 의미한다.
- 자바스크립트는 렉시컬 스코프(lexical scope)를 사용하며, 내부 함수는 외부 함수의 변수에 접근할 수 있다.
- 스코프 체이닝은 변수 탐색 시 현재 스코프에서 찾지 못하면 상위 스코프로 올라가서 찾는 과정이다.

```javascript
function outer() {
  let x = 10;
  function inner() {
    console.log(x); // 10 (outer의 x에 접근)
  }
  inner();
}
outer();
```

## for...in, for...of 차이와 사용 예시

- `for...in`은 객체의 열거 가능한 속성 이름(키)을 순회할 때 사용한다.
- `for...of`는 이터러블 객체(배열, 문자열 등)의 값을 순회할 때 사용한다.

```javascript
const obj = { a: 1, b: 2 };
for (const key in obj) {
  console.log(key); // a, b
}

const arr = [10, 20, 30];
for (const value of arr) {
  console.log(value); // 10, 20, 30
}
```

## continue, break, label

- `continue`는 반복문의 현재 반복을 건너뛰고 다음 반복으로 넘어간다.
- `break`는 반복문을 즉시 종료한다.
- `label`은 중첩된 반복문에서 특정 반복문을 제어할 때 사용한다.

```javascript
for (let i = 0; i < 5; i++) {
  if (i === 2) continue; // i가 2일 때 건너뜀
  if (i === 4) break; // i가 4일 때 반복 종료
  console.log(i); // 0,1,3
}

outerLoop: for (let i = 0; i < 3; i++) {
  innerLoop: for (let j = 0; j < 3; j++) {
    if (i === 1 && j === 1) break outerLoop; // outerLoop 종료
    console.log(i, j);
  }
}
```

## 함수의 일급 객체 성질, 콜백, 고차함수, 커링

- 자바스크립트 함수는 일급 객체로, 변수에 할당, 인자로 전달, 반환값으로 사용할 수 있다.
- 콜백 함수는 다른 함수에 인자로 전달되어 실행되는 함수이다.
- 고차 함수는 함수를 인자로 받거나 함수를 반환하는 함수이다.
- 커링(currying)은 여러 인자를 받는 함수를 인자를 하나씩 받는 함수들의 연속으로 변환하는 기법이다.

```javascript
function greet(name) {
  return function (message) {
    console.log(`${name}, ${message}`);
  };
}

const greetAlice = greet("Alice");
greetAlice("Hello!"); // Alice, Hello!

// 고차 함수 예시
function map(arr, fn) {
  const result = [];
  for (const item of arr) {
    result.push(fn(item));
  }
  return result;
}

const numbers = [1, 2, 3];
const doubled = map(numbers, (x) => x * 2);
console.log(doubled); // [2, 4, 6]
```

## arguments와 rest parameter

- `arguments` 객체는 함수에 전달된 인자들을 배열처럼 접근할 수 있게 해준다. 하지만 배열이 아니므로 배열 메서드를 직접 사용할 수 없다.
- Rest 파라미터는 함수 선언 시 `...` 문법으로 가변 인자들을 배열로 받을 수 있다.

```javascript
function sum() {
  let total = 0;
  for (let i = 0; i < arguments.length; i++) {
    total += arguments[i];
  }
  return total;
}

console.log(sum(1, 2, 3)); // 6

function sumRest(...numbers) {
  return numbers.reduce((acc, cur) => acc + cur, 0);
}

console.log(sumRest(1, 2, 3)); // 6
```

## 재귀 함수와 스택 오버플로우 주의

- 재귀 함수는 자기 자신을 호출하는 함수이다.
- 종료 조건이 없거나 너무 깊은 재귀 호출은 스택 오버플로우를 발생시킨다.

```javascript
function factorial(n) {
  if (n <= 1) return 1;
  return n * factorial(n - 1);
}

console.log(factorial(5)); // 120
```

- 재귀 호출 시 종료 조건을 반드시 명확히 해야 한다.

## IIFE (즉시 실행 함수)와 용도

- IIFE(Immediately Invoked Function Expression)는 선언과 동시에 실행되는 함수이다.
- 주로 변수의 유효 범위를 제한하거나 초기화 코드를 실행할 때 사용한다.

```javascript
(function () {
  const temp = "IIFE 내부 변수";
  console.log(temp);
})();

// temp는 외부에서 접근 불가
```

## 원시 타입 vs 참조 타입, 불변성

- 원시 타입(Primitive)은 `number`, `string`, `boolean`, `null`, `undefined`, `symbol`, `bigint` 등이 있다.
- 참조 타입(Reference)은 객체, 배열, 함수 등이다.
- 원시 타입은 값 자체가 저장되고 불변(immutable)이다.
- 참조 타입은 메모리 주소가 저장되고, 내부 값은 변경 가능하다.

```javascript
let str = "hello";
str[0] = "H"; // 불가능, 문자열은 불변
console.log(str); // hello

const arr = [1, 2, 3];
arr[0] = 10; // 가능
console.log(arr); // [10, 2, 3]
```

## 객체 메서드 정의 방식과 this 주의사항

- 객체 메서드는 함수 표현식, 축약 메서드, 화살표 함수 등으로 정의할 수 있다.
- 화살표 함수는 자신의 `this`를 가지지 않고, 상위 스코프의 `this`를 사용하므로 메서드로 사용할 때 주의해야 한다.

```javascript
const obj = {
  name: "Alice",
  greet: function () {
    console.log(`Hello, ${this.name}`);
  },
  greetShort() {
    console.log(`Hi, ${this.name}`);
  },
  greetArrow: () => {
    console.log(`Hey, ${this.name}`); // this는 상위 스코프 (전역)
  },
};

obj.greet(); // Hello, Alice
obj.greetShort(); // Hi, Alice
obj.greetArrow(); // Hey, undefined (or 전역 객체의 name)
```
