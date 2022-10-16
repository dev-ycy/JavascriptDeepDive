# 33 7번째 데이터 타입 Symbol
## 심벌이란?
- ES6에서 도입된 7번째 데이터 타입으로 변경 불가능한 원시 타입 값.
- 다른 값과 중복되지않는 유일무이한 값이다.
- 사용 목적 : 이름 충돌위험이 없는 유일한 프로퍼티 키를 만들기 위해 사용.
- 10.3절 프로퍼티에서 살펴본 바와 같이 프로퍼티 키로 사용할 수 있는 값은 빈 문자열을 포함하는 모든 문자열 또는 심벌 값이다.


## 33.2 심벌 값의 생성
### 33.2.1 Symbol함수
- umdefined, null타입의 값은 리터럴 표기법을 통해 값을 생성하는 반명, 심벌 값은 Symbol함수를 호출하여 생성.
- 이 때 생성된 심벌값은 노출되지 않아 확인할 수 없고, 다른값과 절대 중복되지 않는 유일무이한 값이다.

```
//Symbol함수를 호출하여 유일무이한 심벌 값을 생성한다.
const mySymbol = Symbol();
console.log(typeof mySymbol);//symbol

//심벌 값은 외부로 노출되지 않아 확인 할 수 없다.
console.log(mySymbol)//Symbol()
```

- 언뜻보면 생성자 함수로 객체를 생성하는 것처럼 보이지만 Symbol함수는 String, Number, Boolean 생성자 함수와 달리 new 연산자로 호출하지 않음. new 연산자와 함께 생성자 함수 또는 클래스를 호출하면 객체(인스턴스)가 생성되지만 심벌 값은 변경 불가능한 원시 값이다.
```
new Symbol();//TypeError: Symbol is not a constructor
```

- Symbol 함수는 선택적으로 문자열을 인수로 전달 가능.
- 이 문자열은 생성된 심벌 값에 대한 설명으로 디버깅 용도로만 사용되며 심벌 값 생성에 어떠한 영향도 주지 않음.
- 즉, 설명이 같더라도 생성된 심벌 값은 유일무이한 값임.
- 심벌 값도 문자열, 숫자, 불리언과 같이 객체처럼 접근하면 암묵적으로 래퍼 객체를 생성한다.
```
const mySymbol = Symbol('mySymbol');

//심벌도 래퍼 객체를 생성한다
console.log(mySymbol.description);//mySymbol
console.log(mySymbol.toString());//Symbol(mySymbol)
```

- 심벌 값은 암묵적으로 문자열이나 숫자 타입으로 변환되지 않는다.
- 불리언 타입으로는 변환된다.
```
const mySymbol = Symbol();
console.log(!!mySymbol);//true
if(mySymbol) console.log('mySymbol is not empty');
```

### 33.2.2 Symbol.for / Symbol.keyFor메서드
- Symbol.for메서드는 인수로 전달받은 문자열을 키로 사용하여 키와 심벌 값의 쌍들이 저장돼있는 전역 심벌 레지스트리에서 키와 일치하는 심벌 값을 검색한다.
1. 검색에 성공하면 새로운 심벌 값을 생성하지 않고, 검색된 심벌 값 반환
2. 검색에 실패하면 새로운 심벌 값 생성, Symbol.for메서드의 인수로 전달된 키로 전역 심벌 레지스트리에 저장한 후 생성된 심벌 값 반환

```
// 전역 심벌 레지스트리에 mySymbol이라는 키로 저장된 심벌 값이 없으면 새로운 심벌 값을 생성
const s1 = Symbol.for('mySymbol');
// 전역 심벌 레지스트리 mySymbol이라는 키로 저장된 심벌 값이 있으면 해당 심벌 값 반환
const s2 = Symbol.for('mySymbol');

cnsole.log(s1 === s2); true
```

- Symbol.for메서드를 사용하면 어플리케이션 전역에서 중복되지 않는 유일무이한 상수인 심벌 값을 단 하나만 생성하여 전역 심벌 레지스트리를 통해 공유할 수 있다.
- Symbol.keyFor 메서드를 사용하면 전역 심벌 레지스트리에 저장된 심벌 값의 키를 추출할 수 있다.
```
// 전역 심벌 레지스트리에 mySymbol이라는 키로 저장된 심벌 값이 없으면 새로운 심벌 값을 생성
const s1 = Symbol.for('mySymbol');
// 전역 심벌 레지스트리에 저장된 시멉 ㄹ값의 키를 추출
Symbol.keyFor(s1); //->mySymbol

// Symbol 함수를 호출하여 생성한 심벌 값은 전역 심벌 레지스트리에 등록되어 관리되지 않는다.
const s2 = Symbol('foo');
// 전역 심벌 레지스트리에 저장된 심벌 값의 키 추출
Symbol.keyFor(s2); // ->undefined
```

## 33.3 심벌과 상수
```
const Direction = {
    UP : 1,
    DOWN : 2,
    LEFT : 3,
    RIGHT : 4
}

const myDirection = Direction.UP;
if(myDirection === Direction.UP){
    console.log("You are going UP');
}
```
- 상수 이름 자체에 의미가 있는 경우가 있고, 다른 변수값이 중복될 가능성이 있는데, 심벌 값을 사용하면 유일무이한 심벌 값을 사용할 수 있음.

## 33.4 심벌과 프로퍼티 키
- 객체의 프로퍼티 키는 빈 문자열을 포함하는 모든 문자열 또는 심벌 값으로 만들수 있고, 동적으로 생성가능
- 심벌 값을 프로퍼티로 사용하려면 대괄호를 활용.
- 프로퍼티에 접근할 때도 대괄호를 사용
```
const obj = {
    // 심벌 값으로 프로퍼티 키를 생성
    [Symbol.for('mySymbol')]: 1
};

obj [Symbol.for('mySYmbol')]; // -> 1
```
- 심벌 값은 유일무이한 값이므로 심벌 값으로 프로퍼티 키를 만들면 다른 프로퍼티 키와 절대 충돌하지 않는다.

## 33.5 심벌과 프로퍼티 은닉
- 심벌 값을 프로퍼티 키로 하면, for...in문이나 Object.key, Object,getOwnPropertyNames메서드로 찾을 수 없음
- 노출할 필요가 없는 프로퍼티 은닉가능
- 근데 꼭 그런것만도 아님 getOwnPropertySymbols 메서드.

## 33.6 심벌과 표준 빌트인 객체 확장
- 일반적으로 표준 빌트인 객체에 사용자 정의 메서드를 직접 추가하여 확장하는 것은 권장하지 않는다. 표준 빌트인 객체는 읽기 전용으로 사용하는 것이 좋음
- 그 이유는 이름 중복의 위험이 있기 때문
- 중복될 가능성이 없는 심벌 값으로 프로퍼티 키를 생성하여 표준 빌트인 객체를 확장하면 충돌의 위험이 사라짐으로 안전하게 표준 빌트인 객체를 확장할 수 있음

## 33.7 Well-known Symbol
- 자바스크립트가 기본 제공하는 빌트인 심벌 값이 있다.
- 빌트인 심벌 값은 Symbol함수의 프로퍼티에 할당돼 있다.
- 기본제공 빌트인 심벌 값을 Well-known Symbol이라고 부름
- JS엔진 내부 알고리즘에 사용됨
- Array, String, Map, TypedArray, arguments 등 순회 가능한 빌트인 이터러블은 Symbol.iterator를 키로 갖는 메서드를 가지고 이터레이터를 반환. 이터레이션 프로토콜을 준수
