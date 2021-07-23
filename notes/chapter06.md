# 타입 별칭(Type Aliases)

[Heropy](https://heropy.blog/) 님의 "[한 눈에 보는 타입스크립트](https://edu.goorm.io/learn/lecture/22106/%ED%95%9C-%EB%88%88%EC%97%90-%EB%B3%B4%EB%8A%94-%ED%83%80%EC%9E%85%EC%8A%A4%ED%81%AC%EB%A6%BD%ED%8A%B8/)" 강좌(@Goorm Edu)의 챕터 6, "타입 별칭" 강의를 정리한 노트입니다.

## 타입 별칭에 대하여

`type`으로 새로운 타입 조합을 만들 수 있다. 일반적인 경우, 둘 이상의 조합으로 구성하기 위해 유니언을 많이 사용한다. 접두어로 타입을 의미하는 별칭인 `T`를 붙여 표현하기도 한다.

```ts
type MyType = string;
type YourType = string | number | boolean;
type TUser = {
  name: string,
  age: number,
  isValid: boolean
} | [string, number, boolean];

let userA: TUser = {
  name: 'Neo',
  age: 85,
  isValid: true
};
let userB: TUser = ['Evan', 36, false];

function someFunc(arg: MyType): YourType {
  switch (arg) {
    case 's':
      return arg.toString(); // string
    case 'n':
      return parseInt(arg); // number
    default:
      return true; // boolean
  }
}
```
