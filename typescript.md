---
title: "typescript 코딩 스타일"
date: 2023-10-16

---

### 참조문서

이 코딩 스타일은 아래의 문서를 참고함.

* [POCU c# 코딩 표준](https://docs.popekim.com/ko/coding-standards/csharp)

<!-- ### IDE 도우미 -->
<!-- `WebStorm`에서 import할 수 있는 세팅은 [여기서]() 다운받을 수 있습니다. -->

<!-- `Visual studio Code`에서 import할 수 있는 세팅은 [여기서]() 다운받을 수 있습니다. -->

## I. 기본 규칙
코드의 가독성을 최우선으로 함

1. `any`, `as`, `!(non-null-assertion)` 키워드를 사용하지 않으려 노력한다.

2. 변수 & 함수 이름은 `camelCase` 사용

   ```ts
   const someNumber = 1;

   function someMethod(someParam: number): void {}
   ```

3. 상수는 `SNAKE_CASE` 사용 ( 참조형의 경우 `as const`키워드 붙여주기 )

   ```ts
   const SOME_CONSTANT = 1;

   const COLOR_PALETTE = ["red", "green", "blue"] as const;
   ```

5. `boolean` 상태를 `return`하는 함수 or 변수 이름은 최대한 `is`, `can`, `has`, `should` 사용
( 부자연스러울 경우 상태를 나타내는 다른 동사 사용 )

   ```ts
   const isValidYear = true;
   const hasId = true;
   const canCreate = true;
   const shouldRedirect = true;
   
   function isAlive(Person person): boolean;
   function hasChild(Person person): boolean;
   function canAccept(Person person): boolean;
   function shouldDelete(Person person): boolean;

   function exists(Person person): boolean;
   ```

6. 형변환은 최대한 `String(), Number(), Boolean()` 함수를 사용
   ```ts
   // Good
   let num = Number(11);
   let str = String(num);
   let bool = Boolean(num);

   // Bad
   let x = 11;
   let s = x + "";
   let b = !!x;
   let n = +s;
   ```

7. 복수형 데이터는 `s`를 붙여준다.

11. 3항연산자가 `1 depth`를 넘어가면 함수로 바꿔 작성한다
    ```ts
    // Bad
    const value = x ? y : z ? w : t; // 중간에 브레이크 걸기 애매함

    // Good
    const value = (() => {
      if (x) {
        return y;
      }

      if (z) {
        debugger; // 중간에 브레이크 걸기 편함
        return w;
      }

      return t;
    })()
    ```

12. 변수에 할당된 값이 없다면 타입을 명시해준다.
    ```ts
    // Bad
    let result;
    
    // Good
    let result: TMonster[]; 

    if (x) {
      result = x.filter(y => y > 0);
    }
    else {
      result = z.map(zz => zz.id);
    }

    return result;
    ```

14. `enum`을 선언할 때는 앞에 `E`를 붙인다
    ```ts
    enum EDirection
    {
        North,
        South,
    }
    ```

15. `type alias`를 선언할 때는 앞에 `T`를 붙인다
    ```tsx
    type TUser = { name: string; age: nunber; }
    ```

16. `interface`를 선언할 때는 앞에 `I`를 붙인다.
    ```cs
    interface ISomeInterface;
    ```

17. 함수에서 `null, undefined`을 `return`할 때는 함수 이름 뒤에 `OrNull, orUndef`을 붙인다.

    ```ts
    // Good
    function getUserOrNull(id: number): TUser | null

    // Bad
    function getUser(id: number): TUser | null
    ```

19. 매개변수가 3개 이상이면 `object`로 받는다
    ```ts
    // Good
    type Form = { 
      Guid: numer;
      id: number;
      age: number;
    };
    function initForm({ Guid, id, age }: Form);

    // Bad
    function initForm(Guid: number, id: number, age: number)
    ```

21. 외부로부터 들어오는 데이터의 유효성은 외부/내부 경계가 바뀌는 곳에서 검증(validate)하고 문제가 있을 경우 내부 함수로 전달하기 전에 `return`해 버린다. 이는 경계를 넘어 내부로 들어온 모든 데이터는 유효하다고 가정한다는 뜻이다.

22. 따라서 내부 함수에서 예외를 던지지 않으려 노력한다. 예외는 경계에서만 처리하는 것을 원칙으로 한다.

23. 위 규칙의 예외: `enum` 형을 `switch` 문에서 처리할 때 실수로 처리 안 한 `enum` 값을 찾기 위해 `default:` 케이스에서 예외를 던지는 것은 허용.

    ( + `debugger`와 [babel-plugin-no-debugger](https://babeljs.io/docs/babel-plugin-transform-remove-debugger)를 같이 사용해도 좋음 )

    ```cs
    switch (accountType)
    {
        case AccountType.Personal:
            return something;
        case AccountType.Business:
            return somethingElse;
        default:
            debugger;
            throw new NotImplementedException($"unhandled switch case: {accountType}");
    }
    ```

  24. 재사용되지 않는 함수는 만들지 않으려고 노력한다.

      **단 아래의 경우 함수 추출 허용**
         1.  `unit test`가 꼭 필요한 경우
         2.  코드가 길어서 가독성을 해치는 경우
         3.  메모이제이션 해야하는 경우

## II. 소스 코드 포맷팅

1. 탭(tab)은 2칸을 탭으로 사용한다. [(Prettier) Tabs](https://prettier.io/docs/en/options.html#tabs)

2. 세미콜론을 사용한다. [(Prettier) Semicolons](https://prettier.io/docs/en/options.html#semicolons)

3. 쌍따옴표를 사용한다 [(Prettier) Quotes: false](https://prettier.io/docs/en/options.html#quotes)

4. 중괄호 안( `{ }` )에 코드가 한 줄만 있더라도 반드시 중괄호를 사용한다. [(ESLint) curly: all](https://eslint.org/docs/latest/rules/curly)

    ```ts
    // Good
    if (true) {
      return;
    }

    // Bad
    if (true) return; // 중간에 디버깅 걸기가 애매함.
    ```

5. 제어문(if/else, try/catch) 사용시 다음줄에 적어준다 [(ESLint) brace-style: Stroustrup](https://eslint.org/docs/latest/rules/brace-style#rule-details:~:text=5-,One,-common%20variant%20of)
    ```ts
    // Good
    if (true) {
      return;
    }
    else {
      return;
    }

    // Bad
    if (true) {
      return;
    } else {
      return;
    }
    ```
