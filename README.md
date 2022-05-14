# 14장 Utility Type
---
유틸리티 타입은 정의해 놓은 타입을 변환할 때 사용하기 좋은 TypeScript가 제공하는 도구이다. 유틸리티 타입을 쓰지 않더라도 기본 문법으로 타입을 변환할 수 있지만 유틸리티 타입을 사용하면 좀 더 간결하게 타입을 변환할 수 있다.

## 14.1 Partial&lt;Type&gt;
`Type` 집합의 모든 프로퍼티를 선택적으로 타입을 변환하는 유틸리티 타입이다.

타입을 설정할 때 선택적으로 설정하지 않는다면 autor와 publisher가 없다고 에러가 뜬다.
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
} // 불가능
```

이 때 `Partial<Type>`을 사용하면 Book의 모든 프로퍼티를 선택적으로 바꿔줌으로서 에러를 잡을 수 있다.

```typescript
interface Book {
  title: string;
  description: string;
  author: string;
  publisher: string;
}

let typescript: Partial<Book> = {
  title: "알잘딱깔센 TypeScript",
  description: "타입스크립트 입문자가 읽으면 좋은 책",
} // 가능
```
## 14.2 Required&lt;Type&gt;
`Required<Type>`은 `Partial<Type>`과 반대의 개념이다. 
`Partial<Type>`이 `Type` 집합의 모든 프로퍼티를 선택적으로 타입을 변환하는 유틸리티 타입이라면,
`Required<Type>`은 `Type` 집합의 모든 프로퍼티를 필수로 설정한 타입을 생성하는 유틸리티 타입이다.

author와 publisher에 선택적으로 바꿔주면서 아래 예시는 에러가 뜨지 않는다.
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
} // 가능
```

하지만 `Required<Type>` 유틸리티 타입을 사용한다면 모든 프로퍼티를 필수로 바꾸기 때문에 autor와 publisher가 없다고 에러가 뜬다.
```typescript
interface Book {
  title: string;
  description: string;
  author?: string;
  publisher?: string;
}

let typescript: Required<Book> = {
  title: "알잘딱깔센 TypeScript",
  description: "타입스크립트 입문자가 읽으면 좋은 책",
} // 불가능
```

## 14.3 Readonly&lt;Type&gt;
`Type` 집합의 모든 프로퍼티를 `읽기 전용(Readonly)` 으로 설정한 타입을 생성한다.


