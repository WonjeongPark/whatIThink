[Building first component]
src/features/event/EventDashboard/EventDashboard.jsx
EventDashboard.jsx - rcc 탭/class 이름,export 뒤로빼기/Grid를semantic-ui-react에서임포트/Grid나누기(Grid.Column width={10})
src/app/layout/App.jsx - EventDashboard.jsx 임포트/<EventDashboard/>
[add Navbar and styling]
features/nav/NavBar/NavBar.jsx
NavBar.jsx - rcc~/s_3.4.2탭/Menu, Container, Button임포트
App.jsx - NavBar임포트/<NavBar/><E~/>
public/assets폴더생성 - 로고이미지추가
index.css - s_css탭/ nav-style css 확인
App.jsx - Container 임포트/<div><Nav~/><Container className="main"><EventDa~/><Container><div>수정

[add event list items component]-Left Column
event/EventList/EventList.jsx, EventListAttendee.jsx, EventListItem.jsx - rcc~/
EventListItem.jsx - s_3.4.1탭/Segment, Item, Icon, List, Button 임포트
EventList.jsx - EventlistItem임포트/<EventlistItem/>*3
EventDashboard.jsx - EventList임포트/LeftColumn-><EventList/>
EventListAttendee.jsx - List,Image임포트/<List.Item><Image as='a' size='mini' circular src='https://randomuser.me/api/portraits/women/42/jpg'></List.Item>
EventListItem.jsx - ItemImage src="https://randomuser.me/api/portraits/women/42/jpg"/EventListAttendee임포트/<List><EventListAttendee>*3/<Segment clearing><span>Description~

[add EventForm]-Right Coulmn
event/EventForm/EventForm.jsx
EventForm.jsx - rcc~/s_3.4.3/Segment, Form, Button임포트
EventDashboard.jsx - Button,EventForm임포트/Right Coulmn-><Button positive content="Create Event"/><EventForm/>
[Pass down static props to components]
EventDashboard.jsx - s_3.7.1/<EventList/>-><EventList events(whatever)={eventsEventDashBoard}(위에만든것도 이름수정)
EventList.jsx - const{events}=this.props;/<EventListItem>*3->*1
                {events.map((event) => ( <EventListItem key={Event.id} event={event} /> ))(key!체크/브라우저에 3->2 이벤트리스트 개수확인)
EventListItem.jsx - const{event} = this.props; (꼭 필요한건 아니지만 사진 사용을 위해)
                    <Item.Image ... src={this.props.event.hostPhotoURL} (this.props. 생략가능-프로퍼티 밖에 개별적인 이벤트를 가져오는 거라서)
                    <Item.Header>Event Title-> {event.title}/<Item.Description><a>hosted by->{event.hostedBy}
                    <span><Icon name="clock>date->{event.date}/time->{event.venue}/<Segment><span>Description...->{event.description}
                    <EventListAttendee>*3->{event.attendees.map((attendee) => (<EventListAttendee key={attendee.id} attendee={attendee} />))} (key없으면 inspect에 오류)
EventListAttendee.jsx - const{attendee} = this.props;/<Image ... src={attendee.photoURL}/>

[Identifying state]
(지금까지 구조 app - NavBar
                    EventDashboard - EventList - EventListItem - EventListAttendee
                                     EventActivity
                                     EventForm
State - NOT passed in from p parent via props/Change over time
        NOT Compute it based on other state of props in component.
*EventList(Events[]), EventListItem(Event:{}/Attendees:[]), EventListAttendees(Attendee:{}) - No state -All have props passed down by parents
*EventDashboard - Events[] - stateful / isOpen:boolean - stateful
*EventForm - Forms manage state stateful
*Navbar - CreateEvent, Login, Signout - all rely on user interaction - stateful)

[add state to our application]
EventDashboard.jsx - class~Component{아래에 constructor(props){super(props) this.state ={events:eventsDashboard(const에 pass되어있는),isOpen:false}}
                     <EventList events={EventDashBoard} -> {this.state.events}/>로 수정
[Inverse date flow]
EventDashboard.jsx - class~render()사이에 handleFormOpen(){this.setState({isOpen:true})}
                     <Button positive~에 onClick={this.handleFormOpen()}추가 ( ()를 추가하여 page를 render하자마자 function이 실행되는 것 방지)
                     constructor 내에 가장아래에 this.handleFormOpen = this.handleFormOpen.bind(this); 하면 버튼클릭시 창 오픈이 실행되지만 UGLY
                     그래서 위의 방법 X cancel기능까지 넣고 바꿔보자([Code Improvements] 참고)
                     handleFormOpen(){~아래에 handleCancel(){this.setState({isOpen:false})}추가. ( this.handleCancel = this.handleCancel.bind(this);까지 추가해야 실행가능)
                     <EventForm />내에 handleCalcel={this.handleCancel}
                     <Button positive content="Create Event">아래에 {this.state.isOpen &&만 쓰고 넘어감
EventForm.jsx - const {handleCancel} = this.props / <Button>Cancel~ -> <Button onClick={handleCancel}~ 수정
[Code Improvements]
EventDashboard.jsx - this.handleFormOpen = this.handleFormOpen.bind(this);를 없애기 위해서
                     onClick={this.handleFormOpen}부분을
                     onClick={this.handleFormOpen.bind(this)}로 수정해도 가능하지만 매번 새로운 function을 만드는 것과 같다(과부화가능성)
                     대안은 onClick={() => this.handleFormOpen}으로 화살표함수를 추가하는 것/화살표함수가 자동으로 바인딩해줌
                     but 버튼에 추가하는 것보다 function을 만든 부분을
                     handleFormOpen = () => {~로 수정하는 것이 더 낫다! onClick={this.handleFormOpen}/this.handleFormOpen = this.handleFormOpen.bind(this);삭제
                  V    (But this is well because i am not passing anything out of this button to the methods.
        다시      V     <Button onClick={handleFormOpen('a string')}~에서 a string이 console되려면
                  V     <Button onClick={() => handleFormOpen('a string')}~ / HandleFormOpen = (thing) => {console.log(thing)}
                  V     OR <Button onClick={handleFormOpen('a string')}~/ HandleFormOpen = (thing) => () => {console.log(thing)}로 가능!
                     handleCancel = () => {~ 로 수정하고 this.handleCancel = this.handleCancel.bind(this)삭제
                     더 이상 메소드를 바인딩할 필요X(화살표함수) constructor(props){super(props) ... }부분 삭제/this.state -> state로 수정 후 살림
                     
                     
                     
                     


