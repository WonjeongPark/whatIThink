# Redux-saga

redux나 redux thunks, redux-saga에 관한 지식들은 검색해보면 많다.<br>
나 또한 도대체 무엇인지 검색하고 많은 글을 읽었지만 이해가 쉽지 않았다.<br>
아직도 redux관련 모든 지식을 아는 것은 아니지만 그래도 이것이 redux구나, 정확히는 redux-saga구나 느끼기 시작한 것은<br>
작성된 코드를 보고 그간 읽었던 글 속의 지식들을 껴 맞춰보고 나서 인것 같다.<br>
[이전에 redux에 대한 글](https://github.com/WonjeongPark/whatIThink/blob/master/whatRedux.md)을 썼지만 그때와 지금의 이해하는 것은 다르다. 아마 시간이 갈수록 더 다를 것이라 생각한다.<br>
내가 이해한 의식의 흐름 순서대로 정리해본다.<br>

![redux](https://github.com/WonjeongPark/whatIThink/blob/master/IMG/redux.png?raw=true)

## redux와 redux-thunks
redux는 예전에 쓴 글에 언급했듯이<br>
**외부에 하나의 스토어두고 컴포넌트와 스토어 사이에 변경사항을 반영해주는 라이브러리**이다!<br>
`redux-thunk`는 redux에서 사용하는 리덕스 미들웨어이고<br>
여기서 `thunk`는 표현식을 감싸고 안에 있는 내용의 실행을 지연시킬 수 있는 함수이다.<br>
```
const a = 4+5; //바로 4+5가 되어 a = 9

const thunkEx = () => 4+5; // thunkEx 함수가 호출되어야 4+5가 실행된다. --> thunk함수
```

### 미들웨어를사용하지 않은 redux는 '사실'인 것만 반환한다.<br>

```
function fetchSecretSauce() {
    return fetch('http://www.google.com/search?q=secret+sauce');
}

function makePizza(forPerson, secretSauce) {
    return {
        type: 'MAKE_PIZZA',
        forPerson,
        secretSauce
    }
}

function aplogize(fromPerson, toPerson, error) {
    return {
        type: 'APOLOGIZE',
        fromPerson,
        toPerson,
        error
    };
}

function withdrawMoney(amout) {
    return {
        type: 'WITHDRAW',
        amout
    };
}
/.../
store.dispatch(withdrawMoney(100));
```

### redux-thunks는 성공여부에 따라 Success(아래에선 sauce), failure(error)액션을 반환한다.
선택적으로 몇 가지 파라미터를 인수로 취하고 또 다른 함수를 반환하고 내부함수로 dispatch와 getState함수를 사용한다.<br>

```
function makePizzaWithSecretSauce(forPerson) {
    //dispatch를 사용하여 함수를 반환, 디스패치 나중에 가능

    return function (dispatch) {
        return fetchSecretSauce().then(
            sauce => dispatch(makePizza(forPerson, sauce)),
            error => dispatch(aplogize('The Pizza Shop', forPerson, error))
        );
    };
}
```

특정 조건이 만족되면 디스패치 하거나, 액션이 디스패치될 때 딜레이를 하는 기능이 있다.<br>

```
const INCREMENT_COUNTER = 'INCREMENT_COUNTER';

function increment() {
    return {
        type: INCREMENT_COUNTER

function incrementOdd() {
    return (dispatch, getState) => {
        const { counter } = getState();

        if(counter % 2 === 0) {      //조건문
            return;
        }
        
        dispatch(increment());
    }
}
```
```
function incrementAsync() {
    return dispatch => {     
        setTimeout(() => {
            dispatch(increment()); // 나중에 실행
        }, 1000)
    }
}
```
코드에서 알 수 있듯이 redux thunk는 redux에서 비동기 개념이 포함된 것이다.<br>
더불어 `Promise Chain`(ex. 을 사용하여 thunk의 반환값을 반환하는 과정을 제어할 수 있다.<br>

```
store.dispatch(
    makePizzaWithSecretSauce('My wife')
).then(() => {
    console.log('Done!')
})
```

<br>또한 다른 액션객체와 사용 가능하고 Promise로 반환가능하다.<br><br>

## redux-Saga !! [참고](https://meetup.toast.com/posts/140)

`제네레이터(generator)`함수는 `function*`과 같이 `별표`가 붙는 것이 특징이며<br>
기본적으로 **`yield`를 만날 때 마다** 실행되거나 멈춰있거나 건너뛰며 진행된다.<br>
제네레이터함수(=Callee)를 호출하고 위 과정처럼 내부 로직 제어를 하는 것이 Runner(Caller)이다.<br>
redux-saga에서는 `saga`는 제네레이터함수 callee이고 Runner(caller)가 미들웨어인 셈이다.<br>

제네레이터함수 = {`yield`+`이펙트생성자=명령`+(`이펙트=명령처리`)}로 이루어진다.<br>

```
function* sendSaga() {
  const action: SendMessageAction = yield take("SEND_REQUEST"); ****
  const { title, message } = aciton.payload;
  try {
    yield call(api.send, title, message);
  } catch (err) {
    yield put(sendFailure(err));
    return;
  }
  yield put(sendSuccess());
}
```

<br>위의 간단한 예로 보면<br>
sendSaga가 즉시 실행되고나서 SEND_REQUEST가 dispatcher되기 전까지 yield는 첫번째 줄에서 멈추고 후에 실행된다.<br>


## redux, redux-thunk, redux-saga 간단비교!
사실만 반환하는 redux, 그리고 thunk를 사용하는 것으로 함수를 반환하고 성공 여부에 따라 액션을 전달!<br>
그러나 action에 응답을 할 수 없는 thunks, saga를 통해서는 액션에 응답을 하는 것으로 실행을 제어하고 saga가 실행!<br>


## saga helper = 이펙트 생성자 
`effect`는 미들웨어가 수행할 명령을 가진, call이나 put 등의 생성자를 통해 생성되는 JS객체이다.<br>
`redus-saga`에서는 yield에 속한, 다양한 명령을 가진 `effect`들을 수행하는데,<br>
effect에게 명령을 내리는 것, 즉 effect가 존재되어 실행가능하도록 하는 것이 `effect creator=이펙트 생성자`이다.<br>
saga가 effect만 yield해야하는 것은 아니지만 되도록 effect만을 yield하는 saga를 작성하는 것이 좋다.<br>


>select: State로부터 필요한 데이터를 꺼낸다.<br>
put: Action을 dispatch한다.<br>
take: Action을 기다린다. 이벤트의 발생을 기다린다.<br>
call: Promise의 완료를 기다린다.<br>
fork: 다른 Task를 시작한다.<br>
join: 다른 Task의 종료를 기다린다.<br>
takeEvery: 동시에 시작되는 여러 개의 fetchData instance들을 허용한다.<br>
takeLatest: 가장 마지막에 발생된 request의 response를 얻고 싶다면 사용한다. <br>
이 외에도 많은 effect creators들이 존재하고 [이 곳](https://redux-saga.js.org/docs/api/)을 참고하면 된다.<br>

## connect [참고](https://code.tutsplus.com/ko/tutorials/getting-started-with-redux-connecting-redux-with-react--cms-30352) [참고2](https://velog.io/@velopert/Redux-3-%EB%A6%AC%EB%8D%95%EC%8A%A4%EB%A5%BC-%EB%A6%AC%EC%95%A1%ED%8A%B8%EC%99%80-%ED%95%A8%EA%BB%98-%EC%82%AC%EC%9A%A9%ED%95%98%EA%B8%B0-nvjltahf5e#connect-%ED%95%A8%EC%88%98%EB%A5%BC-%EC%82%AC%EC%9A%A9%ED%95%98%EC%97%AC-%EC%BB%B4%ED%8F%AC%EB%84%8C%ED%8A%B8%EC%97%90-%EC%8A%A4%ED%86%A0%EC%96%B4-%EC%97%B0%EB%8F%99%ED%95%98%EA%B8%B0)
`redux`를 이용하여 `store`를 만든 후에 `react`와 어떻게 연결할 것인지를 생각해봐야한다.<br>
우선 APP컴포넌트에 `<Provider />`를 사용하여 props에 store객체를 전달한다.<br>
그리고 `store`에 `action`을 `dispatch`하고 state를 업데이트할 `component`에 `connect()`를 사용하고<br>
리덕스와 연결된 component를 container component라고 부른다.<br>
>import { connect } from 'react-redux';

여기서 mapDispatchToProps와 mapStateToProps의 메서드가 등장한다.<br>
두 메서드는 객체를 반환하는데 객체의 key는 props가 된다.<br>
`mapStateToProps` - store를 subscribe하여 **store의 state**를 인수로 받아 **props로 매핑**<br>
`mapDispatchToProps` - **action을** dispatch하고 그 callback을 **props에 매핑**<br>

```
export default connect(
  mapStateToProps,
  mapDispatchToProps
)(ComponentName)
```

### Redux는 필수적일까?
redux를 사용하는 방법을 적은 것은 아니지만 redux, redux-thunk, redux-saga가 무엇인지 이해를 돕기 위한(?)<br>
정확히는 내 머릿속을 정리하기 위한 글을 쓰기위해 다른 개발자들이 쓴 글을 많이 읽어봤다.<br>
react를 이용하여 개발하는 데에 redux는 `손이 많이가지만 사용하면 유용한 점이 분명 존재`한다.<br><br>

redux를 사용한다면 엄격한 규칙을 강제하는 것으로 데이터와 액션 등 텍스트로 기록이 남게 되기 때문에<br>
사용자가 데이터의 변경이나 실행된 액션이 무엇인지 쉽게 알 수 있다. 따라서 디버깅도 수월할 것이다.<br>
또한 적용 후에는 유지보수가 보다 간편할 수 있다.<br><br>

**그렇다면 redux는 필수일까?**<br>
감히 내가 이 것이 필수다, 그렇지 않다를 논하지 못하지만 그럼에도 이야기 할 수 있는 것은<br>
**처음 react를 공부하고 사용하면서부터 redux를 사용하지 않은 것이 잘한 일이라 느낀다는 것이다.** <br>
redux가 무엇인지 감도 못잡았을 시절 더불어 react라는 것도 버겁던 과거에<br>
react하면 따라다니는 redux라는 개념(?)을 무조건 배워야 할 것 같다는 압박이 있었다.<br><br>
결론적으로 멘토님의 조언으로 redux없이 howto를 만들었고(만들고 있고)<br>
redux를 공부하고 사용하는 지금 howto개발 당시 redux를 사용하지 않은 것이 더 많은 도움이 되었다고 요새 많이 느낀다..<br>
redux를 사용하지않고 번거롭더라도(?) 힘들더라도 순수 react로 상태관리를 하며 howto를 만들었기 때문에<br>
지금 redux가 주는 편리함을 제대로 느낄 수 있는 것이라 생각한다.<br>
또한 redux는 react에서 사용하는 일종의 도구인 것이지 react = redux 가 아니기 때문에 react에 대한 이해도도 많이 높아졌다고 생각한다.<br>
firebase도 같은 맥락에서 howto에 적용하지 않은 것을 잘한 일이라 생각한다.<br><br>

그래서 redux가 필수적이냐는 질문의 답은...... 없다!<br>
개인의 판단이고 취향이지만 적어도 react가 처음이라면 사용하지 않는 것을 추천한다!
