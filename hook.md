리액트의 불편한점
1. handling input(updating value into input)
2. Fetching(getting data from API or updating input ex signupform

hook - functional component에서 state를 가질수 있게 해줌
= 앱을 리액트로 만든다면 class component, componentDidMount, render 등을 안해도 된다
=funcional programming style이 된다는 것
이게 코드를 더 좋게 만들어 준다고 생각하다

# Hook

`hook`은 리액트 16.8부터 본격 사용가능하게 된 신박한 기능이다.<br><br>
`hook`이 나타난 배경을 살펴보자면 <br>
애초에 바라던대로 component에 속한 stateful logic이 재사용이 어렵고 <br>
render props나 HOC로 대체해서 사용하는 방법 또한 <br>
사용한 코드를 재설계해야하는 번거로움과 코드가 복잡해지는 등 결과가 나타날 수 있다.<br>
이런 종류의 문제를 해결하기 위하여 나온 개념이 `hook`이다.<br><br>
(render props = 값(value)이 함수인 prop을 사용해서 component간의 code를 공유하는 패턴)<br>
(HOC = 컴포넌트에서 자주 반복되는 코드를 내부에서 컴포넌트화하여 다른 컴포넌트에 부여가능한 함수)<br><br>

> <br>`hook`은 class를 사용하지 않고 state나 다른 React Features를 사용가능하게 해준다!<br><br>


## State Hook

> useState (작성중)

함수컴포넌트에 local state를 추가할 경우에 사용한다.<br>
`usestate`는 **current state value**와 **function**을 짝으로 return한다.<br>
다수의 rerender를 하는 경우에도 this state는 유지되고 eventhandler와 같은 함수를 function으로 불러 올수있다.<br><br>
`usestate`는 class 내부의 `this.setState`와 비슷하고,<br>
다른 점이 있다면, 이전의 state와 새로운 state를 병합하지 않는다.<br>



