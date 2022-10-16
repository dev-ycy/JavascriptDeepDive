## 함수의 구분

```
  var foo = function () {
    return 1;
  }
// 일반 함수 호출
foo(); // -> 1

// 생성자 함수로서 호출
new foo(); // foo {}

// 메서드로서 호출
var obj = { foo };
obj.foo(); // 1
```

- es6 이전의 모든 함수는 일반 함수로서 호출 가능하고, 생성자 함수로서 호출할 수 있다.
- ES6 이전의 모든 함수는 callable 이면서 constructor 이다.

```
  var obj = {
      x : 10,
      f: function () { return this.x }
  }

  obj.f();

  var bar = obj.f;

  bar();

  new obj.f();
```

- 객체에 바인딩 된 함수를 생성자 함수로 호출하는 방식이 문법상 가능하다.
- 성능면에서 문제가 있다. 사용하지 않는것을 권장한다.
- 객체에 바인딩된 함수가 프로토타입 프로퍼티를 가지며, 객체도 생성한다는것을 의미한다.
- 콜백함수도 constructor 이기 때문에 불필요한 프로토타입 객체를 생성한다.
- es6는 이러한 문제를 해결하기 위해 사용 목적에 따라 세가지 종류로 명확히 구분했다.
  - 이외에 async, 제너레이터 함수가 존재한다. 이에 대해서는 46장에서 자세히 다룬다.

|  ES6 함수  | constructor | prototype | super | arguments |
| :--------: | :---------: | :-------: | :---: | :-------: |
|  일반함수  |      O      |     o     |   X   |     o     |
|   메서드   |      X      |     X     |   O   |     o     |
| 화살표함수 |      X      |     X     |   X   |     x     |

---

## 메서드

```
const obj = {
  x :1,
  foo() { return this.x },
  bar : function { return this.x }
};

// foo 는 메서드다.
// bar 는 바인딩 된 일반함수다.
```

- 메서드는 인스턴스를 생성할 수 없는 non-constructor다. 생성자 함수로 호출 불가하다.
- 프로토타입 프로퍼티가 없고 프로토타입도 생성하지 않는다.
- 메서드는 자신을 바인딩한 객체를 가리키는 내부슬롯 [[HomeObject]] 를 갖는다.
- super 참조는 내부슬롯을 사용하여 수퍼클래스의 메서드를 참조하므로 내부슬롯을 갖는 메서드는 super 키워드를 사용할 수 있다.

## 화살표 함수

- function 키워드 대신 화살표를 사용하여 간략하게 함수를 정의할 수 있다.
- 내부 동작도 기존의 함수보다 간략하다.
- 콜백 함수 내부에서 this가 전역 객체를 가리키는 문제를 해결하기 위한 대안으로 유용하다.

### 화살표 함수 정의

```
const multiply = (x, y) => x * y;
multiply(2, 3); // -> 6
```

- 매개변수 선언

  ```
  const arrow1 = (x, y) => { ... };
  const arrow1 = x => { ... };
  const arrow1 = () => { ... };
  ```

  - 매개변수가 여러개인 경우 소괄호 안에 선언한다.
  - 매개변수가 한개인 경우 소괄호 생략이 가능하다.
  - 매개변수가 없다면 소괄호를 생략할 수 없다.

- 함수 몸체 정의

  ```
  const spower = x => x ** 2;
  const spower = x => { return x ** 2 };

  const create = (id, content) => ({ id, content });
  const create = (id, content) => { return { id, content }};

  const poerson = (name => ({
    sayHi() { return `Hi? MY name is ${name}` }
  }))("Lee");
  ```

  - 하나의 문으로 구성되면 몸체를 감싸는 중괄호 {}를 생략할 수 있다.
  - 함수 몸체 내부의 문이 값으로 평가될 수 있는 표현식인 문이라면 암묵적으로 반환된다.
  - 함수 몸체를 감싸는 중괄호를 생략한 경우 표현식이 아닌 문이라면 에러가 발생한다.
  - 객체 리터럴을 반환한다면 소괄호로 감싸주어야 한다.
  - 즉시실행 함수로 사용할 수 있다.

### 화살표 함수와 일반 함수의 차이

- 화살표 함수는 non-constructor 이다.
- 중복된 매개변수 이름을 선언할 수 없다.
- 화살표 함수는 함수 자체의 this, arguments, super, new.target 바인딩을 갖지 않는다.

### this

- 콜백 함수 내부의 this 문제를 해결하기 위해 의도적으로 설계되었다.
- this 바인딩은 함수가 어떻게 호출되었는지에 따라 동적으로 결정된다.(22장)

```
class Prefixer {
  constructor (prefix) {
    this.prefix = prefix
  }

  // 일반함수
  add(arr) {
    const that = this;
    return arr.map(function (item) {
      retrun that.prefix + item;
    })
  }

  // 화살표 함수
  add(arr) {
    return arr.map(item => this.prefix + item)
  }
}

```

- es6 이전엔 위의 일반함수처럼 this 를 사용했다. 이 외에도 this를 다시 바인딩해주는 방법이 있다. (p.479 ~ p.480 예제 참고)
- 화살표 홤수는 위와 같이 사용하여 this 문제를 해결할 수 있다.
- 함수 자체의 this 바인딩을 갖지 않는다.
- this 를 참조하면 상위 스코프의 this를 그대로 참조한다.
- 이를 lexical this 라 한다.
- 마치 렉시컬 스코프와 같이 정의된 위치에 의해 결정된다는 것을 이야기한다.

```
class Person {
  name = "Lee",

  // 화살표함수
  sayHi = () => console.log(`Hi ${this.name}`)
  // 메서드 축약 표현
  sayHi { console.log(`Hi ${this.name}`) }
};
```

- 클레스 필드 정의 제안으로 위처럼 사용할 수 있다.
- 하지만 클래스 필드에 할당한 화살표 함수는 프로토타입 메서드가 아니라 인스턴스 메서드가 된다.
- 메서드를 정의할 때는 ES6 메서드 축약 표현으로 정의한 ES6 메서드를 사용하는 것이 좋다.

### super

- 함수자체의 super 바인딩을 가지지 않는다.
- this 와 같이 상위 스코프의 super를 참조한다.

### arguments

- 함수자체의 arguments 바인딩을 가지지 않는다.
- this, super 와 같이 상위 스코프의 arguments를 참조한다.

## Rest 파라미터

### 기본 문법

```
  function foo (...rest) {
    const [ param, ...rest2 ] = rest
    console.log(rest)
    console.log(param)
    console.log(rest2)
  }
  foo(1,2,3,4,5)
```

- rest 파라미터는 이름 그대로 먼저 선언된 매개변수에 할당된 인수를 제외한 나머지 인수들로 구성된 배열이 할당된다.
- 반드시 마지막 파라미터여야 하며, 하나만 선언할 수 있다.

### rest 파라미터와 arguments 객체

- ES5 에서 사용된 arguments 는 ES6에서도 사용 가능하다.
- 일반 함수에서는 모두 사용 가능하지만, 화살표 함수에서는 arguments 를 사용할 수 없다.

## 매개변수 기본값

```
const foo = (x, y = 2 ) => x + y
console.log(foo(2)) // 4

const sayHi = (name = "kim") => `Hi ${name}`
console.log(sayHi)
```

- 매개변수를 전달하지 않으면 해당 매개변수의 값은 undefined 다.
- 이를 방지하기 위해 기본값을 할당해줄 수 있다.
