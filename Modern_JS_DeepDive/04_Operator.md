# 연산자(Operator)

연산자는 하나 이상의 표현식을 대상으로 산술, 할당, 비교, 논리, 타입, 지수 연산등을 수행해 하나의 값을 만든다. 이떄 연산의 대상을 `피연산자 operand` 라 한다. 피연산자는 값으로 평가 될 수 있는 표현식이어야 한다.

```jsx
// 산술 연산자
5 * 4 // 20
// 문자열 연결 연산자
'my name is' + 'Jun'
// 할당 연산자
color = 'black'
// 비교 연산자
3 > 5 // false
// 논리 연산자
true && false // false
// 타입 연산자
typeof 'Hi' // string
```

- 피연산 자는 ‘값’ 이라는 명사 역할
- 연산자는 “피연산자를 연산하여 새로운 값을 만든다” 라는 동사 역할

# 이항 산술 연산자 (binary)

모든 이항 산술 연산자는 피연산자의 값을 변경하는 `부수 효과 side effect` 가 없다.

# 단항 산술 연산자 (unary)

- `증가/감소(++/--) 연산자`는 피연산자의 값을 변경하는 부수 효과가 있다.
- `증가/감소(++/--) 연산자` 는 위치에 의미가 있다.
    - 피연산자 앞에 위치한 전위 증가/감소 연산자는 먼저 피연산자의 값을 증가/감소 시킨 후, 다른 연산을 수행한다.
    - 피연산자 뒤에 위치한 전위 증가/감소 연산자는 먼저 다른 연산을 수행한 후, 피연산자의 값을 증가/감소 시킨다.
    
    ```jsx
    var x = 5, result;
    
    // 선할당 후증가(postfix increment operator)
    result = x++;
    console.log(result, x); // 5 6
    
    // 선증가 후할당(prefix increment operator)
    result = ++x;
    console.log(result, x); // 7 7
    ```
    
- 숫자 타입이 아닌 피연산자에 +/- 단항 연산자를 사용하면 피연산자를 숫자 타입으로 변환하여 반환한다.
- 이는 피연산자를 변경하는 것은 아니고 숫자 타입으로 변환한 값을 생성해서 반환 하는 것이므로 부수 효과는 없다.

```jsx
// 문자열을 숫자로 타입 변환
-'10' // -10
// 불리언 값을 숫자로 타입 변환
-true // -1
// 문자열은 숫자로 타입 변환할 수 없으므로 NaN을 반환
-'Hello'; // NaN
```

# 문자열 연결 연산자

```jsx
// 문자열 연결 연산자
'1' + 2; // '12'

// true는 1로 타입 변환
1 + true // 2

// null은 0으로 타입 변환
1 + null // 1

// undefined는 숫자로 타입 변환되지 않는다.
+undefined // NaN
1 + undefined // NaN
```

- 위 예제에서 주목할 것은 개발자의 의도와는 상관없이 자바스크립트 엔진에 의해 암묵적으로 타입이 자동 변환 되기도 한다는 것이다.
- 이를 `암묵적 타입 변환 implicit coercion` 또는 `강제 변환 type coercion` 이라고 한다.

# 비교 연산자

`동등비교(loose equailty)` 와 `일치비교(strict equality)` 연산자는 엄연히 다르다.

`동등 비교(==)` 연산자는 좌항과 우항의 피연산자를 비교할 때 먼저 암묵적 타입 변환을 통해 타입을 일치시킨 후 같은 값인지 비교한다.

```jsx
'0' == ''; // false
0 == ''; // true
0 == '0'; // true
false == 'false'; // false
false == '0'; // true
false == null; // false
false == undefined; // false
```

이처럼 `동등 비교(==)` 연산자는 예측하기 어려운 결과를 만들어낸다. 대신 `일치 비교(===)` 연산자를 사용하는 것이 좋다. `일치 비교` 연산자는 좌항과 우항의 피연산자가 타입과 값이 모두 같은 경우에 한하여 true를 반환한다. 다시 말해 , 암묵적 타입 변환을 하지 않고 값을 비교한다.

