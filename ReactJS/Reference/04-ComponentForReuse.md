### JavaScript 함수를 이용한 React Component 만들기
```
function Welcome(props) {
  return <h1>Hello, {props.name}</h1>;
}
```
- 함수 컴포넌트라고 부른다. (function component O, functional component X)
- 그냥 코드만 보았을 때는 JSX를 반환하는 것이지만 컴파일이 되면서
```
const element = React.createElement(
  'h1',
  {className: 'greeting'},
  'Hello, world!'
);
```
가 되고 createElement 함수가 실행되면서 불변객체인 React-element를 반환한다.
```
const element = {
  type: 'h1',
  props: {
    className: 'greeting',
    children: 'Hello, world!'
  }
};
```
이런 형태이다.

### 리액트 컴포넌트의 정의
- 리액트 컴포넌트는 함수든 클래스이든 리액트 앨리먼트를 생성하는 단위이면 리액트 컴포넌트라고 할 수 있다.

### JavaScript Class를 이용한 React Component 만들기
```
class Welcome extends React.Component {
  render() {
    return <h1>Hello, {this.props.name}</h1>;
  }
}
```

### 리액트의 관점
- 리액트의 관점이란 불변객체인 React-element를 만들어서 상태 변경 전의 React-element와 상태 변경 후의 React-element를 비교하여 브라우저에 랜더링하는 의미이다.
- 함수 컴포넌트와 클래스 컴포넌트는 결국에는 React-element를 만드는 것이기 때문에 리액트의 관점에서 동일하다.

### 컴포넌트 랜더링
```
const element = <div />;
```
- 이 element 변수는 반환되지 않은 값이므로 React-element가 아닌 JSX이다.

```
function Welcome(props) {
  return <h1>Hello, {props.name}</h1>;
}

const element = <Welcome name="Sara" />;
```
- 마찬가지로 JSX인데 Welcome이라는 컴포넌트가 JSX안에 들어간 것이다.
- 그런데, Welcome()으로 함수를 실행하지 않았기 때문에 ```<Welcome name="Sara" />```은 단지 JSX 표현일 뿐이다.
- 컴파일이 되면서 Welcome 컴포넌트를 React.createElement 함수가 React-element로 변환하여 불변객체를 만든다.

### JSX 어트리뷰트 전달
```
function Welcome(props) {
  return <h1>Hello, {props.name}</h1>;
}

const element = <Welcome name="Sara" />;
ReactDOM.render(
  element,
  document.getElementById('root')
);
```
- Welcome 컴포넌트에 literal Object를 전달한다. 이 literal Object는 ```{name : "Sara"}```가 전달된다.


### Component 나누기
```
function Comment(props) {
  return (
    <div className="Comment">
      <div className="UserInfo">
        <img className="Avatar"
          src={props.author.avatarUrl}
          alt={props.author.name}
        />
        <div className="UserInfo-name">
          {props.author.name}
        </div>
      </div>
      <div className="Comment-text">
        {props.text}
      </div>
      <div className="Comment-date">
        {formatDate(props.date)}
      </div>
    </div>
  );
```
- 어디부터 어디까지가 하나의 동작을 나타내고 있는지 파악하는데 시간이 걸린다. 컴포넌트 단위로 만들어 클린코드를 만들어 줘야 한다.
- 컴포넌트는 분리되어 동작할 수 있게 만들 수 있다. 컴포넌트는 리액트의 하나의 기능 단위이기 때문에 컴포넌트 단위 곧 기능단위로 구분하는 것이 수정, 제거, 확장을 하기 편리하다.


#### 컴포넌트로 분리
```
function Avatar(props) {
  return (
    <img className="Avatar"
      src={props.user.avatarUrl}
      alt={props.user.name}
    />
  );
}
```

```
function UserInfo(props) {
  return (
    <div className="UserInfo">
      <Avatar user={props.user} />
      <div className="UserInfo-name">
        {props.user.name}
      </div>
    </div>
  );
}
```

```
function Comment(props) {
  return (
    <div className="Comment">
      <UserInfo user={props.author} />
      <div className="Comment-text">
        {props.text}
      </div>
      <div className="Comment-date">
        {formatDate(props.date)}
      </div>
    </div>
  );
}
```
- 상위 컴포넌트 -> 하위 컴포넌트 순 : Comment -> UserInfo -> Avatar
- 상위 컴포넌트는 하위 컴포넌트를 포함하는 관계라는 것을 알 수 있다.

### props는 읽기 전용이다
- 컴포넌트는 동일 입력 값에 대해 동일 결과를 반환해야 하기 때문에 순수 함수 형태로 컴포넌트를 만들어 줘야 한다.

```
function sum(a, b) {
  return a + b;
}
```
- sum은 순수 함수 컴포넌트이다.

```
function withdraw(account, amount) {
  account.total -= amount;
}
```
- withdraw는 props를 변경하기 때문에 순수 함수 컴포넌트가 아니다.


### 리액트의 원칙
#### 모든 React 컴포넌트는 자신의 props를 다룰 때 반드시 순수 함수처럼 동작해야 합니다.

---

Reference : https://ko.reactjs.org/docs/components-and-props.html


Descripted by N0FreeLunch
