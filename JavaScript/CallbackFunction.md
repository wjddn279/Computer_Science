## Callback Function

"다른 함수의 인자로 전달되는 함수"

### 1급 객체(First-Class Citizen)

"다른 객체들에 일반적으로 적용 가능한 연산을 모두 지원하는 객체"

아래와 같은 특징을 지니면 1급 객체라고 부른다.

1. return 값으로 사용 가능
2. 함수의 인자로 사용 가능
3. 변수에 할당 가능

JavaScript(+Python)의 함수는 위와 같은 특징 3가지를 모두 만족하므로 1급 객체의 특성을 가짐.

Callback은 이러한 특징 중 "인자로 넘어간다". 라는 특징에 해당한다. 우리는 지금까지 이러한 특징을 알게 모르게 사용해왔다.

1.Python

```python
numbers = [1,2,3]

def add_one(number):
	return number + 1

map(add_one, numbers)

# django
from django.urls import path
from . import views

urlpatterns = [
    path('',views.index)
]
```

2.JavaScript

```javascript
// Array Helper Method - forEach
const numbers = [1,2,3,4]
const newNumbers = []

numbers.forEach(function (number){
    newNumbers.push(number)
})

// addEventListener
const button = document.querySelector('button')

button.addEventListener('click', function() {
    alert('콜백 함수 호출!')
})
```

우리가 직접적으로 함수를 호출하지 않고 어떠한 순간(이벤트 발샹), 혹은 과정 속에서 함수를 호출한다.

비동기 처리의 과정은 요총한 결과가 처리되는 '특정 시점'에 '어떠한 일(콜백함수)'을 이어나가는 형태로 이루어진다. (주의 해야할것은 비동기 처리에 콜백 함수는 반드시 필요하지만, 콜백 함수를 사용하는 모든 로직이 비동기는 아니다.)

어떠한 일의 결과가 다른 일의 trigger가 되는 과정이 연속적으로 발생하면 콜백 함수의 콜백 함수를 넣는 과정이 반복되고 이는 콜백 지옥으로 이어진다.