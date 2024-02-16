Styled Comopnent

## `styled-components`라이브러리에서 주로 사용되는 항목

### styled
- `import styled from 'styled-components';`로 불러올 수 있음
- 컴포넌트 스타일을 정의할 때 사용

```javascript
import styled from 'styled-components';

const StyledDiv = styled.div`
  color: red;
  font-size: 16px;
`;
```

### css
- `import { css } from 'styled-components';`로 불러올 수 있음
- 스타일을 정의할 때 사용되는 함수.

```javascript
import { css } from 'styled-components';

const shadow20 = css`
  box-shadow: 0 10px 15px rgba(0, 0, 0, 0.2);
`;
```

### styled, css 같이 사용하기
styled를 사용하여 컴포넌트에 스타일을 지정하고, css를 사용하여 재사용 가능한 스타일 블록을 만든다.

```javascript
import styled, { css } from 'styled-components';

// 재사용 가능한 스타일 블록
const boxShadowStyle = css`
  box-shadow: 0 10px 15px rgba(0, 0, 0, 0.2);
`;

// 스타일이 적용된 컴포넌트 정의
const StyledDiv = styled.div`
  color: red;
  font-size: 16px;
  ${boxShadowStyle} // 재사용 가능한 스타일 블록을 포함
`;

// 다른 컴포넌트에도 동일한 스타일 적용
const AnotherStyledDiv = styled.div`
  background-color: lightblue;
  ${boxShadowStyle} // 재사용 가능한 스타일 블록을 포함
`;
```

---

## Nesting
CSS규칙 안에서 CSS규칙을 만드는것.

### & 선택자
- 부모 선택자를 의미한다.
```javascript
import styled from 'styled-components';

const Button = styled.button`
  background-color: #6750a4;
  border: none;
  color: #ffffff;
  padding: 16px;

  &:hover,
  &:active {
    background-color: #463770;
  }
`;

export default Button;
```
> 위 코드에서 ```&:hover,
  &:active {
    background-color: #463770;
  }``` 부분은 기본 css에서 ```.Button:hover,
.Button:active {
  background-color: #463770;
}```와 같은 의미이다.

### 컴포넌트 선택자
- ${}같이 컴포넌트 자체를 템플릿 리터럴 안에 넣어주면 된다.
```javascript
import styled from 'styled-components';
import nailImg from './nail.png';

const Icon = styled.img`
  width: 16px;
  height: 16px;
`;

const StyledButton = styled.button`
  background-color: #6750a4;
  border: none;
  color: #ffffff;
  padding: 16px;

  & ${Icon} {
    margin-right: 4px;
  }

  &:hover,
  &:active {
    background-color: #463770;
  }
`;

function Button({ children, ...buttonProps }) {
  return (
    <StyledButton {...buttonProps}>
      <Icon src={nailImg} alt="nail icon" />
      {children}
    </StyledButton>
  );
}

export default Button;
```
자손 결합자로 쓴 `& ${Icon} { ... }`부분을 CSS로 표현해본다면 
```javascript
.StyledButton {
  ...
}

.StyledButton .Icon {
  margin-right: 4px;
}
```
위 코드와 같다.

> &와 자손 결합자를 사용하는 경우에는 &를 생략할 수 있다. 자손 결합자로 Nesting할 때는 생략하는것이 권장된다.

---

> Nesting은 여러겹으로 중첩해서 할 수도 있다.

```javascript
const StyledButton = styled.button`
  ...
  &:hover,
  &:active {
    background-color: #7760b4;

    ${Icon} {
      opacity: 0.2;
    }
  }
`;
```

`&:hover, &:active { ... }` 안에 있는 `${Icon}` 선택자를 CSS 코드로 표현해 보면 아래와 같다.

```javascript
.StyledButton:hover .Icon,
.StyledButton:active .Icon {
  opacity: 0.5;
}
```

