# JS MEMO

## 동등 연산자 vs 일치 연산자

동등 연산자(equality operator) `==`은 `0`과 `false`를 구별 X

```javascript
console.log(0 == false); // true
```

피연산자가 빈 문자열일 때도 같은 문제가 발생

```javascript
console.log('' == false); // true
```

### **Why?**

- 동등 연산자 `==`가 형이 다른 피연산자를 비교할 때, 피연산자를 숫자형으로 바꾸기 때문에 발생

- 빈 문자열과 `false`는 숫자형으로 변환하면 0

### **일치 연산자를 사용하자**

- 일치 연산자는 엄격한 동등 연산자

    - 자료형의 동등 여부까지 검사하기 때문에 피연산자 `a`와 `b`가 다른 자료형이면 `false` 반환

    ```javascript
    console.log(0 === false); // false, 피연산자의 형이 다르기 때문에
    ```

> '불일치' 연산자 `!===`는 부등 연산자 `!=`의 엄격한 버전

#### null과 undefined을 활용해보자.

```javascript
console.log(null === undefined); // false

console.log(null == undefined); // true
```

### null vs 0

```javascript
console.log(null > 0); // false
console.log(null == 0); // false
console.log(null >= 0); // true
```

출력 결과가 다른 이유는 동등 연산자와 기타 비교 연산자의 동작 방식이 바르기 때문임

기타 비교 연산자의 동작 원리에 따라 `null`이 숫자형으로 변환돼 `0`이 되기 때문임

> `undefined`는 비교 불가능

---

## switch/case 문

`switch` 문과 `case` 문은 모든 형태의 표현식을 인수로 받음

```javascript
let a = "1";
let b = 0;

switch (+a) {
    case b + 1:
        console.log("표현식 +a는 1, 표현식 b+1는 1이므로 이 코드가 실행됩니다.");
        break;

    default:
        console.log("이 코드는 실행되지 않습니다.");
}
```

표현식 +a를 평가하면 1

이 값은 첫 번째 `case` 문의 표현식 `b + 1`을 평가한 값과 일치하므로 첫 번째 `case` 문이 실행됨

### "case" 문 묶기

```javascript
let a = 3;

switch (a) {
  case 4:
    alert('계산이 맞습니다!');
    break;

  case 3: // (*) 두 case문을 묶음
  case 5:
    alert('계산이 틀립니다!');
    alert("수학 수업을 다시 들어보는걸 권유 드립니다.");
    break;

  default:
    alert('계산 결과가 이상하네요.');
}
```

`case 3`과 `case 5`는 동일한 코드를 실행하므로 두 `case` 문을 묶어서 코드를 줄일 수 있음

## 지역 변수 vs 외부 변수 vs 매개 변수

### 지역 변수

함수 내에서 선언한 변수인 지역 변수는 함수 안에서만 접근 가능

```javascript
function showMessage() {
  let message = "안녕하세요!"; // 지역 변수

  alert( message );
}

showMessage(); // 안녕하세요!

alert( message ); // ReferenceError: message is not defined (message는 함수 내 지역 변수이기 때문에 에러가 발생합니다.)
```

### 외부 변수

함수 내부에서 함수 외부의 변수인 외부 변수에 접근 가능(수정도 가능)

```javascript
let userName = 'John';

function showMessage() {
  userName = "Bob"; // (1) 외부 변수를 수정함

  let message = 'Hello, ' + userName;
  alert(message);
}

alert( userName ); // 함수 호출 전이므로 John 이 출력됨

showMessage();

alert( userName ); // 함수에 의해 Bob 으로 값이 바뀜
```

외부 변수는 지역 변수가 없는 경우에만 사용 가능

함수 내부에 외부 변수와 동일한 이름을 가진 변수가 선언되었다면, 내부 변수는 외부 변수를 가리게 됨

```javascript
let userName = 'John';

function showMessage() {
  let userName = "Bob"; // 같은 이름을 가진 지역 변수를 선언합니다.

  let message = 'Hello, ' + userName; // Bob
  alert(message);
}

// 함수는 내부 변수인 userName만 사용합니다,
showMessage();

alert( userName ); // 함수는 외부 변수에 접근하지 않습니다. 따라서 값이 변경되지 않고, John이 출력됩니다.
```

### 매개 변수

임의의 데이터를 함수 안에 전달 가능

매개 변수는 **인자**라고 불리기도 함

```javascript
function showMessage(from, text) {  // 인자: from, text

  from = '*' + from + '*'; // "from"을 좀 더 멋지게 꾸며줍니다.

  alert( from + ': ' + text );
}

let from = "Ann";

showMessage(from, "Hello"); // *Ann*: Hello

// 함수는 복사된 값을 사용하기 때문에 바깥의 "from"은 값이 변경되지 않습니다.
alert( from ); // Ann
```

