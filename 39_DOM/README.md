# 39.6

## 노드 정보 취득
`Node.prototype.nodeType`을 이용하여 노드의 정보를 취득할 수 있습니다.  
- Node.ElEMENT_NODE: 요소 노드 타입을 나타내는 상수 1 반환
- Node.TEXT_NODE: 텍스트 노드 타입을 나타내는 상수 3을 반환
- Node.DOCUMENT_NODE: 문서 노드 타입을 나타내는 상수 9를 반환  

`Node.prototype.nodeName`를 이용하여 노드의 이름을 문자열로 반환합니다.
- 요소 노드: 대문자 문자열 태그 이름("UL", "LI"등)을 반환
- 텍스트 노드: 문자열 "#text"를 반환
- 문서 노드: 문자열 "#document"를 반환

## 요소 노드의 텍스트 조작
- `nodeValue`
`nodeValue` 프로퍼티는 getter, setter 모두 존재하는 접근자 프로퍼티입니다.    
텍스트 노드 객체의 값(텍스트)을 반환합니다. 문서 노드나 요소 노드의 `nodeValue`로 참조 시 null을 반환합니다.  
**순서**
1. 텍스트 변경 할 요소 노드 취득 후 `firstChild` 프로퍼티를 이용하여 검색합니다.
2. 탐색한 텍스트 노드의 `nodeValue` 프로퍼티를 사용하여 텍스트 노드의 값을 변경합니다.

- `textContent`  
`textContent` 프로퍼티는 getter, setter 모두 존재하는 접근자 프로퍼티로써 요소 노드의 텍스트와 모든 자식 노드의 텍스트를 모두 취득하거나 변경합니다.
`textContent`로 변경시 텍스트로 취급되어 HTML 마크업이 파싱되지 않습니다.
비슷한 프로퍼티인 `innerText`가 존재하는데 css에 순종적이어서 css에 의해 비 표시로 지정된 요소 노드의 텍스트를 반환하지 않고 css를 고려하기 때문에 속도가 느리기 때문에 사용하지 않는 것이 좋습니다.

## 39.6 DOM 조작
DOM 조작은 리플로우나 리페인트가 일어날 수 있으므로 조심해서 다뤄야 합니다.

* 리플로우란?
브라우저가 레이아웃 계산을 다시 실행하는 것을 말하며, 노드의 추가 삭제나 요소의 크기 변경등의 레이아웃에 영향을 미치는 변경이 일어났을 경우에 일어납니다.

* 리페인트란?
재결합된 렌더트리를 기반으로 다시 페인트 하는 것을 말합니다.

### innerHTML
요소 노드의   innerHTML 프로퍼티에 문자열을 할당하면 노드의 모든 자식 노드가 제거되고, 할당한 문자열의 HTML 마크업이 파싱되어 자식 노드의 DOM에 반영됩니다.
**크로스 사이트 스크립팅 공격에 취약할 수 있습니다.**
-> 파싱과정에서 악성코드마저 그대로 실행이 될 수 있기 때문에

### 노드 생성 및 추가
- `createElement`  
요소 노드를 생성하여 반환

- `createTextNode`
텍스트 노드를 생성하여 반환

**복수의 노드 생성 및 추가**
`DocumentFragment` 노드를 생성 후에 그 노드에 다른 노드들을 생성 후 추가시킵니다.  
그 후 `DomcumentFramgment` 만 기존 DOM에 추가하면 리플로우와 리페인트를 줄일 수 있습니다.

### 노드 삽입
- `before`  
노드 객체 또는 DOM 문자열을 선택한 노드 앞에 삽입

`after`  
노드 객체 또는 DOM 문자열을 선택한 노드 뒤에 삽입

`prepend`  
노드 객체 또는 DOM 문자열을 선택한 요소의 첫 번째 자식요소로 삽입

- `append`  
노드 객체 또는 DOM 문자열을 선택한 요소의 마지막 자식요소로 삽입
반환 데이터 없음

- `appendChild`  
노드 객체만 삽입 가능  
추가한 노드 객체 반환

**insertAdjacentHTML(position, htmlText), insertAdjacentElement(position,element), insertAdjacentText(position,text)를 사용하여 지정한 위치에 넣는 방법도 있습니다.**

### 노드 이동
이미 존재하는 노드를 취득하여 `appendChild` 또는 `insertBefore` 메서드를 사용하여 다시 DOM에 추가시, 현재 위치에서 노드가 삭제되고 새로운 위치에 노드를 추가합니다.

### 노드 복사
`cloneNode([deep:boolean]` 메서드를 이용하여 노드의 사본을 생성하여 반환합니다.  
true = 모든 자손 노드가 포함된 사본 생성(깊은 복사)
false = 노드 자신만의 사본 생성(얕은 복사)

### 노드 교체
`removeChild(newChild,oldChild)` 메서드를 이용하여 노드를 교체합니다.

### 노드 삭제
`removeChild(child)` 메서드를 이용하여 노드를 삭제합니다.  
인수로 전달한 노드는 메서드를 호출한 노드의 자식 노드여야 합니다.
