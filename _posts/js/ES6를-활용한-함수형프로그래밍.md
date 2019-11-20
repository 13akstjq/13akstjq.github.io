



\* TOC

{:toc}





이 글은 유인동님의 함수형프로그래밍 강의를 보고 작성한 글입니다. 



## 1. 함수형 프로그래밍 기초 

함수형 프로그래밍을 익히기 위해서 기본적으로 숙지해야할 개념인 **평가**, **일급**, **고차함수**에 대해서 알아보겠습니다.

### 평가 

- 코드가 계산되어 값을 만드는 것 

### 일급 

​	일급은 아래 조건을 따릅니다. 

- 값으로 다룰 수 있다. 

  ```js
  const add10 = a => a + 10;
  ```

- 변수에 담을 수 있다. 

  ```js
  const add10 = a => a + 10;
  ```

- 함수의 인자로써 사용될 수 있다. 

- ```js
  const add10 = a => a + 10;
  log(add10(5));
  ```

- 함수의 리턴 값으로 사용될 수 있다. 

  ```js
  const add10 = a => a + 10;
  const add15 = a => add10(a) + 5;
  ```

### 고차함수 

- 함수를 값으로 다루는 함수 

  1. 함수를 인자로 받는 함수 (Applicative Programming)

     ```js
     const apply1 = f => f(1);
     const add2 = a => a + 2;
     log(apply2(add2));
     // 3 
     ```

  2. 함수를 만들어서 리턴하는  수 (클로저를 만들어서 리턴하는 함수)

     함수가 함수를 만들어서 리턴한다는 것은 **클로저**를 만들어서 리턴한다는 것.

     ```js
     const addMaker = a => b => a + b;
     const add10 = addMaker(10);
     log(add10(5));
     // 15
     ```

***



## 2. ES6 리스트 순회 

- ES5에서의 리스트 순회 

  ```js
  const list = [1,2,3];
  for(var i = 0; i < list.length; i++){
  	log(list[i]);
  }
  ```

- ES6에서의 리스트 순회 

  ```js
  for(const a of list){
  	log(a);
  }
  ```

위 두가지 코드를 보면 코드가 간결해지기 위해 `for of`문을 사용하는 것은 아닙니다. 좀 더 자세히 알기 전에 순회를 이해하기 위한 개념에 대해 알고 넘어가겠습니다. 

- 이터러블 : 이터레이터를 리턴하는 `[Symbol.iterator]()`를 갖고있는 값 
- 이터레이터 : `{value : , done : }`을 리턴하는 `next()`를 가지고 있는 값 
- 이터러블/ 이터레이터 프로토콜 : 이터러블이 `for of ` 혹은 `...`을 사용할 수 있게 한 규약

예를 들어서 정리해보자면, 

"Array는 `[Symbol.iterator]()`를 갖고 있는 이터러블로서 for of에서 정상적으로 순회 가능하기 때문에 이터러블/이터레이터 프로토콜을 따른다."   라고 할 수 있습니다. 

###  이터러블

```js
const list = [1, 2, 3];
list[Symbol.iterator] = null; // iterator를 없애면 for of 문을 정상적으로 실행하지 못함.
for (const a of list) {
  console.log(a);
}
// Uncaught TypeError: list is not iterable
```

위 코드처럼 이터러블에 존재하는 `[Symbol.iterator]()`를 지우게 되면 아래와 같은 에러를 띄우게 됩니다. 

```js
const list = [1, 2, 3];
let iter = list[Symbol.iterator]();
iter.next();
for (const a of iter) {
  console.log(a);
}
// 1
// 2
```

위 코드처럼 `iterator`의 	`next()`를 한번 실행시킨 후 `iterator`를 순회하게 되면 나머지만 순회하게 됩니다. 위 방식은 `Set` `Map`에서도 동일하게 적용됩니다.  



## 3. 사용자 정의 이터러블



### 3.1  사용자 정의 이터러블 생성 

내장된 이터러블 말고 직접 이터러블을 만들어보며 이해해보겠습니다. 우선 이터러블은 위에서 말했다시피 `[Symbol.iterator]()`를 갖고 있는 값입니다. 그렇기 때문에 아래와 같이 작성해줍니다.  

```js
const iterable = {
  [Symbol.iterator]() {
    let i = 3;
    return {
      next() {
        return i === 0 ? { done: true } : { value: i--, done: false };
      }
    };
  }
};
```

