# 호이스팅(Hoisting)

# 호이스팅이란?

함수 안에 있는 선언들을 모두 끌어올려서 각 함수 유효범위 최상단에 선언한 것과 같은 효과를 보여주게 하는 것을 말한다. 이로 인해서 선언문 보다 실행문이 상단에 있어도 런타임시에 정상적으로 동작하게 된다.

```jsx
console.log(v1);
var v1 = 'value1';

/* ↓↓↓↓↓ Hositing ↓↓↓↓↓ */

var v1; // undefined
console.log(v1);
v1 = 'value1';
```

# 함수 호이스팅

함수 호이스팅은 가장 먼저 발생한다. 그리고 함수의 선언문은 식별자 수집 객체가 함수 선언문을 수집할 때 부가적으로 함수 참조에 대한 초기화까지 이루어지기 때문에 호이스팅 후 호출을 해도 정상적인 호출이 가능하다.

함수 선언식과 함수 표현식은 서로 다른 방식으로 호이스팅이 발생하는데 함수 선언식은 선언부가 위로 끌어올려 지는 반면 함수 표현식은 익명 함수를 변수에 대입하는 구조라서 변수 선언문이 위로 끌어올려지게 된다.

```jsx
/* 함수 선언식 호이스팅 */
foo('a');

function foo(args){
    console.log(args);
}

foo('b');

/* ↓↓↓↓↓ Hositing ↓↓↓↓↓ */

function foo(args){
    console.log(args);
}

foo('a'); // 'a'

foo('b'); // 'b'
```

```jsx
/* 함수 표현식 호이스팅 */
foo('a');

var foo = function (args){
    console.log(args);
};

foo('b');

/* ↓↓↓↓↓ Hositing ↓↓↓↓↓ */

var foo; // undefinde

foo('a'); // foo 변수가 undefinde로 초기화 되어 있기 때문에 TypeError 발생

foo = function (args){
    console.log(args);
};

foo('b');

```

# 변수 호이스팅

변수 호이스팅은 var 키워드와 let, const 키워드가 서로 다른 방식으로 작동된다.

우선 변수는 3단계를 걸쳐서 생성이 된다.

1. 선언 단계
    - 파싱 과정에서 선언문을 수집한다.
2. 초기화 단계
    - 식별자에 메모리를 할당하고 undefined 상태를 부여한다
3. 할당 단계
    - 변수 안에 직접 값을 넘겨준다.
    

var 키워드로 생성된 변수는 호이스팅이 발생하면 선언과 초기화가 동시에 이루어 지도록 설계되어 있다. 이때 초기화는 undefinde로 초기화가 되기 때문에 변수 선언과 할당을 동시에 하는 선언부보다 위에서 변수를 호출하게 되면 undefinde가 출력이 된다.

```jsx
console.log(a);

var a = 1;

/* ↓↓↓↓↓ Hositing ↓↓↓↓↓ */

var a; // undefinde

console.log(a);

a = 1;
```

let과 const 키워드로 생성된 변수는 호이스팅이 발생하면 선언만 이루어지고 값 할당은 실질적인 선언부를 만나기 전 까진 초기화가 이루어지지 않는다. 이렇게 실질적인 선언부를 만나기 전 까지 초기화가 되지 않는 구간을 TDZ(일시적 사각지대)라고 한다.

# 동일 식별자의 호이스팅 처리

지양되어야할 일이지만 var 키워드로 선언된 변수명과 함수명이 동일한 경우에도 호이스팅이 발생한다. 이때 호이스팅 우선순위는 다음과 같다.

- 변수 선언과 할당이 동시에 이루어지는 실질적인 선언부가 있으면 변수 할당을 우선한다.
- 변수 선언과 함수 선언만 있을 경우 함수 선언을 우선한다.

```jsx
function msg(){
    console.log('bye');
}

var msg = 'Hello';

console.log(typeof msg); // string
```

```jsx
var msg;

function msg(){
    console.log('bye');
}

console.log(typeof msg); // function
```