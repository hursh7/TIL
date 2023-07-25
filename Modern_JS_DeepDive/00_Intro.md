# Intro

# **구문(Syntax)과 의미(Semantics)**

아래의 예제는 **구문(문법적)** 상으로는 문제가 없지만 **의미** 적으로는 옳지 않다.

```jsx
const number = "string";
console.log(number * number); // NaN
```

- 개발자는 결국, 프로그래밍 언어가 제공하는 문법을 적절히 사용하여 `변수를 통해 값을 저장하고 참조`하며
- 연산자로 `값을 연산, 평가`하고
- 조건문과 반복문에 의한 `흐름제어로 코드의 실행 순서를 제어`하고
- 함수로 `재사용 가능한 문의집합을 만들며`
- 객체, 배열 등으로 `자료를 구체화`한다.

# 자바스크립트 발전 과정

### **초기 자바스크립트**

- 웹 페이지의 보조 적인 기능을 수행하기 위해 한정적인 용도로만 사용
- 대부분 로직은 웹 서버에서 실행
- 브라우저는 단지, 서버로부터 전달 받은 HTML & CSS를 단순히 렌더링하는 수준이었다.

Ajax(Asynchronous Javascript and XML)

- 자바스크립트를 이용해 서버와 브라우저가 비동기 방식으로 데이터를 교환할 수 있는 통신 기능
- `XMLHttpRequest` 라는 이름으로 등장
- Ajax 이전의 웹페이지는 완전한 HTML 코드를 서버로부터 전송받아 웹페이지를 렌더링하는 방식으로 동작
    - 화면 전환 시 서버로 부터 새로운 HTML을 전송 받아 웹페이지 전체를 리 렌더링
    - 변경할 필요 없는 부분까지 리 렌더링 되면 불필요한 데이터 통신이 발생하며 성능에 좋지 않고 화면이 순간적으로 깜빡이는 현상이 발생한다.
- Ajax 등장 이후
    - 서버로부터 필요한 데이터만 전송 받아 변경해야 하는 부분만 `한정적으로 렌더링하는 방식이 가능`해졌다.
    - 웹 브라우저에서도 데스크톱 애플리케이션과 유사한 빠른 성능과 부드러운 화면 전환이 가능해졌다.

### **JQuery**

DOM(Document Object Model)을 더욱 쉽게 제어하고, **크로스 브라우징** 이슈를 어느 정도 해결해준 자바스크립트 라이브러리 

### V8 자바스크립트 엔진

자바스크립트가 데스크톱 애플리케이션과 유사한 사용자 경험을 제공할 수 있는 웹 애플리케이션 프로그래밍 언어로 정착 되었다.
과거 웹 서버에서 수행되던 로직 들이 대거 클라이언트(브라우저)로 이동하며 프론트엔드 영역이 주목 받는 계기로 작용했다.

# 브라우저 vs Node.js

`브라우저`와 `Node.js` 둘 다 자바스크립트 엔진을 내장하고 있다.

- 브라우저: HTML, CSS, JS를 실행해 웹페이지를 브라우저 화면에 렌더링 하는 것이 주 목적이다.
    - `DOM API` 제공
- Node.js 는 브라우저 외부에서 자바스크립트 실행 환경을 제공 하는 것이 주 목적이다.
    - `DOM API`를 제공하지 않음 (브라우저 외부 환경에서는 HTML 요소를 파싱해서 객관화한 DOM을 직접 다룰 필요가 없기 때문이다.)
    - `FileRead` 제공

💡 따라서, 브라우저와 Node.js 는 공통적인 ECMAScript를 실행할 수 있지만, 이외에 추가로 제공하는 기능은 호환하지 않는다.

# 자바스크립트와 ECMAScript

### 자바스크립트

- `명령형(Imperative)`, `함수형(Functional)`, `프로토타입 기반(Prototype-based) 객체지향 프로그래밍`을 지원하는 `멀티 패러다임 프로그래밍 언어`
- 자바스크립트는 클래스 기반 객체지향 언어보다 효율적이면서 강력한 `프로토타입 기반의 객체지향 언어`
- 브라우저가 별도로 지원하는 클라이언트 사이드 Web API
(즉, `DOM`, `BOM`, `Canvas`, `XMLHttpRequest`, `fetch`, `requestAnimationFrame`, `SVG`, `Web Storage`, `Web Component`, `Web Worker` 등을 아우르는 개념)

### **ECMAScript**

자바스크립트 표준으로, 프로그래밍 언어의 값, 타입, 객체와 프로퍼티, 함수 표준 빌트인 객체 등 핵심 문법을 규정한 것.

### **ECMAScript 버전별 특징**

💡 ES5 → ES6로 업데이트하면서 많은 변화가 발생했다.

| 버전 | 특징 | 출시 연도 |
| --- | --- | --- |
| ES1 | ... | 1997 |
| ES2 | ... | 1998 |
| ES3 | 정규 표현식, try - catch | 1999 |
| ES5 | HTML5와 함께 출현한 표준안, JSON, strict mode, 접근자 프로퍼티 ,프로퍼티 어트리뷰트 제어 ,향상된 배열 조작 기능(forEach, map, filter, reduce, some, every) | 2009 |
| ES6(ECMAScript 2015) | let/const, 클래스, 화살표 함수, 템플릿 리터럴, 디스트럭처링 할당, 스프레드 문법, rest 파라미터, 심벌, 프로미스, Map/Set, 이터러블 , for - of, 제너레이터, Proxy, 모듈 import/export | 2015 |
| ES7(ECMAScript 2016) | 지수(**)연산자, Array.prototype.includes, String.prototype.includes | 2016 |
| ES8(ECMAScript 2017) | async/await, Object 정적 메서드 (Object.values, Object.entriesm Object.getOwnPropertyDescriptors) | 2017 |
| ES9(ECMAScript 2018) | ... | 2018 |
| ES10(ECMAScript 2019) | ... | 2019 |
| ES11(ECMAScript 2020) | ... | 2020 |