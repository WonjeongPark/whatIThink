# Hook

## `Hook`은 리액트 16.8부터 본격 사용가능하게 된 기능이다.<br><br>

>`Hook`이 나타난 배경<br>

component에 속한 stateful logic이 재사용이 어렵고 <br>
render props나 HOC로 대체해서 사용하는 방법 또한 <br>
사용한 코드를 재설계해야하는 번거로움과 코드가 복잡해지는 등 결과가 나타날 수 있다.<br>
이런 종류의 문제를 해결하기 위하여 나온 개념이 `Hook`이다.<br><br>
(render props = 값(value)이 함수인 prop을 사용해서 component간의 code를 공유하는 패턴)<br>
(HOC = 컴포넌트에서 자주 반복되는 코드를 내부에서 컴포넌트화하여 다른 컴포넌트에 부여가능한 함수)<br><br>

>Hook은 class없이 state를 사용할 수 있는 새로운 기능이다! = 함수형 컴포넌트 <br>

class없이 state를 사용할 수 있으려면 우리가 알고있는 stateless component의 형태로 선언되어야 한다.<br>
```
const Example = (props) => {
  // You can use Hooks here!
  return <div />;
}
```
혹은
```
function Example(props) {
  // You can use Hooks here!
  return <div />;
}
```
<br>
Hook은 이러한 경우에서 state의 사용을 가능하게 하므로 function component, 즉 함수형 컴포넌트라 부를 수 있다.<br>

## State Hook 그리고 Effect Hook

### State Hook (useState)

> useState - 단일 상태 관리

아주 간단한 Count 로직을 생각보자.<br>
```
class Number extends React.Component {
  constructor(props) {
    super(props);
    this.state = {
      count: 0
    };
  }

  render() {
    return (
      <div>
        <p>{this.state.count}</p>
        <button onClick={() => this.setState({ count: this.state.count + 1 })}>
          Increment
        </button>
      </div>
    );
  }
}
```

Class component가 필요하고, State는 {count}로 초기값 0으로 시작해서 <br>
사용자가 버튼을 누를 때 마다 this.setState()가 호출되면서 count를 증가시킬 것이다.<br>
count함수 자체보다, 하나의 API를 실행하는 과정으로 이해하면 쉽다.<br><br>
React hook은 this가 없다. 함수형 component를 사용하고 this.state를 사용하지 않고 대신에 useState 사용한다<br>

```
function Number(){
  const [count, setCount] = useState(0);
  
    return(
      <div>
        <div>{count}</div>
        <button onClick={() => setCount(count +1)}>Increment</button>      
      </div>
    );
  }
  ```
  
<br>아주 간단해 보인다. 여기서 기본값은 0이고,<br>
`usestate`를 호출하여 현재 state 값과 이 값을 설정하는 함수를 파라미터로 가지는 배열을 반환한다.<br><br>
count,setCount 자리에 jelly, getJelly와 같이 어떠한 이름이 들어가도 상관없다.<br>
중요한건, useState는 class안에서 this.state와 완벽히 동일한 기능을 한다는 것이다.<br><br>

> useState - 다수 상태 관리

```
const Form = () => {
  const [name, setName] = useState('');
  const [job, setJob] = useState('');

  const onSubmit = e => {
    e.preventDefault();
    alert(`${name}: ${job}`);
    setName('');
    setJob('');
  };

  return (
    <form onSubmit={onSubmit}>
      <input placeholder="성함" value={name} onChange={e => setName(e.target.value)}/>
      <input placeholder="직업" value={job} onChange={e => setJob(e.target.value)}/>
      <button type="submit">확인</button>
    </form>
  );
};
export default Form;
```
인풋에 입력값을 alert을 통해 아래의 사진처럼 얻을 수 있다.<br>

