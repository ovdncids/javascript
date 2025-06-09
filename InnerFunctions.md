# 내장 함수 (Inner Functions)

## window 객체
window.html
```html
<script>
</script>
```

### var를 이용해서 변수 선언
```js
var v1 = 'a';
v2 = 'b';
const condition1 = v1 === window.v1;
const condition2 = v2 === window.v2;
```
* IE11 이후 부터 var는 사용하지 않는다.
* 여러 파일에서 동시에 사용 해야할 경우 `window.변수명` 이렇게 명시화 해서 사용한다.
```js
window.v3 = 'c';
```

### window 객체 안으로 function 만들기
```js
function f1() {}
const condition3 = f1 === window.f1;
```

### confirm, alert, console.log
```js
if (window.confirm('진행 하시겠습니까?')) {
  window.console.log('진행');
} else {
  window.alert('멈춤');
}
```
* `window` 안에 요소들은 `window.` 생략 가능

### JSON
```js
const string1 = '[1, 2, 3]';
const array1 = JSON.parse(string1);
const string2 = JSON.stringify(array1);
```

* JSON 이쁘게 출력 (pretty print JSON string)
```js
JSON.stringify([{"abc": 123}], null, 2);
console.log(JSON.stringify([{"abc": 123}], null, 2));
```

### sessionStorage, localStorage
* 브라우저에서 사용하는 DB로 쿠키와 비슷하다고 생각하면 된다.

### sessionStorage CRUD
```js
// storage Create
sessionStorage.setItem('ss1', 1);
sessionStorage.setItem('ss2', '이');
sessionStorage.setItem('ss3', undefined);

// storage Read
sessionStorage.getItem('ss1');
const ss1 = sessionStorage.getItem('ss1');
const ss2 = sessionStorage.getItem('ss2');
const ss3 = sessionStorage.getItem('ss3');

// storage Update
sessionStorage.setItem('ss1', [1, 2, 3]);
sessionStorage.setItem('ss2', {
  key1: 'value1'
});
sessionStorage.setItem('ss3', function() {});

// storage Delete
sessionStorage.removeItem('ss1');
sessionStorage.clear();
```
* sessionStorage 저장 기간: 새로고침 해도 남아 있지만, 해당 탭이 닫히면 사라진다.
* localStorage 저장 기간: 영구 보관이 기본이나, 브라우저에 따라 모바일 환경에 따라 다르다. 용량은 5MB까지
* ❔ Console 창에서 `sessionStorage`를 `localStorage` 변경하여 실행 해보기
* ❔ `localStorage.setItem('ss1', 1);`만 실행후 `새 탭`을 열고 `localStorage`가 남아 있는지 확인

<!--
### Prototype과 toString()
```js
const a = {};
a.__proto__.toString = function() {
  return 123;
};
a.toString();
String(a);
String(a.toString());
```
-->

### JSON을 사용해 sessionStorage에 배열 넣기
```js
const storageGet = sessionStorage.getItem('array2');
const storageLogical = storageGet || '[1, 2, 3]';
const array2 = JSON.parse(storageLogical);
const plus1 = array2.length + 1;
array2.push(plus1);
const storageSet = JSON.stringify(array2);
sessionStorage.setItem('array2', storageSet);
```

