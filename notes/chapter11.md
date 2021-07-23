# 모듈

[Heropy](https://heropy.blog/) 님의 "[한 눈에 보는 타입스크립트](https://edu.goorm.io/learn/lecture/22106/%ED%95%9C-%EB%88%88%EC%97%90-%EB%B3%B4%EB%8A%94-%ED%83%80%EC%9E%85%EC%8A%A4%ED%81%AC%EB%A6%BD%ED%8A%B8/)" 강좌(@Goorm Edu)의 챕터 11, "모듈" 강의를 정리한 노트입니다.

## 내보내기(export)와 가져오기(import)

변수, 함수, 클래스 뿐만 아니라 인터페이스나 타입 별칭도 모듈로 내보낼 수 있다.

```ts
// 예시1 - 내보내기
// myTypes.ts

// 인터페이스 내보내기
export interface IUser {
  name: string,
  age: number
}

// 타입 별칭 내보내기
export type MyType = string | number;
```

```ts
// 예시2 - 가져오기
// 선언한 모듈(myTypes.ts) 가져오기
import { IUser, MyType } from './myTypes';

const user: IUser = {
  name: 'HEROPY',
  age: 85
};

const something: MyType = true; // Error - TS2322: Type 'true' is not assignable to type 'MyType'.
```

각 모듈별 가저오는 방법은 아래와 같으며, 컴파일 옵션에 `"esModuleInterop" : true`를 설정하면, ES6 모듈의 Default Import 방식도 사용할 수 있다.

```ts
// 예시3 - 모듈별 가저오기
// CommonJS/AMD/UMD
import ABC = require('abc');
// or
import * as ABC from 'abc';
// or `"esModuleInterop": true`
import ABC from 'abc';
```

## 모듈의 타입 선언1(Ambient module declaration)

Loadash를 설치한 후, `main.ts`를 아래처럼 작성해보면 `import`에서 에러가 발생한다. 이는 타입스크립트 컴파일러가 확인할 수 있는 모듈의 타입 선언(Ambient module declaration)이 없기 때문이다.

```ts
// 예시1
// main.ts

import * as _ from 'lodash'; // Error - TS2307: Cannot find module 'lodash'.

console.log(_.camelCase('import lodash module'));
```

모듈 구현(implement)과 타입 선언(declaration)이 동시에 이뤄지는 타입스크립트와 달리, 구현만 존재하는 자바스크립트 모듈을 사용하는 경우, 컴파일러가 이해할 수 있는 모듈의 타입 선언이 필요하며, 이를 대부분 `.d.ts` 파일로 만들어 제공한다. `loadsh.d.ts`를 루트 폴더에 만든다.

```ts
// 예시2
// lodash.d.ts
declare module 'lodash' {
  // 1. 타입(인터페이스) 선언
  interface ILodash {
    camelCase(str?: string): string
  }

  // 2. 타입(인터페이스)을 가지는 변수 선언
  const _: ILodash;

  // 3. 내보내기(CommonJS)
  export = _;
}
```

참조 태그(Tripe-slash directive, `<reference />`)를 이용해 컴파일에 포함시킨다.

참조 태그의 특징

- 참조 태그로 가져오는 것은 모듈 구현이 아니라 타입 선언이기 때문에, `import` 키워드로 가져오지 않음
- 삼중 슬래시 지시자는 자바스크립트로 컴파일되면 단순 주석.
- `path` 속성은 가져올 타입 선언의 상대 경로를 지정하며, 확장자를 꼭 입력해야 함
- `types` 속성은 `/// <reference types="lodash" />`와 같이 모듈 이름을 지정하며, 이는 컴파일 옵션 `typeRoots` 와 `Definitely Typed(@types)`를 기준으로 함

```ts
// 예시3
// 참조 태그(Triple-slash directive)
/// <reference path="./lodash.d.ts" />

import * as _ from 'lodash';

console.log(_.camelCase('import lodash module'));
```

콘솔에서 실행해본 결과 화면

```sh
$ npx ts-node main.ts
# importLodashModule
```

### Definitely Typed(@types)

프로젝트에 사용하는 모든 모듈에 대해 매번 직접 타입 선언을 작성하는 것(Typing)은 매우 비효율적이다. 이런 경우에 "Definitley Typed"를 사용할 수 있다.

- `npm install -D @types/{모듈이름}`으로 설치
- `npm info @types/{모듈이름}`으로 특정 모듈의 타입 선언이 존재하는지 확인할 수 있음

```sh
$ npm i -D @types/lodash
```

이렇게 하고 나면 별도의 설정 없이 다양한 Loadash API를 사용할 수 있다.

```ts
// 예시
// main.ts

import * as _ from 'lodash';

console.log(_.camelCase('import lodash module'));
console.log(_.snakeCase('import lodash module'));
console.log(_.kebabCase('import lodash module'));
```

```ts
# 실행
$ npx ts-node main.ts
# importLodashModule
# import_lodash_module
# import-lodash-module
```

## 모듈의 타입 선언2(Ambient module declaration)

### `typeRoots`와 `types` 옵션

직접 타입선언을 작성해서 제공할 경우, 컴파일 옵션 `typeRoots`를 사용할 수 있다. 만약 Loadash의 타입 선언을 직접 제공할 경우,

1. `types/lodash` 경로에 `index.d.ts` 파일을 생성
1. `tsconfig.json` 파일의 컴파일옵션으로 `typeRoots: ["./types"]`를 작성

`typeRoots` 옵션의 특징

- 기본값은 `"typeRoots" : ["./node_modules/@types"]`
- `typeRoots` 옵션은 지정된 경로에서 `index.d.ts` 파일을 먼저 탐색함
- `index.d.ts` 파일이 없다면 `package.json`의 `types` 혹은 `typings` 속성에 작성된 경로와 파일 이름을 탐색
1. 타입 선언을 찾을 수 없다면 컴파일 오류 발생

폴더 명은 `types` 뿐 아니라 `@types`, `_types`, `typings` 등 자유롭게 사용할 수 있다.

컴파일러 옵션 `types`를 통해 화이트리스트 방식으로 사용할 모듈만을 작성할 수 있다.

- `"types": ["loadash"]`: `types` 폴더에서 Lodash의 타입 선먼만을 사용
- `"types": []`: `types` 폴더의 모든 타입 선언을 사용하지 않음
- `types`: `types` 폴더의 모든 타입 선언을 사용함

> 일반적인 경우 `types` 옵션은 잘 사용하지 않는다.