## 다이나믹 스타일링

### `${ ... }` 안에 값(변수) 사용하기
- 가장 기본적인 사용법은 JavaScript 변수를 그대로 넣는 방식으로 평소에 템플릿 문자열을 만들 때 사용하는 방식이다.

### `${ ... }` 안에 함수 사용하기
- 함수의 파라미터로는 Props를 받고, 리턴 값으로는 스타일 코드를 리턴하면 된다. => 템플릿 리터럴의 기능이 아니라 Styled Components가 내부적으로 처리해 주는 것.
- 구조 분해(Destructing)으로 작성도 가능하다.
	`font-size: ${(props) => SIZES[props.size]}px;` => `font-size: ${({ size }) => SIZES[size]}px;`
> 여기서 `size Prop`이 값이 없거나 잘못된 값일경우 styled Components에서는 `undefined` 값을 빈 문자열로 처리해 주기 때문에 잘못된 CSS코드가 된다. => 널 병합 연산자를 사용해 해결 할 수 있다.

### 논리 연산자 사용하기
```javascript
const Button = styled.button`
  ...
  ${({ round }) => round && `
      border-radius: 9999px;
    `}
`;
```
`round` 값이 참이면 그 뒤 값까지 계산하기때문에 `border-radius: 9999px`이라는 문자열이 리턴돼서 적용된다. `round`값이 거짓이면 `false`가 리턴돼서 아무런 값도 적용되지 않는다.

### 삼항 연산자 사용하기
`border-radius: ${({ round }) => round ? `9999px` : `3px`};`
좀더 간단하게 코드를 작성할 수 있다.

---

## 스타일 재사용: 상속
JSX 문법으로 직접 만든 컴포넌트나, Styled Components를 사용해 이미 만들어진 다른 컴포넌트에 스타일을 입히려면  상속을 사용하면 된다.

### `styled()` 함수
`Styled Components`로 만들어진 컴포넌트를 상속하려면 `styled()` 함수를 사용하면 된다. => 여기서 상속은 다른 컴포넌트의 스타일을 가져와서 원하는 대로 사용할 수 있는 것.

### JSX로 직접 만든 컴포넌트에 `styled()` 사용하기
`styled.tagname`으로 만든 컴포넌트는 바로 `styled()` 함수를 사용할 수 있지만, 그렇지 않은 컴포넌트는 따로 처리가 필요함.
=> `Styled Components`를 사용하지 않고 직접 만든 컴포넌트는 `className` 값을 `Prop`으로 따로 내려줘야 `styled()` 함수를 사용할 수 있다.

> 직접 만든 컴포넌트에 `className Prop`을 따로 내려주는 건 `styled()` 함수가 적용될 부분의 `className`을 별도로 정해주는 거라고 이해하시면 된다. => 정리하자면, 스타일 상속을 하려면 `styled()` 함수를 사용하면 되는데, `styled.tagname`으로 만든 컴포넌트는 styled() 함수로 바로 상속하면 되고, `Styled Components`를 사용하지 않고 직접 만든 컴포넌트에는 클래스 이름을 내려준 후에 `styled()` 함수로 상속해야 한다.

## 스타일 재사용: css함수
중복되는 CSS 코드들을 변수처럼 저장해서 여러 번 다시 사용하고 싶을 때 사용된다.
```javascript
const fontSize = css`
  font-size: ${({ size }) => SIZES[size] ?? SIZES['medium']}px;
`;
``` 

```javascript
const Button = styled.button`
  ...
  ${fontSize}
`;

const Input = styled.input`
  ...
  ${fontSize}
`;
```
일반적인 템플릿 리터럴을 쓰는 게 아니라 `css`라는 함수를 붙여서 쓴다. => Props를 받아서 사용하는 함수가 들어있기 때문에 반드시 `css`함수를 사용해야 한다. 항상 `css`함수를 사용하도록 습관화 하자.
