### 리액트 element는 객체이다.
- JSX의 리액트 엘리먼트는 브라우저의 DOM-element와는 다른 React-element이다.
- Render 함수에는 트리형식으로 React-element들을 나열할 수 있다.
```
const element = <h1>Hello, world</h1>;
```
위에서 ```<h1>Hello, world</h1>``` 부분이 React-element이다.

### 리액트 엘리먼트는 불변객체이다. 
- render 함수를 통해 랜더링이 이뤄지면 불변 객체로서의 다음과 같은 literal object가 생성된다. 
```
const element = {
  type: 'h1',
  props: {
    className: 'greeting',
    children: 'Hello, world!'
  }
};
```

### 브라우저에 랜더링
#### HTML
```
<div id="root"></div>
```
- 리액트 돔을 집어 넣는 DOM-element의 태그를 root DOM-Node라고 한다.
- 리액트를 브라우저 화면에 랜더링 하기 위해서는 반드시 root DOM-Node를 통해 실제 화면에 보여줄 영역을 지정해야만 한다.

#### JavaScript
```
import ReactDOM from 'react-dom';

const element = <h1>Hello, world</h1>;
ReactDOM.render(element, document.getElementById('root'));
```
- render 함수를 통해서 리액트의 React-element를 DOM-Node로 변환한다.


### UI 랜더링 방법
```
function tick() {
  const element = (
    <div>
      <h1>Hello, world!</h1>
      <h2>It is {new Date().toLocaleTimeString()}.</h2>
    </div>
  );
  ReactDOM.render(element, document.getElementById('root'));
}

setInterval(tick, 1000);
```
- 1000ms 주기로 tick 함수를 호출하며 tick 함수는 ```<div id="root"></div>``` 부분에 랜더링을 한다.
- 이 방식은 ```<div id="root"></div>``` 돔 엘리먼트 안의 innerHTML을 계속 변경 시키는 방식이다.
- 그런데 리액트는 ReactDOM.render을 여러번 호출해도  ```<div id="root"></div>```안의 innerHTML 전체를 변경시키는 것이 아니라 변경이 이뤄진 reactElement부인 ```<h2>It is {new Date().toLocaleTimeString()}.</h2>``` 이 부분의 돔만 DOM-element에서 변경한다. 리액트에서 변경을 감지하는 단위는 하나의 태그 단위로 변경을 인식하고 변경된 태그를 화면에서 바꿔준다.


---
Reference : https://ko.reactjs.org/docs/rendering-elements.html


Descripted by N0FreeLunch
