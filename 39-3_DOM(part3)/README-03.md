# 39장 DOM (part3)

## 39.7 어트리뷰트

### 어트리뷰트 노드와 attributes 프로퍼티

- HTML 요소는 여러 개의 어트리뷰트(속성)을 가질 수 있다.
- HTML 어트리뷰트는 HTML 요소의 시작 태그에 `어트리뷰트 이름="어트리뷰트 값"` 형식으로 정의한다.
- HTML 문서가 파싱될 때 어트리뷰트당 하나의 어트리뷰트 노드가 생성된다.
- 모든 어트리뷰트 노드의 참조는 유사배열객체이자 이터러블인 NamedNodeMap 객체에 담겨서 요소 노드의 attributes 프로퍼티에 저장된다.
- 요소 노드의 모든 어트리뷰트 요소는 Element.prototype.attributes 프로퍼티로 취득할 수 있다. (읽기전용)


### HTML 어트리뷰트 조작
- Element.prototype.`getAttribute`(attributeName) : 어트리뷰트 값 참조
- Element.prototype.`setAttribute`(attributeName, attributeValue) : 어트리뷰트 값 변경
- Element.prototype.`hasAttribute`(attributeName) : 어트리뷰트가 존재하는지 확인
- Element.prototype.`removeAttribute`(attributeName) : 특정 어트리뷰트 삭제


### HTML 어트리뷰트 vs. DOM 프로퍼티
- 요소 노드 객체에는 HTML 어트리뷰트에 대응 하는 DOM 프로퍼티가 존재한다.
- 즉, HTML 어트리 뷰트는 DOM에서 중복 관리 되는 것처럼 보인다.
    - 요소 노드의 attributes 프로퍼티에서 관리하는 어트리뷰트 노드
    - 요소 노드의 DOM 프로퍼티

- 요소 노드는 2개의 상태, 초기 상태와 최신 상태를 관리해야 한다.
- 어트리뷰트 노드 : 요소 노드의 초기 상태 관리
- DOM 프로퍼트 : 요소 노드의 최신 상태 관리
```
<!DOCTYPE html>
<html>
<body>
  <input id="user" type="text" value="ungmo2">
  <script>
    const $input = document.getElementById('user');

    // 사용자가 input 요소의 입력 필드에 값을 입력할 때마다 input 요소 노드의
    // value 프로퍼티 값, 즉 최신 상태 값을 취득한다. value 프로퍼티 값은 사용자의 입력에
    // 의해 동적으로 변경된다.
    $input.oninput = () => {
      console.log('value 프로퍼티 값', $input.value);
    };

    // getAttribute 메서드로 취득한 HTML 어트리뷰트 값, 즉 초기 상태 값은 변하지 않고 유지된다.
    console.log('value 어트리뷰트 값', $input.getAttribute('value'));
  </script>
</body>
</html>
```

### data 어트리뷰트와 dataset 프로퍼티

- data 어트리뷰트와 dataset 프로퍼티를 사용하면 HTML 요소에 정의한 사용자 정의 어트리뷰트와 자바스크립트 간에 데이터를 교환할 수 있다.
- data 어트리뷰트는 data- 접두사 뒤에 임의의 이름을 붙여 사용한다.
- data 어트리뷰트 값은 HTMLElement.dataset 프로퍼티로 취득할 수 있다.
- 존재 하지 않는 이름을 키로 dataset 프로퍼티에 값을 할당하면 HTML 요소에 data 어트리뷰트가 추가된다.

<br />

## 39.8 스타일

### 인라인 스타일 조작
- HTMLElement.prototype.style 프로퍼티는 getter, setter 모두 존재하는 접근자 프로퍼티로서 요소 노드의 인라인 스타일을 취득하거나 추가 또는 변경한다.
```
<!DOCTYPE html>
<html>
<body>
  <div style="color: red">Hello World</div>
  <script>
    const $div = document.querySelector('div');
 
    <-- 인라인 스타일 취득 -->
    console.log($div.style); // CSSStyleDeclaration { 0: "color", ... }
 
    <-- 인라인 스타일 변경 -->
    $div.style.color = 'blue';
 
    <-- 인라인 스타일 추가 -->
    $div.style.width = '100px';  // 단위를 꼭 지정해야 함
    $div.style.height = '100px';
    $div.style.backgroundColor = 'yellow';
    $div.style['background-color'] = 'yellow';  // 케밥케이스의 CSS 프로퍼티를 그대로 사용하려면 대괄호 표기법을 사용
  </script>
</body>
</html>
```