함수는 언제나 복사된 값을 사용하기 때문에 함수가 `from`을 변경하지만, 변경 사항은 외부 변수 `from`에 반영 X

- 매개 변수는 함수 선언 방식 괄호 사이에 있는 변수(선언 시 쓰이는 용어)

- 인수는 함수를 호출할 때 매개 변수에 전달되는 값(호출 시 쓰이는 용어)

즉, 함수 선언 시 매개 변수를 나열하게 되고, 함수를 호출할 땐 인수를 전달해 호출함

## 메서드와 this

메서드 내부에서 `this` 키워드를 사용하면 객체에 접근 가능

```javascript
let user = {
  name: "John",
  age: 30,

  sayHi() {
    // 'this'는 '현재 객체'를 나타냅니다.
    alert(this.name);
  }

};

user.sayHi(); // John
```

`user.sayHi()`가 실행되는 동안에 `this`는 `user`를 나타냄

`this`를 사용하지 않고 외부 변수를 참조해 객체에 접근하는 것도 가능

```javascript
let user = {
  name: "John",
  age: 30,

  sayHi() {
    alert(user.name); // 'this' 대신 'user'를 이용함
  }

};
```

## this

JS의 `this`는 동작 방식이 다름(모든 함수에 `this` 사용 가능)

아래와 같이 코드 작성 가능

```javascript
function sayHi() {
  alert( this.name );
}
```

`this` 값은 런타임에 결정됨

동일한 함수라도 다른 객체에서 호출했다면 'this'가 참조하는 값이 달라짐

```javascript
let user = { name: "John" };
let admin = { name: "Admin" };

function sayHi() {
  alert( this.name );
}

// 별개의 객체에서 동일한 함수를 사용함
user.f = sayHi;
admin.f = sayHi;

// 'this'는 '점(.) 앞의' 객체를 참조하기 때문에
// this 값이 달라짐
user.f(); // John  (this == user)
admin.f(); // Admin  (this == admin)

admin['f'](); // Admin (점과 대괄호는 동일하게 동작함)
```

## 생성자 함수

- 함수 이름의 첫 글자는 대문자로 시작

- 반드시 `new` 연산자를 붙여 실행

```javascript
function User(name) {
  this.name = name;
  this.isAdmin = false;
}

let user = new User("보라");

alert(user.name); // 보라
alert(user.isAdmin); // false
```

`new User(...)`를 써서 함수를 실행하면 아래와 같은 알고리즘이 동작

1. 빈 객체를 만들어 `this`에 할당

2. 함수 본문을 실행. `this`에 새로운 프로퍼티를 추가해 `this`를 수정

3. `this`를 반환

```javascript
function User(name) {
  // this = {};  (빈 객체가 암시적으로 만들어짐)

  // 새로운 프로퍼티를 this에 추가함
  this.name = name;
  this.isAdmin = false;

  // return this;  (this가 암시적으로 반환됨)
}
```

`let user = new User("보라");` 는 아래 코드를 입력한 것과 동일하게 동작

```javascript
let user = {
  name: "보라",
  isAdmin: false
};
```

## new.target

`new.target` 프로퍼티를 사용하면 함수가 `new`와 함께 호출되었는지 여부를 알 수 있음

`new`와 함께 호출한 경우 `new.target`은 함수 자체를 반환해줌

```javascript
function User() {
  alert(new.target);
}

// 'new' 없이 호출함
User(); // undefined

// 'new'를 붙여 호출함
new User(); // function User { ... }
```

## 콜백

JS는 호스트 환경이 제공하는 여러 함수를 사용하면 비동기 동작을 스케줄링 할 수 있음

`src`에 있는 스크립트를 읽어오는 함수 `loadScript(src)`를 예시로 비동기 동작 처리가 어떻게 일어나는지 살펴보기

```javascript
function loadScript(src) {
  // <script> 태그를 만들고 페이지에 태그를 추가합니다.
  // 태그가 페이지에 추가되면 src에 있는 스크립트를 로딩하고 실행합니다.
  let script = document.createElement('script');
  script.src = src;
  document.head.append(script);
}
```

함수 `loadScript(src)`는 `<script src='...'>`를 동적으로 만들고 이를 문서에 추가

브라우저는 자동으로 태그에 있는 스크립트를 불러오고, 로딩이 완료되면 스크립트를 실행

```javascript
// 해당 경로에 위치한 스크립트를 불러오고 실행함
loadScript('/my/script.js');
```

