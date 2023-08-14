---
title: "typescript 코딩 스타일"
date: 2023-08-14

---

### 참조문서

이 코딩 스타일은 아래의 문서를 참고함.

* [POCU c# 코딩 표준](https://docs.popekim.com/ko/coding-standards/csharp)

<!-- ### IDE 도우미 -->
<!-- `WebStorm`에서 import할 수 있는 세팅은 [여기서]() 다운받을 수 있습니다. -->

<!-- `Visual studio Code`에서 import할 수 있는 세팅은 [여기서]() 다운받을 수 있습니다. -->

## I. 기본 규칙

0. `any`, `as`, `!(non-assertion)` 키워드를 사용하지 않으려 노력한다.

1. 함수 이름은 기본적으로 동사

   ```ts
   function getAge(): number {};

   function updateUser(): void {};
   ```

2. 변수 & 함수 이름은 `camelCase` 사용한다.

   ```ts
   const someNumber = 1;

   function someMethod(someParam: number): void {}
   ```

3. 상수의 이름은 모두 대문자로 하되 밑줄로 각 단어를 분리한다. 참조형의 경우 `as const`키워드를 붙여준다.

   ```ts
   const SOME_CONSTANT = 1;

   const COLOR_PALETTE = ["red", "green", "blue"] as const;
   ```

4. `boolean` 값은 앞에 `is`, `has`, `can`, `should` 중에 하나를 붙인다.

    ```ts
    const isValidYear = true;
    const hasId = true;
    const canCreate = true;
    const shouldRedirect = true;
    ```

5. 단순히 `boolean` 상태를 return하는 함수 이름은 최대한 `is`, `can`, `has`, `should` 를 사용한다.
( 부자연스러울 경우 상태를 나타내는 다른 동사 사용 )

   ```ts
   function isAlive(Person person): boolean;
   function hasChild(Person person): boolean;
   function canAccept(Person person): boolean;
   function shouldDelete(Person person): boolean;

   function exists(Person person): boolean;
   ```

6. 형변환은 최대한 `String(), Number(), Boolean()` 함수를 사용한다
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

8. 단순 반복문에 사용되는 변수가 아닌 경우엔 `i`, `e` 같은 변수명 대신 `index`, `employee` 처럼 변수에 저장되는 데이터를 한 눈에 알아볼 수 있는 변수명을 사용한다.

9. 값을 반환하는 함수의 이름은 무엇을 반환하는지 알 수 있게 짓는다.

    ```ts
    function getAge(): number {
      return 100;
    };
    ```

10. 익명 함수의 매개변수는 바로 앞에있는 값의 이니셜을 사용한다.
    ```ts
    // Bad
    const cpIds = controlPoints.map(x => x.id).filter(x => x > 100);

    // Good
    const cpIds = controlPoints.map(cp => cp.id).filter(cpId => cpId > 100);

    const livingMonsters = monster.filter(m => m.HP > 0).map(lm => lm.Name);
    ```

11. 3항연산자가 `1 depth`를 넘어가면 함수로 바꿔 작성한다
    ```ts
    // Bad
    const value = x ? y : z ? w : t; // 중간에 디버깅 걸기 애매함

    // Good
    const value = (() => {
      if (x) {
        return y;
      }

      if (z) {
        return w;
      }

      return t;
    })()
    ```

12. 변수에 할당된 값이 없다면 타입을 명시해준다.
    ```ts
    let result; // Bad
    let result: Monster[]; // Good

    if (x) {
      result = x.filter(y => y > 0);
    }
    else {
      result = z.map(zz => zz.id);
    }

    return result;
    ```

13. 재귀 함수는 이름 뒤에 `Recursive`를 붙인다.
    ```ts
    function FibonacciRecursive(): number {}
    ```

14. `enum`을 선언할 때는 앞에 `E`를 붙인다
    ```ts
    enum EDirection
    {
        North,
        South,
    }
    ```

15. 인터페이스를 선언할 때는 앞에 `I`를 붙인다.
    ```cs
    interface ISomeInterface;
    ```

16. 함수에서 `null`을 반환할 때는 함수 이름 뒤에 `OrNull`을 붙인다.

    ```ts
    function GetNameOrNull(): string | null
    ```

17. `null` 매개변수를 사용할 경우 변수명 뒤에 `OrNull`를 붙인다

    ```ts
    function getUser(userIdOrNull: number | null)
    {
    }
    ```

18. 매개변수가 3개 이상이면 `object`형태로 받는다
    ```ts
    // Good
    type Form = { name: string, age: number, phone: string };
    function initForm({ name, age, phone }: Form); // 매개변수 object

    // Bad
    function initForm(name: string, age: number, phone: string)
    ```

19. `switch` 문에서 `default:` 케이스가 절대 실행될 일이 없는 경우, `throw` 문을 추가한다.

    ```cs
    switch (type)
    {
      case 1:
        ...
        break;
      default:
        throw "절대 실행되면 안되는 코드. 혹시 실행되면 터트려서 확인하기";
    }
    ```

20. 컴포넌트는 각각 독립된 소스 파일에 있어야 한다. 단, 작은 컴포넌트 몇 개를 한 파일 안에 같이 넣어두는 것이 상식적일 경우 예외를 허용한다.

21. 상수로 사용하는 멤버 변수에는 `static readonly`를 사용한다.
    ```ts
    public static readonly MY_CONST_OBJECT = new MyConstClass();
    ```

22. 초기화 후 값이 변하지 않는 멤버변수는 `readonly`로 선언한다.

    ```ts
    public class Account
    {
      private readonly mPassword: string;

      public Account(password: string)
      {
        mPassword = password;
      }
    }
    ```

23. 싱글턴 패턴 대신에 정적(static) 클래스를 사용한다.

24. 외부로부터 들어오는 데이터의 유효성은 외부/내부 경계가 바뀌는 곳에서 검증(validate)하고 문제가 있을 경우 내부 함수로 전달하기 전에 반환해 버린다. 이는 경계를 넘어 내부로 들어온 모든 데이터는 유효하다고 가정한다는 뜻이다.
    ```ts
    const firstMemberIdOrNull = users.find(u => u.isMember);
    // 해당 id가 없을경우,
    // getRegisterDateById로 전달하기 직전 경계에서 예외처리
    if (!firstMemberIdOrNull)
    {
      return;
    }

    getRegisterDateById(firstMemberIdOrNull);

    ----------------------------------
    // 외부에서 항상 옳바른 id를 받는다고 가정
    function getRegisterDateById(userId: number): Date
    {
      return users.find(u => u.id === userId).joinDate;
    };
    ```

25. 따라서 내부 함수에서 예외(익셉션)를 던지지 않으려 노력한다. 예외는 경계에서만 처리하는 것을 원칙으로 한다.

26. 위 규칙의 예외: `enum` 형을 `switch` 문에서 처리할 때 실수로 처리 안 한 `enum` 값을 찾기 위해 `default:` 케이스에서 예외를 던지는 것은 허용.

    ```cs
    switch (accountType)
    {
        case AccountType.Personal:
            return something;
        case AccountType.Business:
            return somethingElse;
        default:
            throw new NotImplementedException($"unhandled switch case: {accountType}");
    }
    ```

  27. 재사용되지 않는 함수가 많아지지 않게 노력한다.

      ( 단, `unit test`가 꼭 필요한 경우 함수 추출 허용. )

## II. 소스 코드 포맷팅

1. 탭(tab)은 2칸을 탭으로 사용한다. [(Prettier) Tabs](https://prettier.io/docs/en/options.html#tabs)

2. 세미콜론을 사용한다. [(Prettier) Semicolons](https://prettier.io/docs/en/options.html#semicolons)

3. 쌍따옴표를 사용한다 [(Prettier) Quotes: false](https://prettier.io/docs/en/options.html#quotes)

2. 중괄호 안( `{ }` )에 코드가 한 줄만 있더라도 반드시 중괄호를 사용한다. [(ESLint) curly: all](https://eslint.org/docs/latest/rules/curly)

    ```ts
    // Good
    if (true) {
      return;
    }

    // Bad
    if (true) return; // 중간에 디버깅 걸기가 애매함.
    ```

3. 한 줄에 변수 하나만 선언한다.

   **틀린 방식:**

   ```cs
   int counter = 0, index = 0;
   ```

   **올바른 방식:**

   ```cs
   int counter = 0;
   int index = 0;
   ```


4. if/else를 사용할때 다음줄에 적는다 [(ESLint) brace-style: Stroustrup](https://eslint.org/docs/latest/rules/brace-style#rule-details:~:text=5-,One,-common%20variant%20of)
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
