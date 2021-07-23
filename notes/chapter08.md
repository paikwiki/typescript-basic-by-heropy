# 함수

[Heropy](https://heropy.blog/) 님의 "[한 눈에 보는 타입스크립트](https://edu.goorm.io/learn/lecture/22106/%ED%95%9C-%EB%88%88%EC%97%90-%EB%B3%B4%EB%8A%94-%ED%83%80%EC%9E%85%EC%8A%A4%ED%81%AC%EB%A6%BD%ED%8A%B8/)" 강좌(@Goorm Edu)의 챕터 8, "함수" 강의를 정리한 노트입니다.

## this

함수 내부의 `this`는 전역 객체를 참조하거나(sloppy mode), `undefined`(strict mode)가 되는 등 우리가 원하는 콘텍스트(context)를 잃고 다른 값이 되는 경우가 있다.

예시 - `this`의 참조

```ts
const obj = {
  a: 'Hello~',
  b: function () {
    console.log(this.a); // obj.a
    // Inner function
    function b() {
      console.log(this.a); // global.a
    }
  }
};
```

`this`로 인한 문제는 '호출하지 않은 메소드'를 사용하는 경우에 주로 발생한다. 이렇게 하면 `this`가 유효한 콘텍스트를 잃어버려 함수를 참조할 수 없게 된다.

```ts
const obj = {
  a: 'Hello~',
  b: function () {
    console.log(this.a);
  }
};

obj.b(); // Hello~

const b = obj.b;
b(); // Cannot read property 'a' of undefined

function someFn(cb: any) {
  cb();
}
someFn(obj.b); // Cannot read property 'a' of undefined

setTimeout(obj.b, 100); // undefined
```

이런 상황에서 `this` 콘텍스트를 유지하여 `a` 속성을 참조하도록 하는 방법으로는 두 가지 방법이 있다.

1. bind 메소드를 사용해 `this`를 직접 연결
1. 화살표 함수를 사용하는 방법

### `bind` 메소드로 `this`를 직접 연결하기

```ts
obj.b(); // Hello~

const b = obj.b.bind(obj);
b(); // Hello~

function someFn(cb: any) {
  cb();
}
someFn(obj.b.bind(obj)); // Hello~

setTimeout(obj.b.bind(obj), 100); // Hello~
```

> 타입스크립트에서 `bind()`, `call()`, `apply()`는 인수 타입 체크를 하지 않기 때문에, 컴파일러 옵션에서 `strict: true`(혹은 `strictBindCallApply: true`)를 지정해줘야 정상적으로 타입 체크를 한다.

### 화살표 함수를 이용하기

화살표 함수는 호출된 곳이 아닌 함수가 생성된 곳에서 `this`를 캡처한다.

```ts
obj.b(); // Hello~

const b = () => obj.b();
b(); // Hello~

function someFn(cb: any) {
  cb();
}
someFn(() => obj.b()); // Hello~

setTimeout(() => obj.b(), 100); // Hello~
```

클래스의 메소드 멤버를 정의하는 경우, 프로토타입(prototype) 메소드가 아닌, 화살표 함수를 사용할 수 있다.

```ts
class Cat {
  constructor(private name: string) {}
  getName = () => {
    console.log(this.name);
  }
}
const cat = new Cat('Lucy');
cat.getName(); // Lucy

const getName = cat.getName;
getName(); // Lucy

function someFn(cb: any) {
  cb();
}
someFn(cat.getName); // Lucy
```

이렇게 하면 인스턴스를 생성할 때마다 개별적인 `getName()`이 만들어지는데, 일반적인 메소드 호출에서의 화살표 함수 사용은 비효율적이지만, 메소드를 주로 콜백으로 사용하는 경우에는 프로토타입의 새로운 클로저 호출보다 화살표 함수로 생성된 `getName()` 참조가 훨씬 효율적일 수 있다.

### 명시적 `this`

아래 코드는 `strict: true`(혹은 `noImplicitThis: true`)인 경우, `this`가 암시적인(implicitly) `any` 이기 때문에 에러가 발생한다.

```ts
interface ICat {
  name: string
}

const cat: ICat = {
  name: 'Lucy'
};

function someFn(greeting: string) {
  console.log(`${greeting} ${this.name}`); // Error - TS2683: 'this' implicitly has type 'any' because it does not have a type annotation.
}
someFn.call(cat, 'Hello'); // Hello Lucy
```

이런 경우 다음과 같이 첫 번째 가짜(fake) 매개변수로 `this`를 선언하여, `this`를 명시적으로(explicitly) 정의할 수 있다.

```ts
interface ICat {
  name: string
}

const cat: ICat = {
  name: 'Lucy'
};

function someFn(this: ICat, greeting: string) {
  console.log(`${greeting} ${this.name}`); // ok
}
someFn.call(cat, 'Hello'); // Hello Lucy
```

## 오버로드 (Overloads)

타입스크립트의 '함수 오버로드(Overloads)`는 이름은 같지만 매개변수 타입과 반환 타입이 다른 여러 함수를 가질 수 있는 것을 말한다.

```ts
function add(a: string, b: string): string; // 함수 선언
function add(a: number, b: number): number; // 함수 선언
function add(a: any, b: any): any { // 함수 구현
  return a + b;
}

add('hello ', 'world~');
add(1, 2);
add('hello ', 2); // Error - No overload matches this call.
```

인터페이스와 타입 별칭의 메소드 정의에서도 타입 단언이나 타입 가드를 통해 함수 선언부의 동적인 매개변수와 반환 값을 정의할 수 있다.

```ts
interface IUser {
  name: string,
  age: number,
  getData(x: string): string[];
  getData(x: number): string;
}

let user: IUser = {
  name: 'Neo',
  age: 36,
  getData: (data: any) => {
    if (typeof data === 'string') {
      return data.split('');
    } else {
      return data.toString();
    }
  }
};

user.getData('Hello'); // ['h', 'e', 'l', 'l', 'o']
user.getData(123); // '123'
```
