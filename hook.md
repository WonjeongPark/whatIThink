# Hook

## `Hook`은 리액트 16.8부터 본격 사용가능하게 된 기능이다.<br><br>

>`Hook`이 나타난 배경<br>

바라던대로 component에 속한 stateful logic이 재사용이 어렵고 <br>
render props나 HOC로 대체해서 사용하는 방법 또한 <br>
사용한 코드를 재설계해야하는 번거로움과 코드가 복잡해지는 등 결과가 나타날 수 있다.<br>
이런 종류의 문제를 해결하기 위하여 나온 개념이 `Hook`이다.<br><br>
(render props = 값(value)이 함수인 prop을 사용해서 component간의 code를 공유하는 패턴)<br>
(HOC = 컴포넌트에서 자주 반복되는 코드를 내부에서 컴포넌트화하여 다른 컴포넌트에 부여가능한 함수)<br><br>

>Hook은 class없이 state를 사용할 수 있는 새로운 기능이다! = 함수형 컴포넌트 <br>

class없이 state를 사용할 수 있을면 우리가 알고있는 stateless component의 형태로 선언되어야 한다.<br>
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
Hook은 이러한 경우에서 state의 사용을 가능하게 하므로 **function component, 즉 함수형 컴포넌트라 부를 수 있다.**<br>

## State Hook 그리고 Effect Hook

### State Hook

> useState - 단일 상태 관리

아주 간단한 Count 로직을 생각보자.<br>
Class component가 필요하고, State도 {count:0}으로 시작해서 <br>
사용자가 버튼을 누를 때 마다 this.setState()가 호출되면서 state.count를 증가시킬 것이다.<br>
count함수 자체보다, 하나의 API를 실행하는 과정으로 이해하면 쉽다.<br><br>
React hook은 this가 없다. 함수형 component를 사용하고 this.state를 사용하지 않고 대신에 useState Hook을 사용해보자<br>

```
function countNumber(){
  const [count, setCount] = useState(0);
  
    return(
      <div>
        <div>{count}</div>
        <button onClick={() => setCount(count +1)}>Increment</button>      
      </div>
    );
  }
  ```
  
<br>아주 간단해 보인다.<br>
여기서 기본값은 0이고, `usestate`를 호출하여 현재 state 값과 이 값을 설정해주는 함수를 파라미터로 가지는 배열을 반환한다.<br><br>
위와 같이 count,setCount 대신 어떠한 이름이 들어가도 상관없다.<br>
중요한건, useState는 class안에서 this.state와 동일한 기능을 하여, 보통 함수를 벗어나면 사라지는 변수들을 저장한다는 점이다.<br>

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
아래와 같은 결과를 얻을 수 있다.<br>

![20190225_1](https://github.com/WonjeongPark/whatIThink/blob/master/20190225_1.png?raw=true)

이와 같이 State Hoo라고도 불리는 useState 함수를 호출하는 것으로<br>
다수의 state variables를 사용할 필요 없이 함수형 컴포넌트에 local state를 추가할 수 있고<br>
this.setState와 달리 업데이트 되는 state variable의 값을 merging하는 것이 아니라 replacing한다.

### Effect Hook

>
