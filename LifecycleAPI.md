# Lifecycle function<br>
React에는 **LifeCycle**이 존재한다.<br>
생물도 아니고 무슨 LifeCycle인가 싶을 수 있지만,<br><br>
_정확히는 React의 Component가 실행되거나(Mounting), 업데이트되거나(Updating), 제거될(Unmounting) 때<br>
발생하는 특정한 이벤트에 관한 LifeCycle이다._<br><br>
개발자는 Lifecycle을 재정의하는 과정을 통해 Component를 제어하므로 Lifecycle은 중요하다고 볼 수 있다.<br><br>
`미리 말하자면 리액트 17부터는
componentWillMount, componentWillUpdate, componentWillReceiveProps라는 Lifecycle API는 사용하지 않는다!(deprecated)`<br><br>
![ExampleImage2](https://github.com/WonjeongPark/whatIThink/blob/a2f558053e5a4061cf5a0117d613beecd37dd4f5/Lifecycle.jpeg?raw=true)
<br><br>
## Mount<br><br>
물론 앞서 컴포넌트가 새로 만들어 질 때 마다 호출되는 컴포넌트 생성자 함수 constructor가 존재한다.<br>
```
constructor(props){
  super(props);
  }
```

<br>

Mounting된다는 것은 컴포넌트가 처음 실행된다는 것과 같은 뜻으로 이해하면 쉽다.<br>
이 과정에는 Mounting 전후로 componentWillMount, componentDidMount 두 가지의 Lifecycle API가 실행된다.<br>

>컴포넌트가 실행되는 과정은<br>컴포넌트 시작 >context, defaultProps, state저장 >~~componentWillMount~~ ><br>(render->DOM)Mount >componentDidMount호출로 생각하면 된다.<br><br>

1. `componentWillMount` -**deprecated**<br>
componentWillMount는 Mounting되기 직전, 즉 화면에 나가기 직전에 호출된다.<br>
render전이기 때문에 DOM에 접근할 수 없고, props, state에 변화를 주면 안된다.<br>
앞서 말한 것 처럼, 이 API는 더이상 사용하지 않기 때문에 자세히 알아보지 않을 것이다.<br>
>(또한, componentWillMount에서 하던 일들은 constructor와 아래의 componentDidMount에서 처리 가능하고<br>
UNSAFE_componentWillMount()로 수정하여 사용은 가능하다.)<br>
<br>
        rendering을 통해 컴포넌트가 DOM에 부착되고 Mount가 완료되면 componentDidMount가 호출된다.<br><br>

2. `**componentDidMount**`<br>
componentDidMount는 컴포넌트가 mounting된 직후 호출되는 API이다.<br>
이름처럼 해당 페이지에 모든 element들이 올바르게 render되면 이 메소드가 호출된다.<br><br>
componentDidMount는 setState()메소드를 통해 state를 변경하고 <br>
JSX에 업데이트된 데이터를 로드하는 render()를 호출하는 완벽한 장소이다. <br>
Mounting후에 호출되므로 DOM에 접근이 가능하고, 따라서 AJAX를 요청하거나 변화를 줄 수 있기 때문이다.<br><br>
예를 들어 API로 부터 어떤 data를 가져오려 한다면(fetch) lifecycle 메소드 안에 존재해야한다.
```
import React, { Component } from 'react';

class App extends Component {

  constructor(props){
    super(props);
    this.state = {
      data: 'Harry Potter'
    }
  }

  getData(){
    setTimeout(() => {
      console.log('data is fetched');
      this.setState({
        data: 'Hello Hogwarts'
      })
    }, 1000)
  }

```

component가 올바르게 rendering되면 componentDidMount가 호출되고<br>
```
componentDidMount(){
    this.getData();
  }

  render() {
    return(
      <div>
      {this.state.data}
    </div>
    )
  }
}

export default App;
```
그 안에 존재하는 getData()도 호출되기 때문이다.


**주의할 점은, ComponentDidMount는 오직 한번만 호출 된다는 것이다.**<br>
_componentDidMount is only called once in the lifecycle of any component, re-render will not reinitialize the component.<br> componentDidUpdate will be called where you can manage your logic._<br><br>
componentDidMount는 컴포넌트 lifecycle에서 오직 한번만 호출된다.<br>
따라서 re-rendering을 해도 컴포넌트는 다시 초기화 되지 않고 componentDidUpdate가 호출될 것이다.<br><br>

**또한 setState()와 같이 사용하는 것에 주의가 필요하다**<br>
Mounting 후에 호출되는 componentDidMount안에 setState()가 존재하는 경우,<br>
이미 화면에 보여진 후, setState()를 통해 state가 변화하면 다시 rendering된다.<br>
화면에 업데이트된 것이 보이기 전에 추가적인 rendering이 발생하는 것이다.<br>
하나의 케이스에서 두번의 rendering이 일어난다면, 사용자는 중간 과정에서 사라진 state를 알 수 없다.<br>
그렇게 때문에 대부분의 경우에서는 랜더링 전에 constructor()에서 해주는 것이 좋다.<br><br>

## Props Updating와 State Updating<br><br>
**Props Updating**<br>
Updating이 발생하면 이를 감지하는 과정에서 componentWillReceiveProps API가 실행된다.<br>
이 후에는 shouldComponentUpdate, componentWillUpdate 순서로 호출되고 업데이트가 완료되면(->render)<br>
componentDidUpdate가 호출된다.<br>
> Update >~~componentWillReceiveProps~~(staticgetDerivedStateFromProps) >shouldComponentUpdate >~~componentWillUpdate~~ >(render->) > getSnapshotBeforeUpdate> componentDidUpdate<br>

- 업데이트 완료를 기준으로 업데이트 전의 API들은 바뀔 Props,<br>
- 후의 API는 바뀌기 전의 Props를 가지고 있다. (componentDidUpdate)<br>

1. `componentWillReceiveProps`-**deprecated**<br>
componentWillReceiveProps는 새로운 props를 받게 되는 경우<br>
현재의 props와 새로운 props가 있어서 변화하는 부분을 알 수 있다.<br>
새로운 props를 받기 전의 props가 this.props이고, 새로 받게 될 props는 nextProps이다.<br>
이 API 또한 리액트 17 이후로 UNSAFE_componentWillReceiveProps로 사용 가능하나,<br>
상황에따라 대체하여 사용하는 것이 권장되고 staticgetDerivedStateFromProps()이 그것이다.<br>
<br>

2. `static getDerivedStateFromProps` -`new`<br>
<br>
**static getDerivedStateFromProps(props, state)**
<br>
getDerivedStateFromProps는 처음 실행되는 Mounting과 후속 업데이트에 호출되는 render()직전에 호출되고<br>
변화된 state나 변화된 부분이 없을 경우에는 null을 반환한다.<br>
이름 그대로 state의 값에 props로 받은 값을 동기화해야하는 경우에 사용한다.<br><br>

**State Updating**<br>

3. `**shouldComponentUpdate**`<br>
앞서 포스팅된 'whyReact-whatReact.md'에서 언급 했듯이,<br>
react는 Virtual DOM에 렌더링 해주는 과정을 통해 변화가 발생한 부분만 업데이트가 가능하다<br>
변화가 없다면, DOM이 아닌 Virtual DOM에만 그저 렌더링하면 되는데,<br>
하지만 이 조차도 무수히 많은 처리를 하게 된다면 낭비라 할 수있다.<br>
여기서 shouldComponentUpdate API가 사용된다.<br>
아직 렌더링 되기 전에 실행되는 shouldComponentUpdate에서 true가 아니라<br>
false를 return한다면 렌더링을 취소 할 수 있고 쓸데 없는 update를 방지할 수 있다.<br>
컴포넌트를 최적화하는 작업에 매우 유용하다.<br>
<br>

4. `componentWillUpdate`-**deprecated**<br>
새로운 props나 state를 받아 시행되는 rendering 직전에 호출되기 때문에 <br>
업데이트가 발생하기 전에 준비하는 기회로 사용되기도 한다. <br>
그러나 이 API 또한 리액트 17 이후로 UNSAFE_componentWillUpdate로 사용 가능하나,<br>
상황에따라 대체하여 사용하는 것이 권장되고 본래의 기능은 getSnapshotBeforeUpdate()로 대체 가능하다.<br>
<br>

5. `**getSnapshotBeforeUpdate**`<br>
rendering이 발생한 직후 실직적인 DOM에 변화가 일어나기 전에 발생하는 API이다.<br>
컴포넌트가 DOM이 변화하기 전에 DOM으로부터 특정 정보를 받아 오게 해주는 것이다.<br>
이 API에서 return되는 값는 componentDidUpdate의 파라미터값이 될 수 있다.<br>
흔히 사용하지 않지만, 특별한 방법으로 스크롤 위치를 조절하는 경우에 사용한다.<br>
snapshot의 값은 null 혹은 특정한 값을 리턴해야한다.<br>
<br>

6. `**componentDidUpdate**`<br>
updating이 발생되고 즉시 호출되는 API로, 컴포넌트가 업데이트될 때 DOM을 조작하기위해 사용한다.<br>
즉 shouldComponentUpdate에서 true를 반환하는 경우 (updating이 발생한 경우) 호출되는 것인데<br>
(false일 경우에는 호출되지 않는다.)<br>
변화한 this.props와 변화하기 전 prevProps를 비교할 때 사용하기 좋다.<br>
**주의 할 점은** setState()와 사용시 무한 루프에 빠질 수 있으니 props를 비교하는 if조건문을 사용해서 감싸줘야한다.<br>
또, 흔하지 않게 getSnapshotBeforeUpdate의 lifecycle API가 실행되는 경우에는<br>
그 리턴 값을 세번째 파라미터 'snapshot'으로 받는다. 그렇지 않은 경우에는 snapshot의 값을 정의하지 않는다.<br>
<br>

## Unmount<br><br>
컴포넌트가 제거되는 것을 UnMount라고 표현한다.<br>
이를 위해 호출 되는 API는 componentwillUnmount이다. (componentDidUnmount는 없다.)<br>
제거된 컴포넌트는 이벤트를 발생시키지 않으므로 주로 이벤트 리스너를 제거하거나<br>
setTimeout을 clearTimeout을 통해 제거하는 등의 역할을 한다.<br>
<br><br>

## error<br><br>
에러 발생시 componentDidCatch API가 유용하게 사용될 수 있다.<br>
자식 컴포넌트에서 에러가 throw되는 경우 호출되고, error와 info라는 두 개의 파라미터를 가진다.<br>
error는 throw된 에러이고, info는 에러를 throw한 컴포넌트의 핵심적인 정보에 관한 것이다.<br>
```
componentDidCatch(error, info) {
  console.error(error, info);
}
```
최상위 컴포넌트에 한 번만 넣어주면 된다.

## 마무리
<br><br>
위에서 언급한 것 처럼 Lifecycle API를 이용해 개발자들은 컴포넌트를 컨트롤 한다.<br>
따라서 구조적으로 잘 이해하고 필요한 사항을 알아두면<br>
나중에 해결해야하는 문제가 있을 때 적재적소에 사용하면 된다.<br>

