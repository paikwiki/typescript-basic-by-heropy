# 타입 기본

[Heropy](https://heropy.blog/) 님의 "[한 눈에 보는 타입스크립트](https://edu.goorm.io/learn/lecture/22106/%ED%95%9C-%EB%88%88%EC%97%90-%EB%B3%B4%EB%8A%94-%ED%83%80%EC%9E%85%EC%8A%A4%ED%81%AC%EB%A6%BD%ED%8A%B8/)" 강좌(@Goorm Edu)의 챕터 4, "타입 기본" 강의를 정리한 노트입니다.

## 타입 지정

일반 변수, 매개 변수(Parameter), 객체 속성(Property) 등에 `: TYPE` 형태로 지정한다.

```ts
function add(a: number, b: number) {
  return a + b;
}
const sum: number = add(1, 2);
console.log(sum); // 3
```

## 타입 에러

타입 오류시, 컴파일 단계에서 에러가 발생한다.

> 강좌에는 없지만 에러 코드를 어디서 볼 수 있는지 궁금해서 찾아보니, 소스 코드상에서는 오류코드를 찾을 수 없었다. 단, 아래의 경로에서 메시지 객체를 발견할 수 있었는데, 아마도 [`processDiagnosticMessages.ts`](https://github.com/microsoft/TypeScript/blob/v4.3.5/scripts/processDiagnosticMessages.ts)에서 생성하고 있는 듯 하다.
>
> 오류코드 파일 경로: `~/.nvm/versions/node/v14.16.0/lib/node_modules/typescript/lib/ko/diagnosticMessages.generated.json`

## 타입 선언1

### 기본 타입

- 불린: `boolean`
- 숫자: `number`
- 문자열: `string`
- 배열: `Array<T>` 또는 `T[]`. `T`는 인터페이스(`interface`)나 커스텀 타입도 사용할 수 있음

### 읽기 전용 배열

`readonly` 키워드나 `ReadonlyArray<T>` 타입으로 읽기 전용 배열을 생성할 수 있다.

```ts
let arrA: readonly number[] = [1, 2, 3, 4];
let arrB: ReadonlyArray<number> = [0, 9, 8, 7];

arrA[0] = 123; // Error - TS2542: Index signature in type 'readonly number[]' only permits reading.
arrA.push(123); // Error - TS2339: Property 'push' does not exist on type 'readonly number[]'.

arrB[0] = 123; // Error - TS2542: Index signature in type 'readonly number[]' only permits reading.
arrB.push(123); // Error - TS2339: Property 'push' does not exist on type 'readonly number[]'.
```

## 타입 선언2

### 튜플(`[T1, T2...]`)

배열과 유사하지만 정해진 타입의 고정된 길이(length)의 배열로만 사용 가능하다.

```ts
let tuple: [string, number];
tuple = ['a', 1];
tuple = ['a', 1, 2]; // Error - TS2322
tuple = [1, 'a']; // Error - TS2322
```

> 중요: Tuple은 정해진 타입의 고정된 길이 배열을 표현하지만, 할당(Assign)에 국한할 뿐, `.push()`, `.splice()`를 통한 대입은 막을 수 없다.

`readonly` 키워드로 읽기 전용 튜플을 생성할 수 있다.

### 열거형(`enum`)

숫자 혹은 문자열 값 집합에 이름을 부여할 수 있는 타입이다. 기본적으로 0부터 시작해 1씩 증가한다. 수동으로 값을 변경하면 변경한 부분부터 다시 1씩 증가한다.

```ts
enum Week {
  Sun,
  Mon == 22,
  Tue // 23
  Wed // 24
  .
  .
}
```

역방향 매핑(Reverse Mapping)을 지원해서, 열거된 멤버(예: `Sun`, `Mon`)로 값에 접근하는 것 뿐만 아니라 값으로 멤버에 접근할 수 있다.

```ts
enum Week {
  // ...
}
console.log(Week);
console.log(Week.Sun); // 0
console.log(Week['Sun']); // 0
console.log(Week[0]); // 'Sun'
```

Enum을 문자열 값으로 초기화할 경우, 개별적으로 초기화해야하며, 역방향 매핑은 지원하지 않는다.

```ts
enum Color {
  Red = 'red',
  Green = 'green',
  Blue = 'blue'
}
console.log(Color.Red); // red
console.log(Color['Green']); // green
```

## 타입 선언3

### 모든 타입: `any`

최상위 타입. 외부 자원을 활용해 개발할 때 불가피하게 타입을 단언할 수 없는 경우, 유용할 수 있다. 만약 강타입 시스템의 장점을 유지하기 위해 `any` 사용을 금지하려면, 컴파일 옵션 `"noImplicitAny": true`를 지정한다.

### 알 수 없는 타입: `unknown`

`any`와 함께 최상위 타입. `unknown`에는 어떤 타입의 값이든 할당할 수 있지만 `unknown`을 다른 타임에 할당할 수 없다.

> Unknown은 타입 단언(Assertions)이나 타입 가드(Guards)를 필요로 한다. Unknown 보다는 명확한 타입을 쓰는 게 좋다.

```ts
let a: any = 123;
let u: unknown = 123;

let v1: boolean = a; // 모든 타입(any)은 어디든 할당할 수 있습니다.
let v2: number = u; // 알 수 없는 타입(unknown)은 모든 타입(any)을 제외한 다른 타입에 할당할 수 없습니다.
let v3: any = u; // OK!
let v4: number = u as number; // 타입을 단언하면 할당할 수 있습니다.
```

다양한 타입을 반환할 수 있는 API에서 `unknown`을 사용하는 예시

```ts
type Result = {
  success: true,
  value: unknown
} | {
  success: false,
  error: Error
}

export default function getItems(user: IUser): Result {
  // Some logic...
  if (id.isValid) {
    return {
      success: true,
      value: ['Apple', 'Banana']
    };
  } else {
    return {
      success: false,
      error: new Error('Invalid user.')
    }
  }
}
```

### 객체: `object`

`typeof` 연산자가 "`object`"로 반환한는 모든 타입을 나타낸다. 단, 컴파일러 옵션에서 `strict: true` 설정시 `null`은 포함하지 않는다.

여러 타입의 상위 타입이라 유용하지 않으므로 객체 속성(Properties)에 대한 타입을 개별 지정하는 더 나은 방법이 있다.

```ts
let userA: { name: string, age: number } = {
  name: 'HEROPY',
  age: 123
};

let userB: { name: string, age: number } = {
  name: 'HEROPY',
  age: false, // Error
  email: 'thesecon@gmail.com' // Error
};
```

이 방법을 반복 사용할 경우 `interface`나 `type` 사용을 추천한다.

```ts
interface IUser {
  name: string,
  age: number
}

let userA: IUser = {
  name: 'HEROPY',
  age: 123
};

let userB: IUser = {
  name: 'HEROPY',
  age: false, // Error
  email: 'thesecon@gmail.com' // Error
};
```

### `null`과 `undefined`

`null`과 `undefined`는 모든 타입의 하위 타입으로, 서로를 포함해 각 타입에 할당할 수 있다.

```ts
let num: number = undefined;
let str: string = null;
let obj: { a: 1, b: false } = undefined;
let arr: any[] = null;
let und: undefined = null;
let nul: null = undefined;
let voi: void = null;
// ...
```

`"strictNullChecks": true`를 통해 엄격하게 Null과 Undefined 서로의 타입끼리 할당할 수 없게 할 수 있다. 단, `void`에는 `undefined` 할당이 가능하다.

## 타입 선언4

### Void

일반적으로 값을 반환하지 않는 함수에서 사용한다. 다만, 반환값이 없는 함수의 경우 실제로는 `undefined`를 반환한다.

### Never

`never`는 절대 발생하지 않을 값을 나타내며, 어떠한 타입도 적용할 수 없다.

```ts
function error(message: string): never {
  throw new Error(message);
}
```

일반적으로 빈 배열을 타입으로 잘못 선언했을 때 ,`never`를 볼 수 있다.

```ts
const never: [] = [];
never.push(3); // Error - TS2345: Argument of type '3' is not assignable to parameter of type 'never'.
```

### 유니언(Union)

2개 이상의 타입을 허용하는 경우로, `|`를 통해 타입을 구분한다.

### 인터섹션(Intersection)

2개 이상의 타입을 `&`를 이용해 조합(and 연산)하는 경우로, 자주 사용되는 방법은 아니다.

### 함수(Function)

화살표 함수를 이용해 타입을 지정한다.

```ts
// myFunc는 2개의 숫자 타입 인수를 가지고, 숫자 타입을 반환하는 함수.
let myFunc: (arg1: number, arg2: number) => number;
myFunc = function (x, y) {
  return x + y;
};

// 인수가 없고, 반환도 없는 경우.
let yourFunc: () => void;
yourFunc = function () {
  console.log('Hello world~');
};
```

## 타입 추론 (Inference)

명시적으로 타입이 선언되지 않은 경우, 타입스크립트는 타입을 추론해 제공한다.

타입을 추론하는 경우

- 초기화된 변수
- 기본값이 설정된 매개 변수
- 반환값이 있는 함수

> 타입 추론을 잘 사용하여, 더 좋은 코드 가독성을 제공할 수 있다.

## 타입 단언 (Assertions)

타입스크립트가 타입 추론을 통해 판단할 수 있는 타입의 범주를 넘는 경우, '타입 단언'을 통해 추론하지 않도록 지시할 수 있다. 이는 프로그래머가 타입스크립트보다 타입에 대해 더 잘 이해하고 있는 상황을 의미한다.

`as`와 `<T>val` 형식을 쓸 수 있는데, 후자는 JSX 파싱에서 문제가 발생할 수 있어 `.tsx`에서는 사용할 수 없다.

```ts
function someFunc(val: string | number, isNumber: boolean) {
  // some logics
  if (isNumber) {
    // 1. 변수 as 타입
    (val as number).toFixed(2);
    // Or
    // 2. <타입>변수
    // (<number>val).toFixed(2);
  }
}
```

### Not-null 단언 연산자

`!`로 피연산자가 Nullish(`null`이나 `undefined`) 값이 아님을 단언할 수 있다.

```ts
function fn(x: number | null | undefined) {
  return x!.toFixed(2);
}
```

특히 컴파일 환경에서 체크하기 어려운 DOM 사용에서 유용하다.

```ts
// Error - TS2531: Object is possibly 'null'.
document.querySelector('.menu-item').innerHTML;

// Type assertion
(document.querySelector('.menu-item') as HTMLDivElement).innerHTML;
(<HTMLDivElement>document.querySelector('.menu-item')).innerHTML;

// Non-null assertion operator
document.querySelector('.menu-item')!.innerHTML;
```

## 타입 가드 (Guards)

특정 변수의 타입을 보장하기 위해 매번 타입 단언이 필요한 경우, 타입 가드를 이용해 타입스크립트가 추론 가능한 특정 범위(scope)에서 타입을 보장할 수 있다.

타입 가드는 `NAME is TYPE` 형태의 타입 술부(Predicate)를 반환 타입으로 명시한 함수이다.

```ts
// 타입 가드
function isNumber(val: string | number): val is number {
  return typeof val === 'number';
}

function someFunc(val: string | number) {
  if (isNumber(val)) {
    val.toFixed(2);
    isNaN(val);
  } else {
    val.split('');
    val.toUpperCase();
    val.length;
  }
}
```

`typeof`, `in`, `instanceof` 연산자를 직접 사용하는 타입 가드도 있으나, 비교적 단순한 로직에서 추천되는 방식이다.

> `typeof` 연산자는 `number`, `string`, `boolean`,`symbol`만 타입 가드로 인식할 수 있으며, `in` 연산자의 우변 객체(`val`)는 `any` 타입이어야 한다.

```ts
// 기존 예제와 같이 `isNumber`를 제공(추상화)하지 않아도 `typeof` 연산자를 직접 사용하면 타입 가드로 동작합니다.
// https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Operators/typeof
function someFuncTypeof(val: string | number) {
  if (typeof val === 'number') {
    val.toFixed(2);
    isNaN(val);
  } else {
    val.split('');
    val.toUpperCase();
    val.length;
  }
}

// 별도의 추상화 없이 `in` 연산자를 사용해 타입 가드를 제공합니다.
// https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Operators/in
function someFuncIn(val: any) {
  if ('toFixed' in val) {
    val.toFixed(2);
    isNaN(val);
  } else if ('split' in val) {
    val.split('');
    val.toUpperCase();
    val.length;
  }
}

// 역시 별도의 추상화 없이 `instanceof` 연산자를 사용해 타입 가드를 제공합니다.
// https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Operators/instanceof
class Cat {
  meow() {}
}
class Dog {
  woof() {}
}
function sounds(ani: Cat | Dog) {
  if (ani instanceof Cat) {
    ani.meow();
  } else {
    ani.woof();
  }
}
```
