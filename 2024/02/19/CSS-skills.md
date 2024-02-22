# 다양한 CSS기술들

## BEM (Block Element Modifier)
`.block__element--modifier` 형태로 쓴다.

```javascript
<form class="signin-form">
  <label class="signin-form__label">
    Email
    <input type="text" class="signin-form__input">
  </label>
  <label class="signin-form__label">
    Password
    <input type="password" class="signin_form__input signin_form__input--pasword">
  </label>
  <button class="signin-form__button signin-form__button--submit">
    Sign In
  </button>
</form>
```

```javascript
.signin-form { /* 로그인 폼 */ }

.signin-form__input { /* 로그인 폼의 인풋 */ }

.signin-form__input.signin-form__input--password { /* 로그인 폼의 비밀번호 인풋 */ }

.signin-form__button { /* 로그인 폼의 버튼 */ }

.signin-form__button.signin-form__button--submit { /* 로그인 폼의 제출 버튼 */ }
```

---

## 네스팅 예시

### Sass(SCSS)
CSS는 웹 표준이기 때문에 문법이 빠르게 바뀌지 않습니다. 그래서 개발자들이 사용하기 편한 여러가지 문법을 추가한 새로운 언어를 만들기 시작했는데, 그 중에 가장 많이 쓰이는 것이 바로 Sass입니다. 변수, 네스팅(Nesting) 문법, 믹스인(Mixin) 등등 다양한 기능을 제공합니다. 이 중에서 많은 사람들이 좋다고 생각한 문법(변수, 네스팅 등)은 웹 표준으로 흡수되기도 했습니다.

Sass는 프리프로세서(Preprocessor) 스크립트 언어라고 하는데, 프리프로세서라는 프로그램을 통해서 Sass 코드를 CSS 코드로 변환하기 때문에 그렇습니다.

Sass에는 기존 Sass와 SCSS 두 가지 문법이 있는데, 최근에는 CSS의 모든 문법 위에서 확장된 문법을 사용하는 SCSS를 많이 사용합니다.

#### 네스팅 예시

#### Scss

```javascript
nav {
  ul {
    margin: 0;
    padding: 0;
    list-style: none;
  }

  li { display: inline-block; }

  a {
    display: block;
    padding: 6px 12px;
    text-decoration: none;
  }
}
```

#### CSS

```javascript
nav ul {
  margin: 0;
  padding: 0;
  list-style: none;
}

nav li {
  display: inline-block;
}

nav a {
  display: block;
  padding: 6px 12px;
  text-decoration: none;
}
```

#### 믹스인 예시

#### Scss

```javascript
@mixin theme($theme: DarkGray) {
  background: $theme;
  box-shadow: 0 0 1px rgba($theme, .25);
  color: #fff;
}

.info {
  @include theme;
}

.alert {
  @include theme($theme: DarkRed);
}

.success {
  @include theme($theme: DarkGreen);
}
```

#### CSS

```javascript
.info {
  background: DarkGray;
  box-shadow: 0 0 1px rgba(DarkGray, .25);
  color: #fff;
}

.alert {
  background: DarkRed;
  box-shadow: 0 0 1px rgba(DarkRed, .25);
  color: #fff;
}

.success {
  background: DarkGreen;
  box-shadow: 0 0 1px rgba(DarkGreen, .25);
  color: #fff;
}
```


