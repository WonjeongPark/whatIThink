# Redux-saga

redux나 redux thunks, redux-saga에 관한 지식들은 검색해보면 많다.<br>
나 또한 도대체 무엇인지 검색하고 많은 글을 읽었지만 이해가 쉽지 않았다.<br>
아직도 redux관련 모든 지식을 아는 것은 아니지만<br>
그래도 이것이 redux구나, 정확히는 redux-saga구나 느끼기 시작한 것은<br>
작성된 코드를 보고 그간 읽었던 글 속의 지식들을 껴 맞춰보고 나서 인것 같다.<br>
내가 이해한 의식의 흐름 순서대로 정리해본다.<br>

![redux](https://github.com/WonjeongPark/whatIThink/blob/master/IMG/redux.png?raw=true)

## redux, redux-thunks
<비교글추가><br><br>


## Saga
`redux-saga`는 미들웨어이다.<br>
`action`이 `dispatch`되어 `reducer`에서 처리되기 전에 작업을 수행하도록 지정하는 중간자이다.<br>
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
위의 간단한 예로 보면<br>
sendSaga가 즉시 실행되고나서 SEND_REQUEST가 dispatcher되기 전까지 yield는 첫번째 줄에서 멈추고 후에 실행된다.<br><br><br>


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


다음은 redux폴더에 속한 actions, reducer, saga의 예이다.<br>
`action`이 `reducer`에 `dispatch`되기 전 `saga`에서 다양한 `effect`를 이용해서 상황에 따른 행동을 정해주고,<br>
그에 맞게 여러가지 작업들이 실행된다.<br><br>

위에서 언급한 `saga`에서 어떤 행동을 취할지 정해주`는 것이 비동기처리를 하기 위한 준비물, `Effect`이다.<br>

>select: State로부터 필요한 데이터를 꺼낸다.
put: Action을 dispatch한다.
take: Action을 기다린다. 이벤트의 발생을 기다린다.
call: Promise의 완료를 기다린다.
fork: 다른 Task를 시작한다.
join: 다른 Task의 종료를 기다린다.


## connect
<내용추가><br>
