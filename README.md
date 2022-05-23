# 14장 Utility Type

유틸리티 타입은 정의해 놓은 타입을 변환할 때 사용하기 좋은 TypeScript가 제공하는 도구이다. 유틸리티 타입을 쓰지 않더라도 기본 문법으로 타입을 변환할 수 있지만 유틸리티 타입을 사용하면 좀 더 간결하게 타입을 변환할 수 있다.

## 14.1 Partial&lt;Type&gt;

`Type` 집합의 모든 프로퍼티를 선택적으로 타입을 변환하는 유틸리티 타입이다.

타입을 설정할 때 선택적으로 설정하지 않는다면 예제 14-1에서 author와 publisher가 없다고 에러가 뜬다.

예제 14-1

```typescript
interface Book {
  title: string;
  description: string;
  author: string;
  publisher: string;
}

let typescript: Book = {
  title: "알잘딱깔센 TypeScript",
  description: "타입스크립트 입문자가 읽으면 좋은 책",
}; // 불가능
```

이 때 `Partial<Type>`을 사용하면 예제 14-2 처럼 Book의 모든 프로퍼티를 선택적으로 바꿔줌으로서 에러를 잡을 수 있다.

예제 14-2

```typescript
interface Book {
  title?: string;
  description?: string;
  author?: string;
  publisher?: string;
}

let typescript: Partial<Book> = {
  title: "알잘딱깔센 TypeScript",
  description: "타입스크립트 입문자가 읽으면 좋은 책",
}; // 가능
```

## 14.2 Required&lt;Type&gt;

`Required<Type>`은 `Partial<Type>`과 반대의 개념이다.
`Partial<Type>`이 `Type` 집합의 모든 프로퍼티를 선택적으로 타입을 변환하는 유틸리티 타입이라면,
`Required<Type>`은 `Type` 집합의 모든 프로퍼티를 필수로 설정한 타입을 생성하는 유틸리티 타입이다.

예제 14-3에서 author와 publisher 프로퍼티를 선택적으로 바꿔주면서 에러가 발생하지 않는다.

예제 14-3

```typescript
interface Book {
  title: string;
  description: string;
  author?: string;
  publisher?: string;
}

let typescript: Book = {
  title: "알잘딱깔센 TypeScript",
  description: "타입스크립트 입문자가 읽으면 좋은 책",
}; // 가능
```

하지만 `Required<Type>` 유틸리티 타입을 사용한다면 예제 14-4 처럼 모든 프로퍼티를 필수로 바꾸기 때문에 author와 publisher가 없다고 에러가 뜬다.

예제 14-4

```typescript
interface Book {
  title: string;
  description: string;
  author: string;
  publisher: string;
}

let typescript: Required<Book> = {
  title: "알잘딱깔센 TypeScript",
  description: "타입스크립트 입문자가 읽으면 좋은 책",
}; // 불가능
```

## 14.3 Readonly&lt;Type&gt;

`Type`의 모든 프로퍼티를 재할당이 불가능한 `읽기 전용(Readonly)` 으로 설정한 타입을 생성한다.

`읽기 전용(Readonly)`가 아니면 재할당이 가능하다.

예제 14-5

```typescript
interface Book {
  title: string;
  description: string;
  author?: string;
  publisher?: string;
}

let typescript: Book = {
  title: "알잘딱깔센 TypeScript",
  description: "타입스크립트 입문자가 읽으면 좋은 책",
};

typescript.title = "TypeScript Basic"; // 재할당 가능
```

하지만 `Readonly<Type>`을 준다면 예제 14-6처럼 모든 프로퍼티가 **읽기 전용(Readonly)** 이 되면서 재할당이 불가능하게 된다.

예제 14-6

```typescript
interface Book {
  readonly title: string;
  readonly description: string;
  readonly author?: string;
  readonly publisher?: string;
}

let typescript: Readonly<Book> = {
  title: "알잘딱깔센 TypeScript",
  description: "타입스크립트 입문자가 읽으면 좋은 책",
};

typescript.title = "TypeScript Basic"; // 재할당 불가능
```

### 14.4 Record<Keys, Type>

프로퍼티 타입을 `Keys`로, value 타입을 `Type`으로 지정해 생성한다. `Record<Keys,Type>` 유틸리티 타입은 프로퍼티를 다른 타입으로 매핑하고 싶을 때 사용하면 좋다.

예제 14-7

```typescript
interface Food {
  "1팀": "피자" | "치킨" | "햄버거" | "컵라면";
  "2팀": "피자" | "치킨" | "햄버거" | "컵라면";
  "3팀": "피자" | "치킨" | "햄버거" | "컵라면";
  "4팀": "피자" | "치킨" | "햄버거" | "컵라면";
}

const food: Food = {
  "1팀": "피자",
  "2팀": "치킨",
  "3팀": "햄버거",
  "4팀": "컵라면",
};
```

