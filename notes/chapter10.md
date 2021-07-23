# 옵셔널(Optional)

[Heropy](https://heropy.blog/) 님의 "[한 눈에 보는 타입스크립트](https://edu.goorm.io/learn/lecture/22106/%ED%95%9C-%EB%88%88%EC%97%90-%EB%B3%B4%EB%8A%94-%ED%83%80%EC%9E%85%EC%8A%A4%ED%81%AC%EB%A6%BD%ED%8A%B8/)" 강좌(@Goorm Edu)의 챕터 10, "옵셔널(Optional)" 강의를 정리한 노트입니다.

## 매개 변수(Parameters)

타입 선언시 옵셔널 매개 변수(Optional Parameter)를 지정할 수 있다.

```ts
// 예시1
function add(x: number, y?: number): number {
  return x + (y || 0);
}
const sum = add(2);
console.log(sum);
```

위 예시는 아래의 예시와 같다.

```ts
// 예시2
function add(x: number, y: number | undefined): number {
  return x + (y || 0);
}
const sum = add(2, undefined);
console.log(sum);
```

## 속성과 메서드(Properties and Methods)

`?`는 속성과 메서드 타입 선언에서도 사용 가능하다.

```ts
// 예시1
interface IUser {
  name: string,
  age: number,
  isAdult?: boolean
}

let user1: IUser = {
  name: 'Neo',
  age: 123,
  isAdult: true
};

let user2: IUser = {
  name: 'Evan',
  age: 456
};
```

```ts
// 예시2
interface IUser {
  name: string,
  age: number,
  isAdult?: boolean,
  validate?(): boolean
}
type TUser = {
  name: string,
  age: number,
  isAdult?: boolean,
  validate?(): boolean
}
abstract class CUser {
  abstract name: string;
  abstract age: number;
  abstract isAdult?: boolean;
  abstract validate?(): boolean;
}
```

## 체이닝(Chaining)

메서드 체이닝에서도 `?.`를 이용해 옵셔널 체이닝(Optional Chaining)을 쓸 수 있다.

```ts
// 예시1
obj?.prop;
obj?.[expr];
arr?.[index];
func?.(args);
```

```ts
// 예시2
// Error - TS2532: Object is possibly 'undefined'.
function toString(str: string | undefined) {
  return str.toString();
}

// Type Assertion
function toString(str: string | undefined) {
  return (str as string).toString();
}

// Optional Chaining
function toString(str: string | undefined) {
  return str?.toString();
}
```

특히 `&&` 연산자로 각 속성을 Nullish 체크(`null`이나 `undefined`를 확인)할 때 유용하다.

```ts
// 예시3
// Before
if (foo && foo.bar && foo.bar.baz) {}

// After-ish
if (foo?.bar?.baz) {}
```

## Nullish 병합 연산자

`||`를 사용해 Falsy 체크(`0`, `""`, `NaN`, `null`, `undefined`를 확인)할 때 `0`이나 `""`를 유효값으로 사용하는 경우 원치 않는 결과가 발생할 수 있다. 이때 Nullish 병합(Nullish Coalescing) 연산자 `??`를 사용할 수 있다.

```ts
// 예시
const foo = null ?? 'Hello nullish.';
console.log(foo); // Hello nullish.

const bar = false ?? true;
console.log(bar); // false

const baz = 0 ?? 12;
console.log(baz); // 0
```

> 널 병합 연산자 (??) 는 왼쪽 피연산자가 null 또는 undefined일 때 오른쪽 피연산자를 반환하고, 그렇지 않으면 왼쪽 피연산자를 반환하는 논리 연산자이다. - [MDN - Nullish coalescing operator](https://developer.mozilla.org/ko/docs/orphaned/Web/JavaScript/Reference/Operators/Nullish_coalescing_operator)
