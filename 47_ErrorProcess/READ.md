# 47. 에러처리
## 47.1 에러처리의 필요성
- 에러는 언제나 발생할 수 있따. 발생한 에러를 대처하지 않고 방치하면 프로그램은 강제 종료된다.
- try...catch문을 사용해 발생한 에러를 적절하게 대응하면 강제 종료되지 않고 계속해서 코드를 실행시킬 수 있다.
```
console.log("[Start]");

try{
    foo();
}catch(error){
    console.log.error("[에러발생]", error);
    // [에러 발생] ReferenceError: foo is not defined
}

//발생한 에러에 적절한 대응을 하면 프로그램이 강제 종료되지 않는다.
console.log("[End]");
```

- 직접적으로 에러를 발생시키지 않는 '예외'적인 상황이 발생할 수도 있다. 이 상황에 대처를 제대로 하지 않으면 에러로 이어질 가능성이 크다.

```
// DOM에 button 요소가 존재하지 않으면 querySelector 메서드는 에러를 발생시키지 않고 null을 반환한다.
const $button = document.querySelector("button");//null

$button.classList.add("disabled");
// TypeError: Cannot read property 'classList' of null
```
- 위 예제의 querySelector 메서드는 인수로 전달한 문자열이 css선택자 문법에 맞지 않는 경우 에러를 발생시킨다.

하지만 querySelector 메서드는 인수로 전달한 css 선택자 문자열로 DOM에서 요소 노드를 찾을수 ㅇ벗는 경우 에러를 발생시키지 않고 null을 반환한다. 이 때 if 문으로 querySelector 메서드의 반환값을 확인하거나 단축 평가 또는 옵셔널 체이닝 연산자 ?.를 사용하지 않으면 다음 처리에서 에러로 이어질 가능성이 크다.

```
// DOM에 button 요소가 존재하지 않으면 querySelector 메서드는 에러를 발생시키지 않고 null을 반환한다.
const $button = document.querySelector("button"); // null
$button?.classList.add("disabled");
```
- 이처럼 에러나 예외상황에 대응하지 않으면 프로그램은 강제 종료될 것이다. 에러나 예외적인 상황은 너무나 다양하기 때문에 아무런 조치 없이 프로그램이 강제 종료된다면 원인을 파악하여 대응하기가 어렵다.
에러가 발생하지 않는 코드를 작성하는것은 불가능하므로 우리가 작성한 코드는 언제나 예외상황이 발생할 가능성이 있다는 것을 전제하고 이에 대응하는 코드를 작성하는 것이 중요하다.


## 47.2 try...catch...finally문
- 에러처리를 구현하는 방법 크게 두가자.
1. querySelector나 Array#find 메서드처럼 예외적 상황이 발생하면 반환하는 값(null 또는 -1)을 if문이나 단축 평가 또는 옵셔널 체이닝 연산자를 통해 확인해서 처리하는 방법
2. 에러처리 코드를 미리 등록해두고 에러가 발생하면 에러 처리 코드로 점프하도록 하는 방법

- try...catch...finally문은 두 번째 방법
- 일반적으로 이 방법으로 에러 처리를 함.
- finally문은 생략 가능.
```
try{
    // 실행할 코드(에러가 발생할 가능성이 있는 코드)
}catch(err){//err변수이름은 아무 이름이나 상관없음
    // try 코드 블록에서 에러가 발생하면 이 코드 블록의 코드가 실행된다.
    // err에는 try 코드 블록에서 발생한 Error 객체가 전달된다.
}finally{
    //에러 발생과 상관없이 반드시 한 번 실행된다.
}
```

- try...catch...finally문을 실행하면 먼저 try코드 블록이 실행된다.
- 이 때, try코드 블록서 에러가 발생하면
- catch문의 err변수에 전달되고 catch문이 실행된다.
- err변수는 try블록에서 에러가 발생하면 생성되고, catch 블록에서만 유효하다.
- finally블록은 에러발생과 상관없이 반드시 한 번 실행된다.
- try...catch...finally문으로 에러를 처리하면 프로그램이 강제 종료되지 않음.