예제 14-7을 보면 value 값의 반복이 보인다. 이를 `Record<Keys,Type>`을 활용하면 해결할 수 있다.

예제 14-8

```typescript
const food: Record<
  "1팀" | "2팀" | "3팀" | "4팀",
  "피자" | "치킨" | "햄버거" | "컵라면"
> = {
  "1팀": "피자",
  "2팀": "치킨",
  "3팀": "햄버거",
  "4팀": "컵라면",
};
```

훨씬 간결해진 모습이지만 조금 길어 보인다. 이를 `type`을 활용해 정리해 줄 수 있다.

예제 14-8

```typescript
type Team = "1팀" | "2팀" | "3팀" | "4팀";
type Food = "피자" | "치킨" | "햄버거" | "컵라면";

const food: Record<Team, Food> = {
  "1팀": "피자",
  "2팀": "치킨",
  "3팀": "햄버거",
  "4팀": "컵라면",
};
```

`type`을 활용해 정리해 주면 재사용에 용이하다.

### 14.5 Pick<Type, Keys>

`Type`에서 프로퍼티 `Keys`를 `Pick`(선택)해 타입을 생성한다.

아래 예제는 `Person`에서 name과 age를 선택해 kim을 생성한다.
예제 14-9

```typescript
interface Person {
  name: string;
  age: number;
  location: string;
  gender: "M" | "W";
}

const kim: Pick<Person, "name" | "age"> = {
  name: "Kim",
  age: 27,
};
```

만약 name과 age가 아닌 다른 key를 선택하게 되면 에러가 발생한다.
예제 14-10

```typescript
interface Person {
  name: string;
  age: number;
  location: string;
  gender: "M" | "W";
}

const kim: Pick<Person, "name" | "age"> = {
  name: "Kim",
  location: "Jeju", // Error: Type '{ name: string; location: string; }' is not assignable to type 'Pick<Person, "name" | "age">'
```

`Pick<Type,Keys>` 유틸리티 역시 `type`을 사용해 정리할 수 있다.
예제 14-11

```typescript
interface Person {
  name: string;
  age: number;
  location: string;
  gender: "M" | "W";
}

type Kim = Pick<Person, "name" | "age">;

const kim: Kim = {
  name: "Kim",
  age: 27,
};
```

### 14.5 Omit<Type, Keys>

`Pick` 유틸리티 타입과는 반대 개념으로
`Type`에서 프로퍼티 `Keys`를 `Omit`(생략)해 타입을 생성한다.

아래 예제는 `Person`에서 age와 gender를 생략해 kim을 생성한다.
예제 14-12

```typescript
interface Person {
  name: string;
  age: number;
  location: string;
  gender: "M" | "W";
}

const kim: Omit<Person, "age" | "gender"> = {
  name: "Kim",
  location: "Jeju",
};
```

만약 age나 gender를 포함하게 되면 에러가 발생한다.
예제 14-13

```typescript
interface Person {
  name: string;
  age: number;
  location: string;
  gender: "M" | "W";
}

const kim: Omit<Person, "age" | "gender"> = {
  name: "Kim",
  location: "Jeju",
  gender: "M", // Error: Type '{ name: string; location: string; gender: string; }' is not assignable to type 'Omit<Person, "age" | "gender">'
};
```

`Omit<Type,Keys>` 유틸리티 역시 `type`을 사용해 정리할 수 있다.
예제 14-14

```typescript
interface Person {
  name: string;
  age: number;
  location: string;
  gender: "M" | "W";
}

type Kim = Omit<Person, "age" | "gender">;

const kim: Kim = {
  name: "Kim",
  location: "Jeju",
};
```

### 14.5 Exclude<Type, ExcludeUnion>

`ExcludeUnion`에 들어갈 수 있는 모든 유니온을 `Type`에서 제외한 타입을 생성한다.

예제 14-15

```typescript
type T1 = Exclude<"kim" | "lee" | "park", "park">;
// result : type T1 = "kim" | "lee"

type T2 = Exclude<string | number | boolean, number | boolean>;
// result : type T2 = string
```

예제 14-15를 보면 `type` T1에서 "park" 을 제외한 "kim" | "lee"가 남게 된다.
`type` T2 역시 number와 boolean 을 제외한 string이 남게 된다.

예제 14-16

```typescript
type T1 = Exclude<string | number | boolean, number>;
// result : type T1 = string | boolean

type T2 = Exclude<T1, boolean>;
// result : type T2 = string
```

예제 14-16 처럼 사용할 수도 있다.

### 14.6 NonNullable<Type>

`Type`에서 `null`과 `undefined`를 제외하고 타입을 생성한다.

예제 14-17

```typescript
type T1 = NonNullable<string | number | boolean | null | undefined>;
// result : type T1 = string | number | boolean
```

T1 에서 null과 undefined을 제외한 string과 number, boolean이 남게 된다.
