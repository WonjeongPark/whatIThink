# Routing
라우팅(routing)은 쉽게 말하자면 주소에따라 다른 화면(뷰)를 보여주는 것이다.<br>
웹앱의 복잡도가 높아지면, routing에 관한 방법이 필요해진다.<br>
React router 4는 오늘날 리액트를 사용하는 사람들의 대다수가 사용하는 routing기법이라 할 수 있다.

### React router 4
> SPA는 라우터가 필요하다.

SPA는 Single Page Application의 약자이다.<br>
SPA는 말 그대로 1개의 페이지로 구성된 어플리케이션 형태이고,<br>
기존에 다수의 페이지가 존재하는 형태와는 사뭇 다르다.<br><br>
그렇다고 종류가 한 가지라는 뜻은 아니다.<br>
개발을 하다보면 당연히 다양한 종류의 SPA가 필요하기 때문에<br>
주소마다 다른 뷰를 그려주는 **React router**가 필요하다.<br>

> 클라이언트에서 라우팅이 이루어진다.(<->서버)

**HTML처럼 전통적인 방식**은 유저의 요청에 따라 그때 그때 새로 페이지를 로드하기 때문에<br>
서버에서 파일을 제공하는 서버쪽 라우팅이라면<br><br>
**react**에서는 모든 파일을 뿌려준 후<br>
클라이언트 라우팅을 통해 가상으로 내부에서 각각의 주소마다 다른 컴포넌트를 보이게 하는 것이다.<br><br>
작은 정보의 변화가 활발한 경우라면 클라이언트 라우팅이 적합하고<br>
불필요한 렌더링과 트래픽낭비를 방지하는데 도움이 될 수 있다.<br>
(반대로 서버에서 한번에 많은 양의 데이터를 받아와<br>
라이언트 쪽에서는 읽기만 하는 경우에는 서버쪽 라우터가 적합하다.)<br>

`하지만 개발자의 성향에 따라 다르기 때문에 정답은 없다.`<br>

**클라이언트쪽 라우터 Ex) Facebook과 같은 사이트에 좋아요누르기, 댓글달기등의 경우<br>
서버쪽의 라우터 Ex) 게시판의 게시글이나 많은 양의 글을 한꺼번에 받아오는 경우**<br>

> React Router v3와 React Router v4

v3은 static requests를 처리하는 과정으로<br>
모든 라우팅에 관한 정보를 한 곳에(최상위페이지) 계층구조로 모아두기 때문에 관리가 쉽지만<br>
정적이라는 특성때문에 확장성과 재사용성 부분에서는 비효율적일 수 있었다. <br>
그러나  Dynamic routing이 적용된 v4는 필요한 컴포넌트에 직접 붙여 사용하기 때문에<br>
한 곳에 모아둘 필요가 없고 더 효율 적이다.<br><br>
또한 설치모듈도 `react-router`라는 코어모듈에 `react-router-dom`,`react-router-native`가 추가되었다.<br>

> History 

`History`를 저장한다는 것은 현재 location을 저장하고,<br>
언제든지 Rerender가 가능하게 하고 주소변경시 페이지 전체를 리로드할 필요가 없다는 것을 뜻한다.<br>
v3에서는 `browserHistory`에 저장하여 Router의 프로퍼티로 넣어줘서<br>
유저가 뒤로가기를 눌러도 이전 페이지를 불러 올 수 있게 하였다.<br>
v4에서는 `BrowserRouter`가 자체적으로 처리하기 때문에 위의 절차를 할 필요는 없다.<br>

라우터가 아닌 컴포넌트에서 라우터의 객체 `history`를 포함한 `location`, `match`를 사용하려면 <br>
**Higher order component**인 `withRouter`를 사용해야한다.<br><br>
![withRouter1](https://github.com/WonjeongPark/whatIThink/blob/master/withRouter1.png?raw=true)
<br>라우터가 아닌 NavBar에서 history props를 적용해 사용하고 싶다면<br><br>
![withRouter2](https://github.com/WonjeongPark/whatIThink/blob/4defdd4107a8fbed179a158a83ff04f15d709b6e/withRouter2.png?raw=true)
<br>NavBar에 withRouter를 사용하면 된다.<br>
적용방법은 조금씩 다르겠지만, withRouter의 역할이 이런 것이다고 이해하면 된다.<br>
