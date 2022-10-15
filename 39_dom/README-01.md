# DOM

## 노드

### HTML 요소와 노드 객체

- html 요소는 HTML 문서를 구성하는 개별적인 요소를 의미한다.
- HTML 요소는 렌더링 엔진에 의해 파싱되어 DOM을 구성하는 요소 노드 객체로 변환된다.
- HTML 요소의 어트리뷰트는 어트리뷰트 노드로, HTML 요소의 텍스트 콘텐츠는 텍스트 노드로 변환된다.
- HTML 문서는 HTML 요소들의 집합으로 이뤄지며, HTML 요소는 중첩관계를 갖는다.
- HTML 요소의 콘텐츠 영역에는 텍스트뿐만 아니라 다른 HTML 요소도 포함할 수 있다.

- 이 때 HTML 요소의 부자 관계가 형성된다.(parent-child)
- 이러한 HTML 요소를 객체화한 모든 노드 객체들은 트리자료 구조로 구성한다.

- 노드 객체로 구성된 트리 자료구조를 DOM 이라고 한다.
- 노드 객체의 트리로 구조화되어 있기 때문에 DOM 트리라고도 부른다.

### 노드 객체의 타입

- p.679 예제와 그림을 한번 확인해보자
- DOM은 다소 복잡할 수도 있으나, 결국 최상위 root node 인 document 로 부터 파생된 트리구조이다.

#### 문서노드

- document 객체를 가리킨다.
- document 객체는 브라우저가 렌더링한 HTML 문서 전체를 가리키는 객체이다.
- 전역 객체 window의 document 프로퍼티에 바인딩되어 있다.
- 문서노드는 window.document 또는 document 로 참조할 수 있다.
- 브라우저는 하나의 window 를 공유한다.
- HTML 문서당 document 객체는 유일하다.
- 요소, 어트리뷰트, 텍스트 노드에 접근하려면 문서 노드를 통해야 접근할 수 있다.

#### 요소 노드

- HTML 요소를 가리키는 객체다.
- 요소도느는 문서의 구조를 표현한다.

#### 어트리뷰트 노드

- HTML 요소의 어트리뷰트를 가리키는 객체다.
- 어트리뷰트 노드는 어트리뷰트가 지정된 HTML 요소의 요소노드와 연결되어 있다.
- 어트리뷰트를 참조하거나 변경하려면 우선 요소노드에 접근해야 한다.

#### 텍스트 노드

- 요소 노드가 문서의 구조를 표현한다면 텍스트 노드는 문서의 정보를 표현한다고 할 수 있다.
- 텍스트 노드는 요소 노드의 자식 노드이며, 자식을 가질 수 없는 리프 노드다.
- 텍스트 노드에 접근하려면 부모인 요소에 접근해야 한다.

### 노드 객체의 상속 구조

- DOM 은 HTML 문서의 계층적 구조와 정보를 표현하며, 이를 제어할 수 있는 API, 즉 프로퍼티와 메서드를 제공하는 트리 자료구조라고 했다.
- 노드객체는 DOM API를 사용할 수 있다.
- 이를 통해 객체 자신의 형제, 부모, 자식을 탐색할 수 있고, 어트리뷰트와 텍스트를 조작할 수 있다.

- DOM 을 구성하는 노드는 ECMAScript 사양에 정의된 표준 빌트인 객체가 아니라 브라우저 환경에서 추가적으로 제공하는 호스트 객체다.
- 노드 객체도 자바스크립트 객체이므로 프로토타입에 의한 상속 구조를 갖는다.
- 상속 구조는 그림 39-5를 참조하자

- 모든 노드는 Object, EventTarget, Node 인터페이스를 상속받는다.
- 문서 노드는 Document, HTMLDOcument 인터페이스를 상속받는다.
- 어트리뷰트 노드는 Attr, ㅌ게스트 노드는 CharacterData 인터페이스를 각각 상속받는다.
- 모든 노드는 Element 인터페이스를 상속받는다.
- 또한 요소 노드는 추가적으로 HTMLElement와 태그의 종류별로 세분화된 HTMLHtmlElement, HTMLHeadElement, HTMLBodyElement 등의 인터페이스를 상속받는다.

- input 요소를 파싱하여 객체화한 input 요소 노드는 HTMLInputElement, HTMLElement, Element, Node, EventTarget, Object의 prototype에 바인딩되어 있는 프로토타입 객체를 상속받는다.
- input 요소 노드는 프로토타입 체인에 있는 모든 프로토타입의 프로퍼티나 메서드를 상속받아 사용할 수 있다.

- 상속구조는 크롬 개발자도구에서 상세히 살펴볼 수 있다.

