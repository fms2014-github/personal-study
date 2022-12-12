# ECMAScript

# ECMAScript란?

동적 웹페이지 구현을 목적으로 나온 자바스크립트의 표준화를 위해 ECMA International 에서 만든 규격이다. 1997년 6월에 처음 출판되었고 2009년에 출판된 개정판인 5버전 부터 ES5라는 명칭으로 개정판 버전을 표현하고 있다.

# ECMAScript 버전별 특징

여타 다른 언어와 같이 버전업이 진행될 수록 표준 기능 추가, 결함있는 기능 지원중단, 새로운 문법 추가/제거가 이루어진다. 또 상위 버전에서 표준기능이 비표준기능으로 격하된게 아닌 문법들은 그대로 지원하기 때문에 하위 호환이 이루어 진다고 볼 수 있다.

## ECMAScript 2009(ES5)

2009년에 출판된 표준으로 jsp나 asp.net, jquery로 개발된 대부분의 웹서비스들은 ES5의 문법으로 작동된다고 보면 된다.

### ES5에서 추가된 기능

- “use strict”
    - 다른말로 ‘엄격 모드’라고 부르며 기존 자바스크립트에서 관용적으로 묵인되던 권장하지 않는 문법들을 작성할 때 ReferenceError이나 SyntaxError가 발생한다.
    - ‘엄격 모드’에서는 ‘this’가 undefined가 되는 조건일 때 전역 객체(window 혹은 global)가 바인딩 되지 않는다.
    
    ```jsx
    "use strict"
    function f1(){
        console.log("f1's this : ", this); // undefined
    }
    
    function f2_1(){
        console.log("f2_1's this : ", this); // undefined
        f2_2();
        function f2_2(){
            console.log("f2_2's this : ", this);// undefined
        }
    }
    
    let f3_1 = {
        foo: function(){
            console.log("f3_1's this : ", this); // f3_1
            f3_2();
            function f3_2(){
                console.log("f3_2's this : ", this);// undefined
            }
        }
    }
    ```
    
    - 엄격 모드에서 허용되지 않는 문법들
        - 선언부 없이 변수나 객체를 사용
        
        ```jsx
        "use strict"
        v1 = 1; // Error
        o1 = {key1: "value1", key2: "value2"}; // Error
        ```
        
        - 선언된 함수 및 변수 삭제 불가
        
        ```jsx
        "use strict"
        function f1(p1, p2){};
        delete f1; //Error
        
        let v1 = 1;
        delete v1; //Error
        ```
        
        - 변수에 8진수 리터럴 및 8진수 이스케이프 사용 불가
        
        ```jsx
        "use strict"
        let octalValue1 = 071; //Error
        let octalValue1 = "\071"; //Error
        ```
        
        - 읽기 전용 property에 쓰기 작업 불가
        
        ```jsx
        "use strict"
        let obj = {};
        Object.defineProperty(obj, "x", {value:0, writable:false});
        obj2.x = 3.14; //Error
        ```
        
        - arguments 및 eval로 변수명 사용 불가
        
        ```jsx
        "use strict"
        var eval = "abcd"; // Error
        const arguments = "test"; // Error
        ```
        
        - with문법 사용 불가
        - eval에서 선언한 변수는 외부에서 접근 불가
        
        ```jsx
        "use strict"
        eval("var v1 = 1");
        v1 = 2; // Error
        ```
        
- String[number] access
    - 문자열을 인덱스로 접근할 수 있다.
    
    ```jsx
    let string = "ABCDEFG";
    console.log(string.charAt(0)); // "A"
    console.log(string[2]); // "C"
    ```
    
- Multiline strings
    - 문자열 리터럴을 이스케이프 문자(\)를 이용해 줄바꿈 시켜서 표현할 수 있다.
    
    ```jsx
    let string = "ABCDE\
    FGHI";
    console.log(string); // ABCDEFGHI
    ```
    
- String.trim()
    - 문자열 양 끝의 공백 문자를 제거할 수 있다.
- Array.isArray()
    - 배열 객체인지 확인할 수 있는 메소드
