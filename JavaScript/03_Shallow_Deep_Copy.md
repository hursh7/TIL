# Shallow Copy & Deep Copy

변수에 문자열, 숫자와 같은 원시형 데이터를 할당하게 되면 데이터 자체가 변수에 할당되지만, 
오브젝트를 변수에 할당하면 변수에는 오브젝트가 메모리에 들어있는 주소인, 참조값이 할당된다.  

```jsx
const num = 2; // num에는 숫자 2가 들어 있다.
const numObj = { num: 2 }; // numObj에는 x123 주소가 들어 있다. (x123)은 임의의 주소이다.
```

### 오브젝트를 가리키는 변수를 다른 변수에 할당하면?

```jsx
const a = { id: '1', count: 0 }; // x123
const b = { id: '2', count: 0 }; // x234
const c = b; // x234
// 메모리에 생성 되어진 오브젝트는 2개, 변수는 3개이다.
```

b변수를 c에 할당하니, b변수에 들어있던 참조값 x234가 c변수에 복사 되어져서 할당된다. 

이처럼 오브젝트는 값(Value) 자체가 아니라, 참조값(Reference)가 변수에 저장되기 때문에, b를 통해서든 c를 통해서든 오브젝트를 수정하면 서로 수정된 내용을 바로 적용 받게 된다.

(const라고 상수 변수로 저장해 두어도, 참조값 자체는 바꿀 수 없지만, 자체의 수정이 가능하다.)

### 배열의 경우

```jsx
const array = [ // x567
	{ id: '1', count: 0 }, // x123
	{ id: '2', count: 0 }, // x234
];
```

생성된 오브젝트는 id가 1인 오브젝트, id가 2인 오브젝트, 그리고 배열 자체의 오브젝트로 총 3개이다.

```jsx
const array = [ // x567
	{ id: '1', count: 0 }, // x123
	{ id: '2', count: 0 }, // x234
];
const array2 = array; // x567
const array3 = [...array]; // x999
// Spread Operator 를 이용한 새로운 배열 만들기 
// (array에 있는 모든 아이템들을 새로운 배열로 가지고 와서 새로운 배열을 만든다.)
```

Spread Operator는 배열 안의 모든 오브젝트 내용들을 일일이 복사해서 새로운 것을 만드는 것이 아니라, 오브젝트는 그대로 두고 array 배열을 빙글 빙글 돌면서 각각의 아이템들의 참조값을 복사한다.  즉, array3 배열 안에는 array 안에 들어있는 동일한 오브젝트들이 들어 있고, 배열 오브젝트 자체만 새롭게 만들어진다.   

```jsx
array[0].count = 2;
console.log(array[0]); // {id: '1', count: 2}
console.log(array2[0]); // {id: '1', count: 2}
console.log(array3[0]); // {id: '1', count: 2}
// array에서 id가 1인 오브젝트를 변경하는 경우, array, array2, array3 모두 변경된다.
```

array, array2, array3 모두 배열 첫번째 요소는 참조값 x123을 가진 id가 1인 동일한 오브젝트를 가리키고 있기 때문에, 오브젝트 변경시 모두 변경된다.

```jsx
array.push({ id: '3', count: 0 });
console.log(array.length); // 3
console.log(array2.length); // 3
console.log(array3.length); // 2
// array 배열에 새로운 아이템을 추가하는 경우 총 배열의 길이는 달라진다.
```

array2는 array와 동일한 배열을 가리키기 때문에 새로운 아이템이 추가되지만, array3는 새로운 배열 오브젝트이기 때문에 새로운 아이템이 추가되지 않는다.

## Shallow Copy (얕은 복사)

<aside>
💡 **Shallow-Cloning(Copy)** 이란 배열을 복사할 때, 단순히 제일 상위의 배열 껍데기만 새로운 껍데기로 바꿔주고 안의 오브젝트는 예전의 참조 값을 복사하는 동작을 뜻한다. 그러므로, Spread Operator를 이용하면 처음에는 안에 들어 있는 내용물들을 복사해 오지만 (값이 아니라 레퍼런스, 참조 값만 복사) 배열 자체는 새로운 것을 만들기 때문에 배열에 아이템을 삭제 하거나, 추가 하면 배열의 내용은 달라질 수 있다.

</aside>

