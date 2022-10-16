## 스프레드 문법

- 하나로 뭉쳐 있는 여러 값들의 집합을 펼쳐서 개별적인 값들의 목록으로 바꾼다.
- Array, String, Map, Set, DOM 컬랙션, arguments 와 같이 for ... of 문으로 순회할 수 있는 이터러블에 한정된다.

### 함수 호출문의 인수 목록에서 사용하는 경우

```
const max = Math.max(...[1, 2, 3]);
```

- 앞에서 살펴본 Rest 파라미터와 형태가 동일하여 혼동할 수 있으므로 주의할 필요가 있다.

```
  function foo (...rest) {
    const [ param, ...rest2 ] = rest
    console.log(rest)
    console.log(param)
    console.log(rest2)
  }
  foo(1,2,3,4,5)
```

### 배열 리터럴 내부에서 사용하는 경우

```
var arr = [1,2].concat([3,4])

const arr2 = [...[1,2], ...[3,4]]

const arr01 = [1,4]
const arr02 = [2,3]

// 기대결과는 [1,2,3,4] 이다.

// ex) arr01.concat(arr02).flat();
// 여러방법이 있으나 스프레드 문법을 이용해보자

arr01.splice(0,1, ...arr02);
```

### 배열복사

- 얕은 복사가 가능하다.
- 스터디 중 해당 내용을 가지고 대화를 하였으므로, 예제는 생략하겠다.

### 이터러블을 배열로 변환

```
function sum() {
  // 이터러블이면서 유사배열 객체인 arguments 반환
  return [...arguments].reduce((pre, cur) => pre + cur, 0)
}

const sum2 = (...args) => args.reduce((pre, cur) => pre + cur, 0)


```

- 이터러블이 아닌 유사배열 객체는 스프레드 문법의 대상이 될 수 없다.
- 이 때는 Array.form() 메서드를 사용하면 된다.

### 객체 리터럴 내부에서 사용하는 경우

```
const obj = {x : 1, y : 2}
const obj2 = {x : 3, y : 4, z : 5}

const merge = { ...obj, ...obj2 } // { x: 3, y: 4, z: 5 }


const change = {...obj2, x: 1 } // { x: 1, y: 4, z: 5 }
const add = {...obj2, sum : obj2.x + obj2.y + obj2.z } // { x: 1, y: 4, z: 5, sum: 12 }

```
