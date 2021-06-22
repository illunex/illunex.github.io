# 3월 1주차 블로그

블로그 제목: React Hooks
작성일자: 2021년 3월 25일 오후 6:03
작성자: 두현 박
카테고리: 프론드앤드
태그: Apollo, GraphQL, React

![01.png](/assets/images/posts/2020-05-06/01.png)

안녕하세요. 일루넥스 개발팀 박두현입니다.

이번 글의 내용은 React Hooks에 관한 내용입니다.

일루넥스의 프론트엔드는 모두 React를 사용하고 있습니다.

초창기 이펙트몰에서는 React Class Component 방식으로 개발하였으나 React의 버전업이 되면서

React Functional Component와 Hook을 이용하여 개발을 진행하고 있습니다.

오늘은 React에 내장된 Hook들에 대해 정리해보겠습니다.

---

## React Hook?

Hook은 React 16.8에 새로 추가된 기능이다. 
Hook은 함수형 컴포넌트에서 State와 생명주기(LifeCycle)를 연동(hooking)한 함수이다. 
Hook은 class를 작성하지 않고도 state와 다른 React의 기능들을 사용할 수 있게 해준다.

### React Hook을 사용하는 이유

**1. 기존 React에서는 State와 관련된 로직을 재사용하기 어렵다.**
로직 재사용을 위해 컴포넌트를 감싸는 래퍼(wrapper)가 많아 불편하다.

→ Hook을 사용하면 컴포넌트 자체에서 State 로직을 추상화 할 수 있고 컴포넌트의 계층 변화 없이 로직을 재사용할 수 있게 해준다.

**2. 복잡한 생명주기(LifeCycle)를 이해하기 어렵다.** 
각각 생명주기마다 기본적인 메서드가 있어서 원하는 로직을 특정한 타이밍에, 해당하는 로직만실행시키기 어렵다.
예를 들면 데이터를 가져오는 로직은 componentDidMount와 componentDidUpdate에서 수행할 수 있지만 언제 어느 곳에서 로직을 수행해야할 지 특정할 수 없다.

→ Hook을 사용하면 함수로 컴포넌트를 나눌 수 있다. 로직에 적절한 생명주기가 이미 Hooking 되어있어서 생명주기 안에서 어떤 일이 일어나는지까지 알 필요가 없으며, 로직의 용도에 맞는 Hook을 사용하면 되므로 훨씬 이해하기 쉽다.

**3. Class의 불편함**
Class는 javascript에서 this와 event handler의 동작을 정확히 알아야 하고, 컴포넌트와 함수를 언제 어떻게 사용해야하는지 알아야 문제가 생기지 않는다. 
또한, Class 컴포넌트를 리팩토링하여 코드를 줄이기도 쉽지 않다. 

→ Hook과 함수형 프로그래밍을 사용하면 문법이 더 자유롭고, 코드의 분리와 재사용을 원하는 대로 할 수 있다.

---

## React Hooks

- **함수형 컴포넌트에서 Hook의 사용**
1. 화살표 함수 사용

```jsx
const Example = (props) => {
	// 여기서 Hook을 사용할 수 있다.
	return <div />;
}
```

2. 일반 함수 사용

```jsx
function Example(props) {
	// 여기서 Hook을 사용할 수 있다.
	return <div />;
}
```

★ **함수형 컴포넌트는 Stateless 컴포넌트지만, Hook이 State를 함수 안에서 사용할 수 있게 해준다.**

→ Hook은 Class안에서는 사용할 수 없다.

---

- **useState**

useState Hook은 State와 State를 바꾸는 함수를 ES6의 구조분해할당 문법으로 선언한다.
class 컴포넌트에서 **constructor** 역할을 useState Hook이 해준다.
useState의 괄호 ( ) 안에는 State의 초기상태 또는 임의의 초기화 함수가 인자로 들어갈 수 있다.

```jsx
import React, { useState } from 'react';

const Example = () => {
  **const [count, setCount] = useState(0);**

  return (
    <div>
			<p>{count}</p>
      <button onClick={() => setCount(count + 1)}>
				Count
      </button>
		</div>
  );
}
```

위의 코드는 setCount로 count라는 State를 바꾸는 예시이다.
State를 바꾸는 함수( 여기서는 setCount )가 호출되는 순간 **컴포넌트가 리렌더링** 되면서 {count}의 숫자가 바뀐다.

---

- **useEffect**

React Component의 Side Effect는 함수형 컴포넌트 안에서는 허용되지 않는다.
그래서 useEffect에 함수를 전달하여 **렌더링 완료 후 수행**되게 한다.

→ **Side Effect** :  데이터를 받거나 구독하기, 혹은 DOM을 직접 조작하는 행위
→ componentDidMount와 componentDidupdate와는 다르게, useEffect로 전달된 함수는 지연 이벤트 동안에 레이아웃 배치와 렌더링을 완료한 후 실행된다. 

**기본 동작은 렌더링 직후 함수를 실행시키지만, 특정 값이 변했을 때 실행되게도 할 수 있다.**
→ useEffect의 두 번째 인자인 배열은 effect가 종속되어있는 값의 배열이다. 배열 안에 원하는 조건에 해당하는 값을 명시하면, useEffect는 그 값이 변할 때만 실행된다.

**user 정보를 useEffect로 가져와서 set 하는 예시**

