JS는 OOP 이다

this => 무조건 어떤 object를 지칭



function() {} 정의 할 때, this 가 window가 아닌경우

1. method 안의 this -> 해당 method가 정의된 객체(object)
2. 생성자 함수 안의 this

 ### var, let, const

- var
  - 호이스팅의 매커니즘을 따른다.
- const 
  - 한번 선언한 값에 대해서 변경할 수 없다
  - 하지만 객체나 배열의 내부는 변경이 가능하다.
- let
  - 한번 선언한 값에 대해서 다시 선언할 수 없다
  - 변경은 가능
- 