이 때 스크립트는 '비동기적으로' 실행됨 -> 로딩은 지금 당장 시작되더라도 실행은 함수가 끝난 후에야 되기 때문임

`loadScript(...)` 아래에 있는 코드들은 스크립트 로딩이 종료되는 걸 기다리지 않음

```javascript
loadScript('/my/script.js');
// loadScript 아래의 코드는
// 스크립트 로딩이 끝날 때까지 기다리지 않습니다.
// ...
```

`loadScript(...)`를 호출하자마자 내부 함수를 호출하면 원하는 대로 작동하지 않음

```javascript
loadScript('/my/script.js'); // script.js엔 "function newFunction() {…}"이 있습니다.

newFunction(); // 함수가 존재하지 않는다는 에러가 발생합니다!
```

에러는 브라우저가 스크립트를 읽어올 수 있는 시간을 충분히 확보하지 못했기 때문에 발생함

`loadScript`의 두 번째 인수로 스크립트 로딩이 끝난 후 실행될 함수인 `콜백(callback)` 함수를 추가

```javascript
function loadScript(src, callback) {
  let script = document.createElement('script');
  script.src = src;

  script.onload = () => callback(script);

  document.head.append(script);
}
```

새롭게 불러온 스크립트에 있는 함수를 콜백 함수 안에서 호출하면 원하는 대로 외부 스크립트 안의 함수를 사용 가능

```javascript
loadScript('/my/script.js', function() {
  // 콜백 함수는 스크립트 로드가 끝나면 실행됩니다.
  newFunction(); // 이제 함수 호출이 제대로 동작합니다.
  ...
});
```

이렇게 두 번째 인수로 전달된 함수는 원하는 동작이 완료되었을 때 실행

아래는 실제 존재하는 스크립트를 이용해 만든 예시

```javascript
function loadScript(src, callback) {
  let script = document.createElement('script');
  script.src = src;
  script.onload = () => callback(script);
  document.head.append(script);
}

loadScript('https://cdnjs.cloudflare.com/ajax/libs/lodash.js/3.2.0/lodash.js', script => {
  alert(`${script.src}가 로드되었습니다.`);
  alert( _ ); // 스크립트에 정의된 함수
});
```

이런 방식을 **'콜백 기반(callback-based)'** 비동기 프로그래밍이라고 함

무언가를 비동기적으로 수행하는 함수는 함수 내 동작이 모두 처리된 후 실행되어야 하는 함수가 들어갈 `콜백`을 인수로 반드시 제공해야 함

스크립트가 두 개 있는 경우, 두 스크립트를 순차적으로 불러오고 싶다면 콜백 안에서 두 번째 스크립트를 불러오면 됨

```javascript
loadScript('/my/script.js', function(script) {

  alert(`${script.src}을 로딩했습니다. 이젠, 다음 스크립트를 로딩합시다.`);

  loadScript('/my/script2.js', function(script) {
    alert(`두 번째 스크립트를 성공적으로 로딩했습니다.`);
  });

});
```

이렇게 중첩 콜백을 만들면 바깥에 위치한 `loadScript`가 완료된 후, 안쪽 `loadScript`가 실행됨

### 에러 핸들링

`loadScript` 에서 로딩 에러를 추적할 수 있게 개선

```javascript
function loadScript(src, callback) {
  let script = document.createElement('script');
  script.src = src;

  script.onload = () => callback(null, script);
  script.onerror = () => callback(new Error(`${src}를 불러오는 도중에 에러가 발생했습니다.`));

  document.head.append(script);
}
```

`loadScript`는 스크립트 로딩에 성공하면 `callback(null, script)`, 실패하면 `callback(error)`를 호출

```javascript
/// loadScript 사용법
loadScript('/my/script.js', function(error, script) {
  if (error) {
    // 에러 처리
  } else {
    // 스크립트 로딩이 성공했을 때
  }
});
```

이렇게 에러를 처리하는 방식은 **'오류 우선 콜백(error-first callback)'** 이라 부름

1. `callback`의 첫 번째 인수는 에러를 위해 남겨둠. 에러가 발생하면 이 인수를 이용해 `callback(err)`이 호출

2. 두 번째 인수(필요하면 인수를 더 추가할 수 있음)는 에러가 발생하지 않았을 때를 위해 남겨둠. 원하는 동작이 성공한 경우엔 `callback(null, result1, result2...)` 이런 식으로 호출

## 프라미스

`promise` 객체는 아래와 같은 문법으로 만들 수 있음

```javascript
let promise = new Promise(function(resolve, reject) {
  // executor (제작 코드, "가수")
});
```

