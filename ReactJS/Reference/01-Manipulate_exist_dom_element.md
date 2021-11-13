## 설명
특정 돔 엘리먼트를 선택하고 JSX를 표현할 수 있다.

```
import ReactDOM from 'react-dom';
```
사용을 위해서는 react-dom 패키지를 로드해야 한다.

### usage
```
ReactDOM.render(
  JSX,
  DOM node element
);
```

### Example
```
import ReactDOM from 'react-dom';

ReactDOM.render(
  <h1>Hello, world!</h1>,
  document.getElementById('root')
);
```

---

## Reference
- https://ko.reactjs.org/docs/hello-world.html