순회할 길이가 3인 이터러블을 만든 것입니다. 이터러블은 `[Symbol.iterator]()` 를 가지고 있는 객체이면서 `[Symbol.iterator]()` 는 `next()`를 리턴해주는 값입니다. 



위에서 작성한 이터러블을 for of 문을 통해서 순회하게 되면 아래와 같습니다.  

```js
for(const a of iterable){
	console.log(a);
}
// 3
// 2
// 1
```



### 3.2 사용자 정의 이터레이터 사용하기 

만들어진 이터러블을 통해서 아래와 같이 이터레이터를 얻어낼 수 있습니다. 

```js
let iterator = iterable[Symbol.iterator]();
console.log(iterator.next());
console.log(iterator.next());
// {value: 3, done: false}
// {value: 2, done: false}

```

하지만, 아래와 같이 이터레이터를 순회하려고하면 에러가 발생합니다.  

```js
let iterator = iterable[Symbol.iterator]();
console.log(iterator.next());
console.log(iterator.next());

for (const a of iterator) {
  console.log(a);
}
// {value: 3, done: false}
// {value: 2, done: false}
// Uncaught TypeError: iterator is not iterable
```

위와 같은 에러가 발생하는 이유는 이터러블이 리턴해준 `iterator`는 `[Symbol.iterator]()`를 갖고있지 않기 때문에 이터러블이 될 수 없기 때문입니다. 그래서 이터러블이 리턴해주는 이터레이터에도  `[Symbol.iterator]()` 를 만들어주면 이것을 해결할 수 있습니다.  

```js
const iterable = {
  [Symbol.iterator]() {
    let i = 3;
    return {
      next() {
        return i === 0 ? { done: true } : { value: i--, done: false };
      },
      [Symbol.iterator]() {
        return this;
      }
    };
  }
};

let iterator = iterable[Symbol.iterator]();
for (const a of iterator) {
  // 이터레이터는 next()만 가지고 있기 때문에 하위에 또 순회를 하기 위해서는 [Symbol.iterator]()를 가져야함.
  console.log(a);
}
```

위와 같이 이터레이터가 `next()`외에도 `[Symbol.iterator]()`를 리턴해주게 되면 위와 같이 iterator를 순회하더라도 순회가 가능해집니다.  



위와 같이 순회가 가능한 이터레이터의 형태를 `Well-Formed Iterator`라고 부릅니다.

`Well-Formed Iterator`는 `[Symbol.iterator]() `를 호출하면 자기자신을 리턴해주는 이터레이터 입니다.

  ```js
let iterator = iterable[Symbol.iterator]();
console.log(iterator);
console.log(iterator[Symbol.iterator]());
// {next: ƒ, Symbol(Symbol.iterator): ƒ}
// {next: ƒ, Symbol(Symbol.iterator): ƒ}
  ```

위와 같이 `iterator` 와 `iterator[Symbol.iterator]()` 이 같은 경우 잘 만들어진 이터레이터라고 할 수 있습니다. 



## 4. 제너레이터 

이터레이터이자 이터러블을 리턴하는 함수입니다.  



 아래는  제너레이터를 호출해 이터레이터를 리턴받안 `next()`를 사용한 코드입니다. 

```js
function* gen() {
  yield 1;
  yield 3;
  yield 2;
  return 0;
}

const iter = gen();
console.log(iter.next());
console.log(iter.next());
console.log(iter.next());
console.log(iter.next());
// {value: 1, done: false}
// {value: 3, done: false}
// {value: 2, done: false}
// {value: 0, done: true}
```



아래는 제너레이터를 호출한 값이 이터러블이기 때문에 가능한 `for of`문을 사용한 코드입니다.  

```js
function* gen() {
  yield 1;
  yield 3;
  yield 2;
  return 0;
}

const iterable = gen();
for (const a of iterable) {
  console.log(a);
}
// 1 
// 2 
// 3
```



제너레이터의 return값은 순회하는 값이 아니라 done이 될 경우 나오는 값입니다. 그리고 아래와 같이 순회할 값을 문장으로써 사용할 수 있습니다. 

```js
function* gen(n) {
  yield 1;
  yield 3;
  if (n % 2) yield 2;
  return 0;
}

const iterable = gen(4);
for (const a of iterable) {
  console.log(a);
}

// 1
// 3
```



위 코드처럼 자바스크립트는 제너레이터를 통해서 모든 것을 순회가능하도록 만들 수 있습니다. 이것이 함수형 프로그래밍에서 중요한 역할을 합니다.  



