## 고차함수

- 함수를 인수로 전달받거나 함수를 반환하는 함수를 말한다.
- 고차함수는 외부 상태의 변경이나 가변 데이터를 피하고 불변성을 지향하는 함수형 프로그래밍에 기반을 두고 있다.

### 함수형 프로그래밍

- 함수형 프로그래밍은 순수 함수와 보조 함수의 조합을 통해 로직 내에 존재하는 조건문과 반복문을 제거하여 복잡성을 해결하고 변수의 사용을 억제하여 상태 변경을 피하려는 프로그래밍 패러다임이다.
- 조건문이나 반복문은 로직의 흐름을 이해하기 어렵게하여 가독성을 해치고, 변수는 누군가에 의해 언제든지 변경이 될 수 있어 오류 발생의 근본적인 원인이 될 수 있기 때문이다.
  - 해당 부분들은 typescript 로 어느정도 보안되었다고 생각한다.
- 모든 고차함수는 lodash 에 존재한다. 사용성에 맞게 사용하면 될 것 같다.

### sort

- 기본적으로 오름차순을 정렬한다,
- 한글 문자열과 영어 문자열 요소들도 적용된다.
- .reverse() 함수를 이용해 내림차순도 적용할 수 있다.

  ```
    const fruits = ["Banana", "Orange", "Apple"];
    fruits.sort();

    console.log(fruits);

    fruits.reverse();

    console.log(fruits);
  ```

- 숫자는 의도한대로 정렬되지 않는다.
- 유니코드 포인트의 순서를 따른다.
- sort 메소드는 숫자를 문자열로 변환 후에 정의한다.

  ```
  [1, 10, 2, 100, 3].sort(); // [1, 10, 100, 2, 3]
  ```

- sort 의 함수는 함수를 인자로 받을 수 있다.
- 정렬 순서를 정의하는 비교 함수를 인수로 전달하면 된다.

  ```
  const d = [1, 10, 2, 100, 3];

  d.sort((a, b) => a-b); // 오름차순

  d.sort((a, b) => b-a); // 내림차순
  ```

- 객체를 요소로 가지는 배열도 정의할 수 있다.

  ```
  const arg = [
    {id: 4, content : "Javascript"},
    {id : 1, content : "HTML"},
    {id : 3, content : "CSS"},
    ];

    function compare(key) {
      return (a, b)=> (a[key] > b(key) ? 1 : a[key] < b(key) ? -1 : 0);
    }

  arg.sort(compare("id"));
  ```

- sort 는 quicksort 알고리즘을 사용했었으나, ES10 스팩부터 timsort 알고리즘을 사용하도록 변경되었다.

### forEach, map

- 가장 많이 사용되는 고차함수중 하나인 배열의 반복문들이다.
- 가장 큰 차이점은 forEach는 언제나 undefined 를 반환하고, map 은 새로운 콜백함수의 반환값을로 구성된 새로운 배열을 반환한다.
- forEach는 반복문을 대체하기 위한 고차함수, map은 요소값을 다른값으로 매핑한 새로운 배열을 생성하기 위한 고차함수이다.
- forEach 와 map 은 continue, break 가 없다.
- 배열의 모든 요소를 사용해야하며, 중단 혹은 건너뛰기가 사용되지 않는다.
- 후에 나올 비동기인 async, await 도 사용할 수 없다.

#

- forEach 예제

  - forEach는 원본 배열을 변경하지 않지만, 콜백 함수를 통해 원본 배열을 변경할 수는 있다.

    ```
    const numbers = [1,2,3];

    numbers.forEach((n, i, arr)=>{
      arr[i] = item **2;
    })

    console.log(numbers);
    ```

  - 희소 배열을 이용해 순회 대상에서 제외될 수있다.
  - 이는 순회배열인 map, filter, reduce 메서드 등도 마찬가지이다.
  - forEach 는 for 문에 비해 성능이 좋지는 않지만 가독성은 더 좋다.
  - 비동기형태로 작동하거나, 요소가 많거나, 시간이 많이 걸리는 복잡한 코드 또는 성능의 이슈가 있다면 for 문을 사용하는게 좋고, 그 외에 forEach를 사용하는 것을 권장한다.

