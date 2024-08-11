---
layout: single
title:  "프론트 개발에 필수적인 타입스크립트"
categories: Frontend
tag: [TypeScript]
toc: true
toc_label: "table of content" # toc 이름 설정
toc_icon: "bars" # 아이콘 설정
toc_sticky: true # 마우스 스크롤과 함께 내려갈 것인지 설정
typora-root-url: ../
---



# 1. TypeScript 소개

## 1.1 타입스크립트란

1. 마이크로소프트가 추진하는 오픈소스 자바스크립트 지원 개발 언어이다.
2. 별도의 컴파일 과정을 통해 순수 자바스크립트로 변환할 수 있는 프로그래밍 언어인 AltJS 언어의 일종이다.
3. 자바스크립트 기반 프론트엔드 및 백엔드 개발 표준 언어로 발전 중이다.

## 1.2 AltJS란

1. AltJS는 JavaScript의 대안으로 개발된 다양한 언어들을 총체적으로 지칭하는 용어이다.

2. AltJS 언어로 개발된 소스를 컴파일 과정을 통해 순수 자바스크립트 소스로 변환해 사용하는 패턴이 적용된다.

3. 자바스크립트 언어의 한계점을 극복하기 위한 수단으로 많이 활용되고 있다.

4. AltJS 언어들은 JavaScript로 컴파일되는 과정을 통해 최종 자바스크립트가 웹 브라우저에서 실행된다.

5. AltJS 언어를 이용해 JavaScript의 한계를 극복하고, 개발자들이 선호하는 다른 프로그래밍 패러다임이나 문법을 사용할 수 있게 지원한다.

6. AltJS 언어들은 JavaScript로 컴파일되어 최종 웹 브라우저나 Node Framework 기반에서 실행된다.

7. 대표적인 AltJS 언어로는 TypeScript, CoffeeScript, Elm이 있다.

   **AltJS 개발소스 -> AltJS 컴파일러 컴파일링 -> 순수 자바스크립트 생성 -> 순수 자바스크립트 실행환경 실행**

## 1.3 TypeScript 등장 배경 및 사용 목적

1. 자바스크립트 언어의 불안정을 해결하기 위해 등장하였다.
2. 자바스크립트는 컴파일 과정이 없어서 사전에 에러를 찾고 해결하기 어렵다.
3. 자바스크립트는 서비스 시점에 예기치 못한 문제가 발생할 우려가 크다.
4. 특히 타입이 없는 자바스크립트의 특성으로 협업 개발 시 다양한 어려움이 발생한다.
5. C#이나 Java와 같은 정적 타입 기반 컴파일 언어의 장점을 자바스크립트 언어 기반 개발환경에 도입하였다.
6. 타입스크립트로 개발된 소스를 컴파일 과정을 통해 순수 자바스크립트 소스로 변환해 사용하는 패턴을 사용한다.

## 1.4 TypeScript 사용의 목적과 주요 장단점

1. 컴파일 과정을 통해 사전 에러 검출 가능

2. 정적 타입 적용을 통한 코드의 안정성 보장

3. 대규모 웹 어플리케이션 개발 및 협업 시 재사용 가능한 모듈 환경 지원으로 개발 생산성이 높음

4. 자바스크립트 문법을 확장한 상위 호환성 제공(SuperSet)을 통해 순수 자바스크립트와 혼용하여 사용 가능

5. 타입스크립트가 제공하는 추가적인 기능 중 가장 중요한 기능은 정적 타입 검사 기능임

6. 정적 타입 검사 기능을 통해 코드를 실행하기 전에 타입 오류를 잡아내어 버그를 줄이고, 코드의 가독성과 유지 관리성을 향상시킴

7. 타입스크립트는 최신 JavaScript 기능을 지원하며, 이러한 기능들은 모든 웹 브라우저나 Node Framework에 호환성을 제공함

   주요 단점으로는:

8. 대규모 어플리케이션이나 소스가 큰 경우 컴파일 타임이 길어질 수 있음

9. 팀 내 타입스크립트 경험자가 없는 경우 도입이 어렵거나 초기 학습 비용이 발생

## 1.5 전역 패키지로 TypeScript 노드 패키지를 설치하기

```bash
bash코드 복사
npm i -g typescript
```

# 2. JavaScript와 TypeScript 문법 비교하기

## 2.1 타입스크립트 작동방식 실습

```
hello1.js
jsx코드 복사
const userId = 'hong';
const userName = '홍길동';

function sayHello() {
    console.log(`안녕하세요! ${userName}님. 아이디는 ${userId}입니다.`);
}

sayHello(userId, userName);
hello2.ts
tsx코드 복사
const userId: string = "hong";
const userName: string = "홍길동";

function sayHello(userId: string, userName: string): void {
    console.log(`안녕하세요. ${userName}님! 당신의 아이디는 ${userId}입니다.`);
}

sayHello(userId, userName);
```

