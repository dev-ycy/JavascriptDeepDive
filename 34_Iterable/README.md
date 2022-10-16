# 34 이터러블
## 34.1 이터레이션 프로토콜
- ES6에서 도입된 이터레이션 프로토콜은 순회 가능한 데이터 컬렉션(자료구조)를 만들기 위해 ECMAScript사양에 정의하여 미리 약속한 규칙
- ES6 이전에 순회 가능한 데이터 컬렉션, 즉 배열, 문자열, 유사 배열 객체, DOM 컬렉션 등은 통일된 규약없이 각자 나름의 구조를 가지고 for문, for...in문, forEach메서드 등 다양한 방법으로 순회할 수 있었다.
- ES6에서는 순회 가능한 데이터 컬렉션을 이터레이션 프로토콜을 준수하는 이터러블로 통일하여 for...of문, 스프레드 문법, 배열 디스트럭처링 할당의 대상으로 사용할 수 있도록 일원화
- 이터레이션 프로토콜에는 이터러블 프로콜과 이터레이터 프로토콜이 있다.

1. 이터러블 프로토콜
- Well-known Symbole인 Symbol.iterator를 프로퍼티 키로 사용한 메서드를 직접 구현하거나 프로토타입 체인을 통해 상속받은 Symbol.iterator메서드를 호출하면 이터레이터 프로토콜을 준수한 이터레이터를 반환한다. 이러한 규약을 이터러블 프로토콜이라 하며 이터러블 프로토콜을 준수한 객체를 이터러블이라 함. 이터러블은 for...of 문으로 순회할 수 있으며 스프레드 문법과 배열 디스트럭처링 할당의 대상으로 사용할 수 있다.

2. 이터레이터 프로토콜
- 이터러블의 Symbol.interator 메서드를 호출하면 이터레이터 프로토콜을 준수한 이터레이터를 반환.
- next 메서드를 호출하면 이터러블을 순회하며, value와 done 프로퍼티를 갖는 이터레이터 리절트 객체를 반환
- 이터레이터 프로토콜을 준수한 객체를 이터러블이라 함
- 이터레이터는 이터러블의 요소를 탐색하기위한 포인터 역할을 한다.

### 34.1.1 이터러블
- Symbol.iterator를 프로퍼티 키로 사용한 메서드를 직접 구현하거나 프로토타입 체인을 통해 상속받은 객체를 말함

```
const isIterable = v => v !== null && typeof v[Symbol.iterator] === 'function';

//배열, 문자열, Map, Set등은 이터러블이다.
isIterable([]); // true
isIterable(''); // true
isIterable(new Map()); // true
isIterable(new Set()); // true
isIterable({}); // false 
```

- 이터러블은 for...of문으로 순회
- 스프레드 문법과 배열 디스트럭처링 할당 대상으로 사용
```
const array = [1, 2, 3];

//배열은 Array.prototype의 Symbol.iterator 메서드를 상속받는 이터러블이다.
console.log(Symbol.iterator in array); // true

// for...of 배열로 순회
for(const item of array){
    console.log(item);
}

// 스프레드 문법의 대상으로 사용
consol.elog([...array]); // [1, 2, 3]

// 디스트럭처링 할당 대상으로 사용
const [a, ...rest] = array;
console.log(a, rest); // 1, [2, 3]
```

- Symbole.iterator 메서드를 직접 구현하지 않거나 상속받지 않은 일반 객체는 이터러블 프로토콜을 준수한 이터러블이 아님
- 일반 객체는 for...of문으로 순회할 수 없고, 스프레드 문법과 배열 디스트럭처링 할당 대상으로 사용할 수 없음

- 일반 객체에 스프레드 문법 사용을 허용하는 예외도 있긴 함.
- 일반 객체도 이터러블 프로토콜을 준수하도록 구현하면 이터러블이 됨 34.6절 참고

### 34.1.2 이터레이터
- 이터러블의 Symbole.iterator 메서드가 반환한 이터레이터는 next메서드를 가짐
```
//배열은 이터러블 프로토콜을 준수한 이터러블
const array = [1, 2, 3];

// Symbol.iterator 메서드는 이터레이터를 반환
const iterator = array[Symbol.iterator]();

//Symbol.iterator 메서드가 반환한 이터레이터는 next메서드를 가짐
console.log('next' in iterator); // true
```

- next는 각 요소를 순회하는 포인터 역할
- 이터레이터 리절트 객체 반환
```
//배열은 이터러블 프로토콜을 준수한 이터러블
const array = [1, 2, 3];

// Symbol.iterator 메서드는 이터레이터를 반환
const iterator = array[Symbol.iterator]();

//next는 이터레이터 리절트 객체를 반환
// 이터레이터 리절트 객체는 value, done 프로퍼티를 갖는 객체
console.log(iterator.next()); // {value : 1, done : false}
console.log(iterator.next()); // {value : 2, done : false}
console.log(iterator.next()); // {value : 3, done : false}
console.log(iterator.next()); // {value : undefined, done : true}
```

- value : 현재 순회 중인 이터러블 값
- done : 순회완료여부

## 34.2 빌트인 이터러블
- 자바스크립트는 이터레이션 프로토콜을 준수한 객체인 빌트인 프로토콜을 제공
- Array, String, Map, Set, TypedArray, arguments, DOM컬렉션

## 34.3 for...of
- for...of 문은 이터러블을 순회하며 이터러블 요소를 변수에 할당
- 프로퍼티 키가 심벌인 프로퍼티는 열거하지 않음
```
for(const item of [1,2,3]){
    // item 변수에 순차적으로 1,2,3이 할당됨
    console.log(item);
}
```
## 34.4 이터러블과 유사 배열 객체
- 유사배열객체 : 마치 배열처럼 인덱스로 프로퍼티 값에 접근, length프로퍼티를 갖는 객체.
- for문으로 순회할 수 있고, 인덱스로 프로퍼티 값에 접근 가능
- 이는 일반객체다. for...of문으로 순회할 수 없다.
- 단 arguments, NodeList, HTMLCollection은 유사배열 객체이면서 이터러블
- 배열도 마찬가지.
- 하지만 모든 유사배열객체가 이터러블인 것은 아니기 떄문에 Array.from메서드를 사용하여 간단하게 변환->유사배열객체 또는 이터러블을 인수로 전달받아 배열로 반환.

## 34.5 이터레이션 프로토콜의 필요성
- 이터레이션 프로토콜은 다양한 데이터 공급자가 하나의 순회방식을 갖도록 규정해 데이터 소비자가 효율적으로 다양한 데이터 공급자를 사용할 수 있도록 하는 데이터 소비자와 데이터 공급자를 연결하는 인터페이스

## 34.6 사용자 정의 이터러블 // 책 참고
### 34.6.1 사용자 정의 이터러블 구현
### 34.6.2 이터러블을 생성하는 함수
### 34.6.3 이터러블이면서 이터레이터인 객체를 생성하는 함수
### 34.6.4 무한 이터러블과 지연 평가


