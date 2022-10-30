# 46장 제너레이터와 async/await

## 46.1 제너레이터란?
- ES6에 도입된 제너레이터는 코드 블록의 실행을 일시중지했다가 필요한 시점에 재개할 수 있는 특수한 함수다.
- 일반 함수와 다른 제너레이터의 특징
    1. 제너레이터 함수는 `함수 호출자(caller)에게 함수 실행의 제어권을 양도(yield)`할 수 있다.
    2. 제너레이터 함수는 `함수 호출자와 함수의 상태를 주고받을 수 있다.`
    3. 제너레이터 함수를 호출하면 `제너레이터 객체를 반환`한다.

<br />

## 46.2 제너레이터 함수의 정의
- 제너레이터 함수는 `function*` 키워드로 선언, 하나 이상의 `yield` 표현식 포함한다.
```jsx
// 제너레이터 함수 선언문
function* genDecFunc() {
    yield 1;
}

// 제너레이터 함수 표현식
const genExpFunc = function* () {
    yield 1;
};

// 제너레이터 메서드
const obj = {
    * getObjMethod() {
    yield 1;
    }
};

// 제너레이터 클래스 메서드
class MyClass {
    * genClsMethod() {
    yield 1;
    }
}
```
- 애스터리스크(*)의 위치는 function 키워드와 함수 이름 사이라면 어디든지 상관없지만 function 키워드 바로 뒤에 붙이는 것을 권장한다.
- 제너레이터 함수는 화살표 함수로 정의할 수 없다.
- 제너레이터 함수는 new 연산자오 함께 생성자 함수로 호출할 수 없다.

<br />

## 46.3 제너레이터 객체
- 제너레이터 함수 호출시 일반 함수처럼 코드 블록 실행하는 것이 아니라 제너레이터 객체를 생성해 반환한다.
- 제너레이터 객체는 iterable 이면서 동시에 iterator 이다.
    - 즉, Symbol.iterator 메서드를 상속받는 이터러블인 동시에
    - { value, done } 프로퍼티를 갖는 이터레이터 리절트 객체를 반환하는 next 메서드를 소유한 이터레이터

- 제너레이터 객체는 추가적으로 이터레이터에는 없는 return, throw 메서드를 갖는다.
- `next` 메서드 호출  
    - 제너레이터 함수의 yield 표현식까지 코드 블록 실행
    - yield된 값을 value프로퍼티 값으로, false를 done프로퍼티 값으로 갖는 이터레이터 리절트 객체를 반환   
    { value: yield된 값, done: false}

- `return` 메서드 호출
    - 인수로 전달받은 값을 value 프로퍼티 값으로, true를 done 프로퍼티 값으로 갖는 이터레이터 리절트 객체를 반환  
    { value: 인수, done: true}
- `throw` 메서드 호출
    - 인수로 전달 받은 에러 발생시킴
    - undefined 를 value 프로퍼티 값으로, true 를 done 프로퍼티 값으로 갖는 이터레이터 갖는 이터레이터 리절트 객체를 반환  
    { value: undefined, done: true}

<br />

## 46.4 제너레이터의 일시 중지와 재개

```jsx
function* genFunc() {
    try {
        yield 1;
        yield 2;
        yield 3;
    } catch (e) {
        console.log(e);
    }
}

const generator = genFunc();

console.log(generator.next()); // {value: 1, done: false}
console.log(generator.next()); // {value: 2, done: false}
console.log(generator.next()); // {value: 3, done: false}

// 남은 yield 표현식이 없으므로 제너레이터 함수의 마지막까지 실행함.
console.log(generator.next()); // {value: undefined, done: true}
```


<br />

## 46.5. 제너레이터의 활용
1. 이터러블 구현
```jsx
// 무한 이터러블을 생성하는 제너레이터 함수
const infiniteFibo = (function* () {
    let [pre, cur] = [0, 1];

    while (true) {
        [pre, cur] = [cur, pre + cur];
        yield cur;
    }
})();

for (const num of infiniteFibo) {
    if (num > 1000) break;
    console.log(num); // 1 2 3 5 8 13 21 ... 987
}
```

2. 비동기 처리
- 제너레이터를 활용해 비동기 처리를 동기 처리처럼 구현할 수 있음
- (async/await 을 사용하면 됨)

<br />

## 46.6 async/await
- ES8 에서 제너레이터보다 간단하고 가독성 좋게 비동기 처리를 동기처리처럼 동작하도록 구현할 수 있는 async/await 가 도입

### async 함수
- await 키워드는 반드시 async 함수 내부에서 사용해야 한다.
- async 함수는 언제나 프로미스를 반환한다.
- async 함수가 명시적으로 프로미스를 반환하지 않더라도 async 함수는 암묵적으로 반환값을 resolve하는 프로미스를 반환한다.
- 클래스의 constructor 메서드는 async 함수가 될 수 없다.

```jsx
async function foo(n) { return n; }
foo(1).then(v => console.log(v)); // 1

const bar = async function(n) { return n; }
bar(2).then(v => console.log(v)); // 2

const baz = async n => n;
baz(3).then(v => console.log(v)); // 3

const obj = {
  async foo(b) { return n; }
}
obj.foo(4).then(v => console.log(v)); // 4

class MyClass {
  async bar(n) { return n; }
}
const myClass = new MyClass();
myClass.bar(5).then(v => console.log(v)); // 5
```

### await 키워드
- await 키워드는 프로미스가 settled 상태가 될 때까지 대기하다가 settled 상태가 되면 프로미스가 resolve한 처리 결과를 반환한다.
- await 키워드는 `반드시 프로미스 앞에서 사용`해야 한다.
- 모든 프로미스에 await 키워드를 사용하는 것은 옳지 않다.
    - 각각의 비동기 처리가 서로 연관성 없이 개별적으로 수행되도 상관없는 것이라면, 굳이 앞선 비동기 처리가 완료될 때까지 대기할 필요가 없기 때문
    - 따라서, 모든 프로미스가 `서로 연관되어 순서가 보장되면서 실행되어야 하는 경우에만 모든 프로미스의 await 를 적용`한다.

```jsx
async function foo() {
    const a = await new Promise((resolve) => setTimeout(() => resolve(1), 3000));
    const b = await new Promise((resolve) => setTimeout(() => resolve(2), 2000));
    const c = await new Promise((resolve) => setTimeout(() => resolve(3), 1000));

    console.log([a, b, c]);  // [1,2,3]
}

foo(); // 약 6초 소요
```

```jsx
async function foo() {
    const res = await Promise.all([
        new Promise((resolve) => setTimeout(() => resolve(1), 3000)),
        new Promise((resolve) => setTimeout(() => resolve(2), 2000)),
        new Promise((resolve) => setTimeout(() => resolve(3), 1000))
    ]);

    console.log(res);  // [1, 2, 3]
}

foo();  // 약 3초 소요
```

<br />

### 에러 처리
- async/await 에서 에러처리는 try...catch 문을 사용할 수 있다.

```jsx
// 1. 일반적인 try...catch
const foo = async () => {
  try {
    const wrongUrl = '...';

    const response = await fetch(wrongUrl);
    const data = await response.json();
  } catch (e) {
    console.error(e); // TypeError: Failed to fetch
  }
};

foo();

// 2. 후속처리 메서드 사용
const foo = async () => {
  const wrongUrl = '...';

  const response = await fetch(wrongUrl);
  const data = await response.json();
  return data;
};

foo()
  .then(console.log)
  .catch(console.error) // TypeError: Failed to fetch
```