- Array forEach()
    - 배열 요소에 대해 각각 callback함수를 실행하는 메소드
- Array map()
    - 배열 요소 각각에 대한 콜백 함수의 반환 값으로 구성한 새로운 배열을 생성하는 메소드
    
    ```jsx
    let array = [1, 2, 3, 4, 5];
    let newArray = array.map((element) => element * 2);
    console.log(newArray); // [2, 4, 6, 8, 10]
    ```
    
- Array filter()
    - 콜백 함수의 결과가 true인 요소로 구성된 새로운 배열을 생성하는 메소드
    
    ```jsx
    let array = [1, 2, 3, 4, 5, 6, 7, 8];
    let newArray = array.filter((element) => {return element % 2 === 0});
    console.log(newArray); // [2, 4, 6, 8]
    ```
    
- Array reduce()
    - 콜백 함수의 반환값을 누적시킨 값을 반환하는 메소드
    
    ```jsx
    let array = [1, 2, 3, 4, 5];
    let result = array.reduce((accumulator, currentValue) => accumulator + currentValue);
    console.log(result); // 15
    ```
    
- Array reduceRight()
    - Array reduce()와 비슷하나 요소 탐색 순서가 배열 맨 끝 요소부터 맨 처음 요소까지의 순서로 진행된다.
- Array every()
    - 모든 요소가 판별 함수를 통과하는지 확인하는 메소드. 요소들 중 하나라도 판별함수로 부터 false의 반환값을 얻어낸다면 every() 메소드는 false를 반환한다.
- Array some()
    - 하나 이상의 요소가 판별 함수를 통과하는지 확인하는 메소드. 요소들 중 하나라도 판별함수로 부터 true의 반환값을 얻어낸다면 every() 메소드는 true를 반환한다.
- Array indexOf()
    - 배열 요소들 중 특정 값이 있는지 조회하고 값이 있으면 배열 인덱스를 반환하는 메소드
- Array lastIndexOf()
    - indexOf()와 비슷하나 탐색 순서가 배열의 맨 끝 요소부터 맨 처음 요소까지의 순서로 진행된다.
- JSON.parse()
    - JSON 형태의 String을 자바스크립트 객체로 변환시켜주는 메소드
- JSON.stringify()
    - 자바스크립트 객체를 JSON 형태의 String으로 변환시켜주는 메소드
- Date.now()
    - UTC 기준 1970년 1월 1일 0시 0분 0초부터 현재까지 경과된 밀리초를 반환하는 메소드
- Date toISOString()
    - ISO 표준에 맞는 시간을 문자열로 반환하는 메소드
- Date toJSON()
    - JSON 날짜 포멧 표준인 ISO-8601형식으로 날짜를 문자열로 반환하는 메소드
- Property getters and setters
    - 객체 메서드에 대해 getter과 setter을 지정할 수 있다.
    - setter는 최대 하나의 매개변수만 가질 수 있다.
    
    ```jsx
    let object1 = {
        op1: 0,
        op2: 0,
        set setOp1(value) { op1 = value},
        set setOp2(value) { op2 = value},
        get sum() {return op1 + op2}
    }
    
    object1.setOp1 = 2;
    object1.setOp2 = 4;
    
    let result = object1.sum;
    console.log(result); // 6
    ```
    
- Reserved words as property names
    - property 이름으로 예약어를 사용해도 문법에러가 발생하지 않는다.
    
    ```jsx
    let object1 = {let: 1, var: 2}
    console.log(object1) // {let: 1, var: 2}
    ```
    
- Object methods
    - Object 메소드 다수 추가
- Function.bind()
    - Function 객체에 this를 바인딩 할 수 있는 메소
- Trailing commas
    - 객체 property 맨 마지막에 콤마를 붙여도 에러 발생 안함

## ECMAScript 2015(ES6)

### ES6 에서 추가된 기능

- let 키워드
    - 블록 레벨 스코프로 변수 선언을 하는 키워드
- const 키워드
    - 블록 레벨 스코프로 상수 선언을 하는 키워드