다음 명령어로 ts 파일을 실행한다.

```bash
bash코드 복사
tsc hello2.ts
```

**hello2.ts(타입스크립트 파일 작성) ---> compile(tsc 컴파일) ----> hello2.js(자바스크립트 파일 생성) ----> node hello2.js(실행)**

`hello2.js`라는 파일이 생성된다.

```
hello2.js
jsx코드 복사
var userId = "hong";
var userName = "홍길동";
function sayHello(userId, userName) {
    console.log("안녕하세요. ".concat(userName, "님! 당신의 아이디는 ").concat(userId, "입니다."));
}
sayHello(userId, userName);
```

## 2.2 타입스크립트 컴파일 옵션 --strictNullChecks 엄격체크 옵션의 의미

```bash
bash코드 복사
tsc --strictNullChecks hello2.ts
```

1. **-strictNullChecks**는 TypeScript 컴파일러 옵션 중 하나이다.

2. 이 옵션을 사용하면 변수의 값에 대해 `null`과 `undefined`를 허용하지 않도록 강력한 타입 체크를 한다. 이를 통해 `null`과 `undefined`로 인한 에러를 줄일 수 있다.

   **옵션 사용 시의 변화**

3. 기본적으로 TypeScript에서는 변수를 선언하면 값이 할당되지 않았을 때 `undefined`가 자동으로 할당된다.

4. 하지만 `-strictNullChecks` 옵션을 사용하면, 변수를 선언하고 값을 할당하지 않은 경우 이를 에러로 처리한다.

### 2.2.1 예시 코드

옵션을 사용하지 않았을 때:

```tsx
tsx코드 복사
let age: number;
console.log('나이는 ' + age + '세 입니다.');
```

위 코드에서는 `age` 변수가 선언되었지만 값이 할당되지 않았으므로 `undefined`가 할당된다. 이 경우 런타임에서 오류가 발생할 수 있다.

옵션을 사용했을 때:

```tsx
tsx코드 복사
let age: number;
console.log('나이는 ' + age + '세 입니다.');
```

1. `-strictNullChecks` 옵션을 사용하면 `age` 변수가 초기화되지 않았기 때문에 컴파일 오류가 발생한다. 이는 개발자가 사전에 이러한 오류를 확인하고 수정할 수 있도록 도와준다.

### 2.2.2 undefined vs null 이해하기

1. **undefined**: 자바스크립트에서 변수를 선언하면 자동으로 `undefined` 값이 할당된다. 이는 변수가 아직 값이 할당되지 않았음을 의미한다.
2. **null**: 자바스크립트에서 `null` 값은 변수가 값이 없음을 의미하며, 이는 객체가 없다는 것을 나타낸다. `null`의 타입은 `object`이다.

# 3. TypeScript 문법

## 3.1 Type Annotation 이해하기

1. Annotation은 주석이라고 해석되며, 사전적 의미는 어려운 낱말이나 문장을 쉽게 풀이한 것을 뜻하지만, 타입스크립트에서는 `변수 함수 객체 속성의 타입을 지정`하는 것을 말한다.

### 3.1.1 Type Annotation

1. 변수명, 함수명, 객체 속성명 뒤에 type을 써서 데이터 타입을 지정하는 것을 Type Annotation이라고 한다.

```tsx
tsx코드 복사
// 예시
let age: number;
```

1. type 형태는 자바스크립트의 원시 타입(number, string, boolean)이나 객체, 클래스, 인터페이스, 개발자 정의 타입, 함수 등을 대표적으로 사용이 가능하다.
2. Type annotation을 사용하여 type 검사를 수행하여 잠재적 에러를 확인할 수 있다.
3. Type Annotation 지정은 필수 사항은 아니지만, 아래와 같은 장점을 제공하기 때문에 그 사용을 권장한다.

### 3.1.2 Type Annotation 사용 시 장점

1. 타입스크립트 컴파일러가 type을 확인하여 빌드타임 타입 체크 및 에러 확인 기능 제공
2. data type을 처리 시 사전 오류 방지에 도움

### 3.1.3 Type Annotation 사용 시 주요 타입

1. 원시 데이터 타입: string, number, boolean, null, undefined, symbol
2. 참조 타입: 배열(Array []), 튜플(tuple), 객체(object), 함수(Function), 클래스(Class)
3. 기타 타입: Void, Never, Any, Type Assertion(as), Union(타입 결합), Type Alias(개발자 타입 정의), Literal, 열거형(enum)

## 3.2 타입별 기초 사용법 개념

