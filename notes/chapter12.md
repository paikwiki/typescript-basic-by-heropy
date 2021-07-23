# TS 유틸리티 타입

[Heropy](https://heropy.blog/) 님의 "[한 눈에 보는 타입스크립트](https://edu.goorm.io/learn/lecture/22106/%ED%95%9C-%EB%88%88%EC%97%90-%EB%B3%B4%EB%8A%94-%ED%83%80%EC%9E%85%EC%8A%A4%ED%81%AC%EB%A6%BD%ED%8A%B8/)" 강좌(@Goorm Edu)의 챕터 12, "TS 유틸리티 타입" 강의를 정리한 노트입니다.

## TS 유틸리티 타입에 대하여

타입스크립트는 전역 유틸리티 타입들을 제공한다.

> 타입 변수 T는 타입(Type), U는 또 다른 타입, K는 속성(key)을 의미하는 약어다. 이해를 돕기 위해 타입 변수를 `T`는 `TYPE` 또는 `TYPE1`, `U`는 `TYPE2`, `K`는 `KEY`로 명시한다.

| 유틸리티 이름 | 설명 (대표 타입) | 타입 변수 |
|:--|:--|:--|
| `Partial` | `TYPE`의 모든 속성을 선택적으로 변경한 새로운 타입 반환 (인터페이스) | `<TYPE>` |
| `Required` | `TYPE`의 모든 속성을 필수로 변경한 새로운 타입 반환 (인터페이스) | `<TYPE>` |
| `Readonly` | `TYPE`의 모든 속성을 읽기 전용으로 변경한 새로운 타입 반환 (인터페이스) | `<TYPE>` |
| `Record` | `KEY`를 속성으로, `TYPE`를 그 속성값의 타입으로 지정하는 새로운 타입 반환 (인터페이스) | `<KEY, TYPE>` |
| `Pick` | `TYPE`에서 `KEY`로 속성을 선택한 새로운 타입 반환 (인터페이스) | `<TYPE, KEY>` |
| `Omit` | `TYPE`에서 `KEY`로 속성을 생략하고 나머지를 선택한 새로운 타입 반환 (인터페이스) | `<TYPE, KEY>` |
| `Exclude` | `TYPE1`에서 `TYPE2`를 제외한 새로운 타입 반환 (유니언) | `<TYPE1, TYPE2>` |
| `Extract` | `TYPE1`에서 `TYPE2`를 추출한 새로운 타입 반환 (유니언) | `<TYPE1, TYPE2>` |
| `NonNullable` | `TYPE`에서 `null`과 `undefined`를 제외한 새로운 타입 반환 (유니언) | `<TYPE>` |
| `Parameters` | `TYPE`의 매개변수 타입을 새로운 튜플 타입으로 반환 (함수, 튜플) | `<TYPE>` |
| `ConstructorParameters` | `TYPE`의 매개변수 타입을 새로운 튜플 타입으로 반환 (클래스, 튜플) | `<TYPE>` |
| `ReturnType` | `TYPE`의 반환 타입을 새로운 타입으로 반환 (함수) | `<TYPE>` |
| `InstanceType` | `TYPE`의 인스턴스 타입을 반환 (클래스) | `<TYPE>` |
| `ThisParameterType` | `TYPE`의 명시적 `this` 매개변수 타입을 새로운 타입으로 반환 (함수) | `<TYPE>` |
| `OmitThisParameter` | `TYPE`의 명시적 `this` 매개변수를 제거한 새로운 타입을 반환 (함수) | `<TYPE>` |
| `ThisType` | `TYPE`의 `this` 컨텍스트(Context)를 명시, 별도 반환 없음! (인터페이스) | `<TYPE>` |


```ts
/**
 * Make all properties in T optional
 */
type Partial<T> = {
    [P in keyof T]?: T[P];
};

/**
 * Make all properties in T required
 */
type Required<T> = {
    [P in keyof T]-?: T[P];
};

/**
 * Make all properties in T readonly
 */
type Readonly<T> = {
    readonly [P in keyof T]: T[P];
};

/**
 * From T, pick a set of properties whose keys are in the union K
 */
type Pick<T, K extends keyof T> = {
    [P in K]: T[P];
};

/**
 * Construct a type with a set of properties K of type T
 */
type Record<K extends keyof any, T> = {
    [P in K]: T;
};

/**
 * Exclude from T those types that are assignable to U
 */
type Exclude<T, U> = T extends U ? never : T;

/**
 * Extract from T those types that are assignable to U
 */
type Extract<T, U> = T extends U ? T : never;

/**
 * Construct a type with the properties of T except for those in type K.
 */
type Omit<T, K extends keyof any> = Pick<T, Exclude<keyof T, K>>;

/**
 * Exclude null and undefined from T
 */
type NonNullable<T> = T extends null | undefined ? never : T;

/**
 * Obtain the parameters of a function type in a tuple
 */
type Parameters<T extends (...args: any) => any> = T extends (...args: infer P) => any ? P : never;

/**
 * Obtain the parameters of a constructor function type in a tuple
 */
type ConstructorParameters<T extends new (...args: any) => any> = T extends new (...args: infer P) => any ? P : never;

/**
 * Obtain the return type of a function type
 */
type ReturnType<T extends (...args: any) => any> = T extends (...args: any) => infer R ? R : any;

/**
 * Obtain the return type of a constructor function type
 */
type InstanceType<T extends new (...args: any) => any> = T extends new (...args: any) => infer R ? R : any;

/**
 * Marker for contextual 'this' type
 */
interface ThisType<T> { }
```

## Partial

```ts
Partial<TYPE>
```

`TYPE`의 모든 속성을 옵셔널(`?`)로 변경한 새로운 타입을 반환한다.

```ts
interface IUser {
  name: string,
  age: number
}

const userA: IUser = { // TS2741: Property 'age' is missing in type '{ name: string; }' but required in type 'IUser'.
  name: 'A'
};
const userB: Partial<IUser> = {
  name: 'B'
};
```

## Required

```ts
Required<TYPE>
```

`TYPE`의 모든 속성을 필수로 변경한 새로운 타입을 반환한다.

```ts
interface IUser {
  name?: string,
  age?: number
}

const userA: IUser = {
  name: 'A'
};
const userB: Required<IUser> = { // TS2741: Property 'age' is missing in type '{ name: string; }' but required in type 'Required<IUser>'.
  name: 'B'
};
```

## Readonly

```ts
Readonly<TYPE>
```

`TYPE`의 모든 속성을 읽기 전용(`readonly`)로 변경한 새로운 타입을 반환한다.

```ts
interface IUser {
  name: string,
  age: number
}

const userA: IUser = {
  name: 'A',
  age: 12
};
userA.name = 'AA';

const userB: Readonly<IUser> = {
  name: 'B',
  age: 13
};
userB.name = 'BB'; // TS2540: Cannot assign to 'name' because it is a read-only property.
```

## Record

```ts
Record<KEY, TYPE>
```

`KEY`를 속성으로, `TYPE`을 속성의 타입으로 지정하는 새로운 타입을 반환한다.

```ts
type TName = 'neo' | 'lewis';

const developers: Record<TName, number> = {
  neo: 12,
  lewis: 13
};
```

## Pick

```ts
Pick<TYPE, KEY>
```

`TYPE`에서 `KEY`로 속성을 선택한 새로운 타입을 반환한다. `TYPE`은 반드시 속성을 갖는 인터페이스나 객체 타입이어야 한다.

```ts
interface IUser {
  name: string,
  age: number,
  email: string,
  isValid: boolean
}
type TKey = 'name' | 'email';

const user: Pick<IUser, TKey> = {
  name: 'Neo',
  email: 'thesecon@gmail.com',
  age: 22 // TS2322: Type '{ name: string; email: string; age: number; }' is not assignable to type 'Pick<IUser, TKey>'.
};
```

## Omit

```ts
Omit<TYPE, KEY>
```

`TYPE`에서 `KEY`로 속성을 생략하고 나머지를 담은 새로운 타입을 반환한다. `TYPE`은 반드시 속성을 갖는 인터페이스나 객체 타입이어야 한다.

```ts
interface IUser {
  name: string,
  age: number,
  email: string,
  isValid: boolean
}
type TKey = 'name' | 'email';

const user: Omit<IUser, TKey> = {
  age: 22,
  isValid: true,
  name: 'Neo' // TS2322: Type '{ age: number; isValid: true; name: string; }' is not assignable to type 'Pick<IUser, "age" | "isValid">'.
};
```

## Exclude

```ts
Exclude<TYPE1, TYPE2>
```

유니언 `TYPE1`에서 유니언 `TYPE2`를 제외한 새로운 타입을 반환한다.

```ts
type T = string | number;

const a: Exclude<T, number> = 'Only string';
const b: Exclude<T, number> = 1234; // TS2322: Type '123' is not assignable to type 'string'.
const c: T = 'String';
const d: T = 1234;
```

## Extract

```ts
Extract<TYPE1, TYPE2>
```

유니언 `TYPE1`에서 유니언 `TYPE2`를 추출한 새로운 타입을 반환한다.

```ts
type T = string | number;
type U = number | boolean;

const a: Extract<T, U> = 123;
const b: Extract<T, U> = 'Only number'; // TS2322: Type '"Only number"' is not assignable to type 'number'.
```

## NonNullable

```ts
NonNullable<TYPE>
```

유니언 `TYPE`에서 `null`과 `undefiend`를 제외한 새로운 타입을 반환한다.

```ts
type T = string | number | undefined;

const a: T = undefined;
const b: NonNullable<T> = null; // TS2322: Type 'null' is not assignable to type 'string | number'.
```

## Parameters

```ts
Parameters<TYPE>
```

함수 `TYPE`의 매개변수 타입을 새로운 튜플(Tuple) 타입으로 반환한다.

```ts
function fn(a: string | number, b: boolean) {
  return `[${a}, ${b}]`;
}

const a: Parameters<typeof fn> = ['Hello', 123]; // Type 'number' is not assignable to type 'boolean'.
```

## ConstructorParameters

```ts
ConstructorParameters<TYPE>
```

클래스 `TYPE`의 매개변수 타입을 새로운 튜플(Tuple) 타입으로 반환한다.

```ts
class User {
  constructor (public name: string, private age: number) {}
}

const neo = new User('Neo', 12);
const a: ConstructorParameters<typeof User> = ['Neo', 12];
const b: ConstructorParameters<typeof User> = ['Lewis']; // TS2741: Property '1' is missing in type '[string]' but required in type '[string, number]'.
```

## Return Type

```ts
ReturnType<TYPE>
```

함수 `TYPE`의 반환 타입을 새로운 타입으로 반환한다.

```ts
function fn(str: string) {
  return str;
}

const a: ReturnType<typeof fn> = 'Only string';
const b: ReturnType<typeof fn> = 1234; // TS2322: Type '123' is not assignable to type 'string'.
```

## Instance Type

```ts
InstanceType<TYPE>
```

클래스 `TYPE`의 인스턴스 타입을 새로운 타입으로 반환한다.

```ts
class User {
  constructor(public name: string) {}
}

const neo: InstanceType<typeof User> = new User('Neo');
```

## ThisParameterType

```ts
ThisParameterType<TYPE>
```

함수 `TYPE`의 명시적 `this` 매개변수 타입을 새로운 타입으로 반환한다. 함수 `TYPE`에 명시적 `this` 매개변수가 없는 경우 알수 없는 타입(`unknown`)을 반환한다.

```ts
// https://www.typescriptlang.org/docs/handbook/utility-types.html#thisparametertype

function toHex(this: number) {
    return this.toString(16);
}

function numberToString(n: ThisParameterType<typeof toHex>) {
    return toHex.apply(n);
}
```

## OmitThisParameter

```ts
OmitThisParameter<TYPE>
```

함수 `TYPE`의 명시적 `this` 매개변수를 제거한 새로운 타입을 반환한다.

```ts
function getAge(this: typeof cat) {
  return this.age;
}

// 기존 데이터
const cat = {
  age: 12 // Number
};
getAge.call(cat); // 12

// 새로운 데이터
const dog = {
  age: '13' // String
};
getAge.call(dog); // TS2345: Argument of type '{ age: string; }' is not assignable to parameter of type '{ age: number; }'.
```

위에서 `age`를 `string` 타입으로 갖는 `dog`은 `getAge()`를 사용할 수 없지만 아래처럼 하면 가능하다.

```ts
const getAgeForDog: OmitThisParameter<typeof getAge> = getAge;
getAgeForDog.call(dog); // '13'
```

> `this.age`에 어떤 값이든 들어갈 수 있다는 것에 주의해야 한다.

## ThisType

```ts
ThisType<TYPE>
```

`TYPE`의 `this` 컨텍스트(Context)를 명시하고 별도의 타입을 반환하지 않는다.

```ts
interface IUser {
  name: string,
  getName: () => string
}

function makeNeo(methods: ThisType<IUser>) {
  return { name: 'Neo', ...methods } as IUser;
}
const neo = makeNeo({
  getName() {
    return this.name;
  }
});

neo.getName(); // Neo
```

`makeNeo()`의 인수인 `getName()`은 내부에서 `this.name`을 사용하고 있기 때문에 `ThisType`을 통해 명시적으로 `this` 컨텍스트를 설정한다. 단, `ThisType`은 별도의 타입을 반환하지 않아서 `makeNeo()` 반환 값(`{ name: 'Neo', ...methods }`)에 대한 타입이 정상적으로 추론(Inference)되지 않는다. 반드시 `as IUser`와 같이 따로 타입을 단언(Assertions)해야 `neo.getName`을 호출할 수 있다.