```jsx
const User = () => {
  const [user, setUser] = useState();
  **useEffect(() => {**
      **const user = getUser();
      setUser(user);
  }, []);** 
  return user && (
		<div>
			<p>{user.name}</p>
		</div>
  )
}
```

함수형 컴포넌트 안에 useEffect를 사용해서 user를 가져오고, State에 set 해주는 로직을 작성했다.

위의 예시에서는 useEffect의 2번째 인자에 빈 배열 [ ]을 주었으므로 컴포넌트가 렌더링되면서 useEffect가 단 한번만 실행된다.

※ **만약 2번째 인자를 주지 않거나, 선언한 State인 user를 넣는다면 렌더링 될 때 마다 useEffect가 실행되어 무한루프에 빠지게 된다.**

→ 2번째 인자가 없을 경우에는 리렌더링 될 때 마다 useEffect가 실행되기 때문이고, 
user를 넣은 경우 useEffect내부 로직에서 setUser로 user State가 update 되었으므로 또 useEffect를 실행하게 되는 것이다.

→ 특정 값이 바뀔 때 마다 useEffect를 사용하고 싶을 때는 배열 안에 그 값만 넣으면 알아서 값이 변경될 때에만 useEffect가 실행된다.

**※ Side Effect를 가지는 컴포넌트가 DOM상에서 사라질 경우 Side Effect를 지워주려면?**

```jsx
useEffect(() => {
    window.addEventListener("scroll", onScroll);
    return () => window.removeEventListener("scroll", onScroll);
  }, []);
};
```

→ useEffect 안에서 이벤트를 해제하는 함수를 반환(return)하면 된다.

---

- **useContext**

컴포넌트 외부 어딘가에 존재하는 context 객체(**React.reacteContext**)를 받아 그 context의 현재 상태(State)를 반환한다. 

context의 상태는 useContext Hook을 호출하는 컴포넌트를 감싸고 있는**Provider**의 **value prop**에 의해 결정된다.

```jsx
**const value = useContext(MyContext);**
```

컴포넌트에서 가장 가까운 Provider가 갱신(렌더링)되면 useContext Hook은 MyContext provider에게 전달된 가장 최신의 context를 사용하여 렌더링한다.

→ useContext로 전달한 인자는 context 객체여야 한다.
→ useContext를 호출한 컴포넌트는 context 값이 변경되면 항상 리렌더링 될 것이다. 

**useContext와 Provider 사용 예시**

```jsx
const themes = {
  light: {
    foreground: "#000000",
    background: "#eeeeee"
  },
  dark: {
    foreground: "#ffffff",
    background: "#222222"
  }
};

const ThemeContext = React.createContext(themes.light);

function App() {
	return (
		**<ThemeContext.Provider value={themes.dark}>**
			<Toolbar />
		</ThemeContext.Provider>
	);
}

function Toolbar(props) {
	const { context } = props;
	const theme = useContext(context);
	return (
		<div>
			<button style={{ background: theme.background, color: theme.foreground }}>
				I am styled by theme context!
			</button>
			</div>
	);
}
```

→ React Context 객체에서 관리하는 themes 값을 Toolbar 라는 컴포넌트에 직접 사용하기 위해
Toolbar 컴포넌트를 **<ThemeContext.Provider>로 감싼 후 value라는 props로 Toolbar에 전달**해주었다.
이렇게 **props로 받은 context를 useContext에 전달하면, 해당 컴포넌트에서 context를 사용할 수 있게 된다.**

→ 위의 코드에서는 Context에 단순히 값만 저장하고 있지만, **State와 State 갱신 함수를 선언하여 전역 State를 관리할 수 있다.**

**★ 언제 useContext를 사용하나?**

→ 여러 컴포넌트에서 사용할 전역 State를 보관하는데 사용한다.

→ 기존 React에서는 부모에서 props로 받은 State를 사용하다보니, 자식 컴포넌트가 많아질수록 부모에서 관리하는 State가 많아진다. 그래서 별도의 State 저장소(redux, mobx)를 사용하여 원하는 컴포넌트에만 State를 전달해주었다.

→ React context가 나오면서 이제 React 자체에서 State를 관리할 수 있는 컴포넌트들이 생겼고(Context, Provider, Consumer) 외부 라이브러리를 사용하지 않아도 전역 State를 useContext Hook를 사용하여 쉽게 관리할 수 있게 되었다.

---

- **useRef**

React에서 특정 DOM을 선택할 때 ref를 사용한다. 클래스형 컴포넌트에서는 콜백 함수를 사용하거나 React.reacteRef 라는 함수를 사용하는데, 함수형 컴포넌트에서 ref 를 사용 할 때에는 useRef( )라는 Hook 함수를 사용한다.

React에서 부모 컴포넌트가 자식 컴포넌트에 props로 특정 값 또는 상태를 전달하여 자식 컴포넌트를 수정한다. **수정하려는 대상이 값이 아니라 컴포넌트나 DOM 엘리먼트 인 경우 useRef를 사용하여 해당 객체를 전달한다.**

```jsx
function Example() {
  **const inputEl = useRef(null);**
  return (
    <div>
      **<input ref={inputEl} type="text" />**
			**<ChildComponent inputEl={inputEl}/>**
    </div>
  );
}
```

→ useRef로 전달하려는 객체를 선언하고, 자식 컴포넌트에 props로 해당 객체를 전달한다.
자식 컴포넌트에서 전달받은 객체를 사용하거나 변경할 수 있다.