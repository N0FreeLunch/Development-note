### 리액트 컴포넌트의 원칙
#### 컴포넌트는 자신의 상태 변경에 대해 책임을 져야 한다.



### 컴포넌트의 원칙을 지키지 않은 경우
```
function Clock(props) {
  return (
    <div>
      <h1>Hello, world!</h1>
      <h2>It is {props.date.toLocaleTimeString()}.</h2>
    </div>
  );
}
```

```
function tick() {
  ReactDOM.render(
    <Clock date={new Date()} />,
    document.getElementById('root')
  );
}
```

```
setInterval(tick, 1000);
```

- 컴포넌트는 자체적으로 하나의 기능을 담당한다. 마치 순수 함수처럼 컴포넌트는 props를 세팅해서 호출 될 때 하나의 기능을 담당해야 한다. 그리고 이 기능은 외부에 의존적으면 안된다.




### 컴포넌트의 원칙을 지키도록 변경 해 보자.
```
class Clock extends React.Component {
  constructor(props) {
    super(props);
    this.state = {date: new Date()};
  }

  render() {
    return (
      <div>
        <h1>Hello, world!</h1>
        <h2>It is {this.state.date.toLocaleTimeString()}.</h2>
      </div>
    );
  }
}

ReactDOM.render(
  <Clock />,
  document.getElementById('root')
);
```
- 이 상태는 컴포넌트가 생성될 때 constructor에 의해서 date: new Date()로 컴포넌트가 생성된 시점의 값을 저장하고 찍어낸다.
- 계속적인 컴포넌트 랜더링을 하기 위해서는 state를 변경 해 줘야 한다.



### 생명주기 메서드
- 메서드인 이유는 클래스 컴포넌트 내부의 React.Component로 부터 상속 받은 메서드를 오버라이딩하여 사용하기 때문
#### componentDidMount()
- DOM에 랜더링이 된 후 실행되는 함수
#### componentWillUnmount()
- 컴포넌트가 소멸되면서 실행되는 함수
- 컴포넌트의 소멸이란 더 이상 React-element로 만들어 지지 않는 컴포넌트 상태



### 지속적인 랜더링 기능 추가하기 
#### 컴포넌트의 원칙을 지킨 경우의 코드
```
class Clock extends React.Component {
  constructor(props) {
    super(props);
    this.state = {date: new Date()};
  }

  componentDidMount() {
    this.timerID = setInterval(
      () => this.tick(),
      1000
    );
  }

  componentWillUnmount() {
    clearInterval(this.timerID);
  }

  tick() {
    this.setState({
      date: new Date()
    });
  }

  render() {
    return (
      <div>
        <h1>Hello, world!</h1>
        <h2>It is {this.state.date.toLocaleTimeString()}.</h2>
      </div>
    );
  }
}

ReactDOM.render(
  <Clock />,
  document.getElementById('root')
);
```
- 랜더링이 되면 setInterval에 의해 tick 메서드를 주기적으로 실행한다.
- tick 메서드의 setState에 의해 state가 바뀌게 되면 랜더링이 새로 일어난다.
- setInterval에 의해 시간에 따라서 컴포넌트의 랜더링이 이뤄진다.
- 컴포넌트가 소멸되면 setInterval 이벤트가 메모리 상에 남아 컨택스트에 의한 클로저로 콜백이 실행이 되기 때문에 컴포넌트 소멸과 동시에 제거 해 줘야 한다.
- 위 코드는 Clock 컴포넌트가 랜더링하고 있는 대상들을 업데이트 하는 로직을 그 컴포넌트 그 자체가 가지고 있다. 곧 랜더링에 대한 책임을 해당 컴포넌트가 가지고 있는 형태이다.



### state
- setState 함수로 state 변수를 변경할 수 있다.
- 클래스 컴포넌트에서 state는 this.state 클래스의 멤버로 접근을 해야 한다.
- 랜더링 시키기 위해서는 state를 직접 변경하면 안 된다. 
```this.state.comment = 'Hello';```
- 랜더링 시키기 위해서는 옵저버 패턴을 적용하기 위해 state 변경과 동시에 랜더링을 발동하는 기능을 호출하는 setState 함수로 state를 변경해야 한다. ```this.setState({comment: 'Hello'});```
- 리액트에서는 state 값을 직접 변경하지 않는 것이 원칙이며 변경할 때는 setState를 통해 랜더링을 통해 변경을 해야 한다. 랜더링 없이 변수에 뭔가 저장하고 싶을 때는 state가 아닌 다른 변수를 사용하도록 한다.
- setState를 사용하지 않고 state에 직접 값을 할당하는 것은 constructor에서만 해 주는 것이 원칙이다.


### setState
```
  constructor(props) {
    super(props);
    this.state = {
      posts: [],
      comments: []
    };
  }
```
- state는 객체 값으로 업데이트 할 수 있다.
- state 안의 멤버들은 독립적이다.

