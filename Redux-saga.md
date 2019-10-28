# Redux-saga

redux나 redux thunks, redux-saga에 관한 지식들은 검색해보면 많다.<br>
나 또한 도대체 무엇인지 검색하고 많은 글을 읽었지만 이해가 쉽지 않았다.<br>
아직도 redux관련 모든 지식을 아는 것은 아니지만 그래도 이것이 redux구나, 정확히는 redux-saga구나 느끼기 시작한 것은<br>
작성된 코드를 보고 그간 읽었던 글 속의 지식들을 껴 맞춰보고 나서 인것 같다.<br>
내가 이해한 의식의 흐름 순서대로 정리해본다.<br>

![redux](https://github.com/WonjeongPark/whatIThink/blob/master/IMG/redux.png?raw=true)

## redux와 redux-thunks
redux는 예전에 쓴 글에 언급했듯이<br>
**외부에 하나의 스토어두고 컴포넌트와 스토어 사이에 변경사항을 반영해주는 라이브러리**이다!<br>
`redux-thunk`는 redux에서 사용하는 리덕스 미들웨어이고<br>
여기서 `thunk`는 표현식을 감싸고 안에 있는 내용의 실행을 지연시킬 수 있는 함수이다.<br>
```
const a = 4+5; //바로 4+5가 되어 a = 9

const thunkEx = () = 4+5; // thunkEx 함수가 호출되어야 4+5가 실행된다. --> thunk함수
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

<br>`Promise Chain`을 사용하여 thunk의 반환값을 반환하는 과정을 제어할 수 있다.<br>

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
그러나 action에 응답을 할 수 없는 thunks, saga를 통해서는 특정 작업이 디스패치될 때 saga가 실행!<br>


## saga helper = 이펙트 생성자 
`effect`는 미들웨어가 수행할 명령을 가진, call이나 put 등의 생성자를 통해 생성되는 JS객체이다.<br>
`redus-saga`에서는 yield에 속한, 다양한 명령을 가진 `effect`들을 수행하는데,<br>
effect에게 명령을 내리는 것, 즉 effect가 존재되어 실행가능하도록 하는 것이 `effect creator=이펙트 생성자`이다.<br>


>select: State로부터 필요한 데이터를 꺼낸다.<br>
put: Action을 dispatch한다.<br>
take: Action을 기다린다. 이벤트의 발생을 기다린다.<br>
call: Promise의 완료를 기다린다.<br>
fork: 다른 Task를 시작한다.<br>
join: 다른 Task의 종료를 기다린다.<br>
takeEvery: 동시에 시작되는 여러 개의 fetchData instance들을 허용한다.<br>
takeLatest: 가장 마지막에 발생된 request의 response를 얻고 싶다면 사용한다. <br>

## connect
<내용추가>
[참고](https://meetup.toast.com/posts/136)<br>
