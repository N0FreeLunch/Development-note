### 리액트 element는 객체이다.
- JSX의 리액트 엘리먼트는 브라우저의 DOM-element와는 다른 React-element이다.
- Render 함수에는 트리형식으로 React-element들을 나열할 수 있다.
```
const element = <h1>Hello, world</h1>;
```
```<h1>Hello, world</h1>``` 부분이 React-element이다.


#### HTML
```
<div id="root"></div>
```
- 리액트 돔을 집어 넣는 DOM-element의 태그를 root DOM-Node라고 한다.
- 리액트를 브라우저 화면에 랜더링 하기 위해서는 반드시 root DOM-Node가 필요하다.

#### JavaScript
```
import ReactDOM from 'react-dom';

const element = <h1>Hello, world</h1>;
ReactDOM.render(element, document.getElementById('root'));
```
