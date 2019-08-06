---
layout: post
title: 함수형 컴포넌트와 React Hooks
tags: [react, react hooks]
author-id: commitnpush
excerpt_separator: <!--more-->
---

#### 들어가며

리엑트는 컴포넌트를 정의하는 데 있어서 두 가지의 방법을 사용할 수 있습니다. 첫 번째 방법은 <b>클래스</b>로 정의하는 방법이며 두 번째는 <b>함수</b>로 정의하는 방법이 있습니다. 두 방법의 차이점은 함수로 정의한 컴포넌트는 뷰를 그려내기만 하는 데에 초점이 맞춰진 컴포넌트이기 때문에 컴포넌트 내에서 state를 사용할 수 없으며 생명주기 메서드를 사용할 수 없다는 점입니다. 하지만 리엑트 16.8버전부터 Hooks라는 새로운 기능이 도입되어 함수형 컴포넌트에 날개를 달아 주었습니다. 이번 포스팅에서는 Hooks에 대해 알아보도록 하겠습니다.

<br>
<!--more-->

#### 함수형 컴포넌트 VS 클래스 컴포넌트

함수형 컴포넌트를 정의해서 사용하다가 state를 사용해야 하거나 생명주기 함수들이 필요하면 클래스 컴포넌트로 변경하는 경험을 리엑트 개발자라면 해보셨을 겁니다. 구조가 크게 다르지 않아 공수가 많이 드는 것은 아니지만 "이럴 거면 처음부터 클래스 컴포넌트로 정의할걸..." 이라는 생각을 하게 되기 마련인데요. 이럴 때면 함수형 컴포넌트는 클래스로 정의한 컴포넌트의 모든 기능을 대신할 수 없기 때문에 굳이 사용해야 할 이유를 느끼지 못하는 정의 방법이라고 치부하기 쉽고 아예 클래스 컴포넌트만을 사용해서 개발을 진행하는 경우도 있을 겁니다. 

하지만 이렇게 기능이 부족한 함수형 컴포넌트도 나름대로 존재 이유가 있습니다.

* "뷰를 그리기 위한 컴포넌트다"라는 명시적인 효과
* 간단한 구조에서 오는 가독성의 증가
* 렌더링 될 때의 props를 유지

