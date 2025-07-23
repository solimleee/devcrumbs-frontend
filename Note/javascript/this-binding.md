# 🔗 자바스크립트의 this 바인딩

자바스크립트에서 `this`는 **함수가 호출될 때 결정되는 객체를 참조하는 키워드**입니다. 다양한 호출 방식에 따라 `this`가 다르게 바인딩되기 때문에 헷갈리기 쉬운 개념 중 하나입니다.

---

## ✅ 기본 개념

- `this`는 **실행 컨텍스트에 따라 동적으로 결정**됨
- 어디서 선언되었는지가 아니라, **어떻게 호출되었는지**에 따라 달라짐

---

## 📌 바인딩 규칙 5가지

### 1. **전역 컨텍스트 (기본 바인딩)**

```js
console.log(this); // 브라우저: window, Node.js: global
```

### 2. **메서드 호출 (묵시적 바인딩)**

```js
const obj = {
  name: '소림',
  greet() {
    console.log(this.name); // '소림'
  },
};

obj.greet(); // this → obj
```

### 3. **함수 호출 (명시적 바인딩 없음)**

```js
function sayName() {
  console.log(this.name);
}

const name = '전역 이름';
sayName(); // this → window (strict 모드에서는 undefined)
```

### 4. **명시적 바인딩 (call, apply, bind)**

```js
function introduce() {
  console.log(`저는 ${this.name}입니다.`);
}

const user = { name: '소림' };
introduce.call(user);  // 저는 소림입니다.
introduce.apply(user); // 저는 소림입니다.

const bound = introduce.bind(user);
bound(); // 저는 소림입니다.
```

### 5. **화살표 함수 (렉시컬 바인딩)**

- `this`가 **선언된 시점의 상위 스코프**를 참조함

```js
const person = {
  name: '소림',
  sayLater() {
    setTimeout(() => {
      console.log(this.name); // '소림' ← 화살표 함수는 person을 가리킴
    }, 1000);
  },
};

person.sayLater();
```

---

## ❗ this 바인딩 주의점

- 콜백 함수에서 일반 함수 사용 시 `this`가 의도와 다르게 바인딩됨
- 이벤트 핸들러, setTimeout 등에서 흔히 혼동

```js
const button = document.querySelector('#btn');

button.addEventListener('click', function () {
  console.log(this); // this → button
});

button.addEventListener('click', () => {
  console.log(this); // this → 외부 스코프 (보통 window)
});
```

---

## ✅ 요약

| 호출 방식            | this 값                                   |
| -------------------- | ----------------------------------------- |
| 전역에서 함수 호출   | window/global (strict 모드에선 undefined) |
| 객체의 메서드 호출   | 해당 객체                                 |
| call/apply/bind 호출 | 명시된 객체                               |
| 화살표 함수          | 상위 스코프의 this                        |

---

> 💡 this는 선언이 아니라 **실행 시점**에 따라 결정됩니다. 특히 콜백, 이벤트 핸들러, 비동기 코드에서 헷갈리기 쉬우니, 호출 맥락을 기준으로 판단하는 습관을 들이세요!
