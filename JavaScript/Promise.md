## Promise

"비동기 작업이 맞이 할 미래의 결과(성공/실패)를 약속하는 객체"

1. 콜백 지옥을 해결하기 위해 ES6부터 등장한 개념
2. 2가지의 약속이 이행되는 상황을 가정한다.
   - 성공
     - .then (callbackfuntion)
     - 성공하고 나면 무엇을 할지
   - 실패
     - .catch(callback funtion)
     - 실패하면 에러를 어떻게 처리할지

### Axios

"**Promise based** HTTP client for browser and node.js"

1. Promise 기반의 비동기 요청을 할 수 있는 JavaScript 라이브러리
2. 사용법

```javascript
axios.get('https://jsonplaceholder.typicode.com/todos/asdf')
// Promise의 성공/실패에 대한 약속 이후의 처리를 할 수 있다.
.then(function(res){
    console.log(res)
    return res.data
})
// 이전 .then 내부의 CB의 return 값은 다음 .then 내부의 CB의 인자로 넘어온다.
.then(function(data){
    console.log(data)
    return data.title
})
.then(function(title){
    console.log(title)
})
// Error는 요청을 1이 아닌 asdf로 변경해서 확인해보자.
.catch(function(err){
    console.log(err)
})
```

3. 참고 사항
   - Axios도 결국 내부적으로 xhr을 사용한다.
   - 결국, 이를 편하고 직관적으로 사용할 수 있도록 만들어 놓은 라이브러리다.

### async & await

"비동기 함수는 이벤트 루프를 통해 비동기적으로 작동하는 함수로, 암시적으로 promise를 사용하여 결과를 반환합니다. 그러나 비동기 함수를 사용하는 코드의 구문과 구조는, 표준 동기함수를 사용하는 것과 많이 비슷합니다. -mdn-"

1. 비동기 방식으로 처리하는 로직을 마치 동기적으로 보이게 만드는 Syntatic Sugar
2. 함수 앞에 async 키워드를 작성하고 내부적으로 비동기로 처리되는 로직 앞에 await 키워드를 작성한다.
   - await 연산자는 promise를 기다리기 위해 사용하는데, async function 내부에서만 사용해야 원하는 방식으로 동작한다.
3. 사용법

```javascript
// 함수 앞에 async 키워드를 작성한다.
async function getTodo() {
	console.log('1')
    // Promsise를 return 하는 로직 앞에 await 키워드를 작성한다.
    await axios.get('https://jsonplaceholder.typicode.com/todos/1')
    .then(function(res){
        console.log(res)
    })
    console.log('2')
}

getTodo()
```

