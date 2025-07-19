# ⏱️ 자바스크립트의 비동기 처리 (Asynchronous JavaScript)

자바스크립트는 싱글 스레드 기반 언어이지만, 비동기 처리를 통해 동시에 여러 작업을 수행하는 것처럼 동작할 수 있습니다. 이 문서는 \*\*비동기 처리의 개념, 방식, 그리고 주요 문법 (콜백, 프로미스, async/await)\*\*에 대해 설명합니다.

---

## ✅ 비동기 처리란?

- 동기 처리: 작업이 순차적으로 실행 → 앞 작업이 끝나야 다음 작업 가능
- 비동기 처리: 작업을 백그라운드에서 실행하고, 나중에 완료되면 처리

```js
console.log('A');
setTimeout(() => console.log('B'), 1000);
console.log('C');
// 출력: A → C → (1초 후) B
```

---

## 🔄 비동기 처리 방식

자바스크립트는 다음과 같은 방식을 통해 비동기 처리를 수행합니다:

### 1. 콜백 함수 (Callback)

- 가장 기본적인 비동기 처리 방법
- 함수의 인자로 다른 함수를 전달

```js
function getData(callback) {
  setTimeout(() => {
    callback('데이터 도착!');
  }, 1000);
}

getData((result) => {
  console.log(result); // 데이터 도착!
});
```

- ❗️문제점: **콜백 지옥(callback hell)** 발생 가능

```js
doSomething((result1) => {
  doAnother(result1, (result2) => {
    doFinal(result2, (result3) => {
      console.log(result3);
    });
  });
});
```

---

### 2. 프로미스 (Promise)

- ES6에서 도입된 비동기 처리 객체
- `resolve`, `reject`의 상태 변화에 따라 `.then()` 또는 `.catch()` 실행

```js
const promise = new Promise((resolve, reject) => {
  setTimeout(() => resolve('성공!'), 1000);
});

promise.then((msg) => console.log(msg)); // 1초 후 '성공!'
```

- ❗️장점: 가독성 향상, 체이닝 가능
- ❗️단점: 긴 체이닝은 여전히 복잡할 수 있음

---

### 3. async/await (ES2017)

- 프로미스를 더 **동기 코드처럼 보이게 작성**할 수 있는 문법
- `async` 함수 안에서만 `await` 사용 가능

```js
function fetchData() {
  return new Promise((resolve) => {
    setTimeout(() => resolve('데이터 완료!'), 1000);
  });
}

async function main() {
  const result = await fetchData();
  console.log(result); // 데이터 완료!
}

main();
```

- ❗️장점: 깔끔하고 직관적인 코드 구조
- ❗️주의: `await`은 **동기처럼 보이지만 실제론 비동기**임

---

## 🔁 이벤트 루프와 태스크 큐

- 비동기 처리를 가능하게 하는 JS의 핵심은 **이벤트 루프(Event Loop)**
- Web API에서 비동기 작업이 완료되면, 콜백은 \*\*태스크 큐(Task Queue)\*\*에 들어감
- 콜 스택이 비면, 이벤트 루프가 큐에서 콜백을 꺼내 실행

```js
console.log('start');

setTimeout(() => {
  console.log('timeout');
}, 0);

Promise.resolve().then(() => {
  console.log('promise');
});

console.log('end');
// 출력: start → end → promise → timeout
```

---

## ✅ 요약 비교표

| 방식        | 장점                   | 단점                              |
| ----------- | ---------------------- | --------------------------------- |
| 콜백        | 간단한 구조            | 콜백 지옥 발생 가능               |
| 프로미스    | 체이닝, 예외 처리 편리 | 긴 체인 시 가독성 하락            |
| async/await | 동기적 표현, 직관적    | `try/catch` 필요, async 제한 있음 |

---

> 🚀 비동기 처리는 자바스크립트 실무에서 매우 중요합니다. 콜백, 프로미스, async/await의 차이를 명확히 이해하고 상황에 맞게 선택하세요!