### 제너레이터 만들어보기 

홀수값을 가진 이터레이터를 리턴해주는 제너레이터를 만들어보려고 합니다.  



아래와 같이 직접 `yield`를 해주는 방식으로 홀수를 순회할 수 있는 이터레이터를 만들 수 있습니다.  

```js
function* odds() {
  yield 1;
  yield 3;
  yield 5;
}

const iter = odds();
console.log(iter.next());
console.log(iter.next());
console.log(iter.next());
// {value: 1, done: false}
// {value: 3, done: false}
// {value: 5, done: false}
```



하지만 위의 제너레이터는 5까지의 홀수 밖에 얻지 못합니다. 이번에는 n을 입력받아서 n이하의 모든 홀수를 가진 이터레이터를 만들어보겠습니다.  

```js
function* odds(n) {
  for (let i = 0; i < n; i++) {
    if (i % 2) yield i;
  }
}

const iter = odds(10);
for (const a of iter) {
  console.log(a);
}
// 1
// 3
// 5
// 7
// 9
```

위와 같이 원하는 값을 넣었을 때 그 값보다 작은 홀수를 가진 이터레이터를 리턴하는 제너레이터를 만들 수 있습니다. 이번엔 더 작은 기능의 제너레이터를 만들어서 같이 사용해보는 방식으로 작성해보겠습니다. 

```js
function* infinity(i = 0) { // 시작 값을 받아서 무한한 이터레이터를 만들어내는 제너레이터
  while (true) yield i++;
}

function* limit(l, iter) { // 제한 값(l) 과 이터레이터를 받아 1부터 l까지 값을 가지고 있는 제너레이터 
  for (const a of iter) {
    if (a == l) return;
    yield a;
  }
}

function* odds(l) { // 제한 값을 입력 받아서 홀수를 리턴해주는 제너레이터
  for (const a of limit(l, infinity(1))) {
    if (a % 2) yield a;
  }
}

const iter = odds(12);
for (const a of iter) {
  console.log(a);
}
// 1
// 3
// 5
// 7 
// 9
// 11
```



추가적으로 이터레이터는 전개연산자, 비구조화 할당에서도 정상적으로 동작합니다. 

```js
const iter = odds(12);
const [head, ...tail] = iter;
console.log(head);
// 1
console.log(tail);
// [3,5,7,9,11]
```

```js
const iter = odds(12);
console.log([...iter]);
// [1, 3, 5, 7, 9, 11]

```



## 5. Map

```js
const products = [
  { name: "반팔티", price: 15000 },
  { name: "긴팔티", price: 20000 },
  { name: "핸드폰케이스", price: 15000 },
  { name: "후드티", price: 30000 },
  { name: "바지", price: 25000 }
];

const map = (f, iter) => {
  let res = [];
  for (const a of iter) {
    res.push(f(a));
  }
  return res;
};

const name = map(p => p.name, products);
// console.log(name);

const prices = map(p => p.price, products);
// console.log(prices);

let m = new Map();
m.set("a", 4);
m.set("b", 5);
console.log(m);

const iter = m[Symbol.iterator]();
console.log(new Map(map(([a, b]) => [a, b * b], m)));



```







## 6. Go & Pipe

- go 
  -  초기 값과 함수를 받아서 순차적으로 함수를 실행하는 함수 

- Pipe 
  - 함수들을 받아서 함수들을 순차적으로 실행해주는 함수를 이턴하는 함수 
  - 처음에는 함수만 받아서 함수를 리턴해준다. 
  - 그다음에 초기값을 받아서 `go`함수를 호출해서 초기값과 함수들을 넣어준다. 
  - pipe를 작성할 때는 첫 인자가 몇개가 들어올지 모르기 때문에 아래와 같이 `(...as)`로 작성해야 합니다.  
  - 그렇게 할 경우 아래처럼 f1, f2, f3처럼 인자의 갯수가 다르더라도 처리가 가능해집니다. 

```js
  const go = (...fs) => reduce((a, f) => f(a), fs);
      // go(add(0, 1), a => a + 1, a => a + 10, a => a + 100, log);
      // 111

      const pipe = (f, ...fs) => (...as) => go(f(...as), ...fs);

      const f1 = pipe(
        a => a + 1,
        a => a + 10,
        a => a + 100
      );
      log(f1(0));
			// 111
      const f2 = pipe(
        (a, b) => a + b,
        a => a + 1,
        a => a + 10,
        a => a + 100
      );
			// 112
      log(f2(0, 1)); // 첫 번째 인자가 여러개가 들어올 수 있음.
     
      const f3 = pipe(
        (a, b, c) => a + b + c,
        a => a + 1,
        a => a + 10,
        a => a + 100
      );
      log(f3(0, 1, 2));
			// 114
```



