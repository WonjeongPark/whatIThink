# Redux-saga

redux나 redux thunks, redux-saga에 관한 지식들은 검색해보면 많다.<br>
나 또한 도대체 무엇인지 검색하고 많은 글을 읽었지만 이해가 쉽지 않았다.<br>
아직도 redux관련 모든 지식을 아는 것은 아니지만<br>
그래도 이것이 redux구나, 정확히는 redux-saga구나 느끼기 시작한 것은<br>
작성된 코드를 보고 그간 읽었던 글 속의 지식들을 껴 맞춰보고 나서 인것 같다.<br>
내가 이해한 의식의 흐름 순서대로 정리해본다.<br>

![redux](https://github.com/WonjeongPark/whatIThink/blob/master/IMG/redux.png?raw=true)

## redux와 redux-thunks
redux는 예전에 쓴 글에 언급했듯이<br>
외부에 하나의 스토어두고 컴포넌트와 스토어 사이에 변경사항을 반영해주는 라이브러리이다!<br>
`redux-thunk`는 redux에서 사용하는 리덕스 미들웨어이고<br>
여기서 `thunk`는 표현식을 감싸고 안에 있는 내용의 실행을 지연시킬 수 있는 함수이다.<br>
```
const a = 4+5; //바로 4+5가 되어 a = 9

const thunkEx = () = 4+5; // thunk의 함수가 호출되어야 4+5가 실행된다. --> thunk함수
```

<br>redux는 '사실'인 것만 반환하고<br>

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

<br>비동기처리뿐만 아니라, API호출이나 라우터 트렌지션 등을 처리할 때는 미들웨어인 Thunk를 사용해야 한다.<br><br>
thunks는 선택적으로 몇가지 파라미터를 인수로 취하고 또 다른 함수를 반환하는 함수이다.<br>
내부함수로 dispatch와 getState함수를 사용하며 성공여부에 따라 Success(아래에선 sauce), failure(error)액션을 반환한다.<br>

```
function makePizzaWithSecretSauce(forPerson) {
    //dispatch를 사용하여 함수를 반환, 디스패치 나중에 가능

    return function (dispatch) {
        return fetchSecretSauce().then(
            sauce => dispatch(makePizza(forPerson, sauce)),
            error => dispatch(aplogize('The Sandwich Shop', forPerson, error))
        );
    };
}
```

특정 조건이 만족되면 디스패치 하거나, 액션이 디스패치될 때 딜레이를 하는 기능이 있다.<br><br>

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
`Promise Chain`을 thunk의 반환값을 리턴하는 만큼 사용하는 것이 가능하다.
```
store.dispatch(
    makePizzaWithSecretSauce('My wife')
).then(() => {
    console.log('Done!')
})
```

또한 다른 액션객체와 사용 가능하고 Promise로 반환가능하다.

## Saga
`redux-saga`도 미들웨어이다.<br>
`action`을 비동기적으로 `dispatch`했을때 `saga`에서 어떤 행동을 취할지 정해주면 `reducer`로 전달된다.<br><br>
`saga`는 `제네레이터(generator)`라고 불리는 함수로 구성된다.<br>
`제네레이터(generator)`함수란 기본적으로 `yield`를 만날 때 마다 실행되거나 멈춰있거나 건너뛰며 진행된다.<br>
`Thunks`는 `actio`n에 응답을 줄 수 없지만 `saga`에서는 `generator함수`에 의해<br>
`store`를 구독하고 특정 작업이 디스패치 될 때 saga가 실행되도록 할수있다.<br>
또한 `function*`과 같이 `별표`가 붙는 것이 특징이다.<br>
```
function* sendSaga() {
  const action: SendMessageAction = yield take("SEND_REQUEST");
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
sendSaga가 즉시 실행되고나서 SEND_REQUEST가 dispatcher되기 전까지 yield는 첫번째 줄에서 멈추고 후에 실행된다.<br><br>


```
// './redux/user/actions.js'

const actions = {
  REQUEST_USER: "REQUEST_USER",
  GET_USER: "GET_USER",
  getUser: user => {
    return { type: actions.REQUEST_USER, user };
  }
};
export default actions;
```

```
// './redux/user/saga.js'

import actions from "./actions";
import { all, takeEvery, put, fork } from "redux-saga/effects";
import axios from "axios";
import { endpoints } from "../../settings";


function* requestUser(action) {
  let d = yield axios.post(endpoints.Userinfo, { user: action.user });
  yield put({ type: actions.GET_USER, data: d.data });
}

export default function* rootSaga() {
  yield takeEvery(actions.REQUEST_USER, requestUser);
}
```

```
// './redux/user/reducers.js'

import actions from "./actions";

export default function userReducer(state = {}, action) {
  switch (action.type) {
    case actions.GET_USER:
      return { ...state, data: action.data };
    default:
      return state;
  }
}
```

### redux, redux-thunk, redux-saga
```사실만 반환하는 redux에 thunk를 사용하는 것으로 함수를 반환하고 성공여부에따라 액션을 전달했다
또한 action에 응답을 할 수 없는 thunks 와 달리
saga를 통해서는 특정 작업이 디스패치될 때 saga가 실행되도록 할 수 있다.
```

## saga effect

다음은 redux폴더에 속한 actions, reducer, saga의 예이다.<br>
`action`이 `reducer`에 `dispatch`되기 전 `saga`에서 다양한 `effect`를 이용해서 상황에 따른 행동을 정해주고,<br>
그에 맞게 여러가지 작업들이 실행된다.<br><br>

위에서 언급한 `saga`에서 어떤 행동을 취할지 정해주는 것이 비동기처리를 하기 위한 준비물, `Effect`이다.<br>

>select: State로부터 필요한 데이터를 꺼낸다.
put: Action을 dispatch한다.
take: Action을 기다린다. 이벤트의 발생을 기다린다.
call: Promise의 완료를 기다린다.
fork: 다른 Task를 시작한다.
join: 다른 Task의 종료를 기다린다.


## connect
<내용추가><br>
