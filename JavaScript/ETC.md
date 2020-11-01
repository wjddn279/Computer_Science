## var, let, const

- var
  - 호이스팅의 매커니즘을 따른다.
  - 전역에 선언된 변수
- const 
  - 한번 선언한 값에 대해서 변경할 수 없다
  - 하지만 객체나 배열의 내부는 변경이 가능하다.
- let
  - 한번 선언한 값에 대해서 다시 선언할 수 없다
  - 변경은 가능
  - 지역에 선언된 변수

## this

- this => 무조건 어떤 object를 지칭

- function() {} 정의 할 때, this 가 window가 아닌경우

  1. method 안의 this -> 해당 method가 정의된 객체(object)

  2. 생성자 함수 안의 this

## clouser

- js에서 함수 내에 함수를 끼워 넣을 수있는데, 중첩된 함수는 그것을 포함하는(외부함수)와 별개입니다. 그것은 클로저를 생성합니다.
- 그 변수("폐쇄"라는 표현)를 결합하는 환경을 자유롭게 변수와 함께 가질 수 있는 표현(전형적인 함수) 입니다.
- 중첩 하수는 클로저 이므로, 중첩된 함수는 그것을 포함하는 함수의 인수와 변수를 상속할 수 있는 것을 의미합니다. 즉 내부 함수는 외부 함수의 범위를 포함.

- 따라서, 내부 함수는 외부 함수의 명령문에서만 액세스할 수 있습니다.
- 내부 함수는 클로저를 형성합니다. 외부 함수는 내부 함수의 인수와 변수를 사용 할 수 없는 반면, 내부 함수는 외부 함수의 인수와 변수를 사용할 수 있습니다.

```javascript
function outside(x) {
    function inside(y) {
        return x + y;
    }
}
fn_inside = outside(3);
# outside 라는 함수는 x에 이때의 3을 기억
result = fn_inside(5);
# return 8
```

- 자바스크립트는 함수의 중첩(함수 안에 함수를 정의하는 것)을 허용하고, 내부 함수가 외부 함수 안에서 정의된 모든 변수와 함수들을 완전하게 접근 할 수 있도록 승인해줍니다.
- 그러나 외부에서 내부함수의 변수를 접근하는 것은 불가능 합니다.(캡슐화)

```javascript
var pet = function(name) {
    var getName = function() {
        return name;
    }
    return getName;
}
myPet = pet("Vivie")

myPet();
```