- **Spread Operator**
- **Object.assgin()**
    
    ```jsx
    	let person = {
        name: "Jun",
        age: 30,
      };
    
      let person2 = Object.assign({}, person);
      myCat2.name = "Steve";
      myCat2.age = 32;
    
      console.log(person); // { name: 'Jun', age: 30 }
      console.log(person2); // { name: 'Steve', age: 32 }
    ```
    

객체가 아닌, 배열(Array)에 대해서도, `Spread Operator` 나 `Array Object` 내장 메소드인 `Array.prototyp.slice()` , `Array.prototype.map()` 같은 메소드들도 기존 배열 데이터에 대해 조작 후, **새로운 배열** 자체를 반환하는 개념이라 객체에서 개념과 똑같은 `Shallow Copy` 가 가능하다.

💡 Shallow copy는 `깊이(Depth) 1` 에서의 복사를 수행한다. 즉, `가장 바깥 껍데기만 복사 되는 방식으로 동작` 한다. 일반 객체의 대입 복사처럼, 객체 내부에 또 다른 **"객체**나 **함수** 프로퍼티에 대해서는, 같은 데이터에 대해 메모리 주소를 **공유**하기 때문에, 완벽한(Deep 한) 복사가 이뤄지지 않는 것이다.

```jsx
let person = {
  name: "Jun",
  age: 30,
	job : {
		developer: 'front-end'
	}
};

let person2 = { ...person };
person2.name = 'Tony';
person2.age = 40;
person2.job.developer = 'back-end`;

console.log(person) // { name: "Jun",  age: 30, job : { developer: 'front-end' } }
console.log(person2) // { name: "Tony",  age: 40, job : { developer: 'front-end' } }

// Depth 2 인 job : { developer : ''} 는 같은 메모리 주소를 공유한다. (완벽한 복사가 이뤄지지 않았다.)
console.log(person.name === person2.name); // false
console.log(person.job=== person2.job); // true 
console.log(person.job.developer === person2.job.developer ); // true 
```

## Deep Copy (깊은 복사)

**Deep Copy(깊은 복사)** 는 원본 객체를 **완전히** 복사하는 것이다. 
방금 전에 살펴본 `Shallow Copy` (Depth에 따른 완벽한 복사가 이뤄지지 않는 것) 와 달리, `새로운 ****메모리 공간을 확보해` 생성하게 되는 것이다.

- `**JSON.parse()` 와 `JSON.stringify()` 함수**
    - `JSON.stringify` 함수를 이용해서, Object 전체를 **문자열**로 변환 후,
    - 다시 `JSON.parse` 함수를 이용해서 문자열을 **Object** 형태로 반환.
    - 그러면, 문자열로 변환하는 순간 **참조 값이 끊기기 때문에 "새로운" Object**로 만들어 사용할 수 있다.
    - 하지만, JSON 함수는 **엄청난 리소스를 잡아먹는 함수이다.** 성능이 좋지 않은 부분을 고려해야 한다.
    - 또한 JSON 데이터에는 `함수 데이터 타입이 없기 때문에` 함수 속성들은 누락된다.
        
        ```jsx
        let obj= {
          name: "obj",
          sayHello: function () {
            console.log("Hello World");
          },
        };
        let copyObj = JSON.parse(JSON.stringify(obj));
        console.log(obj.sayHi); // [Function: sayHi]
        console.log(copyObj.sayHello); // undefined 🔍
        ```
        
    - 이 밖에도 Object Tree 내에 **순환 참조**가 있는 경우, stringify 메소드에서 **`TypeError: Converting circular structure to JSON`** 이라는 오류가 발생한다.
    
- **Lodash의 cloneDeep 함수**
    - Lodash 는 JS 고차 함수 및 함수형 **라이브러리**다.
    - 그 중 **`_.cloneDeep(value)`** 함수를 사용
        
        ```jsx
        	let person = {
            name: "Jun",
            age: 30,
        		job : {
        			developer: 'front-end developer'
        		}
        		sayHello: function () {
        	    console.log("Hello World");
        	  },
          };
        
          let person2 = _.cloneDeep(person);
        ```
        

- **직접 구현(재귀)**
    - 재귀적으로 **Object Tree**를 따라서 말단 노드까지 모두 복사를 해주는 함수가 필요하다.
        
        ```jsx
         function clone(source) {
            var target = {};
            for (let i in source) {
              if (source[i] != null && typeof source[i] === "object") {
                target[i] = clone(source[i]); // resursion
              } else {
                target[i] = source[i];
              }
            }
            return target;
          }
        
        let person2 = clone(person);
        ```