```
console.log("[Start]");

try{
    // 실행할 코드(에러가 발생할 가능성이 있는 코드)
    foo();
} catch(err){
    // try 코드 블록에서 에러가 발생하면 이 코드 블록의 코드가 실행된다.
    // err에는 try 코드 블록에서 발생한 Error객체가 전달된다.
    console.error(err); // Reference: foo is not defied;

}finally{
    //에러발생과 상관없이 반드시 한 번 실행된다.
    console.log("finally");
}
```
## 47.3 Error 객체
- Error 생성자 함수는 에러 객체를 생성한다. Error 생성자 함수에는 에러를 상세히 설명하는 에러메세지를 인수로 전달할 수 있다.
```
const error = new Error("invalid");
```
- Error 생성자 함수가 생성한 에러 객체는 message 프로퍼티와 stack 프로퍼티를 갖는다. 
- message 프로퍼티의 값 : Error 생성자 함수에 인수로 전달한 에러 메시지.
- stack프로퍼티의 값 : 에러를 발생시킨 콜스택의 호출정보를 나타내는 문자열이며 디버딩 목적으로 사용.
- 자바스크립트는 Error 생성자 함수를 포함해 7가지의 에러 객체를 생성할 수 있는 Error 생성자 함수를 제공한다.
- 이 생성자 함수가 생성한 여러 객체의 프로토타입은 모두 Error.prototype을 상속받는다.

  | 생성자함수 | 인스턴스 |
  | ----------- | ------------------------------ |
  | Error | 일반적 에러 객체 |
  | SyntaxError | 자바스크립트 문법에 맞지 않는 문을 해석할 때 발생하는 에러 객체 |
  | ReferenceError | 참조할 수 없는 식별자를 참조했을 때 발생하는 에러 객체 |
  | TypeError | 피연산자 또는 인수의 데이터 타입이 유효하지 않을 때 발생하는 에러 객체 |
  | RangeError | 숫자값의 허용 범위를 벗어났을 때 발생하는 에러 객체 |
  | URIError | encodeURI 또는 decodeURI 함수에 부적절한 인수를 전달했을 때 발생하는 에러 객체 |
  | EvalError | eval 함수에서 발생하는 에러 객체 |

```
1 @ 1; // SyntaxError
foo(); // ReferenceError
null.foo; // TypeError
new Array(-1); // RangeError
decodeURIComponent('%'); // URIError
```

## 47.4 throw문
- Error생성자 함수로 에러 객체를 생성한다고 에러가 발생하는 것은 아님. 즉 에러 객체 생성과 에러 발생은 의미가 다름.
- 에러를 발생시키려면 try 코드 블록에서 throw문으로 에러 객체를 던져야 한다.
```
throw 표현식;
```
- throw문의 표현식은 어떤 값이라도 상관없지만 일반적으로 에러 객체를 저장한다. 에러를 던지면 catch문의 에러 변수가 생성되고 던져진 에러 객체가 할당된다.
- catch코드가 실행된다.
```
try{
    // 에러 객체를 던지면 catch 코드 블록이 실행되기 시작한다.
    throw new Error("something wrong");
}catch(error){
    console.log(error);
}
```


## 47.5 에러의 전파
- 45장 "프로미스"의 45.1.2절 "에러 처리의 한계"에서 살펴본 바와 같이 에러는 호출자 방향으로 전파된다. 즉 콜 스택의 아래 방향(실행 중인 실행 컨텍스트가 푸시되기 직전에 푸시된 실행 컨텍스트 방향)으로 전파된다.

```
const foo = () => {
    throw Error("foo에서 발생한 에러"); // 4
};

const var = () => {
    foo(); // 3
};

const baz = () => {
    bar(); // 2
};

try{
    baz(); // 1
} catch(err){
    console.log.error(err);
}

```
- 1에서 함수를 호출하면 2에서 bar함수가 호출되고 3에서 foo함수가 호출되고 foo함수는 4에서 에러를 throw한다. 이 때 foo함수가 throw한 에러는 다음과 같이 호출자에게 전파되어 전역에서 캐치된다.
![throwProcess]](./throw과정.png)
- 이처럼 throw된 에러를 캐치하지 않으면 호출자 방향으로 전파된다.
- 이 때 throw된 에러를 어디서도 캐치하지 않으면 프로그램은 강제종료된다.
- 비동기 함수인 setTimeout이나 프로미스 후속 처리 메서드의 콜백 함수는 호출자가 없다는 것이다. setTimeout이나 프로미스 후속 처리 메서드의 콜백 함수는 태스크 큐나 마이크로태스크 큐에 일시 저장되었다가 콜 스택이 비면 이벤트 루프에 의 콜 스택으로 푸시되어 실행된다.
- 콜 스택에 푸시된 콜백함수의 실행컨텍스트는 콜 스택의 가장 하부에 존재하게 된다. 따라서 에러를 전파할 호출자가 존재하지 않는다.