#### state 업데이트 병합
```
 componentDidMount() {
    fetchPosts().then(response => {
      this.setState({
        posts: response.posts
      });
    });

    fetchComments().then(response => {
      this.setState({
        comments: response.comments
      });
    });
  }
```
- state 안의 객체 내의 멤버로 posts, comments 두 가지가 있지만 위의 코드에서 보듯이 멤버 하나씩 업데이트 할 수 있다.
```
{
   posts: response.posts
}
```
- 위 객체를 업데이트 했다고 해서 state가 위 객체로 덮어 씌워져서 comments가 사라지는 것이 아니라 shallow merge를 통해 기본 구조와 상태는 그대로 있고 업데이트를 시도한 멤버만 업데이트 하는 방식이다.


### 비동기적 상태 업데이트
- props, state 업데이트는 비동기적이다.
- this.props와 this.state 값의 변경이 비동기적이므로 상태 변경 이후의 props, state를 만들어 줄 때 이전 props, state 값에 의존해서는 안 된다.
```
this.setState({
  counter: this.state.counter + this.props.increment,
});
```
- 현재 상태에 의존적인 state 변경은 비동기적 특성에 의해 계산 값이 의도한 것과 다르거나 잘못된 값의 생성으로 에러의 원인이 될 수 있다.
```
this.setState((state, props) => ({
  counter: state.counter + props.increment
}));
```
- setState에 콜백 함수를 달아주면 업데이트 하기 전의 상태를 인자로 리액트에서 알아서 파라메터로 넘겨준다. 이전 상태를 계산할 때는 콜백 함수를 사용해서 넘겨줘야 이전 상태 값을 통한 상태변경을 정확하게 계산 할 수 있다.
- 리액트는 여러 setState() 호출을 해도 최종적으로는 단일 업데이트로 한꺼번에 처리 되기 때문에 성능 이슈를 자체적으로 줄이는 특성이 있다.

### 단방향 데이터 흐름
- 부모 컴포넌트는 부모 컴포넌트가 포함하고 있는 자식 컴포넌트에 props로 값을 전달할 수 있다.
- 부모 컴포넌트는 자식 컴포넌트에 데이터를 전달할 수 있으며, 자식 컴포넌트는 부모로 부터 전달된 데이터를 사용할 수 있다.

```
<FormattedDate date={this.state.date} />
```

```
function FormattedDate(props) {
  return <h2>It is {props.date.toLocaleTimeString()}.</h2>;
}
```
- FormattedDate는 함수 컴포넌트의 props 변수는 부모 컴포넌트로 부터 값을 전달 받지만, 부모 컴포넌트의 어떤 조작으로 인해 값을 넘겨 받았는지는 알 수 없다. (부모 컴포넌트의 state인지 props인지 유저의 입력값인지 알 수 없다.) 
- 컴포넌트 내부 state는 상위 컴포넌트에서 접근할 수 없다. 컴포넌트의 state를 하위 컴포넌트에 전달 할 수 있기 때문에 state의 전달은 상위 컴포넌트에서 하위 컴포넌트로만 이뤄진다. 

### 캡슐화
- 부모 컴포넌트는 자식 컴포넌트가 자체적인 상태를 가지고 있는지 없는지 함수 컴포넌트인지 클래스 컴포넌트인지에 관계 없이 동작한다.




### 랜더링이 된다는 것
- 랜더링이 된다는 것은 DOM-element를 바꾸는 것이고 이것은 기존의 돔을 바꾸는 것이기 때문에 Mutable 속성을 가지고 있다.
- React-element는 기존에 있는 것을 바꾸는 것이 아니라 새로 생성하는 것이다. 랜더링이 일어나면 Immutable의 React-element라는 객체를 생성한다.
- React-element라는 객체는 메모리 상에서 할당된 메모리를 변경하는 것이 아니라 새로 영역을 할당하는, 생성을 하는 원리이다.
- React-element라는 불변 객체를 생성하여 DOM-element를 Mutable 방식으로 조작하는 것이므로 DOM-element를 변경하는 것은 사이드 이펙트이다.
- DOM-element를 변경하는 사이드 이펙트는 React-element를 생성하는 로직에 전혀 관여하지 않아야 React-element를 생성하는 리액트의 관점에서는 stateless의 속성을 유지하게 된다.



### 상태 업데이트
- 리액트 컴포넌트의 로컬 state 변경에 따라서 로컬 render() 함수가 실행되고 출력이 업데이트 됨 
- state 변경은 자바스크립트 이벤트인 setInterval에 의해 setState 함수를 호출하여 렌더링 됨




---
Reference : https://ko.reactjs.org/docs/state-and-lifecycle.html

Descripted by N0FreeLunch
