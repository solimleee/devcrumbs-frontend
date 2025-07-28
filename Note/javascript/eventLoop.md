# 🔄 자바스크립트의 이벤트 루프 (Event Loop)

자바스크립트는 **싱글 스레드(Single-threaded)** 언어로, 한 번에 하나의 작업만 처리할 수 있습니다.  
그럼에도 비동기 처리가 가능한 이유는 바로 **이벤트 루프(Event Loop)** 메커니즘 덕분입니다.

---

## 📌 핵심 구성 요소

- **Call Stack (콜 스택)**  
  동기적인 함수 실행 순서를 저장하는 스택 구조입니다.

- **Web APIs (브라우저 API)**  
  `setTimeout`, `fetch`, `DOM 이벤트` 등의 비동기 작업은 브라우저가 따로 처리합니다.

- **Callback Queue (Task Queue / Message Queue)**  
  비동기 작업이 완료되면 그 콜백이 이 큐에 등록됩니다.

- **Event Loop (이벤트 루프)**  
  끊임없이 콜 스택이 비어 있는지 확인하고, 비어 있다면 콜백 큐에서 작업을 가져와 실행합니다.

---

## ⏱️ 동작 흐름

1. 동기 코드는 **Call Stack**에서 바로 실행됩니다.
2. `setTimeout`, `fetch` 같은 비동기 함수는 **Web API**에게 위임됩니다.
3. 비동기 작업이 완료되면 콜백이 **Callback Queue**에 등록됩니다.
4. **Call Stack**이 비워지면 **Event Loop**가 **Callback Queue**에서 콜백을 꺼내 실행합니다.

---

## 💡 예시 코드

```js
console.log('Start');

setTimeout(() => {
  console.log('Timeout Callback');
}, 0);

console.log('End');

예상 출력
Start
End
Timeout Callback
- setTimeout의 지연 시간이 0이어도 콜 스택이 비워질 때까지 대기합니다.
```

## 🧠 마이크로태스크 큐 (Microtask Queue)
- Promise.then, queueMicrotask 등은 Microtask Queue에 들어갑니다.
- Event Loop는 매 이벤트 루프 사이클마다 Microtask Queue를 먼저 처리합니다.
```
console.log('Start');

Promise.resolve().then(() => {
  console.log('Microtask');
});

setTimeout(() => {
  console.log('Macrotask');
}, 0);

console.log('End');

예상 출력
Start
End
Microtask
Macrotask
```

## 정리
| 용어              | 설명                       |
| --------------- | ------------------------ |
| Call Stack      | 동기 작업 저장 공간              |
| Web APIs        | 비동기 작업 위임 대상 (브라우저 환경)   |
| Callback Queue  | 완료된 비동기 콜백이 대기하는 큐       |
| Microtask Queue | Promise 등의 우선 처리 대기 큐    |
| Event Loop      | 콜 스택이 비면 큐에서 작업을 가져와 실행함 |

