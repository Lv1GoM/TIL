# React

## State
리액트(React)의 useState는 함수 컴포넌트에서 상태(state)를 관리하는 데 사용되는 훅(Hook) 으로 배열을 반환하며, 첫 번째 요소는 현재 상태의 값이고, 두 번째 요소는 상태를 업데이트하는 함수이다.

- `useState`를 사용하면 함수형 컴포넌트에서도 상태를 지역적으로 관리할 수 있으며, 상태의 변화에 따라 컴포넌트가 리렌더링되어 화면이 업데이트 된다.

- `const [num, setNum] = useState(초기값);` 형식으로 작성 `const numArr = useState();`처럼 작성할 수도 있다. 이 경우 `numArr[0]`으로 값에 접근할 수 있다.

- `num` 값을 변경할때는 반드시 `setNum`함수를 이용해야 한다.


### 참조형 State
`useState`에서 참조형 변수(객체 또는 배열 등)를 사용할 때에는 몇 가지 주의사항이 있다.

#### 1. 객체나 배열의 불변성 유지
	- `useState`로 관리되는 상태 변수에 참조형 변수를 업데이트할 때, 해당 변수를 직접 수정하지 않고 불변성을 유지해야 한다. React는 상태의 변경 여부를 판단할 때 이전 상태와 현재 상태를 비교하는데, 참조형 변수를 직접 수정하면 참조가 같아도 변경으로 간주되지 않을 수 있다.
	- 객체나 배열을 업데이트할 때는 새로운 객체나 배열을 생성하여 업데이트하는 것이 좋다. 이를 위해 전개 연산자나 배열/객체의 메소드를 사용한다.
```javascript
	// 객체의 경우
const [user, setUser] = useState({ name: 'John', age: 25 });

// 올바른 방법: 객체를 복제하고 업데이트
setUser(prevUser => ({ ...prevUser, age: prevUser.age + 1 }));

// 잘못된 방법: 객체를 직접 수정 (불변성이 유지되지 않음)
// user.age += 1;
// setUser(user);
```

#### 2. 이전 상태에 의존하는 업데이트 함수 사용
	- 상태를 업데이트할 때 함수형 업데이트를 사용하는 것이 좋다. 함수형 업데이트를 사용하면 이전 상태에 안전하게 접근할 수 있다.
```javascript
	// 올바른 방법: 함수형 업데이트 사용
setCount(prevCount => prevCount + 1);

// 잘못된 방법: 이전 상태에 의존하지 않음 (비동기 문제 발생 가능)
// setCount(count + 1);
```

#### 3. 의존성 배열을 잘 설정
	- 상태 업데이트가 이전 상태에 의존하는 경우 `useEffect`나 `useCallback` 등에서 의존성 배열을 올바르게 설정해야 한다.
```javascript
	// 의존성 배열에 이전 상태를 포함하는 useEffect
useEffect(() => {
  // 이전 상태에 기반한 로직 수행
  console.log(`Count changed: ${count}`);
}, [count]);
```

> 참조형 변수를 사용할 때 이러한 주의사항을 지키면서 상태를 업데이트하면 React가 상태 변화를 적절하게 감지하고 컴포넌트를 리렌더링할 수 있다.

