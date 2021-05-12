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



---

Reference : https://ko.reactjs.org/docs/components-and-props.html


Descripted by N0FreeLunch
