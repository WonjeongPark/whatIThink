<br>
## Lifecycle function<br>
React에는 LifeCycle이 존재한다.<br>
생물도 아니고 무슨 LifeCycle인가 싶을 수 있지만,<br>
정확히는 React의 Component가 실행되거나(Mounting), 업데이트되거나(Updating), 제거될(Unmounting) 때
발생하는 특정한 이벤트에 관한 LifeCycle이다.<br>
미리 말하자면 리액트 17부터는<br>
componentWillMount, componentWillUpdate, componentWillReceiveProps라는 Lifecycle API는 사용하지 않는다!(deprecated)<br>
![ExampleImage2](C:\Users\LG\Desktop\Lifecycle.jpg)

# Mount
Mounting된다는 것은 컴포넌트가 처음 실행된다는 것과 같은 뜻으로 이해하면 쉽다.
Mount의 과정에는 componentWillMount, componentDidMount 두 가지의 Lifecycle API가 실행된다.
컴포넌트가 실행되는 과정은
컴포넌트 시작 -> context, defaultProps, state저장 -> componentWillMount 호출 -> (render->)Mount -> componentDidMount호출 정도로 생각하면 된다.
Mounting되기 전의 componentWillMount 부분에서는 render하기 전이기 때문에 DOM에 접근할 수 없고, props나 state에 변화를 주면 안된다.
Mounting 후의 componentDidMount에서는 DOM에 접근이 가능하고 따라서 AJAX를 요청하거나 변화를 줄 수 있다.

1. **componentWillMount**
componentWillMount는 Mounting되기 직전, 즉 화면에 나가기 직전에 호출된다.
앞서 말한 것 처럼, 이 API는 더이상 사용하지 않기 때문에 자세히 알아보지 않을 것이다.
componentWillMount에서 하던 일들은 아래의 componentDidMount에서 처리 가능하다.

2. **componentDidMount**<br>
- Called immediately after a compoment is mounted.
Setting state here will trigger re-rendering.<br>
componentDidMount는 컴포넌트가 mounting된 직후 호출되는 API이다.
즉 화면에 나타나게 되었을 때 호출 되는 것이다.

ex>
```
componentDidMount(){
  if (this.props.selectedEvent !== null){
    this.setState({
      event: this.props.selectedEvent
      })
    }
  }
  ```
<br>
# Props Updating
Updating이 발생하면 이를 감지하는 과정에서 componentWillReceiveProps API가 실행된다.
(작성중)

<br>
**componentWillReceiveProps**<br>
- Called when the component may be receiving new props.<br>
React may call this even if porps have not changed,<br>
so be sure to compare new and exising props if you only want to handle changes.<br>
Calling 'component#setState generally does not trigger this method.<br>
<br>
componentWillReceiveProps는 새로운 props를 받게 되는 경우<br>
현재의 props와 새로운 props가 있어서 변화하는 부분을 알 수 있다.<br>
_새로운 props를 받기 전의 props가 this.props이고,_ 새로 받게 될 props는 nextProps이다.<br>
따라서 _업데이트 되기 전의 API가 조회_될 것이다.
ex>
```
componentWillReceiveProps(nextProps){
console.log('current: ', this.props.selectedEvent);
console.log('next: ', nextProps.selectedEvent);
}
```
이 경우 브라우저에서 버튼을 클릭하면 현재의 이벤트와 다음에 나올 이벤트를 콘솔창에서 확인할 수 있다.
![ExampleImage1](C:\Users\LG\Desktop\GithubImg.png)