- 일치 비교 연산자에서 주의할 것은 NaN이다.
- NaN은 자신과 일치하지 않는 유일한 값이다.
- 따라서 숫자가 NaN인지 조사하려면 빌트인 함수 `Number.isNaN` 을 사용한다.

```jsx
// NaN은 자신과 일치하지 않는 유일한 값이다.
NaN === NaN; // false

// 지정한 값이 NaN인지 확인하고 그 결과를 불리언 값으로 반환한다.
Number.isNaN(NaN); // true
Number.isNaN(10); // false
Number.isNaN(1 + undefined); // true
```

숫자 0도 주의하자. `양의 0`과 `음의 0` 은 일치 / 동등 비교 시 모두 true이다.

ES6에서 도입된 `Object.is` 메서드는 다음과 같이 예측 가능한 정확한 비교 결과를 반환한다.

```jsx
-0 === +0; // true
Object.is(-0, +0); // false

NaN === NaN; // false
Object.is(NaN, NaN); // true
```

# 삼항 조건 연산자

- `삼항 조건 연산자 ternary operator` 는 값으로 평가할 수 있는 표현식인 문이다. `if ... else` 문과 달리 값처럼 다른 표현식의 일부가 될 수 있어 매우 유용하다.
- 하지만 조건에 따라 수행해야 할 문이 하나가 아니라 여러 개라면 `if ... else` 문의 가독성이 더 좋다.

# 논리 연산자

`|| 논리합 OR` 

`&& 논리곱 AND`

`! 부정 NOT`

# typeof 연산자

typeof 연산자는 피연산자의 데이터 타입을 `문자열로 반환` 한다.

`string`, `number`, `boolean`, `undefined`, `symbol`, `object`, `function` 

- null을 반환하는 경우는 없다.

```jsx
typeof Symbol() // symbol
typeof null // object
typeof [] // object
typeof [] // object
typeof {} // object
typeof new Date() // object
typeof /test/gi // object
typeof function () {} // function 
```

- typeof 연산자로 null 값을 연산하면 ‘null’ 이 아닌 ‘object’를 반환한다. 이것은 자바스크립트의 첫 번째 버전의 버그다. 하지만 기존 코드에 영향을 줄 수 있기 때문에 아직 까지 수정되지 못하고 있다.

```jsx
var foo = null;

typeof foo === null // false 
foo === null // true
```

- 또 하나 주의할 점은 선언하지 않은 식별자를 typeof 연산자로 연산해 보면 `ReferenceError` 가 발생하지 않고 `undefined` 를 반환한다.

### 지수 연산자

ES7에서 도입된 지수 연산자는 좌항의 피연산자를 `밑 base` 으로, 우항의 피연산자를 `지수 exponent` 로 거듭 제곱 하여 숫자 값을 반환한다.

```jsx
2 ** 2 // 4
2 ** 0 // 1
2 ** -2 // 0.25
```

지수 연산자가 도입되기 이전에는 `Math.pow` 메서드를 사용했다.

```jsx
2 ** (3 ** 2) // 512
Math.pow(2, Math.pow(3, 2)); // 512

// 음수를 거듭제곱의 밑으로 사용하려면 괄호로 묶어야 한다.
-5 ** 2; // SyntaxError
(-5) ** 2; // 25

// 할당 연산자
var num = 5;
num **= 2; // 25

// 지수 연산자는 이항 연산자 중에서 우선순위가 가장 높다.
2 * 5 ** 2 // 50
```

# 그 외 연산자

| 연산자 | 개요 |
| --- | --- |
| ?. | 옵셔널 체이닝 연산자 |
| ?? | null 병합 연산자 |
| delete | 프로퍼티 삭제 |
| new | 생성자 함수를 호출할 때 사용하여 인스턴스 생성 |
| instanceof | 좌변의 객체가 우변의 생성자 함수와 연결된 인스턴스인지 판별 |
| in | 프로퍼티 존재 확인 |