## Lifecycle function<br>
React에는 **LifeCycle**이 존재한다.<br>
생물도 아니고 무슨 LifeCycle인가 싶을 수 있지만,<br><br>
_정확히는 React의 Component가 실행되거나(Mounting), 업데이트되거나(Updating), 제거될(Unmounting) 때<br>
발생하는 특정한 이벤트에 관한 LifeCycle이다._<br><br>
개발자는 Lifecycle을 재정의하는 과정을 통해 Component를 제어하므로 Lifecycle은 중요하다고 볼 수 있다.<br><br>
`미리 말하자면 리액트 17부터는<br>
componentWillMount, componentWillUpdate, componentWillReceiveProps라는 Lifecycle API는 사용하지 않는다!(deprecated)`<br><br>
![ExampleImage2](https://github.com/WonjeongPark/whatIThink/blob/a2f558053e5a4061cf5a0117d613beecd37dd4f5/Lifecycle.jpeg?raw=true)
<br><br>
# Mount
물론 앞서 컴포넌트가 새로 만들어 질 때 마다 호출되는 컴포넌트 생성자 함수 constructor가 존재한다.<br>
```
constructor(props){
  super(props);
  }
```

<br>

_Mounting된다는 것은 컴포넌트가 처음 실행된다는 것과 같은 뜻으로 이해하면 쉽다._<br>
Mount의 과정에는<br> Mounting 전후로 componentWillMount, componentDidMount 두 가지의 Lifecycle API가 실행된다.<br>

>컴포넌트가 실행되는 과정은<br>컴포넌트 시작 >context, defaultProps, state저장 >componentWillMount 호출 >(render->DOM)Mount >componentDidMount호출로 생각하면 된다.<br><br>

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
      data: 'Jordan Belfort'
    }
  }

  getData(){
    setTimeout(() => {
      console.log('Our data is fetched');
      this.setState({
        data: 'Hello WallStreet'
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


주의할 점은, ComponentDidMount는 오직 한번만 호출 된다는 것이다.<br>
_componentDidMount is only called once in the lifecycle of any component, re-render will not reinitialize the component.<br> componentDidUpdate will be called where you can manage your logic._



# Props Updating와 State Updating<br><br>
**Props Updating**<br>
Updating이 발생하면 이를 감지하는 과정에서 componentWillReceiveProps API가 실행된다.<br>
이 후에는 shouldComponentUpdate, componentWillUpdate 순서로 호출되고 업데이트가 완료되면(->render)<br>
componentDidUpdate가 호출된다.<br>
> Update 발생 >componentWillReceiveProps >shouldComponentUpdate >componentWillUpdate >componentDidUpdate<br>

- 업데이트 완료를 기준으로 업데이트 전의 API들은 바뀔 Props,<br> 후의 API는 바뀌기 전의 Props를 가지고 있다.<br>

1. **componentWillReceiveProps**<br>
Called when the component may be **receiving new props**. React may call this even if porps have not changed,<br>
so be sure to compare new and exising props if you only want to handle changes.<br>
Calling component#setState generally does not trigger this method.<br>
componentWillReceiveProps는 새로운 props를 받게 되는 경우<br>
현재의 props와 새로운 props가 있어서 변화하는 부분을 알 수 있다.<br>
_새로운 props를 받기 전의 props가 this.props이고,_ 새로 받게 될 props는 nextProps이다.<br>
ex><br>
```
componentWillReceiveProps(nextProps){
console.log('current: ', this.props.selectedEvent);
console.log('next: ', nextProps.selectedEvent);
}
```
<br>
이 경우 브라우저에서 버튼을 클릭하면 현재의 이벤트와 다음에 나올 이벤트를 콘솔창에서 확인할 수 있다.<br>

![ExampleImage1](https://github.com/WonjeongPark/whatIThink/blob/c06e409d811f0776acefdd56d860bda1c3ca6ef7/20190122_1.png?raw=true)

<br>

2. **shouldComponentUpdate**<br>
앞서 포스팅된 'whyReact-whatReact.md'에서 언급 했듯이,<br>
react는 Virtual DOM에 렌더링 해주는 과정을 통해 변화가 발생한 부분만 업데이트가 가능하다<br>
변화가 없다면, DOM이 아닌 Virtual DOM에만 그저 렌더링하면 된다.<br>
하지만 이 조차도 무수히 많은 처리를 하게 된다면 낭비라 할 수있다.<br>
여기서 shouldComponentUpdate API가 사용된다.<br>
아직 렌더링 되기 전에 실행되는 shouldComponentUpdate에서 true가 아니라<br>
false를 return한다면 렌더링을 취소 할 수 있고 쓸데 없는 update를 방지할 수 있다.<br>