일단 함수형 컴포넌트는 뷰에 집중하는 컴포넌트라는 느낌을 강하게 주며 코드량이 적고 간단한 구조 때문에(기능이 적으니 당연히...) 코드의 가독성이 좋습니다. 이러한 점에서 리덕스로 상태관리를 하는 리엑트 앱에서는 스토어와 긴밀한 관계를 유지 "컨테이너 컴포넌트"와 컨테이너 컴포넌트로부터 전달받은 데이터를 화면에 그리는 "프리젠테이셔널 컴포넌트"으로 구분하여 컴포넌트를 정의하며 프리젠테이셔널 컴포넌트는 상태를 필요로 하지 않기 때문에 함수형 컴포넌트로 작성됩니다. 마지막 항목인 렌더링 될 때의 props를 유지한다는 특징은 비동기 함수 내에서 props에 접근 시의 버그에 관련된 내용으로써 [Dan Abramov님의 포스팅](https://overreacted.io/ko/how-are-function-components-different-from-classes){: target="_blank" }을 참고하시면 도움이 될 것 같습니다.

<br>

#### React <a href="https://reactjs.org/docs/hooks-intro.html" target="_blank">Hooks</a>

Hooks는 리엑트를 함수형 프로그래밍 방식으로 개발하고자 했던 [recompose프로젝트](https://github.com/acdlite/recompose){: target="_blank" }에서 시작되었으며 프로젝트를 진행한 개발자가 리엑트 팀에 합류되면서 Hooks가 릴리즈 되었습니다. Hooks라는 기능은 함수형 컴포넌트에서도 state와 생명주기 메서드를 사용 가능하게 해줍니다. React Hooks의 종류는 다음과 같습니다.

1. 기본 Hooks
- useState
- useEffect
- useContext
2. 추가 Hooks
- useReducer
- useCallback
- useMemo
- useRef
- useImperativeHandle
- useLayoutEffect
- useDebugValue
 
<br>

#### useState Hook

Hooks는 함수 형태로 기능을 제공합니다. 대표적인 Hooks는 바로 <b>useState</b>입니다. 컴포넌트에서 state를 사용하게 해주는 함수로써 Hooks를 사용한다면 가장 많이 사용하게 될 함수가 바로 <b>useState</b>입니다. 

다음은 클래스 컴포넌트를 사용해서 만든 간단한 카운터 예제입니다.
```jsx
import React, { Component } from 'react';

class Counter extends Component{
  state = { count: 0 }

  increase = () => {
    this.setState(prevState => ({count: prevState.count + 1}));
  }

  render(){
    return <button onClick={this.increase}>{this.state.count}</button>
  }
}
```

다음은 위의 예제를 함수형 컴포넌트와 userState Hook을 이용해서 리팩토링 했습니다.

```jsx
import React, { useState } from 'react';

function Counter(){
  const [ count, setCount ] = useState(0);

  const increase = () => {
    setCount(count + 1);
  }

  return <button onClick={increase}>{count}</button>
}
```

useState라는 함수는 파라미터로 사용하고자 하는 state의 기본값(0)을 받으며 배열을 리턴합니다. 배열의 첫 번째 요소는 현재 state(count)의 값이며 두 번째 요소는 state를 갱신할 수 있는 함수입니다. 여기서 예리한 눈을 가지신 분들은 "useState함수가 매번 0을 리턴할 것인데 어떻게 카운터를 만들 수 있나" 라는 의문을 가지실 수도 있습니다. 

useState는 렌더링 될 때마다 계속 실행되는 것이 맞지만 매번 초깃값을 리턴하는 것이 아니며 setCount에 의해 갱신된 count 값을 유지합니다. 그래서 createState라는 이름 대신 useState라는 함수명을 갖게 되었습니다.

<br>

#### useEffect Hook

함수형 컴포넌트에서는 생명주기 함수를 사용할 수 없습니다. 리엑트에서 클래스 컴포넌트는 다음과 같은 다양한 생명주기 함수를 가집니다.

* constructor - 생성자
* componentWillMount - 컴포넌트가 마운트 되기 전
* componetDidMount - 컴포넌트가 마운트 된 후
* componentWillReciveProps - 컴포넌트에 새로운 props가 전달 되기 전
* shouldComponentUpdate - 컴포넌트가 rerender 되어야할지 결정
* componentWillUpdate - 컴포넌트가 rerender 되기 전
* componentDidUpdate - 컴포넌트가 rerender 된 후
* render - render함수
* componentWillUnmount - 컴포넌트가 제거되기 전

함수형 컴포넌트는 위 생명주기 함수 중에서 오직 render함수의 기능밖에 하지 못하기(render함수 그 자체라고 봐도 무방) 때문에 기능적, 성능적인 면에서 제약이 있습니다. 예를 들면 컴포넌트가 마운트 된 후 API를 호출해야하는 경우, 컴포넌트가 언마운트 되기 전에 이벤트를 해제해야하는 경우들이 있습니다. 

이런 한계를 극복해주는 Hooke들이 있는데 대표적인 Hook이 바로 useEffect Hook입니다. useEffect Hook은 componentDidMount, componentDidUpdate, componentWillUpdate, componentWillUnmount의 기능을 수행할 수 있습니다. 컴포넌트는 랜더링을 통해 뷰를 생성하는게 주 임무이지만 API호출, 이벤트 추가 등 부가적인 효과(side effect)를 필요로 하므로 이때 사용하라고 use(side)Effect라는 함수명을 갖게 되었습니다.
```jsx
import React, { useState, useEffect } from 'react';

function Test({}){
  const [count, setCount] = useState(0);
  const [fruit, setFruit] = useState('apple');

  useEffect(()=> {
    console.log('component did mount or update!');
  });

  useEffect(()=> {
    console.log('component did mount!');
  }, []);

  useEffect(()=> {
    console.log('count up!');
  }, [count]);

  useEffect(()=>{
    console.log('effect', count);
    return () => { 
      console.log('component will update or will unmount!', count);
    }
  });

  return (<div>
    <button type="button" onClick={()=>{ setCount(count + 1); }}>{count}</button>
    <input type="text" onChange={(e)=>{ setFruit(e.target.value) }}/>
  </div>)
}
```

useEffect Hook은 첫 번째 파라미터로 실행될 함수를, 두 번째 파라미터로 첫 번째 파라미터의 실행 조건이 될 prop이나 state들을 배열로 받습니다. 실행되는 시점은 랜더링이 끝나고 컴포넌트가 마운드 된 후입니다. 처음 마운트(componentDidMount)때는 무조건 시행이 되고 이후 마운트(componentDidUpdate)부터는 실행 조건 배열에 넣은 값들이 변경되었을 때만 실행됩니다. 실행 조건 배열을 넣어주지 않으면 처음 마운트 될 때만 실행이 되며 빈 배열([])을 넣어주게 되면 다시 렌더링 될 때마다 무조건 실행됩니다. 뒷정리 함수라고 불리는 함수를 리턴하기도 하는데 컴포넌트가 업데이트되기 전 또는 마운트가 해제되기 전에 실행되며 이벤트 해제, 리소스 반환 등의 작업을 할 수 있게 해줍니다.

<br>

#### useMemo Hook

이번에는 예제를 먼저 보시길 바랍니다.

```jsx
import React, { useState } from "react";

function getPrice(strArray) {
  console.log('compute...');
  return strArray.reduce((acc, el) => acc + el.length, 0);
}

function Basket() {
  const [input, setInput] = useState("");
  const [fruits, setFruits] = useState(["apple", "banana"]);
  return (
    <div>
      <input
        type="text"
        value={input}
        onChange={e => {
          setInput(e.target.value);
        }}
      />
      <button
        onClick={() => {
          setFruits(fruits.concat([input]));
        }}
      >
        Add
      </button>
      <ul>
        {fruits.map(el => (
          <li key={el}>{el}</li>
        ))}
      </ul>
      <p>가격 : {getPrice(fruits)}원</p>
    </div>
  );
}
```

간단한 장바구니 컴포넌트입니다. 사용자가 문자열을 입력하고 Add버튼을 누르면 문자열이 fruits에 추가됩니다. 예제의 끝 부분에서 getPrice라는 함수가 하는 역할은 담긴 각 과일들의 문자열 길이를 더해서 가격을 계산합니다. getPrice함수는 렌더링 될 때마다 실행이 되므로 Add버튼이 클릭되었을 때만 실행되는 것이 아닌 타이핑 될 때마다 실행이 되므로 비 효율적입닏다. 지금은 getPrice함수가 간단하지만 API를 호출하는 등, 시간이 오래 걸리는 함수라면 최적화가 필요할 것입니다. Add버튼이 눌릴 때만 가격이 계산되도록 Memo Hook을 사용해 보도록 하겠습니다.

```jsx
import React, { useState } from "react";

function getPrice(strArray) {
  console.log('compute...');
  return strArray.reduce((acc, el) => acc + el.length, 0);
}

function Basket() {
  const [input, setInput] = useState("");
  const [fruits, setFruits] = useState(["apple", "banana"]);
  const price = useMemo(() => getPrice(fruits), [fruits]);
  return (
    <div>
      <input
        type="text"
        value={input}
        onChange={e => {
          setInput(e.target.value);
        }}
      />
      <button
        onClick={() => {
          setFruits(fruits.concat([input]));
        }}
      >
        Add
      </button>
      <ul>
        {fruits.map(el => (
          <li key={el}>{el}</li>
        ))}
      </ul>
      <p>가격 : {price}원</p>
    </div>
  );
}
```

11번째 라인이 추가되었고 33번째 라인이 변경되었습니다. useMomo Hook은 useEffect Hook과 사용하는 방법은 똑같지만 실행 시점이 마운트된 후, 업데이트 후, 마운트 해제 후(뒷정리 함수)가 아닌 렌더 되기 전이라고 생각하시면 됩니다. 

<br>

#### 사용자 정의 Hooks

앞에서는 대표적인 Hooks들을 알아 봤습니다. React Hooks의 진정한 장점은 React에서 제공해주는 Hooks를 조합하고 응용해서 새로운 Hooks를 생성할 수 있다는 점입니다. useState Hook을 사용해서 배열을 관리하는 Hook을 만든 뒤 앞선 Basket 컴포넌트에서 사용해 보도록 하겠습니다.

```jsx
import React, { useState, useMemo } from "react";

function useArray(initArray = []) {
  const [array, setArray] = useState(initArray);
  const push = el => {
    setArray(array.concat([el]));
  };
  const clear = () => {
    setArray([]);
  };
  return [array, push, clear];
}

function getPrice(strArray) {
  console.log("compute...");
  return strArray.reduce((acc, el) => acc + el.length, 0);
}

function Basket() {
  const [input, setInput] = useState("");
  const [fruits, pushFruit, clearFruit] = useArray(["apple", "banana"]);
  const price = useMemo(() => getPrice(fruits), [fruits]);
  return (
    <div>
      <input
        type="text"
        value={input}
        onChange={e => {
          setInput(e.target.value);
        }}
      />
      <button
        onClick={() => {
          pushFruit(input);
        }}
      >
        Add
      </button>
      <button
        onClick={() => {
          clearFruit();
        }}
      >
        Clear
      </button>
      <ul>
        {fruits.map(el => (
          <li key={el}>{el}</li>
        ))}
      </ul>
      <p>가격 : {price}</p>
    </div>
  );
}
```

이러한 사용자 정의 Hooks는 컴포넌트에서 로직을 별도의 함수로 분리해 내어 가독성과 재사용성을 크게 높여주며 컴포넌트가 뷰에만 집중할 수 있도록 도와줍니다. 개인적으로 Hooks를 사용하면서 함수형 프로그래밍을 하고 있다는 느낌을 강하게 받기도 했습니다. 또한 사용자 정의 Hooks는 직접 만들어서 사용할 수도 있지만 다른 개발자 분들이 이미 만들어 놓은 Hooks를 라이브러리처럼 사용할수도 있습니다. 이를 통해 개발 생산성을 크게 높일 수도 있습니다. 이로써 이번 포스팅을 마무리 짓도록 하겠습니다.