### go를 사용해서 가독성 높히기 

아래코드를 읽어보시면 코드 가독성이 향상된 것을 확인하실 수 있습니다.  

```js
log(
	reduce(
		add,
			map(p => p.price,
				filter(p => p.price < 20000, products)))
);

      
```

```js
go(
  products,
  products => filter(p => p.price < 20000, products),
  products => map(p => p.price, products),
  prices => reduce(add, prices),
  log
);
```

위 두 코드는 같은 동작을 하는 코드입니다. 하지만 첫번째 코드는 코드를 읽는 방식이 오른쪽에서 왼쪽으로 거꾸로 읽고 있습니다. 한번에 어떤 동작을 하는지 수비게 알기 힘듭니다. 반면에, 아래 코드는 `go`함수를 통해서 products를 사용할 것이고 products의 가격이 20000보다 작은 것을 필터링하고, 그 중 가격만 뽑아서 모두 더해준다는 형식으로 가독성이 좋습니다.  





### pipe함수를 작성하는 방법에 대한 이해 (추가)

복습을 하던 도중에 pipe함수를 작성할 때 든 생각을 적어보겠습니다.   



파이프 함수는 함수들을 받아서 함수를 리턴하는 함수입니다.  

리턴하는 함수는 **초기 값**을 받아서 **이전에 받아두었던** **함수들을 순차적으로  go함수로 실행**하는 동작을 합니다.   

그렇다면 아래와 같이 작성할 수 있을 것이라 생각했습니다.  

 ```js
// 제가 작성한 pipe 함수 
const pipe = (...fs) => (a) => go( a , ...fs);
log(pipe(
   a => a + 1 , 
   a => a + 10 , 
   a => a + 100
  )(0));
  // 111
 ```

생각한대로 값이 정상적으로 출력 되었습니다. 왜 강의에서는 아래와 같이 작성했는지 이해가 잘 가지 않았습니다. 

```js
// 유인동님이 작성하신 pipe함수 
const pipe = (f, ...fs) => (...as) => go(f(...as) , ...fs);
log(pipe(
   a => a + 1 , 
   a => a + 10 , 
   a => a + 100
  )(0));
  // 111
```



결론부터 말씀드리자면, 파이프라인 구조를 지키기 위해서 라고 생각합니다.  

위 코드는 초기값이 상수 즉, 평가가 완료된 값을 기준으로 작성했습니다. 그렇다면 여러개의 값을 가지고 파이프라인 함수를 동작하고 싶을 경우에는 제가 작성한 코드는 동작이 불가능할까요? 그건 아닙니다. 아래와 같이 작성하면 "동작은" 합니다.   

```js
  // 제가 작성한 pipe 함수 
const pipe = (...fs) => (a) => go( a , ...fs);
 log(pipe(
   a => a + 1 , 
   a => a + 10 , 
   a => a + 100
  )(add(1,2)));
  // 114
```

단, 이렇게 작성하게 되면 pipe함수에 add라는 함수에 1, 2를 통해 평가된 값을 넣어줍니다. 완벽히 파이프라인 함수라고 하기에는 무리가 있어보입니다. 파이프 함수라면 아래와 같이 동작해야한다고 생각합니다.  

```js
  // 제가 작성한 pipe 함수 
const pipe = (...fs) => (a) => go( a , ...fs);
log(pipe(
   (a,b) => a + b,
   a => a + 1 , 
   a => a + 10 , 
   a => a + 100
  )(1,2));
// NaN
```

위와 같이 작성하면 1, 2. 라는 값을 pipe에 넣어주고 Pipe함수가 차례로 함수를 실행시켜 마지막 값을 리턴해주는 예쁜 파이프 함수가 됩니다. 하지만 이렇게 하게 될 경우 보시다시피 `NaN` 을 리턴합니다.  

그렇기 때문에 초기 인자가 여러값이 필요한 경우를 대비해서 `pipe`함수를 수정해줘야 합니다.  



어떻게 수정을 해줘야 하는지 글로 먼저 정리해보겠습니다.  

```js
  // 제가 작성한 pipe 함수 
const pipe = (...fs) => (a) => go( a , ...fs);
```

