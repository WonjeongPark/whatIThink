
# Redux

## React의 짝꿍
리액트에서 대부분 자식컴포넌트는 **부모컴포넌트를 중간역할**로 두고 서로 소통을 한다.<br>
즉, 자식컴포넌트의 변경된 상태를 업데이트하고 다른 자식 컴포넌트에 내려주는 과정을<br>
부모컴포넌트가 하기 때문에 비교적 관리하기에 쉬운 편이다.<br><br>
하지만 우리가 만들고 사용하는 앱은 점점 규모가 커지고 복잡해진다.<br>
그에 따라 컴포넌트의 수가 많아지거나 깊어질 수록, 관련된 데이터와 함수들도 늘어난다.<br>
그럴수록 부모컴포넌트의 길이는 감당하기 힘들정도로 길어지고 관리하기 어려워 질 것이다.<br>
(물론 컴포넌트들끼리 직접소통하는 등 부모컴포넌트를 거치지 않는 방법도 있지만 추천하지않는다.)<br><br>
이런 이유로 우리는 Redux를 사용한다.<br><br>
><br>물론 순수리액트만 이용해서 개발 가능하다! 그러나 사용하면 편리하고 유지보수가 쉬워지는 장점이 있다!<br>
>또한, 꼭 React에서만 사용해야하는 것은 아니지만 React에 잘 어울리는 것은 분명하다!<br><br>

## 그래서 Redux가 뭐지?

### MVC와  FLUX architecture 그리고 Redux.
보편적으로 사용되었던 MVC패턴은 Model과 view가 다수 존재하는 경우 무한루프에 빠질 수 있다.<br>
아래 사진에서 보면 MVC는 많아진 Model, view로 인해 무한루프가 어디인지 찾기 힘들 정도로 복잡하다.<br><br>
![MVCvsFLUX](https://github.com/WonjeongPark/whatIThink/blob/master/IMG/MVCvsFLUX.png?raw=true)
<br><br>그런 MVC패턴의 한계를 해결하기 위해 나온 FLUX디자인이다.<br>
action이 발생하면 Dispatcher가 이를 받아 Store에 업데이트하고 변동사항은 View에 반영한다.<br>
View에서도 action이 발생되어 Dispatcher로 보낼 수 있지만 이미 다른 action을 처리 중이라면 대기 처리 된다.<br><br>
><br>이렇게 MVC의 단점을 보완한 FlUX를 조금 더 편하게 사용하게끔 하는 것이 Redux다<br><br>

## Redux
flux는 다수의 store를 사용하지만<br>
**Redux**는 외부에 하나의 스토어두고 컴포넌트와 스토어 사이에 변경사항을 반영해주는 라이브러리이다!<br>
아무리 깊게 존재하는 컴포넌트라도 직속부모에게 변경사항을 업데이트하고 받아오는 것 처럼 간단해진다<br>
```
"A 컴포넌트 -> 스토어에 변경된 상태 업데이트하고 (Dispatch)
스토어는 -> B컴포넌트로 필요한 상태의 변경사항을 props로 전달 (subscribe)"
```
<br><br>
![Redux01](https://github.com/WonjeongPark/whatIThink/blob/master/IMG/Redux01.png?raw=true)
<br><br>

## Redux 원칙 3가지

## Redux 적용
