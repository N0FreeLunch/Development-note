## JSX

### 설명
```
const element = <h1>Hello, world!</h1>;
```
```<h1>Hello, world!</h1>```부분이 JSX이다.

- 변수에 JSX를 집어 넣을 수 있다.
- 하지만 브라우저나 노드JS에서 바로 JSX를 로드 할 수는 없다.
- 웹펙 등으로 JSX 형식을 컴파일을 해 줘야 JSX를 변수로 사용할 수 있다.

- 컴파일 없이 JSX를 사용하는 것을 pure react를 사용한다고 말한다. 이 때는 리액트 패키지를 사용하는데, 컴파일이 되지 않기 때문에 JSX를 react컴파일 환경 보다 사용하기가 제한적이다. (여기에 관해서는 나중에 좀 더 추가)


#### JSX에서 변수 사용하기
```
const name = 'Josh Perez';
const element = <h1>Hello, {name}</h1>;

ReactDOM.render(
  element,
  document.getElementById('root')
);
```

- JSX 안에 {}를 사용하여 변수를 출력할 수 있었다.


#### JSX에서 자바스크립트 문법 사용하기
```
function formatName(user) {
  return user.firstName + ' ' + user.lastName;
}

const user = {
  firstName: 'Harper',
  lastName: 'Perez'
};

const element = (
  <h1>
    Hello, {formatName(user)}!
  </h1>
);

ReactDOM.render(
  element,
  document.getElementById('root')
);
```
- JSX {}를 사용해서 변수 뿐만 아니라 자바스크립트의 다양한 표현들을 사용할 수 있다. 


#### JSX에서 태그의 어트리뷰트 사용하기
- JSX에서 태그의 어트리뷰트는 camelCase를 사용한다.
- class는 className으로 표기한다.
- tabindex는 tabIndex로 표기한다.
- 리액트에서 사용하는 태그의 명칭 부여 방식은 HTML태그의 어트리뷰트와 완전히 일치하지 않기 때문에 주의 하자.

```
const element = <div tabIndex="0"></div>;
```
```
const element = <img src="file path"></img>;
```


