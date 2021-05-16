
### JSX에서 이벤트 태그 설정
HTML 태그를 통한 이벤트 설정
```
<button onclick="activateLasers()">
  Activate Lasers
</button>
```

JSX를 통한 이벤트 설정
```
<button onClick={activateLasers}>
  Activate Lasers
</button>
```
- onclick가 camelCase인 onClick으로 사용되었다.
- 리액트의 JSX를 통한 이벤트 설정은 camelCase로 설정을 한다.

HTML 태그를 통한 이벤트에 자바스크립트 코드 넣고 실행하게 하기
```
<a href="#" onclick="console.log('The link was clicked.'); return false">
  Click me
</a>
```

### JSX에서 이벤트 실행 자바스크립트 설정
JSX를 통한 이벤트에 자바스크립트 코드 넣고 실행하게 하기
```
function ActionLink() {
  function handleClick(e) {
    e.preventDefault();
    console.log('The link was clicked.');
  }

  return (
    <a href="#" onClick={handleClick}>
      Click me
    </a>
  );
}
```
- ```onclick="console.log('The link was clicked.'); return false"``` 태그에 이벤트를 설정할 때는 return false를 사용하여 디폴트 이벤트 실행을 막을 수 있다. 하지만 리액트에서는 이벤트 기본 동작을 방지하기 위해 반드시 preventDefault를 호출 해 줘야 한다.
- 매개변수 e는 W3C 명세를 따르므로 브라우저 런타임에 바닐라 자바스크립트의 이벤트로 넘겨진 매개변수와 동일하다.

---

Reference : https://ko.reactjs.org/docs/handling-events.html


Descripted by N0FreeLunch
