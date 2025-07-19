# 📤 자바스크립트의 호이스팅 (Hoisting)

자바스크립트의 **호이스팅(Hoisting)**은 변수나 함수 선언이 **코드 실행 전에 해당 범위의 최상단으로 끌어올려지는 것처럼 동작**하는 자바스크립트의 동작 방식입니다.

---

## ✅ 개념 정리

- **선언부만 끌어올려지고, 할당은 끌어올려지지 않음**
- 변수, 함수 선언 모두 호이스팅 대상이지만 **`let`, `const`는 다르게 동작**

```js
console.log(a); // undefined
var a = 10;
```

→ 실제 동작:

```js
var a;
console.log(a); // undefined
a = 10;
```

---

## 🔍 변수 선언의 호이스팅 차이점

### var

- 선언만 호이스팅되며 초기화는 나중에 됨 → `undefined`

```js
console.log(a); // undefined
var a = 1;
```

### let / const

- 호이스팅은 되지만 \*\*Temporal Dead Zone(TDZ)\*\*로 인해 **참조 시 에러 발생**

```js
console.log(b); // ReferenceError
let b = 2;
```

---

## 📦 함수 선언과 표현식의 차이

### 함수 선언문 (Function Declaration)

- 전체 함수가 호이스팅되어 **어디서든 호출 가능**

```js
greet(); // Hello
function greet() {
  console.log('Hello');
}
```

### 함수 표현식 (Function Expression)

- 변수처럼 동작 → `undefined` 혹은 에러 발생

```js
greet(); // TypeError: greet is not a function
var greet = function () {
  console.log('Hello');
};
```

---

## 🚨 주의할 점

- `var`는 의도치 않은 값 접근 가능성 때문에 **사용을 지양**
- `let`과 `const`는 \*\*TDZ(Temporal Dead Zone)\*\*를 이해해야 안정적인 코드 작성 가능
- 함수 선언 vs 함수 표현식 구분은 면접에서도 자주 등장

---

## 💬 요약

| 항목        | 호이스팅 여부 | 초기화 시점      | 접근 가능 여부        |
| ----------- | ------------- | ---------------- | --------------------- |
| var         | O             | undefined        | 가능 (undefined)      |
| let         | O             | TDZ 이후         | 불가 (ReferenceError) |
| const       | O             | TDZ 이후         | 불가 (ReferenceError) |
| 함수 선언문 | O             | 전체 함수        | 가능                  |
| 함수 표현식 | 변수처럼 동작 | undefined or TDZ | 불가                  |

---

> 🧠 호이스팅은 JS 동작 원리를 이해하는 데 핵심적인 개념입니다. 예측 가능한 코드를 위해 항상 선언은 상단에, 표현식은 명확하게 사용하세요!