#

- map 예제

      ```
      const number = [1,4,9];
      const root = number.map(n=>Math.sqrt(n));

      // 위 코드는 다음과 같다.
      const root = number.map(Math.sqrt);
      ```

  - map 은 원본을 변경하지 않는다.
  - map 이 생성하여 반환하는 배열의 length 는 map 을 호출한 배열의 length 와 같다.
  - 1:1 매핑으로 반환된다.
  - forEach 와 마찬가지로 배열의 요소값과 인덱스, map 을 호출한 배열을 인자값으로 전달한다.

#

### filter

- 인수 받은 콜백함수를 반복 호출한다.
- 그중 true 인 값만 반환한다.
- find 와 가장 큰 차이점은 filter는 모든 배열을 순회한다는 점이다.
- map 과의 차이점은 콜백함수로 인해 반환되는 배열이 호출한 배열보다 작을 수도 있다는 점이다.

  ```
  const numbers = [1, 2, 3, 4, 5, 6];
  console.log(numbers.filter(f=> f % 2));
  ```

#

### reduce

- 배열을 모든 요소를 순회하며 인수로 전달받은 콜백 함수를 반복 호출한다.
- 콜백 함수의 반환값을 다음 순회 시에 콜백 함수의 첫 번째 인수로 전달하면서 콜백 함수를 호출하여 하나의 결과값을 만들어 반환한다.
- 이때 원본 배열은 변경되지 않는다.
- 첫 번째 인수로 콜백함수, 두 번째 인수로 초기값을 전달받는다.
- 콜백함수에는 초기값 또는 콜백함수의 이전 반환값, 배열의 요소값과 인덱스, 배열 총4가지를 반환한다.
- 초기값을 생략하면 첫번째 인수가 초기값이 된다.
- 빈배열에 사용하면 에러가 난다. 하지만 초기값을 설정하면 에러가 나지 않는다.

- 예제

  - 합계 구하기
    ```
    const sum = [1, 2, 3, 4] .reduce((acc, cur, i, arr) => acc + cur, 0);
    ```
  - 평균 구하기

    ```
    const values = [1, 2, 3, 4, 5, 6];
    const avg = values.reduce((acc, cur, i, {length}) =>{
      return i === length - 1 ? (acc + cur) / length : acc + cur;
    }, 0);

    console.log(avg);
    ```

  - 최대값 구하기

    ```
    const values = [1, 2, 3, 4, 5, 6];
    const max = values.reduce((acc, cur) => (acc > cur ? acc : cur), 0);

    // Math.max 가 더 직관적이다

    Math.max(...values);
    ```

  - 중복 횟수 구하기

    ```
    const fruits = ["banana", "apple", "orange", "orange", "apple"];
    const count = fruits.reduce((acc, cur)=> {
      // 첫 번째 순환시 acc는 초기값인 {}고, cur 은 첫 번째 요소인 "banana" 다.
      // 초기값으로 전달받은 빈 객체에 요소값을 cur 프로퍼티 키로, 요소의 개수를 프로퍼티 값으로 할당한다.
      // 만약 프로퍼티 값이 undefined이면 프로퍼리 값을 1로 초기화한다.

      acc[cur] = (acc[cur] || 0) + 1;
      return acc;
    }, {});

    console.log(count);
    ```

  - 중첩 배열 평탄화

    ```
    const values = [1, [2, 3], 4, [5, 6]];
    console.log(values.reduce(acc, cur)=> acc.concat(cur).[]);

    // 평탄화는 flat 메서드가 더 직관적이다.

    console.log(values.flat(2));
    // 인수 2는 중첩배열을 평탄화 하기 위한 깊이 값이다.
    ```

  - 중복 제거

    ```
    const values = [1, 2, 1, 3, 5, 4, 5, 3, 4, 4];
    const result = values.reduce(
      (u, val, i, _values) =>
      // 현재 순회중인 요소의 인덱스 i가 val 의 인덱스와 같다면 val 는 처음 순회하는 요소다.
      // 현재 순회중인 요소의 인덱스 i 가 val 인덱스와 다르다면 val 는 중복되는 요소다.
      // 처음 순회하는 요소만 초기값 []가 전달된 u 배열에 담아 반환한다.
      _values.indexOf(val) === i ? [...u, v] : u, []
    )

    console.log(result);

    // filter 가 더 직관적이다.

    values.filter((v, i, _arr) => _arr.indexOf(val) === i);

    // 중복되지 않는 Set을 사용할 수도 있다.
    // 책과 필자는 이 방법을 추천한다.
    // 37.1 절 "Set" 참조

    [...new Set(values)];
    ```

  - 객체의 특정 프로퍼티 값 합산

    - 객체의 프로퍼티 값을 합산하는 경우에는 초기값을 설정해주어야 정확하게 값을 받을 수 있다.

    ```
    const products = [
      {id:1, price : 100},
      {id:2, price : 200},
      {id:3, price : 300},
    ]

    // {...} + number = NaN
    products.reduce((acc, cur) => acc.price + cur.price); // NaN
    products.reduce((acc, cur) => acc.price + cur.price, 0); // 600
    ```

