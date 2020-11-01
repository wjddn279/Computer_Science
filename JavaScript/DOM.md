## JavaScript 로 할 수 있는 것

1. DOM (Document Object Model) 조작
   - 문서(HTML)를 객체로 조작
2. BOM (Browser Object Model) 조작
   - navigator, screen, location, history
3. ECMAScript
   - 프로그래밍 언어로 할 수 있는 것
   - 변수, 타입, 조건문, 반복문,함수

### DOM(Document Object Model)

HTML, XML 등과 같은 문서를 다루기 위한 독립적인 문서 모델 interface

- 모든 문서의 노드는 'DOM Tree'라고 불리는 트리 구조의 모습을 갖는다.
- window, document와 같은 객체들이 존재하는데, window는 브라우저의 최상위 객체를 의미하며 생략이 가능하다.

```javascript
console.log(window)
# ->  Window {window: Window, self: Window, document: document, name: "", location: Location, …}
```

### DOM Manipulation

문서 모델을 객체를 통해 조작

1. 선택(Selection)

```javascript
    // Select -> 기존 document 내의 특정 tag 선택

    // 특정 태그 객체들을 골라서 가져옴
    // id를 style에 사용하지 않는 이유 -> 자바스크립트에 사용하기 위해
    const mainHeader = document.querySelector('h1')
    const langHeader = document.querySelector('#langHeader')
    const frameHeader = document.querySelector('#frameHeader')
    console.log(mainHeader)

    // 3. querySelectorAll
    // 모든 것들을 전부 가져옴(리스트 형태)
    const langLi = document.querySelectorAll('.lang')
    const frameworkLi = document.querySelectorAll('.framework')
    console.log(langLi)

    // querySelector 사용시 여러개의 요소 -> 첫번째로 일치하는 요소
    const selectOne = document.querySelector('.lang')

    // 복합 선택자
    const selectDescendant = document.querySelector('body li')
    const selectChild = document.querySelector('body > li')
    // console.log(selectDescendant)
    // console.log(selectChild)

    // 5. getElementById, getElementByClassName
    const selectSepcialLang = document.getElementById('specialLang')
    const selectAllLangs = document.getElementsByClassName('framework')
    const selectAllLiTags = document.getElementsByTagName('li')
```

- 단일 노드

  - document.getElementById(id)
    - id만 활용하여 선택 가능 (유연성 떨어짐)
  - document.querySelector(selector)
    - id,class,tag, 복합 선택자 등을 모두 활용하여 선택 가능하다.

  - 여러 개의 요소
    - HTMLCollection(live)
      - document.getElementsByTagName(tagName)
      - document.getElementsByClassName(class)
    -  NodeList(non-live)
      - document.querySelectorAll(selector)

2. 변경, 조작 (Manipulation)

```javascript
    // 2. innerText & innerHTML / append & appendChild
    browserHeader.innerText = 'Browsers'
    li1.innerText = 'IE'
    li2.innerText = '<strong>FireFox</strong>'
    li3.innerHTML = '<strong>Chrome</strong>'
    console.log(browserHeader,ul,li1,li2,li3)

    // 3. append DOM
    const body = document.querySelector('body')
    body.appendChild(browserHeader)
    body.appendChild(ul)

    // appendChild -> 맨 앞의 인자 하나만 들어감, append를 해야 전부 들어감
    ul.append(li1,li2,li3)
    // ul.appendChild(li1,li2,li3)
    
    // 4. Element Styling
    li1.style.cursor = 'pointer'
    li2.style.color = 'blue'
    li3.style.color = 'red'

    // 5. set,get Attribute
    // id에 king을 집어넣어라
    li3.setAttribute('id','king')
    // 속성을 빼와라
    const getAttr = li1.getAttribute('style')
    const getAttr2 = li2.getAttribute('style')
    console.log(getAttr)
    console.log(getAttr2)
```

- 속성 변경
  - innerText & innerHTML
  - element.style.color & element.style.textDecoration
  - setAttribute(attribute, value) & getAttribute(attribute)
- 생성해서 DOM에 부착
  - document.createElement(tagName)
  - appendChild & append
  - removeChild & remove