기존에 제가 작성한 함수를 보면 `a`를 받아서 받아두었던 함수를 리턴했는데 이제는 여러 값을 받을 수도 있는 함수로 바꿔줘야 합니다.

```js
  // 제가 작성한 pipe 함수
  // 여러 인자를 받을 수 있도록 변경 
const pipe = (...fs) => (...as) => go( ...as , ...fs);
```

위처럼 변경해줄 수 있습니다. 하지만 저는 pipe함수를 아래와 같이 사용할 것이기 때문에 추가적으로 수정이 필요합니다.  

```d
log(pipe(
   (a,b) => a + b,
   a => a + 1 , 
   a => a + 10 , 
   a => a + 100
  )(1,2));
// NaN
```





여러개의 인자들을 받았다면 파이프 함수의 첫번째 함수에 여러개의 인자를 가지고 평가를 하는 함수가 존재할 것입니다. 그렇다면, go를 호출할 때 pipe함수의 첫번째 함수에 여러인자들을 넣어서 평가한 값을 넣어주면 될 것입니다.  

```js
const pipe = (f,...fs) => (...as) => go(f(...as) , ...fs);
```

`(f,...fs)`로 나눈 이유는 전개 연산자를 통해서 첫번째 함수를 사용하고 싶기 때문입니다. 이제 위와 같이 작성한 pipe함수를 통해서는 아래와 같은 동작을 이상없이 합니다.  

```js
log(pipe(
    (a,b) => a + b, 
   a => a + 1 , 
   a => a + 10 , 
   a => a + 100
  )(0,1));
  // 112  
  log(pipe(
    (a,b,c) => a + b + c, 
   a => a + 1 , 
   a => a + 10 , 
   a => a + 100
  )(0,1,2));
  // 114
```

동작이 아예 안하는 것은 아니지만 함수형 프로그래밍 관점에서 필요한 고민이라고 생각합니다.  



## 7. Curry

curry는 함수를 받아서 함수를 리턴하는 고차함수입니다. 

리턴한 함수를 실행시킬 때 인자가 하나 이상 들어오면 바로 함수에 인자를 다 넣어서 실행시키고,

만약 함수에 인자가 하나만 들어온다면, 바로 실행시키는 것이 아니라 나머지 인자를 받아서 원래 함수에 원래인자와 나중에 받은 인자를 넣고 실행시키는 함수를 리턴합니다.  

제가 글로 적으니까 하나도 이해가 안되네요... 바로 코드로 보겠습니다.  

```js
const curry = f => (a, ..._) =>
        _.length ? f(a, ..._) : (..._) => f(a, ..._);
		
      const multi = curry((a, b) => a * b);

      const multi3 = multi(3);
      log(multi3);
			// (..._) => f(a, ..._)
      log(multi3(2));
			// 6
			log(multi(2, 3));
			// 6
```

위 코드에서 `multi3`은 인자를 하나만 받았기 때문에 `(..._) => f(a, ..._)`와 같은 함수를 리턴해줬고,`multi3(2)`라고 했을 때 비로소 6이라는 값으로 평가되었습니다.  

`multi(2,3)`는 한번에 인자를 2개 주었기 때문에 바로 6이라는 값을 리턴해줬습니다.   

이제 이 Curry라는 것이 어떻게 쓰일 수 있는지 예시를 통해서 정리해보도록 하겠습니다.  

- Curry를 사용하지 않은 경우 

  curry를 사용하지 않은 경우에는 아래와 같이 코드를 작성합니다. 

  ```js
  go(
    products,
    products => filter(p => p.price < 20000, products),
    products => map(p => p.price, products),
    prices => reduce(add, prices),
    log
  );
  ```

  

- Curry를 사용한 경우 

  curry를 사용하면 아래 코드와 같이 변경가능합니다. 

  ```js
  go(
    products,
    products => filter(p => p.price < 20000)(products),
    products => map(p => p.price)(products),
    prices => reduce(add)(prices),
    log
  );
  ```

사실 Curry를 사용한다고 크게 달라질 것이 없어보입니다. 그렇다면 아래 코드를 좀 더 정리해보도록 하겠습니다.  

- Curry를 사용해 정리한 코드 

  ```js
  go(
    products,
    filter(p => p.price < 20000),
    map(p => p.price),
    reduce(add),
    log
  );
  ```

  아래 코드의 p에 products가 들어갈 것이기 때문에 

  ```js
  products => filter(p => p.price < 20000)(products)
  ```

  아래와 같이 평가되는 것이 가능합니다.  

  ```js
  filter(p => p.price < 20000)
  ```

