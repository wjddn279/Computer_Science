## Event

"HTML 문서 내에서 일어나는 사건"

click, submit, keydown, mouseover, submit, change

### addEventListener

"특정 이벤트가 발생하면, 할 일을 등록하는 것"

```
EventTarget.addEventListener(type, listener)
```

1. EventTarget
   - 이벤트 감지를 위한 요소
2. addEventListener
   - EventTarget에 이벤트를 등록할 때 사용하는 이벤트 핸들러
3. type
   - 이벤트의 종류
4. listener
   - (콜백)함수
   - 이벤트가 발생하면 실행되는 함수

```javascript
    // 1. onclick
    function alertMessage() {
        alert("안녕하세요!")
    }
    // 2. addEventListener - click
    const myButton = document.querySelector('#myButton')
    myButton.addEventListener('click',alertMessage)

    // 3. addEventListener - keydown
    const myTextInput = document.querySelector('#myTextInput')
    myTextInput.addEventListener('keydown',function(event){
        console.log(event.target)
        const forBind = document.querySelector('#forBind')
        forBind.innerText = event.target.value
    })

    const changeColorInput = document.querySelector('#changeColorInput')
    changeColorInput.addEventListener('keydown', function(event){
        const h2 = document.querySelector('h2')
        h2.style.color = event.target.value
    })
```

### event.preventDefault()

" 각 태그의 고유한(== 기본으로 설정된) 이벤트가 브라우저에서 동작하지 않도록 막는 행위"

1. 해당 이벤트의 기본 동작을 확인
2. 기본 동작 막기

```javascript
    const checkBox = document.querySelector("#myCheckBox")

    checkBox.addEventListener('click',function(event){
        // cancelable -> 이벤트가 취소 가능한지 여부를 true/false로 반환
        console.log(event)
        console.log(event.cancelable)
        // 이벤트의 발생을 막는다.
        event.preventDefault()
    })

    const form = document.querySelector('#myForm')

    form.addEventListener('submit',(event) => {
        // submit도 true -> 따라서 submit 막힘
        console.log(event)
        console.log(event.cancelable)
        event.preventDefault()
    })

    const link = document.querySelector('#myLink')

    link.addEventListener('click', function(event){
        console.log(event)
        console.log(event.cancelable)
        event.preventDefault()
    })

    const input = document.querySelector('#myInput')

    input.addEventListener('keydown', function(event){
        console.log(event)
        console.log(event.cancelable)
        event.preventDefault()
    })
    // scroll -> preventDefault가 불가능한 event
    document.addEventListener('scroll', function(event){
        console.log(event.cancelable)
        event.preventDefault()
    })
```