- 노드에는 공통으로 갖는 기능도 있고, 노드에 따라 고유한 기능도 있다.
- HTML 요소가 객체화된 요소 노드는 HTML 요소가 갖는 공통적인 기능이 있다.
- 요소 노드는 HTML 요소의 종류에 따라 고유한 기능도 있다.
- input 요소에는 value 가 필요하지만 div 요소에는 value 프로퍼티가 필요하지 않다.
- 노드는 공통된 기능일수록 프로토타입 체인의 상위에, 개별적인 고유 기능일수록 프로토타입 체인의 하위에 프로토타입 체인을 구축하여 노드 객체에 필요한 기능, 즉 프로퍼티와 메서드를 제공하는 상속 구조를 갖는다.
- DOM 은 HTML 문서의 계층적 구조와 정보를 표현하는 것을 물론 노드의 종류, 즉 노드 타입에 따라 필요한 기능을 프로퍼티와 메서드의 집합인 DOM API로 제공한다. 이 DOM API를 통해 HTML의 구조나 내용 또는 스타일 등을 동적으로 조작할 수 있다.

## 요소 노드 취득

- 요소노드를 취득하기 위한 여러가지 방법이 있다.

### ID를 이용한 요소 노드 취득

- document.prototype.getElementById 메서드는 인수로 전달한 id 값을 갖는 하나의 요소 노드를 탐색하여 반환한다.

- getElementById 메서드는 반드시 document 를 통해 호출하여야 한다.
- id 값은 HTML 문서 내에서 유일한 값이어야 하며, class 어트리뷰트와는 달리 공백 문자로 구분하여 여러개의 값을 가질 수 없다.
- HTML id가 동일한 여러 요소가 존재하더라도 에러가 발생하지는 않는다.
- 문서 내에 중복된 id를 찾는 경우, 값을 갖는 첫 번째 요소 노드만 반환한다.
- 언제나 단 하나의 요소만 반환한다.
- 해당 id의 값을 가진 요소가 없을 경우 null 을 반환한다.

```
  foo === document.getElementById("foo"); //true 전역 변수가 암묵적으로 선언된다.
```

- 전역 변수를 선언한 시점부터는 전역 변수에 노드가 재할당 되지 않는다.

### 태그 이름을 이용한 요소 노드 취득

- getElementsByTagName 은 인수로 전달한 태그 이름을 갖는 모든 노드들을 탐색하여 HTMLCollection 객체를 반환한다.
- 함수는 하나의 값만 반환할 수 있으므로 여러 개의 값을 반환하려면 배열이나 객체와 같은 자료구조에 담아 반환해야 한다.
- HTMLCollection 객체는 유사 배열 객체이면서 이터러블이다. (34.4절 참조)
- 모든 요소 노드를 취득하려면 태그이름 인수를 "\*" 로 전달하면 된다.
- Document.prototype과 Element.prototype 에 존재한다.
- document 는 전체 요소중 요소 탐색을 하고, element 는 특성 요소 에서 요소를 탐색한다.
- 인수로 전달한 태그 이름을 갖는 요소가 존재하지 않는 경우 getElementsByTagNAme 메서드는 빈 HTMLCollection 객체를 반환한다.

### CLASS를 이용한 요소 노드 취득

- getElementsByClassName 은 인수로 전달한 태그 이름을 갖는 모든 노드들을 탐색하여 HTMLCollection 객체를 반환한다.
- Document.prototype과 Element.prototype 에 존재한다.
- 인수로 전달한 태그 이름을 갖는 요소가 존재하지 않는 경우 getElementsByTagNAme 메서드는 빈 HTMLCollection 객체를 반환한다.

### CSS 선택자를 이용한 요소 노드 취득

- css 문서 작성시 활용할 수 있는 선택자이다.

```
* {...} // 모든 요소를 선택
p {...} // 모든 P 태그 요소 선택
#foo {...} // id 값이 foo 인 요소를 모두 선택
.foo {...} // class 값이 foo 인 요소를 모두 선택
input[type=text] {...} // input 요소 중 type 이 text 인 요소를 모두 선택
div p {...} // div 의 후손 중 p 요소 모두 선택
div > p {...} // div 의 사직 중 p를 모두 선택
p + ul {...} // p 요소의 형제 요소 중에 p 요소 바로 뒤에 위치하는 ul 요소를 선택
p ~ ul {...} // p 요소의 형제 요소 중에 p 요소 뒤에 위치하는 ul 요소를 모두 선택
a:hover {...} // hover 상태인 a 요소를 모두 선택
p::before {...}
/* p 요소의 콘텐츠의 앞에 위치하는 공간을 선택, 일반적으로 content 프로퍼티와 함께 사용된다. */
```

- querySelect 메서드는 인수로 전달한 CSS 선택자를 만족시키는 하나의 요소 노드를 탐색하여 반환한다.
- querySelectorAll 메서드는 모든 노드를 탐색하여 반환한다.
- 두개의 메서드는 Document.prototype과 Element.prototype 에 존재한다.
- querySelectorAll 메서드는 NodeList 객체를 반환한다.
- 유사배열이며, 이터러블이다.
- 인수로 전달된 CSS 가 문법에 맞지 않으면 DOMException에러를 발생하며, 없을경우 빈 NodeList 를 반환한다.
- HTML 문서의 모든 요소를 취득하려면 querySelectorAll 메서드의 인수로 "\*" 를 전달한다.
- css 선택자 문법을 사용하는 다른 메서드보다 다소 느린 것으로 알려져 있다.

