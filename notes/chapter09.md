# 클래스

[Heropy](https://heropy.blog/) 님의 "[한 눈에 보는 타입스크립트](https://edu.goorm.io/learn/lecture/22106/%ED%95%9C-%EB%88%88%EC%97%90-%EB%B3%B4%EB%8A%94-%ED%83%80%EC%9E%85%EC%8A%A4%ED%81%AC%EB%A6%BD%ED%8A%B8/)" 강좌(@Goorm Edu)의 챕터 9, "클래스" 강의를 정리한 노트입니다.

## 클래스란

클래스의 속성(Properties)은 `name: string;`과 같이 클래스 바디(중괄호(`{}`)로 묶인 영역)에 별도로 타입을 선언한다.

```ts
class Animal {
  name: string;
  constructor(name: string) {
    this.name = name;
  }
}
class Cat extends Animal {
  getName(): string {
    return `Cat name is ${this.name}.`;
  }
}
let cat: Cat;
cat = new Cat('Lucy');
console.log(cat.getName()); // Cat name is Lucy.
```

## 클래스 수식어(Modifiers)

접근 제어자(Access Modifiers)는 클래스 멤버(속성, 메서드)에서 사용할 수 있다.

| 접근 제어자 | 의미 | 범위 |
|:--|:--|:--|
| `public` | 어디서나 자유롭게 접근 가능(생략 가능) | 속성, 메소드 |
| `protected` | 나와 파생된 후손 클래스 내에서 접근 가능 | 속성, 메소드 |
| `private` | 내 클래스에서만 접근 가능 | 속성, 메소드 |

접근 제어자와 함께 쓸 수 있는 수식어

| 수식어 | 의미 | 범위 |
|:--|:--|:--|
| `static` | 정적으로 사용 | 속성, 일반 메소드 |
| `readonly` | 읽기 전용으로 사용 | 속성 |

만약 생성자 메소드에서 인수 타입 선언과 동시에 접근제어자를 사용하면 바로 속성 멤버로 정의할 수 있다.

```ts
class Cat {
  constructor(public name: string, protected age: number) {}
  getName() {
    return this.name;
  }
  getAge() {
    return this.age;
  }
}

const cat = new Cat('Neo', 2);
console.log(cat.getName()); // Neo
console.log(cat.getAge()); // 2
```

정적 속성은 클래스 바디에서 속성의 타입 선언과 함께 사용하며, 정적 메소드와는 다르게 클래스 바디에서 값을 초기화 할 수 없기 때문에 `constructor` 혹은 메소드에서 초기화가 필요하다.

```ts
class Cat {
  static legs: number;
  constructor() {
    Cat.legs = 4; // Init static property.
  }
}
console.log(Cat.legs); // undefined
new Cat();
console.log(Cat.legs); // 4

class Dog {
  // Init static method.
  static getLegs() {
    return 4;
  }
}
console.log(Dog.getLegs()); // 4
```

`static`과 `readonly`는 접근 제어자와 함께 사용할 수 있으며, 이 경우 접근 제어자를 먼저 작성해야 한다.

```ts
class Cat {
  public readonly name: string;
  protected static eyes: number;
  constructor(n: string) {
    this.name = n;
    Cat.eyes = 2;
  }
  private static getLegs() {
    return 4;
  }
}
```

## 추상(Absract) 클래스

다른 클래스가 파생될 수 있는 기본 클래스로, 인터페이스와 유사하다. `absract`는 클래스 뿐만 아니라 속성과 메소드에도 사용할 수 있다. 추상 클래스는 직접 인스턴스를 생성할 수 없기 때문에 파생 클래스에서 인스턴스를 생성해야 한다.

```ts
// Abstract Class
abstract class Animal {
  abstract name: string; // 파생된 클래스에서 구현해야 합니다.
  abstract getName(): string; // 파생된 클래스에서 구현해야 합니다.
}
class Cat extends Animal {
  constructor(public name: string) {
    super();
  }
  getName() {
    return this.name;
  }
}
new Animal(); // Error - TS2511: Cannot create an instance of an abstract class.
const cat = new Cat('Lucy');
console.log(cat.getName()); // Lucy

// Interface
interface IAnimal {
  name: string;
  getName(): string;
}
class Dog implements IAnimal {
  constructor(public name: string) {}
  getName() {
    return this.name;
  }
}
```

추상 클래스가 인터페이스와 다른 점은 속성이나 메서드 멤버에 대한 세부 구현이 가능하다는 점이다.
