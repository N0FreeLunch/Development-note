### 컴포넌트는 자신의 상태 변경에 대해 책임을 져야 한다.

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


###
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
- 계속적인 컴포넌트 랜더링을 하기 위해서는 생명주기 메서드를 사용해야 한다.

### 생명주기 메서드
- 메서드인 이유는 클래스 컴포넌트 내부의 React.Component로 부터 상속 받은 메서드를 오버라이딩하여 사용하기 때문
#### componentDidMount()
- DOM에 랜더링이 된 후 실행되는 함수
#### componentWillUnmount()
- 컴포넌트가 소멸되면서 실행되는 함수
- 컴포넌트의 소멸이란 더 이상 React-element로 만들어 지지 않는 컴포넌트 상태

### 지속적인 랜더링 기능 추가하기
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
- 컴포넌트 내부의 변수를 바꾼다는 것은 무엇을 의미하는가?


### 랜더링이 된다는 것
- 랜더링이 된다는 것은 DOM-element를 바꾸는 것이고 이것은 기존의 돔을 바꾸는 것이기 때문에 Mutable 속성을 가지고 있다.
- React-element는 기존에 있는 것을 바꾸는 것이 아니라 새로 생성하는 것이다. 랜더링이 일어나면 Immutable의 React-element라는 객체를 생성한다.
- React-element라는 객체는 메모리 상에서 할당된 메모리를 변경하는 것이 아니라 새로 영역을 할당하는, 생성을 하는 원리이다.
- React-element라는 불변 객체를 생성하여 DOM-element를 Mutable 방식으로 조작하는 것이므로 DOM-element를 변경하는 것은 사이드 이펙트이다.
- DOM-element를 변경하는 사이드 이펙트는 React-element를 생성하는 로직에 전혀 관여하지 않아야 React-element를 생성하는 리액트의 관점에서는 stateless의 속성을 유지하게 된다.