[TypeScript 한글 문서](https://typescript-kr.github.io/pages/the-handbook.html)

### 3.2.1 불리언

```tsx
tsx코드 복사
let isDone: boolean = false;
```

### 3.2.2 숫자 (number)

```tsx
tsx코드 복사
let decimal: number = 6;
let hex: number = 0xf00d;
let binary: number = 0b1010;
let octal: number = 0o744;
```

### 3.2.3 string

```tsx
tsx코드 복사
let color: string = "blue";
color = 'red';
```

### 3.2.4 string 사용법

1. 템플릿 문자열을 사용하면 여러 줄에 걸쳐 문자열을 작성할 수 있으며, 표현식을 포함시킬 수 있다. 이 문자열은 백틱(`) 문자로 감싸지고,` ${expr}` 과 같은 형태로 표현식을 포함시킬 수 있다.

```tsx
tsx코드 복사
let fullName: string = `KBS`;
let age: number = 37;

let sentence: string = `Hello, my name is ${fullName}.
I'll be ${age+1} years old next month.`;
```

### 3.2.5 배열

1. 타입스크립트는 자바스크립트처럼 값들을 배열로 다룰 수 있게 해준다.
2. 배열 타입은 두 가지 방법으로 사용할 수 있다.
3. 첫 번째 방법은 배열 요소들을 나타내는 타입 뒤에 `[]`를 쓰는 것이다.

```tsx
tsx코드 복사
let list: number[] = [1, 2, 3];
```

1. 두 번째 방법은 제너릭 배열 타입을 사용하는 것이다.

```tsx
tsx코드 복사
let list: Array<number> = [1, 2, 3];
```

### 3.2.6 튜플

1. 튜플 타입을 사용하면, 요소의 타입과 개수가 고정된 배열을 표현할 수 있다.
2. 단, 요소들의 타입이 모두 같을 필요는 없다. 예를 들어, number, string이 쌍으로 있는 값을 나타낼 수 있다.

```tsx
tsx코드 복사
// 튜플 타입으로 선언
let x: [string, number];

// 초기화
x = ["hello", 10]; // 성공

// 잘못된 초기화
x = [10, "hello"]; // 오류
```

1. 정해진 인덱스 외에 다른 인덱스에 있는 요소에 접근하면, 오류가 발생하며 실패한다.

```tsx
tsx코드 복사
x[3] = "world"; // 오류, '[string, number]' 타입에는 프로퍼티 '3'이 없습니다.
```

### 3.2.7 열거(Enum)

1. enum은 값의 `집합에 더 나은 이름을 붙여줄 수` 있다.

```tsx
tsx코드 복사
enum Color {Red, Green, Blue}
let c: Color = Color.Green;
```

1. 기본적으로 enum은 0부터 시작하여 멤버들의 번호를 매긴다.
2. 또는, `멤버 중 하나의 값을 수동으로 설정하여 번호를 바꿀 수` 있다.

```tsx
tsx코드 복사
enum Color {Red = 1, Green, Blue}
let c: Color = Color.Green;
```

1. 혹은 `모든 값을 수동으로 설정`할 수 있다.

```tsx
tsx코드 복사
enum Color {Red = 1, Green = 2, Blue = 4}
let c: Color = Color.Green;
```

1. Enum의 유용한 기능 중 하나는 매겨진 값을 사용해 enum 멤버의 이름을 알아낼 수 있다. 예를 들어:

```tsx
tsx코드 복사
enum Color {Red = 1, Green, Blue}
let colorName: string = Color[2];

console.log(colorName); // 값이 2인 'Green'이 출력됩니다.
```

`Red`를 1로 명시적으로 설정했기 때문에, 그 다음 값들은 1부터 시작하여 순차적으로 증가한다. 따라서 `Green`은 2가 되고, `Blue`는 3이 된다.

### 3.2.8 any 타입

1. `any` 타입은 TypeScript에서 매우 유연한 타입이다. `any`로 선언된 변수는 JavaScript처럼 어떤 종류의 값이든 가질 수 있으며, 컴파일러는 그 값의 타입을 검사하지 않는다. 예를 들어, `any` 타입의 변수는 문자열, 숫자, 함수, 객체 등 무엇이든 될 수 있다. 아래 코드에서 `any` 타입 변수인 `notSure`는 어떤 메서드나 프로퍼티를 호출해도 컴파일러가 오류를 발생시키지 않는다.

```tsx
tsx코드 복사
let notSure: any = 4;
notSure.ifItExists(); // 성공, 컴파일러가 타입을 검사하지 않으므로 가능
notSure.toFixed(); // 성공, toFixed 메서드는 존재하지만, 컴파일러는 검사하지 않음
```

1. 이 코드에서 `notSure` 변수는 숫자 4를 가지고 있지만, `any` 타입이기 때문에 `ifItExists` 같은 존재하지 않는 메서드를 호출해도 컴파일러는 오류를 발생시키지 않는다. 물론 `toFixed` 메서드는 숫자 타입에 실제로 존재하므로 런타임에서 정상적으로 호출된다.
2. 또한, `any` 타입은 타입의 일부만 알고 전체는 알지 못할 때 유용하다. 예를 들어, 여러 다른 타입이 섞인 배열을 다룰 수 있다.

```tsx
tsx코드 복사
let list: any[] = [1, true, "free"];

list[1] = 100;
```

### 3.2.9 Object 타입

1. `Object` 타입은 더 제한적이다. `Object`로 선언된 변수는 여러 타입의 값을 가질 수 있지만, 그 변수에 특정 메서드나 프로퍼티가 있는지 컴파일러가 검사한다. 따라서, 존재하지 않거나 `Object` 타입에서 보장되지 않는 메서드를 호출하면 컴파일러가 오류를 발생시킨다.

```tsx
tsx코드 복사
let prettySure: Object = 4;
prettySure.toFixed(); // 오류: 'toFixed' 메서드는 'Object' 타입에 존재하지 않음
```

1. 여기서 `prettySure` 변수는 숫자 4를 가지고 있지만, `Object` 타입으로 선언되었기 때문에 TypeScript 컴파일러는 `toFixed` 메서드가 `Object` 타입에 없다고 판단하여 오류를 발생시킨다.

### 결론

1. `any` 타입은 타입 검사를 우회할 수 있는 강력한 도구지만, 남용하면 런타임 에러의 가능성이 커진다. 반면, `Object` 타입은 타입 검사 덕분에 더 안전한 코드 작성에 도움이 된다. `Object` 대신 구체적인 타입(예: number, string, object)을 사용하는 것이 더 바람직하다.

### 3.2.10 void

1. `void`는 어떤 타입도 존재할 수 없음을 나타내기 때문에 `any`의 반대 타입이다.
2. `void`는 보통 함수에서 반환값이 없을 때 반환 타입을 표현하기 위해서 쓰인다.

```tsx
tsx코드 복사
function warnUser(): void {
    console.log("This is my warning message");
}
```

## 3.3 TypeScript 문법 실습

### 3.3.1 타입스크립트 REPL 툴

1. REPL은 즉시 코드를 실행하고 결과를 확인할 수 있어, 새로운 언어를 배우거나 코드 스니펫을 테스트하는 데 유용하다.

[TS Playground - An online editor for exploring TypeScript and JavaScript](https://www.typescriptlang.org/play)

### 3.3.2 변수 Type 코딩하고 실행하기

1. 함수에 전달되는 파라미터 변수를 매개변수라고 한다.
2. 함수 내에 값을 전달할 때 사용하는 매개변수(파라미터)들에도 타입을 지정할 수 있다.
3. 함수의 결과값도 타입을 지정할 수 있으며, 결과값이 없으면 `void`를 지정한다.

```tsx
// 변수별 타입을 지정하고 값을 할당합니다.
var memberName: string = "김범수";
let age: number = 40;
let price: number = 5000;
const isMale: boolean = true;

let totalPayPrice: number = 0;

// 함수가 반환값이 없을 때는 함수명():void{}
// 함수에 전달되는 파라미터(매개변수)에도 타입을 지정합니다.
// 함수의 결과값에는 void값을 할당합니다.
function showTotalPrice(price: number, count: number): void {
  totalPayPrice = price * count;
  console.log("총 금액은 " + totalPayPrice + "원 입니다.");
}

console.log("사용자명:", memberName);
console.log("성별:", isMale);
showTotalPrice(price, 2);

// 컴파일 및 실행 명령어
// tsc --strictNullChecks type-variable.ts
// node type-variable.js
```

## 3.4 타입스크립트의 타입 추론 이해하기

1. 타입스크립트에서는 변수의 타입을 명시적으로 지정하지 않아도, 값을 할당하면 타입을 자동으로 추론한다. 이 기능을 이해하는 것은 타입스크립트를 더 효과적으로 사용하는 데 매우 중요하다. 아래 예제들을 통해 타입 추론 개념을 자세히 알아보자.

### 3.4.1 변수 타입 추론

1. 타입스크립트에서는 변수에 값을 할당할 때, 그 값의 타입을 자동으로 추론한다. 예를 들어, 문자열, 숫자, 불린 값을 할당하면 각각의 타입으로 자동 인식된다.

```tsx
var memberName = "김범수"; // 문자열(string)로 자동 인식된다.
var price = 5000;          // 숫자(number)로 자동 인식된다.
var isMale = true;         // 불린(boolean)으로 자동 인식된다.
```

1. 위 예제에서 `memberName`은 문자열, `price`는 숫자, `isMale`은 불린으로 자동 인식된다. 타입을 명시하지 않았지만, 타입스크립트가 할당된 값을 기반으로 타입을 추론한 것이다.
2. 타입스크립트가 `memberName`을 문자열로 인식했기 때문에, 문자열에서 사용할 수 있는 속성을 문제없이 사용할 수 있다.

```tsx
console.log("회원명은 문자열로 인식되어 문자열 속성을 사용할 수 있다:", memberName.length);
```

1. 위 코드에서는 `memberName.length`를 사용했는데, `length` 속성은 문자열에서 사용할 수 있으므로 정상적으로 동작한다.
2. 하지만, 타입스크립트가 숫자로 추론한 변수에 문자열에서 사용하는 속성이나 메서드를 사용하려고 하면 타입 에러가 발생한다.

```tsx
// 아래 코드는 에러를 발생시킨다. price는 숫자형으로 추론되었기 때문에 length 속성을 사용할 수 없다.
// console.log("가격은 숫자형으로 인식되어 숫자에서 사용할 수 없는 속성을 사용할 경우 에러 발생:", price.length);

// price가 숫자형으로 추론되었기 때문에 substring 메서드를 사용할 수 없다.
// console.log("가격은 숫자형으로 인식되어 숫자에서 사용할 수 없는 메서드를 사용할 경우 에러 발생:", price.substring(0, 2));
```

1. 이처럼 타입스크립트는 변수가 어떤 타입인지 자동으로 인식하고, 그에 따라 사용할 수 있는 속성이나 메서드를 제한한다.

### 3.4.2 객체 타입 추론

1. 객체의 속성에 타입을 명시적으로 지정하지 않아도, 타입스크립트는 값을 할당한 후 그 값을 기반으로 속성의 타입을 추론한다.

```tsx
var user = {
  id: 1,                             // 숫자형(number)으로 추론된다.
  name: "John Doe",                  // 문자열(string)으로 추론된다.
  email: "john.doe@example.com",     // 문자열(string)으로 추론된다.
};
```

1. 위에서 `user` 객체의 `id`는 숫자, `name`과 `email`은 문자열로 자동 추론된다. 따라서 `name.length`와 같은 문자열 속성을 정상적으로 사용할 수 있다.

```tsx
console.log("회원명은 문자열로 인식되어 문자열 속성을 사용할 수 있다:", user.name.length);
```

1. 반면, `id`는 숫자형으로 추론되었기 때문에, 문자열에서 사용할 수 있는 `length` 속성을 사용하려고 하면 타입 에러가 발생한다.

```tsx
// 아래 코드는 에러를 발생시킨다. id는 숫자형으로 추론되었기 때문에 length 속성을 사용할 수 없다.
// console.log("회원 아이디는 숫자형으로 인식되어 숫자에서 사용할 수 없는 속성을 사용할 경우 에러 발생:", user.id.length);
```

### 3.4.3 함수의 반환값 타입 추론

1. 타입스크립트는 함수의 매개변수에 타입을 지정하면, 반환값의 타입을 자동으로 추론한다. 함수의 매개변수는 타입을 지정해주어야 하지만, 반환값은 타입스크립트가 자동으로 추론해준다.

```tsx
function plus(a: number, b: number) {
  return a + b;  // 반환값은 숫자형으로 자동 추론된다.
}
```

1. 위 함수에서 `plus` 함수는 두 숫자를 더하여 반환한다. 반환값의 타입을 명시하지 않았지만, 타입스크립트는 반환값이 숫자형이라고 자동으로 추론한다.

```tsx
tsx코드 복사
console.log(plus(1, 2));  // 정상적으로 동작한다: 3
```

1. 하지만, 반환값이 숫자형으로 추론되었기 때문에 숫자형에서 사용할 수 없는 속성이나 메서드를 사용하면 에러가 발생한다.

```tsx
// 아래 코드는 에러를 발생시킨다. 반환값이 숫자형이므로 length 속성을 사용할 수 없다.
// console.log(plus(1, 2).length);
```

### 3.4.4 결론

1. 타입스크립트의 타입 추론 기능은 코드의 가독성을 높이고, 개발자가 타입을 명시적으로 지정하지 않아도 안전한 코드를 작성할 수 있도록 도와준다. 그러나 타입 추론이 잘못된 경우에는 예상치 못한 타입 에러가 발생할 수 있으므로, 타입 추론을 이해하고 올바르게 사용하는 것이 중요하다.

## 3.5 Type Assertion(타입 단언)과 `any` 타입 사용하기

1. *Type Assertion(타입 단언)**은 개발자가 해당 타입에 대해 정확한 확신이 있을 때 사용하는 타입 지정 방식이다. 여기서 "Assertion"이라는 용어는 '단언하다', '확고히 말하다(역설하다)'는 의미를 가지고 있다. Type Assertion은 타입스크립트 컴파일러에게 "이 값은 반드시 이 타입이어야 해" 혹은 "이 값은 이 타입일 거야"라고 알려주며, 해당 값의 타입을 명시적으로 지정해주는 방식이다.
2. 타입 단언은 주로 타입스크립트의 타입 캐스팅(타입 변환) 시 사용된다. 컴파일러가 타입을 체크하지 않거나 데이터의 구조를 신경 쓰지 않아도 된다고 확신할 때 사용하는 것이 적합하다. 타입스크립트에서 `as` 키워드나 angle-bracket(`<타입>`) 문법을 사용하여 타입 단언을 구현할 수 있다.

### 3.5.1 `any` 타입의 이해

1. `any` 타입은 타입스크립트에서 타입이 어떤 것인지 정확히 모를 때 사용할 수 있는 타입이다. 예를 들어, 프론트엔드와 백엔드 간의 데이터 교환, 외부 시스템과의 연동, 또는 다른 팀이 만든 결과 데이터를 받을 때 정확한 타입을 모르는 경우에 사용한다. 하지만 타입스크립트 기반 애플리케이션을 개발할 때 `any` 타입을 남발하는 것은 바람직하지 않다.
2. `any` 타입으로 정의된 변수는 데이터 타입을 변경해 값을 할당해도 문제가 발생하지 않는다.

```tsx
let notSure: any = 4;
notSure = "maybe a string instead";
notSure = false;

console.log("notSure", notSure);
```

1. 위 예제에서 `notSure` 변수는 처음에는 숫자였지만, 이후 문자열과 불린 값으로 변경되었다. `any` 타입은 이런 유연한 변수를 허용한다.

### 3.5.2 Type Assertion을 사용한 타입 변환

1. Type Assertion을 사용하면 `any` 타입을 특정 타입으로 변환할 수 있다. 아래는 `any` 타입을 문자열로 변환하는 두 가지 방법이다.

```tsx
let fullName: any = "KBS-Kim";

// 타입 어서션 방법 1: as 키워드 사용
let fullNameLength1: number = (fullName as string).length;

// 타입 어서션 방법 2: angle-bracket 문법 사용
let fullNameLength2: number = (<string>fullName).length;

const companyName = "MSoftware" as string;
console.log("companyName===>", fullNameLength1, fullNameLength2, companyName);
```

1. 위 예제에서 `fullName`은 처음에 `any` 타입으로 선언되었지만, 타입 어서션을 통해 문자열로 변환되었다. `fullNameLength1`과 `fullNameLength2`는 각각 `as` 키워드와 angle-bracket 문법을 사용해 문자열의 길이를 계산했다.

### 3.5.3 Type Assertion을 사용한 객체 타입 지정

1. Type Assertion은 객체를 특정 인터페이스로 지정할 때도 사용된다. 아래는 `User` 인터페이스와 그 인터페이스를 기반으로 객체를 정의한 예제이다.

```tsx
// User 인터페이스 정의
interface User {
  id: number;
  name: string;
  email: string;
  telephone?: string; // 선택적(옵셔널) 속성은 속성명?로 정의한다.
}

// 사용자 객체변수/타입 정의 및 값 할당
let user = {} as User;
user.id = 1;
user.name = "Eddy.Kange";

console.log("user===>", user);
```

1. 위 코드에서는 `User`라는 인터페이스를 정의하고, `user` 객체를 이 인터페이스로 타입 단언하였다. 이렇게 하면 `user` 객체는 `User` 인터페이스에서 정의한 속성들을 가지게 된다. `telephone` 속성은 선택적(옵셔널) 속성으로 정의되었기 때문에 반드시 값을 할당하지 않아도 된다.

### 3.5.4 코딩 핵심 내용 설명

1. **`any` 타입**은 타입을 정확히 모를 때 사용하는 타입이다. 주로 프론트엔드와 백엔드 간의 데이터 교환, 외부 시스템과의 연동, 다른 팀이 만든 데이터를 받을 때 사용된다. 그러나 `any` 타입을 너무 많이 사용하는 것은 바람직하지 않다.
2. **Type Assertion**은 개발자가 타입에 대해 확신이 있을 때 사용하는 타입 지정 방식이다. 컴파일러보다 개발자가 해당 타입을 더 잘 알고 있을 때, 타입스크립트에게 이 값의 타입을 명시적으로 알려주는 역할을 한다. Type Assertion은 `as` 키워드나 angle-bracket(`<타입>`) 문법을 사용해 구현할 수 있다.

## 3.6 여러 타입으로 타입 정의하고 값 제한하기 - Union Type

타입스크립트는 기존의 타입들을 이용해 새로운 타입을 정의하고 구성하는 방법을 제공한다. 새로운 타입을 재구성하는 방식으로는 Union과 Generic 방식 두 가지가 있다.

이번 시간에는 Union에 대해서만 다루고, 다른 섹션에서 Generic에 대해 다루겠다. Union은 타입이 여러 타입 중 하나일 수 있음을 선언하는 방법으로, 하나의 변수에 여러 타입을 지정할 수 있다.

```tsx
let productCode: string | number;
```

예제 코드처럼 `|` 문자를 이용해 여러 타입을 반복 지정할 수 있다.

### 3.6.1 Union Type 예시 코드 코딩하고 실행하기

```tsx
// Union 타입 예제

// productCode 변수에 문자열과 숫자 타입 2개 타입을 동시에 지정합니다.
let productCode: string | number = "10000";

productCode = 20000;
console.log("productCode===>", productCode);

productCode = "P-20000";
console.log("productCode===>", productCode);

// 함수에 유니온 타입 지정하기
function getCode(code: number | string): string {
  if (typeof code === "number") {
    code = "P-" + code.toString();
  }
  return code;
}

console.log("getCode:", getCode(1000));
console.log("getCode:", getCode("P-2000"));

// 두번째 예시
function getProductCode(pcode: string | number): void {
  if (typeof pcode === "string") {
    console.log(`문자열값: ${pcode}`);
  } else if (typeof pcode === "number") {
    console.log(`숫자형값: ${pcode}`);
  }
}

console.log("getProductCode:", getProductCode("PID-1000"));
console.log("getProductCode:", getProductCode("1000"));

// 배열 내에 여러 데이터 유형을 할당하고 해당 배열의 타입을 Union 타입으로 정의할 수 있습니다.
const userData: (string | number | boolean)[] = ["홍길동2", 40, false]; // Union Type

console.log(
  `${userData[0]} 님은 ${userData[1]}살이고 ${
    userData[2] == true ? "기혼" : "미혼"
  } 입니다.`
);

// Union 타입을 이용해 타입의 특정값만을 지정할 수 있습니다.
// Enum(열거형) 타입과 유사하게 특정 값만을 지정할 수 있습니다.
type ProcessStates = "open" | "closed";
let state: ProcessStates = "open";
console.log("state:", state);

type OddNumbersUnderTen = 1 | 3 | 5 | 7 | 9;
let oddNumber: OddNumbersUnderTen = 3;
console.log("oddNumber:", oddNumber);
```

### 3.6.2 코딩 핵심 내용 설명

1. **Union**을 이용하면 변수에 여러 타입을 지정할 수 있다.
2. 배열 내 여러 타입의 값을 담을 수도 있다.
3. 추후 배울 type alias에 여러 값을 할당할 수 있다.
4. 유니온 타입에 특정 값들을 설정해 변수의 값을 지정된 값들로 제한하는 기능도 제공한다. (Enum 열거형 타입과 동일 기능 제공)

------

## 3.7 Type Alias

타입스크립트에서는 개발자가 정의한 타입을 만들어 사용할 수 있다. `type` 키워드를 이용해 새로운 타입을 정의하고, 이를 변수나 객체, 함수 등에 적용할 수 있다.

### 3.7.1 Type Alias 예시 코드 코딩하고 실행하기

```tsx
// Case 1) 타입 별칭을 사용하여 두 가지 타입을 지원하는 새로운 타입을 정의합니다.
type StringOrNumber = string | number;
let sample: StringOrNumber;
sample = "hello";
console.log("sample:", sample);

sample = 123;
console.log("sample:", sample);

// Case 2) 복잡한 타입도 별칭으로 정의할 수 있습니다.
// MemberType 타입 별칭 구조와 속성 타입을 정의합니다.
type MemberType = {
  name: string;
  age: number;
  address: { city: string; country: string };
};

// 해당 MemberType 타입의 변수를 선언하고 값을 할당합니다.
let personData: MemberType = {
  name: "John Doe",
  age: 25,
  address: {
    city: "Seoul",
    country: "South Korea",
  },
};

// 특정 함수에 파라메터로
function printPersonInfo(person: MemberType) {
  console.log(
    `name: ${person.name}, age: ${person.age}, address: ${person.address.city}, ${person.address.country}`
  );
}

printPersonInfo(personData);

// Case 3) 함수의 구조를 타입으로 정의하기
// 마이너스 함수를 타입으로 minusFunc 타입으로 정의함
// calFunc() 함수의 구조를 타입으로 정의합니다.
// type 함수타입명 =(파라메터)=> 반환값 타입
type calFuncType = (a: number, b: number) => number;

function plus(a: number, b: number) {
  return a + b;
}

function minus(a: number, b: number): number {
  return a - b;
}

// 함수의 파라메터로 특정 함수를 전달할 때는 전달 함수의 파라메터명:(함수 파라메터명:타입, 파라메터명:타입) => 함수 반환타입 형식으로 정의
// calculate() 함수에 파라메터(인수)로 전달되는 함수의 파라메터명과 타입, 반환값을 어떻게 정의하는지 확인하세요.
// 여러 개의 기초 함수를 만들어두고 해당 기초 함수의 공통 함수 타입을 정의 후 특정 함수에 파라메터로 전달해 하나의 함수를 통해 선택적으로 매개변수 함수를 실행할 수 있습니다.
function calculate(a: number, b: number, calFunc: calFuncType): number {
  return calFunc(a, b);
}

// calculate 함수에 plus, minus 함수를 파라메터로 전달해 해당 전달 함수의 로직으로 계산을 수행하게 하는 예시
console.log(calculate(10, 5, plus));
console.log(calculate(10, 5, minus));

// Case 4: 함수 파라메터 인수와 반환값을 위한 type 정의
type OperationInput = {
  a: number;
  b: number;
};

type OperationOutput = {
  result: number;
};

// 함수 구현
function addNumbers(input: OperationInput): OperationOutput {
  const { a, b } = input;
  return { result: a + b };
}

// 사용 예시
const input: OperationInput = { a: 5, b: 3 };
const output: OperationOutput = addNumbers(input);
console.log(output); // { result: 8 }
```

### 3.7.2 코딩 핵심 내용 설명

1. 단일 타입에 여러 타입 지원을 위해 **Union**을 통해 정의할 수 있다.
2. 복잡한 객체의 구조도 별도 타입으로 정의할 수 있다.
3. 함수의 구조도 타입으로 정의해서 사용할 수 있다.
4. 자바스크립트 함수는 함수의 매개변수로 특정 함수를 전달할 수 있다.
5. 함수의 매개변수로 전달되는 함수의 타입을 함수 타입으로 정의해 사용할 수 있다.

------

## 3.8 Literal & Enum Type & Union Type

Literal Type과 Enum Type은 데이터의 집합을 표현하는 타입이다. Literal Type 변수나 Enum(열거형) Type은 미리 정해진 문자열 값들이나 숫자 값들을 미리 정의해두고, 해당 범위 내의 값들만을 사용할 수 있게 강제하는 기능을 기본으로 제공한다. Enum 타입은 열거형이라고 말하며 `enum`이라는 키워드를 사용해 여러 개의 상수 셋을 정의해 구현한다.

### 3.8.1 Literal & Enum Type 예시 코드 코딩하고 실행하기

```tsx
// Case 1) 변수 값으로 문자열 Literal Type 사용하기
let genderType: "Male" | "Female";

genderType = "Male";
console.log("genderType:", genderType);

genderType = "Female";
console.log("genderType:", genderType);
// genderType = "None"; // 스트링 리터널 타입에서 사전 정의된 값이 아니여서 에러 발생

type User = {
  name: string;
  age: number;
  userType: "admin" | "user";
  address: { city: string; country: string };
};

const user: User = {
  name: "John Doe",
  age: 25,
  userType: "admin",
  address: {
    city: "Seoul",
    country: "South Korea",
  },
};

// Case 2) 함수의 반환값으로 숫자형 Literal Type 사용하기
function getUserType(user: User): 1 | 2 {
  if (user.userType === "admin") {
    return 1;
  } else {
    return 2;
  }
}

console.log("getUserType:", getUserType(user)); // 1

// Case 3) 열거형(Enum) 타입

// 게시글 게시 상태코드 열거형
enum DisplayType {
  NoneDisplay = 0,
  Display = 1,
}

const displayCode = 1;
const display = displayCode as DisplayType;

if (display === DisplayType.Display) {
  console.log("해당 게시글은 게시 상태입니다.");
}

// SNS 타입 열거형
enum SNSType {
  None = "",
  Facebook = "facebook",
  Instagram = "instagram",
  Twitter = "twitter",
}

// 회원가입 상태 타입 열거형
// 상수값을 지정하지 않으면 0부터 시작하는 숫자값이 할당됩니다.
enum EntryStatus {
  Inactive, // 0
  Active, // 1
  Pending, // 2
}

// 회원 유형 타입 열거형
enum MemberType {
  Admin = 2,
  User = 1,
  Guest = 0,
}

type Member = {
  id: number;
  email: string;
  password: string;
  entryStatus: EntryStatus;
  memberType: MemberType;
  snsType: SNSType;
  address: { city: string; country: string };
};

const snsTypeCode = "Facebook";

// 해당 MemberType 타입의 변수를 선언하고 값을 할당합니다.
let member: Member = {
  id: 1,
  email: "test@test.co.kr",
  password: "dfdfdfd23sdsdsds..",
  memberType: MemberType.Admin,
  snsType: snsTypeCode as SNSType,
  entryStatus: EntryStatus.Pending,
  address: {
    city: "Seoul",
    country: "South Korea",
  },
};

console.log("member:", member);

// Case 4: Union 타입을 이용해 타입의 특정 값만을 지정할 수 있습니다.
// Enum(열거형) 타입과 유사하게 특정 값만을 지정할 수 있습니다.
type ProcessStates = "open" | "closed";
let state: ProcessStates = "open";
console.log("state:", state);

type OddNumbersUnderTen = 1 | 3 | 5 | 7 | 9;
let oddNumber: OddNumbersUnderTen = 3;
console.log("oddNumber:", oddNumber);
```

### 3.8.2 코딩 핵심 내용 설명

1. **Literal Type**은 문자열, 숫자형 리터널 타입이 존재한다.
2. **Enum Type(열거형 타입)**은 `enum`으로 선언하고 여러 개의 상수 셋을 정의해 구현한다.
3. **Enum Type** 대신 **Union Type**을 통해 특정 값 범위를 제한할 수도 있다.
4. 열거형 상수에 값을 할당하지 않으면 숫자형 값이 순차적으로 0부터 할당된다.