이렇게 보니 코드가 확실히 간결해졌고, 좀 더 함수들을 선언적으로 문장으로 연결해서 사용한 느낌이 들게 되었습니다.   

코드를 읽어보면 상품을 가격으로 필터링하고 가격만 뽑아내서 다 더한 것을 로그로 찍어보겠다 라는 말로 정리가 됩니다.  



- 반복되는 함수를 활용해서 코드 정리하기 

  ```js
   const total_price = pipe(
          map(p => p.price),
          reduce(add)
        );
  
        const base_total_price = pred =>
          pipe(
            filter(pred),
            total_price
          );
  
        go(products, base_total_price(p => p.price < 20000), log);
        go(products, base_total_price(p => p.price < 20000), log);
  ```

  위 코드를 보면 `total_price`라는 함수와 `base_total_price`라는 함수를 선언함으로써 코드가 반복되는 것을 줄일 수 있습니다.  





### 함수 추상화 

- 추상화 신경 안쓴 함수 

  ```js
  const total_quantity1 = go(products, map(p => p.quantity), reduce(add));
  const total_price1 = go(products, map(p => p.price));
  ```

  위 함수는 총 갯수와 가격을 구하는 함수입니다. 하지만 위의 함수는 객체가 products일 때만 사용가능한 추상화레벨이 굉장히 낮고 의존성이 굉장히 높은 함수입니다. 

  

  위 함수에서 함수 자체가 내가 어떤 객체를 다루는지 알 필요가 없어야합니다. 그러기 위해서는 직접 products에 접근하는 것이 아니라, 함수를 넘겨받아 넘겨받은 함수만 동작시켜주는 방식으로 작성해야합니다.    

  

- 추상화를 한 함수 

  ```js
  const total_quantity2 = pipe(
    map(p => p.quantity),
    reduce(add)
  );
  
  const total_price2 = pipe(
    map(p => p.price),
    reduce(add)
  );
  log(total_quantity2(products));
  log(total_price2(products));
  ```

  위 함수에서는 curry로 감싸져 있는 pipe에게 나중에 products라는 인자를 넘겨줌으로서 함수자체에서 어떤 객체를 건드리는지 알지는 못하지만, 객체에 quantity혹은 price가 있을 때만 사용할 수 있는 함수입니다. 좀더 추상화를 시켜보도록 하겠습니다.  

- 추상화 극대화 

  ```js
  const total = f =>
          pipe(
            map(f),
            reduce(add)
          );
  
  log(total(p => p.quantity)(products));
  log(total(p => p.price)(products));
  ```

  위 코드는 애초에 함수를 받아서 map에게 넘겨주기만 할 뿐 어떤 객체의 어떤 key를 사용하는지 함수 자체에서 전혀 알지 못합니다. 위와같이 작성하는 것보다 아래와 같이 작성하는 것이 가독성에 더 좋은 것 같습니다.  

  ```js
  const total2 = (f, iter) => go(iter, map(f), reduce(add));
        log(total2(p => p.quantity, products));
        log(total2(p => p.price, products));
  ```




## 8. 염격한 평가와 지연평가 

평가를 하는 방법에는 엄격한 평가와 지연평가가 있다고 합니다. 

- 엄격한 평가 
  - 값이 필요한지 상관하지 않고 모든 값을 평가해놓고 시작하는 방법
- 지연평가 
  - 값이 필요한 상황이 왔을 때만 평가를 진행하는 방법 

그렇다면 **지연평가** 가 좋은 이유에 대해서 알아봅시다.  

- 값이 필요한 상황에서만 평가를 하기 때문에 불필요한 계산을 하지않아 속도가 빠름. 
- `infinity` 와 같은 무한 자료형을 사용할 수 있음(어차피 필요한 부분까지만 평가할 것이기 때문)
- 많은 데이터를 사용할 때 에러를 쉽게 찾을 수 있다. 



###  동작 방식 

동작 방식을 설명할 때는 역시 비교하면서 하는 게 제일 좋은 것 같습니다.  입력받은 숫자 길이만큼의 배열을 리턴해주는 함수를 작성해보며 엄격한 평가와 지연평가를  비교해보겠습니다. 

