---
title: TypeScript 5.0 Enums
category: javascript
author: "이정훈"
tags: [javascript, typescript, information]
img : https://devblogs.microsoft.com/typescript/wp-content/uploads/sites/11/2023/03/5-0-feature-image-square-bounds-1.png
comments_disable: true
meta_description: "TypeScript 5.0 Enums"
---
# All enums Are Union enums

TypeScript가 처음으로 Enum을 도입했을 때, 그것들은 동일한 타입을 가진 일련의 숫자 상수에 불과했습니다.

```typescript
enum E {
    Foo = 10,
    Bar = 20,
}
```

E.Foo와 E.Bar에 대한 특별한 것은 타입 E를 기대하는 것에 할당할 수 있다는 것뿐이었습니다. 
그 외에는 거의 숫자였습니다.

```typescript
function takeValue(e: E) {}

takeValue(E.Foo); // 작동
takeValue(123); // 오류!
```

TypeScript 2.0에서 Enum 리터럴 타입이 도입될 때까지 Enum이 조금 더 특별해지지 않았습니다. 
Enum 리터럴 타입은 각 Enum 멤버에게 자체 타입을 부여하고, Enum 자체를 각 멤버 타입의 Union으로 
변환했습니다. 

또한 Enum의 타입 중 일부만 참조하고 그 타입을 좁힐 수 있게 해주었습니다.

```typescript
// Color는 Red | Orange | Yellow | Green | Blue | Violet의 유니온과 같습니다.
enum Color {
    Red, Orange, Yellow, Green, Blue, /* Indigo */, Violet
}

// 각 열거형 멤버는 참조할 수 있는 자체 타입을 가집니다!
type PrimaryColor = Color.Red | Color.Green | Color.Blue;

function isPrimaryColor(c: Color): c is PrimaryColor {
    // 리터럴 타입을 좁히면 버그를 잡을 수 있습니다.
    // TypeScript는 여기서 오류를 발생시키는데
    // 'Color.Red'를 'Color.Green'과 비교하게 됩니다.
    // ||를 사용하려고 했지만 실수로 &&를 썼습니다.
    return c === Color.Red && c === Color.Green && c === Color.Blue;
}
```

각 Enum 멤버에게 자체 타입을 부여하는 것의 문제점 중 하나는 그 타입이 어느 정도 멤버의 실제 값과 연관되어 있다는 것이었습니다. 
경우에 따라서는 그 값을 계산할 수 없습니다. 

예를 들어, Enum 멤버는 함수 호출로 초기화될 수 있습니다.

```typescript
enum E {
    Blah = Math.random()
}
```

TypeScript 5.0은 계산된 멤버 각각에 대해 고유한 타입을 생성하여 모든 enum을 union enum type으로 
만들어 줍니다. 

이렇게 하면 모든 enum type이 좁혀지고 멤버를 타입으로 참조할 수 있게 됩니다.

이 변경에 대한 자세한 내용은 [GitHub에서 세부 사항](https://github.com/microsoft/TypeScript/pull/50528)을 확인할 수 있습니다.