`new Promise`에 전달되는 함수는 executor(실행자, 실행 함수)로 부름

executor는 자동으로 실행되는데 여기서 원하는 일이 처리됨

처리가 끝나면 executor는 처리 성공 여부에 따라 `resolve`나 `reject`를 호출

```javascript
let promise = new Promise(function(resolve, reject) {
  // 프라미스가 만들어지면 executor 함수는 자동으로 실행됩니다.

  // 1초 뒤에 일이 성공적으로 끝났다는 신호가 전달되면서 result는 '완료'가 됩니다.
  setTimeout(() => resolve("완료"), 1000);
});
```

1. executor는 `new Promise`에 의해 자동으로 그리고 즉각적으로 호출

2. executor는 인자로 `resolve`와 `reject` 함수를 받음. 이 함수들은 JS 엔진이 미리 정의한 함수이므로 개발자가 따로 만들 필요가 X. 다만, `resolve`나 `reject` 중 하나는 반드시 호출해야 함

  executor '처리'가 시작 된 지 1초 후, `resolve("done")` 이 호출되고 결과가 만들어짐

executor가 에러와 함께 약속한 작업을 거부하는 경우

```javascript
let promise = new Promise(function(resolve, reject) {
  // 1초 뒤에 에러와 함께 실행이 종료되었다는 신호를 보냅니다.
  setTimeout(() => reject(new Error("에러 발생!")), 1000);
});
```

1초후 `reject(...)`가 호출되면 `prromise`의 상태가 `"rejected"`로 변함

> executor는 보통 시간이 걸리는 일을 수행 -> 일이 끝나면 `resolve`나 `reject` 함수를 호출하는데, 이때 프라미스 객체의 상태가 변화

> 이행(resolved) 혹은 거부(rejected) 상태의 프라미스는 '처리된(settled)' 프라미스라고 부르며, 반대되는 프라미스로 '대기(pending)' 상태의 프라미스가 있음

## 소비자: then, catch, finally

### then

`then`은 프라미스가 처리되었을 때 처리 결과를 받아 처리하는 메소드

```javascript
promise.then(
  function(result) { /* 결과(result)를 다룹니다 */ },
  function(error) { /* 에러(error)를 다룹니다 */ }
);
```

`.then`의 첫 번째 인수는 프라미스가 이행되었을 때 실행되는 함수, 여기서 실행 결과를 받음

`.then`의 두 번째 인수는 프라미스가 거부되었을 때 실행되는 함수, 여기서 에러를 받음

```javascript
let promise = new Promise(function(resolve, reject) {
  setTimeout(() => resolve("완료!"), 1000);
});

// resolve 함수는 .then의 첫 번째 함수(인수)를 실행합니다.
promise.then(
  result => alert(result), // 1초 후 "완료!"를 출력
  error => alert(error) // 실행되지 않음
);
```

프라미스가 거부된 경우, 아래와 같이 두 번째 함수가 실행

```javascript
let promise = new Promise(function(resolve, reject) {
  setTimeout(() => reject(new Error("에러 발생!")), 1000);
});

// reject 함수는 .then의 두 번째 함수를 실행합니다.
promise.then(
  result => alert(result), // 실행되지 않음
  error => alert(error) // 1초 후 "Error: 에러 발생!"을 출력
);
```

작업이 성공적으로 처리된 경우만 다루고 싶다면 `.then`에 인수를 하나만 전달하면 됨

```javascript
let promise = new Promise(resolve => {
  setTimeout(() => resolve("완료!"), 1000);
});

promise.then(alert); // 1초 뒤 "완료!" 출력
```

### catch

`.catch`는 `.then(null, errorHandling)`과 동일

```javascript
let promise = new Promise((resolve, reject) => {
  setTimeout(() => reject(new Error("에러 발생!")), 1000);
});

// .catch(f)는 promise.then(null, f)과 동일하게 작동합니다
promise.catch(alert); // 1초 뒤 "Error: 에러 발생!" 출력
```

`.catch(f)`는 문법이 간결하다는 점만 빼고 `.then(null, f)`과 완벽하게 같음

### finally

프라미스가 처리되면(이행이나 거부) `f`가 항상 실행된다는 점에서 `.finally(f)` 호출은 `.then(f, f)`과 유사 -> 결과가 어떻든 마무리가 필요하면 `finally`가 유용함

```javascript
new Promise((resolve, reject) => {
  /* 시간이 걸리는 어떤 일을 수행하고, 그 후 resolve, reject를 호출함 */
})
  // 성공·실패 여부와 상관없이 프라미스가 처리되면 실행됨
  .finally(() => 로딩 인디케이터 중지)
  .then(result => result와 err 보여줌 => error 보여줌)
```