![20190225_1](https://github.com/WonjeongPark/whatIThink/blob/master/20190225_1.png?raw=true)

이와 같이 State Hook으로 불리는 useState 함수를 호출하는 것으로<br>
다수의 state variables를 사용할 필요 없이 **함수형 컴포넌트에 local state를 추가**할 수 있고<br>
this.setState와 달리 업데이트 되는 state variable의 값을 merging하는 것이 아니라 **replacing**한다.<br>

### Effect Hook - useEffect

> useEffect는 lifecycle의 역할을 대신할 수 있다.

lifecycle API 중 componentDidMount, componentDidUpdate, componentWillUnmount를 결합한 기능을 한다.<br><br>
react에서 컴포넌트가 마운트 된 후(componentDidMount) 혹은 업데이트된 후(componentDidUpdate)<br>
같은 작업을 구현하기 위해 중복된 코드를 작성하는 경우가 많다.<br>
위의 useState 단일 상태관리에서 count로직을 다시 생각해 보자.<br>
그 과정에서 count를 title로 출력하려면 componentDidMount와 componentDidUpdate를 각각 사용하여
```
  componentDidMount() {
      document.title = `You clicked ${this.state.count} times`;
    }

  componentDidUpdate() {
       document.title = `You clicked ${this.state.count} times`;
    }
```

중복된 코드를 작성해야 할 것이다.<br>
useEffect를 사용하면 각각의 시점에서 같은 작업을 구현해야 하는 것을 간결하게 처리할 수 있다.<br>
Hook을 활용하여 useState를 import해서 작성하면서, useEffect도 import하여 이렇게 작성해보자.<br>

```
function Number(){
  const [count, setCount] = useState(0);
  
    useEffect(() => {
    document.title = `You clicked ${count} times`;
    });
  
    return(
      <div>
        <div>{count}</div>
        <button onClick={() => setCount(count +1)}>Increment</button>      
      </div>
    );
  }

```

useEffect를 활용할 때 기억해야 하는 사실은 **useEffect에서 나온 effect는 render할 때 마다 호출된다**는 것이다.<br>
특정 상황에만 effect가 실행되게 하고 싶다면 두번째 파라미터를 배열의 형태로 넘겨주면 된다.<br>

`useEffect(() => function(), [count]);`

특정 state, 위의 경우에서는 [count]가 변경된 경우에만 function()가 호출되고(componentDidUpdate의 기능)<br>
빈 배열을 넘겨주면 componentDidMount가 동작하는 것처럼 기능한다.<br><br>

그렇다면 componentWillUnmount의 기능은 어디에 있을까?<br>
앞의 경우들은 clean-up이 필요하지 않는 effect들을 구현한 것이지만 다른 경우도 존재한다.<br>
예를 들면 외부데이터 소스를 구독하는 경우나 chatAPI 적용 시 등 메모리의 낭비를 막기 위한 경우이다.<br>
일반적으로는 componentWillUnmount에서 clean-up 할 것이다.<br><br>

useEffect에서는 function()에 return값이 존재한다면 그것을 hook의 cleanup함수로 인식한다.<br>
componentWillUnmount처럼 그 시점에 한번만 호출 되는건 아니지만,<br>
effect와 짝을 이뤄 다음 effect실행 전에 동작한다.<br><br>

## Hook의 사용 규칙

1. HOOK은 TOP LEVEL에서만 사용해야 한다.<br>

**반복문, 조건문 혹은 감싼 함수 내부에서는 사용하면 안된다.**<br>
component rendering이 될 때 마다 같은 순서로 호출되는 것을 보장하기 위함이다.<br>

2. React 함수에서만 Hook 할 수 있다.<br>

**regular Javascript 함수**에서는 HOOK을 호출하면 안된다.<br>
React function component와 custom hook에서는 가능하다.<br>
(custom hook이란 useState나 useEffect를 응용하여 다양한 hook을 만드는 것. 이미 많은 오픈소스가 존재한다.)
<br><br><br><br>