### 클래스 조작

- class 어트리뷰트에 대응하는 DOM 프로퍼티는 class 가 아니라 className 과 classList (class : 예약어)

#### className

- class 어트리뷰트 값을 취득하거나 변경한다.
- class 프로퍼티는 문자열로 반환하므로 공백으로 구분된 여러 개의 클래스를 반환하는 경우 다루기가 불편하다.


```
<!DOCTYPE html>
<html>
<head>
  <style>
    .box {
      width: 100px; height: 100px;
      background-color: antiquewhite;
    }
    .red { color: red; }
    .blue { color: blue; }
  </style>
</head>
<body>
  <div class="box red">Hello World</div>
  <script>
    const $box = document.querySelector('.box');

    // .box 요소의 class 어트리뷰트 값을 취득
    console.log($box.className); // 'box red'
    console.log($box.classList);
    // DOMTokenList(2) [length: 2, value: "box red", 0: "box", 1: "red"]

    // .box 요소의 class 어트리뷰트 값 중에서 'red'만 'blue'로 변경
    $box.className = $box.className.replace('red', 'blue');
    $box.classList.replace('red', 'blue');
  </script>
</body>
</html>
```

#### classList
- class 어트리뷰트의 정보를 담은 DOMTokenList 객체를 반환한다.
- DOMTokenList 객체는 class 어트리뷰트의 정보를 나타내는 컬렉션 객체로 유사 배열 객체이면서 이터러블이다.
- 주요 메서드
    - add(... className) : 인수로 전달된 1개의 이상의 문자열을 class 어트리뷰트 값으로 추가
    ```
    $box.classList.add('foo'); // -> class="box red foo"
    $box.classList.add('bar', 'baz'); // -> class="box red foo bar baz"
    ```
    - remove(... className) : 인수로 전달한 1개 이상의 문자열과 일치하는 class 어트리뷰트에서 삭제 (일치 하는 클래스가 없으면 무시됨)
    - item(index) : 인수로 전달한 index에 해당하는 클래스를 class 어트리뷰트에서 반환
    ```
    $box.classList.item(0); // -> "box"
    $box.classList.item(1); // -> "red"
    ```
    - contains(className) : 인수로 전달한 문자열과 일치하는 클래스가 class 어트리뷰트에 포함되어 있는지 확인
    ```
    $box.classList.contains('box');  // -> true
    $box.classList.contains('blue'); // -> false
    ```
    - replace(oldClassName, newClassName) : 첫 번째 인수 클래스를 두번째 클래스로 변경
    ```
    $box.classList.replace('red', 'blue'); // -> class="box blue"
    ```
    - toggle(className[, force]) : 인수로 전달한 문자열과 일치하는 클래스가 존재하면 제거하고 존재하지 않으면 추가
    ```
    $box.classList.toggle('foo'); // -> class="box blue foo"
    $box.classList.toggle('foo'); // -> class="box blue"
    ```

### 요소에 적용되어 있는 CSS 스타일 참조
- style 프로퍼티는 인라인 스타일만 반환한다.
- HTML 요소에 적용되어 있는 모든 CSS 스타일을 참조해야할 경우 `getComputedStyle` 메서드를 사용한다.

<br />

## 39.9 DOM 표준

- HTML과 DOM 표준은 W3C 와 WHATWG 라는 두 단체가 나름대로 협력하면서 공통된 표준을 만들어 왔다.
- 2018년부터 구글, 애플, 마이크로소프트, 모질라로 구성된 4개의 주류 브라우저 벤더사가 주도하는 WHATWG 이 단일 표준을 내놓기로 두 단체가 합의했다.
