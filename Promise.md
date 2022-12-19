# Promise

# Promise란?

ECAMScript 2015 발행판에서 추가된 기능으로 비동기(Asynchronous****)****/논블록킹(Non-Blocking) 처리 패턴이다. Promise가 나오기 전에는 단순 콜백 함수를 사용했기 때문에 콜백 함수와 Promise 패턴을 주로 비교한다.

## 콜백 함수와 비동기/논블록킹

### 콜백 함수란?

간단하게 정리하면 다음과 같다.

- 어떤 함수의 인자로 들어가는 함수
- 어떤 이벤트에 의해 호출되어지는 함수

이 조건에 맞춰 코드를 작성하면 다음과 같이 작성할 수 있다.

```jsx
function callback(x){ // 콜백 함수
    console.log(x);
}

function foo(args){
    let x = "CallBack!!!";
    args(x); // foo함수를 호출하면(foo 호출 이벤트) 인자로 들어간 함수가 매개변수 args로 호출이 된다.
}

// 'callback'이란 함수가 foo함수의 인자로 들어간다.
foo(callback); // "CallBack!!!"
```

### 비동기/논블록킹 처리란?

자바스크립트의 함수 처리는 기본적으로 동기/블록킹 방식으로 처리 된다. 선언된 함수 3개가 있을 때 3개를 호출한다면 호출한 순서대로 함수를 실행한다. 이 과정에서 두번째로 호출된 함수가 작업 시간이 오래 걸린다면 두번째 호출된 함수가 끝날 때 까지 세번째 함수는 호출 되지 않는다.

```jsx
function f1() {
    console.log("called f1()");
}

function f2() {
    console.log("called f2()");
}

function f3() {
    console.log("called f3()");
}

f1();
f2();
f3();

// "called f1()"
// "called f2()"
// "called f3()"
```

동기/블록킹 방식의 단점은 한 함수에서 처리가 오래 걸리게 되면 처리가 끝날 때 까지 다음 작업을 진행할 수가 없다. 웹페이지 로딩이 함수 몇 개 때문에 중간 중간 멈추면 사용자는 답답함을 느낄 것이다.

만약 처리가 오래 걸리는 함수를 별개로 동작하는 것 처럼 따로 결과를 받고 처리가 빠른것들을 우선적으로 처리할 수 있게 된다면 조금 더 연속적인 작동을 보여줄 수 있게 될 것이다. 이런 처리를 가능케 하는것이 비동기/논블록킹 방식이다.

비동기/논블록킹은 호출된 함수가 반환된 상태가 아니더라도 다음 함수가 실행이 가능한 처리 방식이기 때문에 앞서 말한 처리시간이 긴 것을 따로 실행해서 결과는 콜백 함수를 통해 나중에 받을 수 있게 하고 처리시간이 짧은 것을 먼저 처리를 시키는 것이 가능하다.

자바스크립트에서 대표적인 비동기/논블록킹 처리 함수는 setTimeout이 있다.

```jsx
function f1() {
    console.log("called f1()");
}

function f2() {
    console.log("asynchronous/non-blocking");
}

function f3() {
    console.log("called f3()");
}

f1();
setTimeout(f2, 800);
f3();

// "called f1()"
// "called f3()"
// "asynchronous/non-blocking" (800ms 뒤에 출력)
```

setTimeout의 두번째 인수는 n밀리초 뒤에 실행할건지 딜레이를 정하는 인수고 첫번째 인수는 n밀리초가 지난뒤 회신받을 함수를 받는 인수다. 비동기 처리 함수이기 때문에 설정된 밀리초에 묶여있지 않고 f1과 f3함수가 먼저 호출된 뒤 setTimeout의 인수로 들어간 f2함수가 호출이 된다. 이때 f2는 setTimeout에 대한 콜백 함수로 볼 수 있다.

## 콜백 함수로 비동기 처리 시 단점

일반적으로 웹개발에서 비동기/논블록킹 처리가 필요한 경우는 서버로 데이터를 요청하는 경우다. 자바스크립트 내장 객체중에 XMLHttpRequest를 이용하면 비동기로 서버 데이터를 요청할 수 있다.

```jsx
/* XMLHttpRequest 예제 */

function getRequest(url, data, successCallback, failCallback){
		var xhr = new XMLHttpRequest();
		
		xhr.open('GET', url);
		
		xhr.onreadystatechange = function() {
		    if(xhr.readyState === XMLHttpRequest.DONE){
		        if(xhr.status === 0 || xhr.status === 200){
		            successCallback(xhr.response);
		        }else{
		            failCallback('Request fail');
		        }
		    }
		}
		
		xhr.send(data);
}

getRequest('https://developer.mozilla.org/', '', (res) => {
    console.log('success', res);
}, (error) => {
    console.log('fail', error);
});
```

서버로 데이터를 요청하다 보면 A api 요청한 결과값을 B api의 매개변수로 사용해야 하는 일이 생각보다 많다. 이렇다 보니 콜백 함수 안에 콜백 함수를 중첩 사용해야 한다. 게다가 요청이 실패할 경우에 어떻게 처리할 것인지도 해결해야 하기 때문에 구현 하고 보면 콜백 함수 투성이인 코드를 볼 수 있게 된다. 이런 상황을 ‘콜백 지옥’이라 부르기도 한다.

