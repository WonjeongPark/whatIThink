```
import React, { Component } from 'react';
import logo from './logo.svg';
import './App.css';

class App extends Component {
state = {
  users : []
};

  componentDidMount() {
      // Call our fetch function below once the component mounts
      // const {users} = this.state;
    this.callBackendAPI()
      // .then(res => this.setState({ dates: [...res[0].dates] }))
      .then(res => {console.log(res);
        this.setState( 
        {users : res}

        // {users: ({ id: this.id++, ...res })}
      )} )
      .catch(err => console.log(err));
  }
    // Fetches our GET route from the Express server. 
    //(Note the route we are fetching matches the GET route from server.js
  callBackendAPI = async () => {
    const response = await fetch('/users');
    const body = await response.json();
    // console.log(body)
    if (response.status !== 200) {
      throw Error(body.message) 
    }
    return body;
  };

  render() {
    // const dates = (this.state.dates).toString().replace(/,/g, '');
    // console.log(dates)
    // const datesformat = Date.parse(dates)
    // .toLocaleString("ko-KR",
    // { weekday: 'long', year: 'numeric', month: 'long', day: 'numeric' });
    // console.log(datesformat)
  ```
  ```
    console.log(this.state.users )
    console.log(this.state.users[0]) 
    console.log(this.state.users[1])
    console.log(this.state.users[0].name)
    console.log(this.state.users[1].name)
  
  //**
    // 0은 되고 1은 안되는 이유는 무엇인가!!!!
  //**
  ```
  ```
    // const dates = (this.state.users.dates).replace(/,/g, '');
    // console.log(dates)
    const datas = JSON.stringify(this.state.users)
    console.log(datas)
    // const datas = users
    return (
      <div className="App">
        <header className="App-header">
          <img src={logo} className="App-logo" alt="logo" />
          <h1 className="App-title">Welcome to React</h1>
        </header>
        <p className="App-intro">
        {datas}
        </p>
        <p></p>
      </div>
    );
  }
}

export default App;
```

> Lifecycle API 와 관련된 이슈<br>
componentDidMount 전후로 console.log(users[1].name)이 실행되므로<br>
State= {users:[{<br>
name:'', gender:''<br>
...<br>
 }]} 의 경우 위에 담기는 users[0].name은 불러올 수 있지만<br>
componentDidMount 전에 users[1].name은 존재하지 않으므로 오류가 난 것.<br>
if 문으로 해결<br>
```
    if(this.state.users.length !== 0){
    console.log(this.state.users )
    console.log(this.state.users[0]) 
    console.log(this.state.users[1])
    console.log(this.state.users[0].name)
    console.log(this.state.users[1].name)
  }
  ```