#### sessionStorage, localStorage 실습
* [데모](https://ovdncids.github.io/javascript-curriculum/usersStorage.html)
* usersStorage.html <- https://raw.githubusercontent.com/ovdncids/javascript-curriculum/master/docs/usersFunction.html

```diff
- const users = [];
```
```js
const usersGet = sessionStorage.getItem('users');
const usersLogical = usersGet || '[]';
const users = JSON.parse(usersLogical);
```

* usersCreate, usersDelete, usersUpdate에 추가
```js
const usersSet = JSON.stringify(users);
sessionStorage.setItem('users', usersSet);
```

* ❔ 문제: 리팩토링
  ```
  1. `usersCreate`, `usersDelete`, `usersUpdate`에 추가된 `공통 부분`을 `함수`로 만들고, `usersSet` `Script 상수`에 넣고
  2. `usersCreate`, `usersDelete`, `usersUpdate`에서 `usersSet` 실행 시키기
  ```
* <details><summary>정답</summary>

  ```js
  const usersSet = function() {
    const usersSet = JSON.stringify(users);
    sessionStorage.setItem('users', usersSet);
  };
  ```
  ```diff
  - const usersSet = JSON.stringify(users);
  - sessionStorage.setItem('users', usersSet);
  + usersSet();
  ```
  * ❕ 공통적으로 반복되는 부분을 함수로 만드는 작업(내부 구조 개선)을 리팩토링(Refactoring)이라 한다.
</details>

* VSCode 서로 다른 파일 비교하는 방법 설명

### document.write, location
document.html
```js
debugger;
document.write('documentWrite1');
document.write('documentWrite2');
document.write('documentWrite3');
document.writeln('documentWrite4');
document.writeln('documentWrite5');
document.writeln('documentWrite6<br>');
document.writeln('documentWrite7');
document.writeln('documentWrite8');
document.writeln('documentWrite9');
// window.location.reload();
// window.location.href = 'https://naver.com';
// window.history.back();
```

#### document.write, location 실습
* [데모](https://ovdncids.github.io/javascript-curriculum/usersDocumentWrite.html)
* documentWrite.html <- https://raw.githubusercontent.com/ovdncids/javascript-curriculum/master/docs/usersStorage.html

* usersRead 추가
```js
for (let index in users) {
  document.writeln(users[index]);
}
```
* usersCreate, usersDelete, usersUpdate에 추가
```js
window.location.reload();
```
* ❔ `데모`와 동일하게 만드려면?

## document 객체
### html 태그
html/index.html

#### html 기본 구조 만들기
```sh
!키 입력 후 탭키 누르기
```

#### .js 파일 부르기 (head 태그 안에 넣기)
```html
<script>
console.time('index1.js');
console.time('index2.js');
console.time('index3.js');
console.time('html bottom');
</script>
<script src="./index1.js"></script>
<script src="./index2.js"></script>
<script src="./index3.js"></script>
```

#### javascript 실행 순서 확인 하기
html/index1.js
```js
// debugger;
console.timeEnd('index1.js');
```

html/index2.js
```js
// debugger;
console.timeEnd('index2.js');
```

html/index3.js
```js
// debugger;
console.timeEnd('index3.js');
```

html/index.html (가장 아래에)
<!--
```html
<body>
  <script>
  // debugger;
  console.timeLog('html bottom');
  </script>
</body>
```
-->
```html
<script>
// debugger;
console.timeEnd('html bottom');
</script>
```

* ❕ Network 탭 설명
* ❕ `싱글 쓰레드(Single Thread)`, `멀티 쓰레드(Multi Thread)` 설명
* ❕ `동기(Sync)`, `비동기(Async)` 설명

```diff
- <script src="./index1.js"></script>
+ <script async src="./index1.js"></script>
```
* Network 탭에서 `No throttling`에서 `Slow 3G`로 변경 후 확인
* ❕ 따라서 `async`는 사용하지 않는다.

```diff
- <script async src="./index1.js"></script>
+ <script defer src="./index1.js"></script>
```
* ❕ `defer` 설명
* ❔ `defer` 다른 `.js` 파일에 적용 한다면
* ❔ `debugger;` 풀고, `index1.js`, `index2.js` 파일에서 `const test1 = 1;` 1번씩 사용한다면

### form 태그
* 주로 구글이나 네이버의 검색창에 사용 된다.

#### form 태그 넣기 (body 태그 안에 넣기)
form/usersForm.html
```html
<form method="get" action="./usersForm.html">
  <input type="text" name="input-text" value="초기값" placeholder="명령을 입력 하세요.">
  <input type="hidden" name="input-hidden" value="숨겨진값">
  <input type="submit" value="전송">
</form>
```
* `get` 메소드 설명 (포털 사이트에서 검색 후에 URL 주소를 복사해서, 다른 사람과 공유 할때 유용 하다. CRUD중 Read에 속함)
* `post` 메소드 설명 (CRUD중 Create, Update, Delete를 처리함)
* ❔ `action`을 `https://naver.com`으로 바꾼다면

#### onsubmit 메소드 추가
```diff
- <form method="get" action="./usersForm.html">
+ <form method="get" onsubmit="debugger; console.log(event, this); event.preventDefault();">
```
* ❔ `event.target === this` 같은지 확인 (Console 창에서 확인)
* ❔ `method` 확인
* ❔ `method === this.method` 같은지 확인
* `event.preventDefault();` 설명
<!--
<form abc="123"> `abc`와 같이 기본 속성이 아닌 경우, `this.abc`에 값이 들어가지 않는다.
-->

#### form 태그 실습
* [데모](https://ovdncids.github.io/javascript-curriculum/form/usersForm.html)
* form/usersForm.js <- https://raw.githubusercontent.com/ovdncids/javascript-curriculum/master/docs/usersDocumentWrite.html
* ❕ `.html` 파일을 `.js`로 수정

```diff
- <form method="get" onsubmit="debugger; console.log(event, this); event.preventDefault();">
+ <form onsubmit="usersSubmit(event, this);">
```

#### .js 파일 부르기 (head 태그 안에 넣기)
```html
<script src="./usersForm.js"></script>
```

#### form/usersForm.js
```diff
- window.location.reload();
```
```js
const usersSubmit = function(event, form) {
  const inputTextObject = form['input-text'];
  try {
    const evalReturn = eval(inputTextObject.value);
    console.log(evalReturn);
  } catch(error) {
    console.error(error);
    alert(error);
    event.preventDefault();
  }
};
```
* `eval` [설명](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/eval) (Console 창에서 `eval` 함수 확인)
* ❔ `eval` 대신 `JSON.parse`를 사용한다면

#### Input.value CRUD
* `inputTextObject` 상수에서 `break point` 걸고, Console 창에서 실행
```js
// Input 객체 Create
inputTextObject.value = 'abc';

// Input 객체 Read
inputTextObject.value;
inputTextObject.a1 = inputTextObject.value;
inputTextObject.a2 = inputTextObject.value;

// Input 객체 Update
inputTextObject.value = 'usersDelete(0)';

// Input 객체 Delete
delete inputTextObject.value;
inputTextObject.value = '';
```

### pre 태그, document.getElementById
form/usersForm.html
```diff
- <script src="./usersForm.js"></script>
+ <script defer src="./usersForm.js"></script>
```
```html
<pre id="tag-pre"></pre>
```

form/usersForm.js
```diff
- for (let index in users) {
-   document.writeln(users[index]);
- }
```
```js
const tagPre = document.getElementById('tag-pre');
for (let index in users) {
  let innerHTML = tagPre.innerHTML + users[index];
  innerHTML += '\n';
  tagPre.innerHTML = innerHTML;
}
```
* ❔ `defer`를 뺀다면
* ❔ 문제: 다음을 한줄로 표현 하라
  ```js
  let innerHTML = tagPre.innerHTML + users[index];
  innerHTML += '\n';
  tagPre.innerHTML = innerHTML;
  ```
* <details><summary>정답</summary>

  ```js
  tagPre.innerHTML += users[index] + '\n';
  ```
</details>

#### Pre.innerHTML CRUD
* Console 창에서 실행
```js
const tagPre = document.getElementById('tag-pre');

// Pre 객체 Create
tagPre.innerHTML = 'abc';

// Pre 객체 Read
tagPre.innerHTML;
tagPre.a1 = tagPre.innerHTML;
tagPre.a2 = tagPre.innerHTML;

// Pre 객체 Update
tagPre.innerHTML = 'usersDelete(0)';

// Pre 객체 Delete
delete tagPre.innerHTML;
tagPre.innerHTML = '';
```

### Query string - get
* 주로 `검색`, `링크 공유`, `뒤로가기` 했을때 페이지 유지에 사용 된다.
* CRUD 중 `Read`에 해당 한다.
```js
const queryString = new URLSearchParams(window.location.search);
const nameText = queryString.get('input-text');
```
* ❕ `new` 키워드는 `클래스`를 바탕으로 `오브젝트`를 만든다.
* <details><summary>클래스</summary>

  ```js
  class Class1 {
    v1 = '멤버 변수';
    m1 = function() {
      return '멤버 메소드';
    };
  }
  const class1 = new Class1();
  const c1 = class1.v1;
  const c2 = class1.m1();
  ```
  * ❔ `new` 키워드를 뺀다면
  * ❔ `Class1.v1`, `Class1.m1` 확인해 보기
</details>

### document.getElementsByName
```js
const inputTextObjects = document.getElementsByName('input-text');
const inputTextObject = inputTextObjects[0];
```

* ❔ 문제: 위의 두줄을 한줄로 표현 하라 (`inputTextObjects` 상수는 삭제)
* <details><summary>정답</summary>

  ```js
  const inputTextObject = document.getElementsByName('input-text')[0];
  ```
  * ❕ `document.getElementsByName` 메소드는 항상 `배열`을 반환한다.
</details>

* ❔ 문제: `input box`의 `value`값을, `Query string`의 `input-text`로 넣기
* <details><summary>정답</summary>

  ```js
  inputTextObject.value = nameText;
  ```
</details>

### Query string - getAll
```js
const inputHiddenList = queryString.getAll('input-hidden');
const inputHidden = inputHiddenList[0];
```

### focus, blur
```js
inputTextObject.focus();
// inputTextObject.blur();
```

### 새로고침 없이 회원 CRUD
* [데모](https://ovdncids.github.io/javascript-curriculum/form/usersNoRefresh.html)
* form/usersNoRefresh.html <- https://raw.githubusercontent.com/ovdncids/javascript-curriculum/master/docs/form/usersForm.html
* form/usersNoRefresh.js <- https://raw.githubusercontent.com/ovdncids/javascript-curriculum/master/docs/form/usersForm.js

#### Read
form/usersNoRefresh.html
```diff
- <script defer src="./usersForm.js"></script>
+ <script defer src="./usersNoRefresh.js"></script>
```

form/usersNoRefresh.js
``` diff
- for (let index in users) {
-   tagPre.innerHTML += users[index] + '\n';
- }
```
```js
tagPre.innerHTML = '';
for (let index in users) {
  tagPre.innerHTML += '<input type="text" name="users-name" value="' + users[index] + '">';
  tagPre.innerHTML += '<button onclick="usersDelete(' + index + ')">Delete</button>';
  tagPre.innerHTML += '\n';
}
console.log('Read', users);
```

#### Delete
``` diff
const usersDelete = function(index) {
- return 'Deleted';
+ return usersRead();
```

#### Update
form/usersNoRefresh.js
```js
tagPre.innerHTML += '<button onclick="usersUpdate(' + index + ')">Update</button>';
```

```diff
- const usersUpdate = function(index, user) {
-   users[index] = user;
-   usersSet();
-   return 'Updated';
- };
```
```js
const usersUpdate = function(index) {
  const name = ❔ 문제
  users[index] = name;
  usersSet();
  return usersRead();
};
```

#### Create
form/usersNoRefresh.html
```diff
- <form onsubmit="usersSubmit(event, this);">
+ <form onsubmit="event.preventDefault(); usersCreate(this);">
```

form/usersNoRefresh.js
```diff
- const usersCreate = function(user) {
-   users.push(user);
-   usersSet();
-   return 'Created';
- };
```
```js
const usersCreate = function(form) {
  const inputTextObject = form['input-text'];
  users.push(❔ 문제);
  usersSet();
  return usersRead();
};
```
* `usersSubmit` 메소드 삭제
* ❔ 문제: `create` 후 입력된 `input box`의 문자 지우기

### div 태그 복사
form/usersNoRefresh.html

* div 태그 설명
```diff
- <pre id="tag-pre"></pre>
```
```html
<div id="tag-div-parent"></div>
<div style="display: none;">
  <div id="tag-div-child">
    <input type="text" name="users-name" value="">
    <button name="users-update" onclick="usersUpdate(index)">Update</button>
  </div>
</div>
```

form/usersNoRefresh.js
```diff
- const usersRead (삭제)
```
```js
const usersRead = function() {
  const tagDivParent = document.getElementById('tag-div-parent');
  tagDivParent.innerHTML = '';
  const tagDivChild = document.getElementById('tag-div-child');
  for (let index in users) {
    const newDivChild = tagDivChild.cloneNode(true);
    tagDivParent.appendChild(newDivChild);
  }
  console.log('Read', users);
  return users;
};
```
<!--
`const tagDivChild`가 `tagDivParent.innerHTML = ''`; 보다 아래 있는 이유
`tagDivParent.innerHTML = ''` 자식을 지운 후에는 'tag-div-child'가 항상 템플릿 하나만 남게 된다.
-->

#### DOM 객체 접근하기
form/usersNoRefresh.js
```js
const usersNameObject = document.getElementsByName('users-name')[index];
const usersUpdateObject = document.getElementsByName('users-update')[index];
usersNameObject.value = users[index];
usersUpdateObject.index = index;
```
* ❕ `onclick` 안에 `index` 설명
* ❔ 문제: `Delete 버튼` 구현하기

### 회원 정보에 나이 추가
* [데모](https://ovdncids.github.io/javascript-curriculum/form/usersObject.html)

#### Create
form/usersNoRefresh.html
```diff
- <input type="text" name="input-text" value="" placeholder="Name">
- <input type="hidden" name="input-hidden" value="숨겨진값">
```
```html
<input type="text" name="user-name" value="" placeholder="Name">
<input type="text" name="user-age" value="" placeholder="Age">
```

form/usersNoRefresh.js
```diff
- const inputTextObject = form['input-text'];
- users.push(inputTextObject.value);
- inputTextObject.value = '';
```
```js
const userNameObject = form['user-name'];
const userAgeObject = form['user-age'];
users.push({
  name: userNameObject.value,
  age: userAgeObject.value
});
userNameObject.value = '';
userAgeObject.value = '';
```

#### Read
form/usersNoRefresh.html
```html
<input type="text" name="users-age" value="">
```

form/usersNoRefresh.js
```js
const usersAgeObject = document.getElementsByName('users-age')[index];
```
```diff
- usersNameObject.value = users[index];
```
```js
usersNameObject.value = users[index].name;
usersAgeObject.value = users[index].age;
```

#### Update
* ❔ 문제: Update 구현하기
* <details><summary>정답</summary>

  ```js
  const age = document.getElementsByName('users-age')[index].value;
  ```
  ```diff
  - users[index] = name;
  ```
  ```js
  users[index] = {
    name: name,
    age: age
  };
  ```
</details>