```jsx
getRequest('https://developer.mozilla.org/', A, (resA) => {
	getRequest('https://developer.mozilla.org/', resA, (resB) => {
		getRequest('https://developer.mozilla.org/', resB, (resC) => {
        // ...
		}, (error) => {
		    console.log('fail', error);
		});
	}, (error) => {
	    console.log('fail', error);
	});
}, (error) => {
    console.log('fail', error);
});
```

또 요청이 실패한 경우나 요청 받은 후 로직 실행 중 에러가 발생했을때의 예외처리를 하는 방법까지 추가된다면 코드는 더욱 복잡해지고 덩달아 에러 추적도 힘들어 진다. 그래서 이런 문제점을 해결하기 위해 Promise라는 비동기 처리 패턴이 등장하게 된다.

## Promise 구현

Promise의 생성은 생성자 함수를 통해 구현해야 한다.

```jsx
const promise = new Promise((resolve, reject) => {
    resolve(); // 성공일 경우
    reject(); // 실패일 경우
})
```

구현된 promise객체를 사용하는 방법은 두 가지가 있다. 먼저 성공한 값을 처리할 때는 resolve 콜백함수에 성공값을 인자로 넣고 then 메소드를 이용해 값을 받아 처리하면 된다.

```jsx
const promise = new Promise((resolve, reject) => {
    resolve('success'); // 성공일 경우
})

promise.then((response) => console.log(response));
```

실패한 값을 처리할 때는 reject콜백함수에 에러값을 인자로 넣고 catch 메소드로 값을 받아 처리한다.

```jsx
const promise = new Promise((resolve, reject) => {
    reject('fail'); // 실패일 경우
})

promise.catch((error) => console.log(error));
```

Promise는 resolve와 reject 중 하나의 콜백 함수만 처리가 가능하다. 만약 별도의 분기 처리 없이 resolve와 reject가 나열되어 있으면 가장 먼저 만나는 콜백 함수를 반환 시킨다.

```jsx
const promise = new Promise((resolve, reject) => {
    resolve('success'); // 성공일 경우
    reject('fail'); // 실패일 경우
})

promise.then((response) => console.log(response))
.catch((error) => console.log(error)); // 'success'
```

이를 위에 작성했던 XMLHttpRequest에 적용시키면 다음 코드로 작성할 수 있다.

```jsx
function getRequest(url, data){
    return new Promise((resolve, reject) => {
				var xhr = new XMLHttpRequest();
						
				xhr.open('GET', url);
						
				xhr.onreadystatechange = function() {
				    if(xhr.readyState === XMLHttpRequest.DONE){
				        if(xhr.status === 0 || xhr.status === 200){
				            resolve(xhr.response);
				        }else{
				            reject('Request fail');
				        }
				    }
				}
						
				xhr.send(data);
    })
}

getRequest('https://developer.mozilla.org/')
.then((res) => console.log(res));
```

then은 반환 값으로 promise 객체를 반환한다. 그래서 then을 연달아 사용하는 메소드 체이닝이 가능하다. 이때 then의 콜백 함수는 return을 해줘야 한다.

```jsx
function getRequest(url, data){
    return new Promise((resolve, reject) => {
				var xhr = new XMLHttpRequest();
						
				xhr.open('GET', url);
						
				xhr.onreadystatechange = function() {
				    if(xhr.readyState === XMLHttpRequest.DONE){
				        if(xhr.status === 0 || xhr.status === 200){
				            resolve(xhr.response);
				        }else{
				            reject('Request fail');
				        }
				    }
				}
						
				xhr.send(data);
    })
}

getRequest('https://developer.mozilla.org/')
.then((res) => {
    console.log(res);
    return res; // then 메소드가 종료되면 이 값을 Promise 객체를 통해 반환된다.
})
.then((res) => {
    console.log(res);
    return res; // then 메소드가 종료되면 이 값을 Promise 객체를 통해 반환된다.
});
```

Promise의 최대 장점은 간단한 에러 처리방법이다. then을 여러개 체이닝을 한다 해도 맨 마지막에 catch를 하나만 선언해주면 then 체이닝 처리 도중 에러가 발생해도 마지막에 넣어준 catch에서 에러 처리가 진행된다.

```jsx
function getRequest(url, data){
    return new Promise((resolve, reject) => {
				var xhr = new XMLHttpRequest();
						
				xhr.open('GET', url);
						
				xhr.onreadystatechange = function() {
				    if(xhr.readyState === XMLHttpRequest.DONE){
				        if(xhr.status === 0 || xhr.status === 200){
				            resolve(xhr.response);
				        }else{
				            reject('Request fail');
				        }
				    }
				}
						
				xhr.send(data);
    })
}

getRequest('https://developer.mozilla.org/')
.then((res) => {
    console.log(res);
    return res; // then 메소드가 종료되면 이 값을 Promise 객체를 통해 반환된다.
}).then((res) => {
    console.log(res);
    return new Error('Error');
}).then((res) => {
    console.log(res);
    return res; // then 메소드가 종료되면 이 값을 Promise 객체를 통해 반환된다.
}).catch((error) => console.log(error));
```