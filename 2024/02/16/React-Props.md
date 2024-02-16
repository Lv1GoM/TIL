# React

## Props
- 리액트에서 컴포넌트에 지정한 속성들
- Props는 Properties의 약자로 컴포넌트에 속성을 지정해주면 각 속성이 하나의 객체로 모여서 컴포넌트를 정의한 함수의 첫 번째 파라미터로 전달된다.
-  props가 객체 형태를 띠고 있으니 Destructuring 문법을 활용해서 조금 더 간결하게 코드를 작성할 수도 있다.

### Children
props에는 children이라는 조금 특별한 프로퍼티(prop, 프롭)가 있다. JSX 문법으로 컴포넌트를 작성할 때 컴포넌트를 단일 태그가 아니라 여는 태그와 닫는 태그의 형태로 작성하면, 그 안에 작성된 코드가 바로 이 children 값에 담기게 된다
```javascript
function Button({ children }) {
  return <button>{children}</button>;
}

export default Button;
```

> JSX 문법으로 컴포넌트를 작성할 때 어떤 정보를 전달할 때는 일반적인 props의 속성값을 주로 활용하고, 화면에 보여질 모습을 조금 더 직관적인 코드로 작성하고자 할 때 children 값을 활용할 수가 있다.