- 화살표 기능
    - 화살표 함수 지원
    
    ```jsx
    let func1 = function (p1) { return p1 * 2};
    
    let func2 = (p1) => p1 * 2;
    
    console.log(func1(2)); // 4
    console.log(func2(2)); // 4
    ```
    
- [... 연산자](https://www.w3schools.com/js/js_es6.asp#mark_spread)
    - 스프레드 연산자(직역하면 전개 구문)이라고 하며 반복가능한 요소나 인수를를 분리 혹은 확장할 수 있는 연산
    
    ```jsx
    let array1 = [1, 2, 3];
    function sum(a, b, c){ return a + b + c; }
    
    console.log(sum(...array1)); // 6
    
    let array2 = 'hello';
    let newArray2 = [...array2];
    
    console.log(newArray2); // ['h', 'e', 'l', 'l', 'o']
    
    let array3 = 'world';
    let newArray3 = [...array2, ...array3];
    
    console.log(newArray3); //['h', 'e', 'l', 'l', 'o', 'w', 'o', 'r', 'l', 'd']
    ```
    
- for/of
    - iterable한 객체(반복 가능한 객체)의 요소를 반복 호출 할 수 있는 문법
    
    ```jsx
    let iterableObject = [1, 2, 3];
    
    for(let element of iterableObject){
        console.log(element);
    }
    
    /* result */
    // 1
    // 2
    // 3
    ```
    
- [Map Objects](https://www.w3schools.com/js/js_es6.asp#mark_map)
    - Map 방식의 자료구조 객체를 사용할 수 있다. 일반적인 객체와의 차이점은 모든 자료형이 key property로 사용될 수 있다는 것이다.
- [Set Objects](https://www.w3schools.com/js/js_es6.asp#mark_set)
    - Set 방식의 자료구조 객체를 사용할 수 있다.
- Class
    - Class 방식의 객체지향 구조를 사용할 수 있다.(기존 자바스크르립트는 Prototype 방식의 객체지향 구조)
- Promise
    - 비동기 처리 방식 중 하나인 Promise 패턴을 사용할 수 있다.
- Symbol()
    - Symbol이라는 데이터 형식 추가.
- 기본 매개변수
    - 매개변수의 초기값을 지정할 수 있다.
    
    ```jsx
    function func1(x, y = 10){
        return x + y;
    }
    
    console.log(func1(20)); // 30
    ```
    
- 함수 나머지 매개변수
    - 하나 이상이 인수를 배열로 처리할 수 있다.
    
    ```jsx
    function sum(...args){
        let result;
        for(let element of args){
            result += element;
        }
    
        return result;
    }
    
    console.log(sum(1, 2, 3, 4, 5)); //15
    ```
    
- String.include()
    - 특정 값이 문자열에 있을 경우 true, 없으면 false를 반환
- String.startsWith()
    - 특정 값으로 문자열이 시작하면 true, 아니면 false를 반환
- String.endsWith()
    - 특정 값으로 문자열이 끝나면 true, 아니면 false를 반환
- Array.from()
    - 반복 가능한 객체를 배열로 변환하는 메소드
- Array.keys()
    - 배열 인덱스를 가지고 있는 Array Iterator 객체를 반환하는 메소드
- Array.find()
    - 판별 함수를 통과하는 첫번째 배열 요소 값을 반환하는 메소드
- Array.findIndex()
    - Array.find() 메소드와 같은 기능을 가지나 배열의 인덱스를 반환한다.
- 신규 Math 메소드
    - Math 객체에 신규 메서드 추가
- 신규 Number 객체 속성
    - Number 객체에 신규 속성 추가
- 신규 Number 객체 메서드
    - Number 객체에 신규 메서드 추가
- 신규 전역 메소드
    - 전역 객체에 신규 메서드 추가
- Object entries()
    - key/value 형식의 Array iterator 객체를 반환한다.
- 자바스크립트 모듈
    - 모듈 패턴을 지원한다.
    
    ```jsx
    /* module1.js */
    export default {
        key1: "value1",
        key2: "value2",
    }
    
    /* callModule1.js */
    import module1 from "./module1.js";
    
    console.log(module1.key1); // "value1"
    
    ```