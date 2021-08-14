# 🔎 Overview

- flex-basis, flex-grow로 쉽게 비율을 나누기
- align-items: stretch 가 기본값 임을 알기
  - 컨테이너 내부의 아이템들이 cross axis 축 방향으로 길이 / 너비가 같아짐을 이해
- align-self 속성으로 아이템 개별적으로 cross axis 축의 정렬을 결정할 수 있음
- flex와 margin: auto 속성으로 space-between과 같은 효과를 얻기
- 디자인 재사용성을 높여주는 속성들.. `inherit, currentColor, transparent`
- flex-wrap, flex-basis 속성으로 n-column layout 만들기
- box-sizing: content-box는 border 때문에 내부 크기가 줄어드는 문제를 해결한다.
- SVG 파일을 사용하기
  - sprite.svg 하나의 이미지로 여러 아이콘을 사용할 수 있다.
  - fill: var(--color-grey-dark-3); 로 색상을 변경할 수 있다
  - background-image로 svg파일을 가져오는 방식과 mask 프로퍼티 사용하기
- flex box 와 반응형
  - side by side 배치를 위 아래로
  - wrap으로 떨어뜨리기도 한다.
  - 반대로 side by side 를 적용하고 내부 아이템을 균등한 크기를 갖도록 할 수 있다.

---

# 📖 Contents

## 플렉스 레이아웃의 이해

### 📍 flex-basis, flex-grow로 쉽게 비율을 나누기

```css
.sidebar {
  flex: 0 0 18%; /* 18% */
}

.hotel-view {
  flex: 1; /* 82% */
}
```

### 📍 align-self 속성으로 아이템 개별적으로 cross axis 축의 정렬을 결정할 수 있음

```css
  .user-nav {
    display: flex; /* flex 컨테이너이자 */
    align-items: center;  /*(아이템들은 수직 가운데 정렬함)*/
    align-self: stretch; /* flex 아이템이다.(자신은 늘리고)*/
```

### 📍 align-items: stretch 가 기본값 임을 알기;📍 flex와 margin: auto 속성으로 space-between과 같은 효과를 얻기

```css
&__stars {
  /*MEMO: flex: 1을 하면 요소의 크기가 늘어나기 때문에, hover 이벤트 시 적용 영역이 너무 커진다.*/
  /* flex: 1 ;*/
  /* MEMO: 옆의 요소와 거리를 최대한 벌리기 위해서 margin: auto를 사용한다. 자동으로 계산하여 최대한 margin을 주게됨.*/
  margin-right: auto;
  /* MEMO: SVG img들의 높이를 같게 해주기 위해 부모 요소에 flex를 적용시켜주었다.*/
  display: flex;
}
```

### 📍 flex-wrap, flex-basis 속성으로 n-column layout 만들기

```css
display: flex;
flex-wrap: wrap;

&__item {
  flex: 0 0 50%; /* 2-column 으로 배치된다. */
  margin-bottom: 0.7rem;
}
```

## 플렉스 레이아웃과 반응형의 조합

### 📍 side by side 배치를 위 아래로 바꾸어주기

```css
@media only screen and (max-width: $bp-medium) {
  flex-direction: column;
}
```

### 📍 wrap으로 떨어뜨리기도 한다.

```css
@media only screen and (max-width: $bp-smallest) {
  flex-wrap: wrap;
  align-content: space-around;
  height: 11rem;
}
```

### 📍 반대로 side by side 를 적용하고 내부 아이템을 균등한 크기를 갖도록 할 수 있다.

```css
  .side-nav {
    /* 생략 */
    @media only screen and (max-width: $bp-medium) {
      display: flex;
      margin: 0;
    }

    &__item {
      position: relative;

      &:not(:last-child) {
        margin-bottom: 0.5rem;

        @media only screen and (max-width: $bp-medium) {
          margin: 0;
        }
      }

      @media only screen and (max-width: $bp-medium) {
        flex: 1;
      }
    }
```

## 기타 팁

### 📍 SVG 파일을 사용하기

- id를 #으로 구분하여 불러온다.

  ```html
  <svg
    style="position: absolute; width: 0; height: 0; overflow: hidden;"
    version="1.1"
    xmlns="http://www.w3.org/2000/svg"
    xmlns:xlink="http://www.w3.org/1999/xlink"
  >
    <defs>
      <symbol id="icon-bookmark" viewBox="0 0 20 20">
        <title>bookmark</title>
        <path
          class="path1"
          d="M14 2v17l-4-4-4 4v-17c0-0.553 0.585-1.020 1-1h6c0.689-0.020 1 0.447 1 1z"
        ></path>
      </symbol>
      <symbol id="icon-chevron-down" viewBox="0 0 20 20">
        <title>chevron-down</title>
        <path
          class="path1"
          d="M4.516 7.548c0.436-0.446 1.043-0.481 1.576 0l3.908 3.747 3.908-3.747c0.533-0.481 1.141-0.446 1.574 0 0.436 0.445 0.408 1.197 0 1.615-0.406 0.418-4.695 4.502-4.695 4.502-0.217 0.223-0.502 0.335-0.787 0.335s-0.57-0.112-0.789-0.335c0 0-4.287-4.084-4.695-4.502s-0.436-1.17 0-1.615z"
        ></path>
      </symbol>
    </defs>
  </svg>
  ```

  ```html
  <svg class="user-nav__icon">
    <use xlink:href="img/sprite.svg#icon-bookmark"></use>
  </svg>

  <svg class="user-nav__icon">
    <use xlink:href="img/sprite.svg#icon-chat"></use>
  </svg>
  ```

  ```css
  fill: var(
    --color-grey-dark-3
  ); /* mark up 한 svg 이미지는 fill 로 색상을 변경할 수 있다. */
  ```

- css 에서 background-image로 svg 사용하기

  - background-image로 svg파일을 가져오는 방식(old browser)은 색상 변경이 불가능하다 -> mask를 사용한다.
  - mask css image 부분만 빵꾸뽕(newer browser)해서 색상을 보이게 한다.

  ```css
  @supports ((-webkit-mask-image: url()) or (mask-image: url())) {
    background-color: var(--color-primary);
    mask-image: url(../img/chevron-thin-right.svg);
    -webkit-mask-image: url(../img/chevron-thin-right.svg);
    mask-size: cover;
    -webkit-mask-size: cover;
  }
  ```

### 📍 box-sizing: content-box는 border 때문에 내부 크기가 줄어드는 문제를 해결한다.

```css
border: 3px solid #fff; /* border 가 두껍기 때문에 내부 크기가 줄어든다.(border-box) */
box-sizing: content-box;
```

### 📍 디자인 재사용성을 높여주는 속성들

```css
font-size: inherit;
border-bottom: 1px solid currentColor;
background-color: transparent;
```