finally와 `then(f, f)`과의 차이점

1. `finally` 핸들러엔 인수가 X -> `finally`에선 프라미스가 이행되었는지, 거부되었는지 알 수 X(`finally`에선 절차를 마무리하는 '보편적' 동작을 수행하기 때문에 성공, 실패 여부를 몰라도 됨)

2. `finally` 핸들러는 자동으로 다음 핸들러에 결과와 에러를 전달
  
    result가 `finally`를 거쳐 `then`까지 전달되는 것을 확인해보자

```javascript
new Promise((resolve, reject) => {
  setTimeout(() => resolve("결과"), 2000)
})
  .finally(() => alert("프라미스가 준비되었습니다."))
  .then(result => alert(result)); // <-- .then에서 result를 다룰 수 있음
```

프라미스에서 에러가 발생하고 이 에러가 `finally`를 거쳐 `catch`까지 전달되는 것을 확인

```javascript
new Promise((resolve, reject) => {
  throw new Error("에러 발생!");
})
  .finally(() => alert("프라미스가 준비되었습니다."))
  .catch(err => alert(err)); // <-- .catch에서 에러 객체를 다룰 수 있음
```

3. `finally(f)`는 함수 `f`를 중복해서 쓸 필요가 없기 때문에 `.then(f, f)`보다 문법 측면에서 더 편리

## 프라미스 체이닝

순차적으로 처리해야 하는 비동기 작업이 여러 개 있을 때의 해결책 중 하나

```javascript
new Promise(function(resolve, reject) {

  setTimeout(() => resolve(1), 1000); // (*)

}).then(function(result) { // (**)

  alert(result); // 1
  return result * 2;

}).then(function(result) { // (***)

  alert(result); // 2
  return result * 2;

}).then(function(result) {

  alert(result); // 4
  return result * 2;

});
```

`result`가 `.then` 핸들러의 체인(사슬)을 통해 전달되는 점에서 착안

1. 1초 후 최초 프라미스가 이행됨 -> `(*)`

2. 이후 첫 번째 `.then` 핸들러가 호출됨 -> `(**)`

3. 2에서 반환한 값은 다음 `.then` 핸들러에 전달됨 -> `(***)`

프라미스 체이닝이 가능한 이유 = `promise.them`을 호출하면 프라미스가 반환되기 때문에 반환된 프라미스엔 당연히 `.then`을 호출할 수 있음

한편 핸들러가 값을 반환할 때엔 이 값이 프라미스의 `result`가 됨

## async와 await

`async`와 `await`는 프라미스를 더 쉽게 사용할 수 있게 해주는 문법

## async 함수

`async` 키워드를 함수 앞에 붙이면 해당 함수는 프라미스를 반환

```javascript
async function f() {
  return 1;
}
```

프라미스가 아닌 값을 반환하더라도 이행 상태의 프라미스(resolved promise)로 값을 감싸 이행된 프라미스가 반환되도록 함

```javascript
async function f() {
  return 1;
}

f().then(alert); // 1
```

명시적으로 프라미스를 반환하는 것도 가능

```javascript
async function f() {
  return Promise.resolve(1);
}

f().then(alert); // 1
```

`async`가 붙은 함수는 반드시 프라미스를 반환하고, 프라미스가 아닌 것은 프라미스로 감싸 반환

### await

```javascript
// await는 async 함수 안에서만 동작합니다.
let value = await promise;
```

`await`는 프라미스가 이행될 때까지 기다린 다음 결과를 반환

```javascript
async function f() {

  let promise = new Promise((resolve, reject) => {
    setTimeout(() => resolve("완료!"), 1000)
  });

  let result = await promise; // 프라미스가 이행될 때까지 기다림 (*)

  alert(result); // "완료!"
}

f();
```

함수를 호출하고, 함수 본문이 실행되는 도중에 `(*)`로 표시한 줄에서 실행이 잠시 '중단'되었다가 프라미스가 처리되면 실행이 재개됨 -> 프라미스 객체의 `result` 값이 변수 result에 할당 -> 따라서 위 예시를 실행하면 1초 뒤에 '완료!'가 출력됨

`await`는 말 그대로 프라미스가 처리될 때까지 함수 실행을 기다리게 만듦 -> 프라미스가 처리되면 그 결과와 함께 실행이 재개(프라미스가 처리되길 기다리는 동안엔 엔진이 다른 일을 할 수 있기 때문에 CPU 리소스를 낭비하지 않음)

`await`는 `then`과 비슷하지만, `then`과 달리 `await`는 `await`가 있는 줄에서 실행이 잠시 중단됨