#

### some, every

#

- some 예제

  - 호출한 배열의 요소를 모두 순회하며 단 한번이라도 참이면 true, 모두 거짓이면 false를 반환한다.

    ```
    const number = [5, 10, 15];
    number.some(n=> n > 5); // true
    number.some(n=> n > 15); // false
    [].some(n => n > 3) // false

    ```

#

- every 예제

  - some 과 반대로 하나라도 거짓이면 false, 모두 참이면 true 를 반환한다.

    ```
    const number = [5, 10, 15];
    number.every(n=> n > 3) // true
    number.some(n=> n > 10); // false
    [].some(n => n > 3) // true
    ```

- 빈배열을 순회했을 경우 some 과 every의 반환값이 다르다.

#

### find, findIndex

- 조건에 충족하는 요소값이 있지 않다면 undefined 를 반환한다.

#

- find 예제

  - filter 와의 차이점은 순회 중 조건에 해당하는 값이 있으면 순회를 멈추고 해당하는 값을 반환한다.

    ```
    const users = [
      {id: 1, name: "Lee"},
      {id: 2, name: "Kim"},
      {id: 3, name: "Choi"},
      {id: 4, name: "Park"},
    ]

    users.find(user => user.id === 2);
    ```

  - filter는 언제나 배열을 반환하지만 find 는 언제나 요소를 반환한다.

#

- findIndx 예제

  - 조건에 맞는 요소의 index 를 반환한다.

  ```
  const users = [
    {id: 1, name: "Lee"},
    {id: 2, name: "Kim"},
    {id: 3, name: "Choi"},
    {id: 4, name: "Park"},
  ]

  function predicate(key, value) {
    return item => item[key] === value;
  }

  users.findIndex(user => user.id === 2);

  user.findIndex(predicate("name", "Park"));
  ```

#

### flatMap

- map을 통해 생성된 배열을 평탄화한다.
- map 과 flat 메서드를 순차적으로 실행한다는 효과가 있다.

  ```
  const arr = ["hello", "world"];
  arr.map(x => x.split()).flat();

  arr.flatMap(x => x.split());
  ```

- flat 처럼 인수를 전달하여 평탄화 깊이를 지정할 수는 없고 1단계만 평탄화한다.
- map 메서드를 통해 생성된 중첩 배열의 평탄화 깊이를 지정해야하면 flat 메서드를 직접 호출해야 한다.
