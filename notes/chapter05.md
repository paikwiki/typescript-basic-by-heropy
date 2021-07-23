# 인터페이스

[Heropy](https://heropy.blog/) 님의 "[한 눈에 보는 타입스크립트](https://edu.goorm.io/learn/lecture/22106/%ED%95%9C-%EB%88%88%EC%97%90-%EB%B3%B4%EB%8A%94-%ED%83%80%EC%9E%85%EC%8A%A4%ED%81%AC%EB%A6%BD%ED%8A%B8/)" 강좌(@Goorm Edu)의 챕터 5, "인터페이스" 강의를 정리한 노트입니다.

## 인터페이스란

`interface`는 타입스크립트에서 여러 객체를 정의하는 일종의 규칙이며 구조다. 접두어로 인터페이스를 의미하는 별칭인 `I`를 붙여 표현하기도 한다.

```ts
interface IUser {
  name: string,
  age: number,
  isAdult: boolean
}

let user1: IUser = {
  name: 'Neo',
  age: 123,
  isAdult: true
};
```

각 속성 끝에 `;`, `,` 혹은 아무 기호를 사용하지 않을 수 있다.

```ts
interface IUser {
  name: string,
  age: number,
}
// Or
interface IUser {
  name: string;
  age: number;
}
// Or
interface IUser {
  name: string
  age: number
}
```

속성의 키 뒤에 `?`를 붙여 '필수가 아닌 속성'인 선택적 속성으로 정의할 수 있다.

```ts
interface IUser {
  name: string,
  age: number,
  isAdult?: boolean // Optional property
}

// `isAdult`를 초기화하지 않아도 에러가 발생하지 않습니다.
let user: IUser = {
  name: 'Neo',
  age: 123
};
```

## 읽기 전용 속성 (Readonly properties)

`readonly` 키워드로 초기화된 값을 유지하는 읽기 전용 속성을 정의할 수 있다.

```ts
interface IUser {
  readonly name: string,
  age: number
}
```

인터페이스의 모든 속성이 `readonly`일 경우, 유틸리티(Utility)나 단언(Assertion) 타입을 활용할 수 있다.

```ts
// Readonly Utility
interface IUser {
  name: string,
  age: number
}
let user: Readonly<IUser> = {
  name: 'Neo',
  age: 36
};

// Type assertion
let user = {
  name: 'Neo',
  age: 36
} as const;
```

## 함수 타입

호출 시그니처(Call signature)는 함수 타입을 인터페이스로 정의하는 경우 사용하며, 다음과 같이 매개 변수(parameter)와 반환 타입을 지정한다.

```ts
interface IName {
  (PARAMETER: PARAM_TYPE): RETURN_TYPE // Call signature
}
```

```ts
// 예시
interface IUser {
  name: string
}
interface IGetUser {
  (name: string): IUser
}

// 매개 변수 이름이 인터페이스와 일치할 필요가 없습니다.
// 또한 타입 추론을 통해 매개 변수를 순서에 맞게 암시적 타입으로 제공할 수 있습니다.
const getUser: IGetUser = function (n) { // n is name: string
  // Find user logic..
  // ...
  return user;
};
```

## 클래스 타입

`implements` 키워드를 이용해 인터페이스로 정의한 클래스를 사용한다.

```ts
interface IUser {
  name: string,
  getName(): string
}

class User implements IUser {
  constructor(public name: string) {}
  getName() {
    return this.name;
  }
}
```

구성 시그니처(Construct signature)로 클래스인 인터페이스를 함수의 인수(parameter)로 사용할 수 있다.

```ts
interface IName {
  new (PARAMETER: PARAM_TYPE): RETURN_TYPE // Construct signature
}
```

```ts
interface ICat {
  name: string
}
interface ICatConstructor {
  new (name: string): ICat;
}

class Cat implements ICat {
  constructor(public name: string) {}
}

function makeKitten(c: ICatConstructor, n: string) {
  return new c(n); // ok
}
const kitten = makeKitten(Cat, 'Lucy');
```

구성 시그니처를 사용한 재미있는 예제

```ts
interface IFullName {
  firstName: string,
  lastName: string
}
interface IFullNameConstructor {
  new(firstName: string): IFullName; // Construct signature
}

function makeSon(c: IFullNameConstructor, firstName: string) {
  return new c(firstName);
}
function getFullName(son: IFullName) {
  return `${son.firstName} ${son.lastName}`;
}

// Anderson family
class Anderson implements IFullName {
  public lastName: string;
  constructor (public firstName: string) {
    this.lastName = 'Anderson';
  }
}
const tomas = makeSon(Anderson, 'Tomas');
const jack = makeSon(Anderson, 'Jack');
getFullName(tomas); // Tomas Anderson
getFullName(jack); // Jack Anderson

// Smith family?
class Smith implements IFullName {
  public lastName: string;
  constructor (public firstName: string, agentCode: number) {
    this.lastName = `Smith ${agentCode}`;
  }
}
const smith = makeSon(Smith, 7); // Error - TS2345: Argument of type 'typeof Smith' is not assignable to parameter of type 'IFullNameConstructor'.
getFullName(smith);
```

> 강좌에서는 "구성 시그니처"라고 하는데, 클래스의 생성자의 타입을 정의하는 것으로 보이므로 "생성 시그니처"같은 이름이 낫지 않을까 생각했다.

## 인덱싱 가능 타입 (Indexable types)

수많은 속성을 갖거나 단언할 수 없는 임의의 속성이 포함되는 구조에서, 인터페이스를 통해 정의하는 데 한계가 있을 경우, 인덱스 시그니처(Index signature)를 사용할 수 있다.

인덱싱 가능 타입: 숫자나 문자의 경우처럼, `arr[2]`, `arr['name']`처럼, 인덱싱에 사용할 수 있는 타입

인덱싱 가능 타입을 정의하는 인터페이스는 인덱스 시그니처(Index signature)를 가질 수 있다. 이때 인덱서 타입은 `string`과 `number`만 지정할 수 있다.

```ts
interface INAME {
  [INDEXER_NAME: INDEXER_TYPE]: RETURN_TYPE // Index signature
}
```

```ts
// 예시1
interface IItem {
  [itemIndex: number]: string // Index signature
}
let item: IItem = ['a', 'b', 'c']; // Indexable type
console.log(item[0]); // 'a' is string.
console.log(item[1]); // 'b' is string.
console.log(item['0']); // Error - TS7015: Element implicitly has an 'any' type because index expression is not of type 'number'.
```

```ts
// 예시2
interface IUser {
  [userProp: string]: string | boolean
}
let user: IUser = {
  name: 'Neo',
  email: 'thesecon@gmail.com',
  isValid: true,
  0: false
};
console.log(user['name']); // 'Neo' is string.
console.log(user['email']); // 'thesecon@gmail.com' is string.
console.log(user['isValid']); // true is boolean.
console.log(user[0]); // false is boolean
console.log(user[1]); // undefined
console.log(user['0']); // false is boolean
```

위 예시에서 `user['0']`이나 `user[0]` 둘다 인덱싱 전에 숫자가 문자열로 변환되어 동일한 결과를 낸다.

인덱스 시그니처를 이용하면 인터페이스에 정의되지 않은 속성을 사용할 때 유용하다.

```ts
interface IUser {
  [userProp: string]: string | number
  name: string,
  age: number
}
let user: IUser = {
  name: 'Neo',
  age: 123,
  email: 'thesecon@gmail.com'
};
console.log(user['name']); // 'Neo'
console.log(user['age']); // 123
console.log(user['email']); // thesecon@gmail.com
```

`keyof`는 인덱싱 가능 타입의 속성 이름을 타입으로 사용하도록 해준다. 인덱싱 가능 타입의 속성 이름들이 유니온 타입으로 적용된다.

```ts
interface ICountries {
  KR: '대한민국',
  US: '미국',
  CP: '중국'
}
let country: keyof ICountries; // 'KR' | 'US' | 'CP'
country = 'KR'; // ok
```

## 인터페이스 확장

클래스처럼 `extends` 키워드로 상속할 수 있다.

```ts
// 예시1
interface IAnimal {
  name: string
}
interface ICat extends IAnimal {
  meow(): string
}
```

같은 이름의 인터페이스를 여러개 만들면 기존 인터페이스에 내용을 추가할 수 있다.

```ts
// 예시2
interface IFullName {
  firstName: string,
  lastName: string
}
interface IFullName {
  middleName: string
}

const fullName: IFullName = {
  firstName: 'Tomas',
  middleName: 'Sean',
  lastName: 'Connery'
};
```
