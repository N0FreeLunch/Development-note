## JSX

### JSX 타입
```
const element = <h1>Hello, world!</h1>;
```
```<h1>Hello, world!</h1>```부분이 JSX이다.

- 변수에 JSX를 집어 넣을 수 있다.
- 하지만 브라우저나 노드JS에서 바로 JSX를 로드 할 수는 없다.
- 웹펙 등으로 JSX 형식을 컴파일을 해 줘야 JSX를 변수로 사용할 수 있다.

- 컴파일 없이 JSX를 사용하는 것을 pure react를 사용한다고 말한다. 이 때는 리액트 패키지를 사용하는데, 컴파일이 되지 않기 때문에 JSX를 react컴파일 환경 보다 사용하기가 제한적이다. (여기에 관해서는 나중에 좀 더 추가)


### JSX에서 변수 사용하기
```
const name = 'Josh Perez';
const element = <h1>Hello, {name}</h1>;

ReactDOM.render(
  element,
  document.getElementById('root')
);
```

- JSX 안에 brace { }를 사용하여 변수를 출력할 수 있다.


### JSX에서 자바스크립트 문법 사용하기
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
- JSX {}를 사용해서 변수 뿐만 아니라 자바스크립트의 다양한 표현들을 사용할 수 있다. 단, {}안의 값은 최종적으로 String으로 자동 형 변환이 될 수 있는 단일 값이어야 한다.


### JSX에서 태그의 어트리뷰트 사용하기
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


### 코드 인젝션 방어
- JSX를 리액트가 해석할 때는 escape 처리를 한므로 브라우저에서 이미 로드된 React 패키지를 대상으로 한 코드 인젝션을 차단할 수 있다.
- 유저의 입력을 받아서 상태가 변경되는 경우 유저의 입력을 통해 자바스크립트가 실행 될 여지를 차단한다.
```
const title = response.potentiallyMaliciousInput;
const element = <h1>{title}</h1>;
```


### 리액트에서의 돔 표현
- 리액트 컴파일을 바벨에 의해 이뤄지고 JSX는 순수 자바스크립트 런타임에서 실행되는 방식으로 변경된다.
- nodeJS런타임에 Create React App을 통해 설치하지 않은 경우, JSX를 사용하기 위해서는 babel에 JSX를 컴파일 해 주는 모듈을 설치해야 한다. 

#### 컴파일 전 코드
```
const element = (
  <h1 className="greeting">
    Hello, world!
  </h1>
);
```


#### 컴파일 후 코드
```
const element = React.createElement(
  'h1',
  {className: 'greeting'},
  'Hello, world!'
);
```


### 가상 돔의 표현
- JSX는 HTML도 브라우저의 DOM NODE도 아니다. virtual-DOM으로 표현된다.
- 리액트에서 virtual-DOM은 상태변화 전후의 랜더링 되는 virtual-DOM을 계산을 하는데 이를 위해 쓰는 방식이 다음과 같은 literal object 형식이다.
```
const element = {
  type: 'h1',
  props: {
    className: 'greeting',
    children: 'Hello, world!'
  }
};
```

---

## eference
- https://ko.reactjs.org/docs/introducing-jsx.html