### 특정 요소 노드를 취득할 수 있는지 확인

- Element.prototype.matcgh 메서드이다.
- 이벤트 위임을 시도할 경우 해당 메서드로 우선적으로 요소 탐색을 끝낸 후에 시도하는 식으로 활용할 수 있다.
- 40.7절 자세히

### HTMLCollection과 NodeList

- 두 객체의 중요 특징은 노드 객체의 상태 변화를 실시간으로 반영하는 살아 있는 DOM 컬렉션 객체다.

- HTMLCollection

  ```
  <body>
  <li class="red">
  <li class="red">
  <li class="red">
  </body>
  <script>
  const $elems = document.getElementByClassName("red");

  for (let i = 0; i < $elems.length; i++) {
    elems[i].className = "blue";
  }

  console.log($elemes.length) //1
  </script>
  ```

  - 위 예제의 경우 실시간으로 반영되어, 첫번 째 반복문이 실행된 이후, 두번 째 반복문이 실행될 때 elems 의 객체는 length 가 2개가 된다.
  - 첫번째 li 의 className 이 blue 로 변경되고 elems[1] 은 세번째 li 가 선택된다.
  - 해당 설명은 p. 697에 자세히 나와있다.

- NodeList

  - HTMLCollection 객체와의 차이점은 실시간으로 노드 상태변경을 반영하지 않는 객체다.
  - childNodes 프로퍼티가 반환하는 NodeList 객체는 HTMLCollection 객체와 같이 실시간으로 노드 객체의 상태 변경을 반영하는 live 객체로 동작한다.
  - 예제 39-23 참조

- 두 객체 모두 예상과 다르게 동작할 수가 있으니 배열로 변환하여 사용하는 것을 권장한다.

## 노드 탐색

### 공백 텍스트 노드

- HTML 요소 사이의 스페이스, 탭, 줄바꿈(개행) 등의 공백 문자는 텍스트 노드를 생성한다.

  ```
  <body>
    <ul id="foo">
      <li class="a">a</li>
      <li class="b">b</li>
      <li class="c">c</li>
    </ul>
  </body>
  ```

- html 문서의 공박 문자는 공백 텍스트 노드를 생성한다.
- 노드를 탐색할 때는 공백 문자가 생성한 공백 텍스트 노드에 주의해야 한다.

  ```
  <ul id="foo"><
  li class="a">a</li><
  li class="b">b</li><
  li class="c">c</li></ul>
  ```

- 위와 같이 선언하면 공백 노드를 없엘수는 있지만, 가독성이 좋지 않아 추천하지 않는다.

### 자식 노드 탐색

| 프로퍼티                            | 내용                                                                  |
| ----------------------------------- | --------------------------------------------------------------------- |
| Node.prototype.childNodes           | 자식 노드를 모두 탐색한다.                                            |
| Element.prototype.children          | 자식 노드중 요소 노드만 모두 탐색한다. 텍스트 노드가 포함되지 않는다. |
| Node.prototype.firstChild           | 첫 번째 자식 노드를 반환한다. 텍스트 혹은 요소노드다.                 |
| Node.prototype.lastChild            | 마지막 자식 노드를 반환한다. 텍스트 혹은 요소노드다.                  |
| Element.prototype.firstElementChild | 첫 번째 자식 요소노드를 반환한다.                                     |
| Element.prototype.lastElementChild  | 마지막 자식 요소노드를 반환한다.                                      |

### 자식 노드 존재 확인

- 자식 노드 존재 확인은 Node.prototype.hasChildNodes 메서드를 사용한다.
- boolean 형을 return 한다.
- 텍스트 노드를 포함한다.
- 요소만 확인하려면 children.length 또는 Element.prototype.childElementCount 프로퍼티를 사용한다.

### 요소 노드의 텍스트 노드 탐색

- 텍스트 노드의 위치는 첫 번째 자식 노드이다.
- firstChild 프로퍼티로 확인할 수 있다.

### 부모 노드 탐색

- Node.prototype.parentNode 프로퍼티를 사용한다.

### 형제 노드 탐색

- 예제 39-33 참조
  | 프로퍼티 | 내용 |
  | --------------------------------- | ------------------------------------------ |
  | Node.prototype.previousSibling | 자신의 이전 형제 노드를 탐색하여 반환한다. |
  | Node.prototype.nextSibling | 자신의 다음 형제 노드를 탐색하여 반환한다. |
  | Element.prototype.previousElementSibling | 자신의 이전 형제 노드를 탐색하여 반환한다. 요소 노드만 반환한다.|
  | Element.prototype.nextElementSibling | 자신의 다음 형제 노드를 탐색하여 반환한다. 요소 노드만 반환한다.|
