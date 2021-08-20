# SCSS
1. [프로젝트 생성](#프로젝트-생성)
1. [주석](#주석)
1. [중첩 with SassMeister](#중첩-with-sassmeister)
1. [부모 선택자 참조](#부모-선택자-참조)
1. [중첩된 속성](#중첩된-속성)
1. [변수](#변수)
1. [산술 연산](#산술-연산)
1. [재활용](#재활용)
1. [반복문](#반복문)
1. [함수](#함수)
1. [색상 내장 함수](#색상-내장-함수)
1. [가져오기](#가져오기)
1. [Overwatch 캐릭터 선택 예제 리펙토링](#overwatch-캐릭터-선택-예제-리펙토링)
1. [데이터 종류](#데이터-종류)
1. [반복문 @each](#반복문-each)
1. [재활용 @content](#재활용-content)

# SCSS

## 프로젝트 생성
[sass](#https://sass-lang.com/) 검색, CSS Preprocessor  
```bash
$ npm init -y
$ npm i -D parcel-bundler
```
`package.json`
```json
"scripts": {
    "dev": "parcel index.html",
    "build": "parcel build index.html"
  },
```
`index.html`
```html
<head>
  <link rel="stylesheet" href="./main.scss">
</head>
```
`main.scss`
```scss
$color: royalblue;

.container {
  h1 {
    color: $color;
  }
}
```

## 주석
`main.scss`
```scss
$color: royalblue;

.container {
  h1 {
    color: $color;
    /* background-color: orange; 보임 */
    // font-size: 60px; 안보임
  }
}
```

## 중첩 with SassMeister
[sassmeister](https://www.sassmeister.com/) scss -> css  
`main.scss`
```html
<body>
  <div class="container">
    <ul>
      <li>
        <div class="name">HEROPY</div>
        <div class="age">85</div>
      </li>
    </ul>
  </div>
</body>
```
`main.scss`
```scss
.container {
  > ul { // 자식 요소
    li {
      font-size: 40px;
      .name {
        color: royalblue;
      }
      .age {
        color: orange;
      }
    }
  }
}
```

## 부모 선택자 참조
```scss
.btn {
  position: absolute;
  &.active {
    color: red;
  }
}

.list {
  li {
    &:last-child {
      margin-right: 0;
    }
  }
}

.fs {
  &-small { font-size: 12px; }
  &-medium { font-size: 14px; }
  &-large { font-size: 16px; }
}
```

## 중첩된 속성
```scss
.box {
  font: {
    weight: bold;
    size: 10px;
    family: sans-serif;
  };  // 세미콜론 주의
  margin: {
    top: 10px;
    left: 20px;
  };
  padding: {
    top: 10px;
    bottom: 40px;
    left: 20px;
    right: 30px;
  };
}
```

## 변수
유효범위 : 블록 레벨  
```scss
.container {
    $size: 200px;
    position: fixed;
    top: $size;
    .item {
        $size: 100px;  // 재할당
        width: $size;
        height: $size;
        transform: translateX($size);
    }
    left: $size;
}

// .box {
//     width: $size;
// }
```

## 산술 연산
```scss
div {
    $size: 30px;
    width: 20px + 20px;
    height: 40px - 10px;
    font-size: 10px * 2;
    margin: 30px / 2;
    // margin: (30px / 2); // 소괄호
    // margin: $size / 2;  // 변수
    // margin: (10px + 12px) / 2;  // 산술 연산자
    padding: 20px * 7;
}
.span {
    font-size: 10px;
    line-height: 10px;
    font-family: serif;
    font: 10px / 10px serif;  // 단축 속성 구분자, font-size / line-height
}
```
```scss
div {
    width: 100% - 200px; // 단위 동일 해야함
    // width: calc(100% - 200px);
}
```

## 재활용
```scss
@mixin center {
    display: flex;
    justify-content: center;
    align-items: center;
}
.container {
    @include center;
    .item {
        @include center;
    }
}
.box {
    @include center;
}
```
```scss
@mixin box($size: 80px, $color: tomato) {  // 매개변수, 기본값
    width: $size;
    height: $size;
    background-color: $color;
}
.container {
    @include box(200px, red); // 인수
    .item {
        @include box($color: green); // 키워드 인수
    }
}
.box {
    @include box;
}
```

## 반복문
```scss
// for (let i = 0; i < 10; i += 1) {
//   console.log(`loop-${i}`)
// }

@for $i from 1 through 10 {
    .box:nth-child(#{$i}) { // 데이터 보관 #{}
        width: 100px * $i;
    }
}
```
## 함수
```scss
@mixin center { // 재활용 모음
    display: flex;
    justify-content: center;
    align-items: center;
}

@function ratio($size, $ratio) { // 연산
    @return $size * $ratio
}

.box {
    $width: 100px;
    width: $width;
    height: ratio($width, 9/16);
    @include center;
}
```

## 색상 내장 함수
`main.scss`
```scss
.box {
  $color: royalblue;
  width: 200px;
  height: 100px;
  margin: 20px;
  border-radius: 10px;
  background-color: $color;
  &:hover {
    background-color: darken($color, 10%);
  }
  &.built-in {
    // background-color: mix($color, red); 
    // background-color: lighten($color, 10%); 
    // background-color: darken($color, 10%);
    // background-color: saturate($color, 40%); // 채도
    // background-color: desaturate($color, 40%);
    // background-color: grayscale($color); 
    // background-color: invert($color);  // 반전
    background-color: rgba($color, .5); // scss 버전
  }
}
```
`index.html`
```html
<body>
  <div class="box"></div>
  <div class="box built-in"></div>
</body>
```

## 가져오기
`main.scss`
```scss
// @import url("./sub.scss"); // css 방식
@import "./sub", "./sub2"; // scss 방식

$color: royalblue;

.container {
  h1 {
    color: $color;
  }
}
```
`sub.scss`
```scss
body {
  .container {
    background-color: orange;
  }
}
```
`sub2.scss`
```scss
body {
  background-color: royalblue;
}
```
`index.html`
```html
<div class="container">
    <h1>Hello SCSS!</h1>
</div>
```

## Overwatch 캐릭터 선택 예제 리펙토링
`./overwatch/`
```bash
$ cd D:\front-workspace\Part1\overwatch\
$ npm init -y
$ npm i -D parcel-bundler
```
rename `main.css` -> `main.scss`  
`package.json`
```json
"scripts": {
    "dev": "parcel index.html",
    "build": "parcel build index.html"
  },
```
`main.scss` refactoring...  
make `.gitignore`  
`./overwatch/`
```bash
$ git add .
$ git commit -m 'scss refactoring'
```

## 데이터 종류
```scss
$number: 1;     // .5, 100px, 1em
$string: bold;  // relative, "../images/a.png"
$color: red;    // blue, #FFFF00, rgba(0,0,0,.1)
$boolean: true; // false
$null: null;
$list: orange, royalblue, yellow;
$map: (
    o: orange,
    r: royalblue,
    y: yellow
);
.box {
    width: 100px;
    color: red;
    position: null;
}
```

## 반복문 @each
```scss
$list: orange, royalblue, yellow;
$map: (
    o: orange,
    r: royalblue,
    y: yellow
);

@each $c in $list {
    .box {
        color: $c;
    }
}
@each $k, $v in $map {
    .box #{$k}{
        color: $v;
    }
}
```

## 재활용 @content
```scss
@mixin left-top {
    position: absolute;
    top: 0;
    left: 0;
    @content;
}
.container {
    width: 100px;
    height: 100px;
    @include left-top;
}
.box {
    width: 200px;
    height: 300px;
    @include left-top { // 추가내용
        bottom: 0;
        right: 0;
        margin: auto;
    }
}
```