- 엄격한 평가 

  ```js
  const  range =  n => {
    let i = -1;
    let res = [];
    while(++i < n){
      log(i, "range");
      res.push(i);
    }
    return res;
  };
  const list = range(5);
  // 0 "range"
  // 1 "range"
  // 2 "range"
  // 3 "range"
  // 4 "range"
  ```

  위와 같이 `range` 라는 함수를 만들어보면 `range`함수를 호출하기만해도 5번의 로그를 찍는 것을 볼 수 있습니다. 

  

- 지연평가 

  ```js
  const L = {};
  L.range = function*(n){
    let i = -1;
    while(++i < n){
      log(i ,"L.range");
      yield i;
    }
  }
  const list2  = L.range(5);
  ```

  반면에, 위와 같이 `*`를 이용한 제너레이터 펑션을 사용하면 `L.range`를 호출하더라도 아무런 동작을 하지 않는 것을 볼 수 있습니다. 그렇다면 L.range는 언제 평가 되는지 확인해보겠습니다. 

  ```js
  console.log(list2.next());
  // 0 "L.range"
  ```

  위와 같이 제너레이터 평션 즉, 이터레이터를 next()를 사용해서 순회할 경우에만 평가가 됩니다. 사용할 부분까지만 평가가 가능하다는 뜻입니다.  

  그렇다면 지연평가가 성능적으로 어떤 의미를 지녔는지 그림을 통해서 알아보도록 하겠습니다.  



###  엄격한 평가와 지연평가 성능 비교 

테스트 함수를 만들어서 range와 L.range가 얼만큼의 성능차이가 있는지 테스트해보겠습니다.  

```js
const test = (name, time , f) => {
  console.time(name);
  while(time--) f();
  console.timeEnd(name);
}

test("range" , 10 , () => reduce(add , range(10000000)));
test("L.range" , 10 , () => reduce(add , L.range(10000000)));
// range: 4058.576171875ms
// L.range: 2466.02880859375ms
```

위와 같이 천만번 반복한다고 했을 때 차이가 나는 것을 확인할 수 있습니다.  



## 9. take

이터러블에서 원하는 갯수만큼의 값을 얻어오고 싶을 때 사용할 수 있는 함수 take를 만들어보려고 합니다. 그리고 take를 이용해 엄격한 평가와 느긋한 평가를 비교해보도록 하겠습니다.  

```js
  const take = (l , iter) => {
    let res = [];
    for(const a of iter){
      res.push(a);
      if(res.length === l) return res;
    }
    return res;
  }

  test("range" , 1 , () => take(2,range(1000000)));
  test("L.range" , 1 , () => take(2,L.range(1000000)));
// range: 28.904296875ms
// L.range: 0.072998046875ms
```

위와 같이 성능에 차이가 나는 것을 확인할 수 있습니다.  



엄격한 평가는 가로로 진행을 한다고 하면, 영리한평가는 세로로 진행합니다. 

map과 filter를 거쳐서 take를 통해 2개를 뽑아내려고할 때를 예를 들어서 비교해보겠습니다.  

- 엄격한 평가 

  ![엄격한평가](https://user-images.githubusercontent.com/46010705/68525944-09ff9800-031a-11ea-813f-8c5056fe2d42.gif)

  ```js
  	map	  1	2	3	4	5
  
  filter  6	7	8	9	10
  
  	Take  11	12
  ```

- 지연 평가 

  ![게으른평가](https://user-images.githubusercontent.com/46010705/68526004-ade94380-031a-11ea-9aeb-913337d9c1f2.gif)

  ```js
  map     1	4		
  
  filter  2	5	
  
  Take    3 6
  ```

지연평가는 아래방향으로 진행하기 때문에 2개를 얻었다면 거기서 멈추게 됩니다. 같은 결과를 보여주지만 엄격한 평가는 12번, 지연평가는 6번의 연산 횟수를 나타냅니다. 







## 10. L.map , L.filter

기존에 만들었던 map함수와 filter함수를 게으른 평가(영리한 평가)방식으로 작성해보겠습니다.  

- map

  ```js
  L.map = function*(f , iter){
    for(const a of iter){
      yield f(a);
    }
  }
  const m = L.map( a => a+ 10, range(5));
  console.log(m.next().value);
  // 10 
  ```

  위와 같이 L.map을 만들게 되면 값을 평가할 시점에 값이 생성되기 때문에 효율적으로 프로그래밍을 할 수 있습니다.   

- filter

  ```js
  L.filter = function*(f , iter){
    for(const a of iter){
      if(f(a)) yield a;
    }
  }
  
  const fil = L.filter(n => n < 3,L.range(5));
  console.log([...fil])
  // [0,1,2]
  ```





## 11. 지연평가 

자바스크립트에서 제공하는 것을 가지고 지연성을 다룰 수 있게 된 것. 이렇게 구현한 지연성은 서로 다른 개발자가 개발하더라도 자바스크립트의 규약을 지키는 것이기 때문에 조합성 재사용성이 높습니다.  



## 12. 결과를 만드는 함수 reduce, take	

Reduce와 Take는 map, filter와 같이 지연 평가를 진행한 값을 실제 연산하는 단계입니다.  

take도 지연 평가를 통해 값을 특정 갯수만 yield할 수 있지만, take는 몇개 일지 모르는 리스트에서 몇개를 사용할지 정하는 함수이기 때문에 take까지 지연평가로 할 필요는 없다.  





## 13. Array.prototype.join보다 다형성 높은 join만들기 

```js
const queryString = obj =>
  go(obj, Object.entries, map(([k, v]) => join("=", [k, v])), join("&"), log);

const join = curry((sep = ",", iter) =>
  reduce((a, b) => `${a}${sep}${b}`, iter)
);

```

```js
queryString({limit : 10 , offset : 10 , type : "notice"});
// limit=10&offset=10&type=notice

const sharpJoin = join("#");
const obj = {limit : 10 , offset : 10 , type : "notice"};

for( const a in obj){
  log(sharpJoin([a , obj[a]]));
}
//  limit#10
//  offset#10
//  type#notice
```

`Object.entries`함수를 통해서 객체를 배열로 만든 후에 각 [key,value]로 이루어진 값을 우선 `=`로 조인한 후에 `&`으로 조인해서 사용했습니다.  



### queryString 코드 정리 (추가)

```js
const queryString = obj =>
  go(obj, Object.entries, map(([k, v]) => join("=", [k, v])), join("&"), log);

const join = curry((sep = ",", iter) =>
  reduce((a, b) => `${a}${sep}${b}`, iter)
);


```

우선 join을 이용한 `andJoin`과 ` equalJoin`을 만들어보겠습니다.  

```js
const equalJoin = join("=");

const andJoin = join("&");
```

위의 함수를 `queryString`함수에서 사용하면 아래와 같습니다.  

```js
const queryString = obj =>
  go(obj, Object.entries, map(([k, v]) => equalJoin([k, v])), andJoin, log);


```

위 코드에서 `map`을 보면 `[k,v]`를 받아서 equalJon에 그대로 사용하는 것을 보실 수 있습니다. 그럴 경우 아래와 같이 코드를 더 정리할 수 있습니다.  

```js
const queryString = obj =>
  go(obj, Object.entries, map(equalJoin), andJoin, log);
```



강의를 따라 치기만 하다가 이렇게 작은 부분이라도 직접 코드를 고쳐보니까 좀 더 함수형 프로그래밍에 대해 흥미가 생기는 것 같습니다.  



## find 

```js
 const users = [
    {age : 21},
      {age : 41},
      {age : 31},
      {age : 45},
      {age : 27},
      {age : 20},
      {age : 18},
      {age : 25},
      {age : 33},
      {age : 13},
    ]

    const find = curry((f , iter) => go(
      iter , 
      L.filter(f),
      take(1),
      ([a]) => a
    ));

    const over30 = find(({age}) => age > 30);
    const over20 = find(({age}) => age > 20);
    log(over30(users));
    log(over20(users))
```





## L.map L.filter로 map, filter작성하기 

- map 

  ```js
  // 수정 전 
  const map = curry((f, iter) => {
    let res = [];
    for (const a of iter) {
      res.push(f(a));
    }
    return res;
  });
  
  // 1차 수정 후 
  const map = curry((f, iter) => go(iter, L.map(f), takeAll));
  
  
  // 2차 수정 후 
  const map = curry(
    pipe(
      L.map,
      takeAll
    )
  );
  ```

  

- filter 

  ```js
  
  // 수정 전 
  const filter = curry((f, iter) => {
    let res = [];
    for (const a of iter) {
      if (f(a)) res.push(a);
    }
    return res;
  });
  
  // 1차 수정 후 
  const filter = curry((f, iter) => go(L.filter(f, iter), takeAll));
  
  // 2차 수정 후 
  const filter = curry(
    pipe(
      L.filter,
      takeAll
    )
  );
  
  ```



