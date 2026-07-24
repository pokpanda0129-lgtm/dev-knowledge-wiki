# JavaScript ES6 이해

## 단어 뜻

- `JavaScript`: 브라우저나 Node.js에서 실행되는 프로그래밍 언어입니다.
- `ECMAScript`: JavaScript가 어떤 문법과 기능을 가져야 하는지 정한 표준 이름입니다.
- `ES6`: 2015년에 나온 ECMAScript 버전입니다. `ES2015`라고도 부릅니다.

## 예시 스토리

작은 온라인 쇼핑몰 팀이 새 기획전용 상품 카드를 만들고 있습니다.
처음에는 상품명, 가격, 할인 문구, 배송 문구를 모두 포스트잇에 적어 책상 위에 펼쳐두었습니다.
상품이 몇 개 없을 때는 괜찮았지만, 카드가 늘어나자 어느 메모가 바뀌면 안 되는 원본인지 헷갈리기 시작했습니다.
한 직원은 같은 할인 문구를 매번 손으로 다시 적다가 오타를 냈고, 다른 직원은 필요한 가격만 꺼내려다 상품 번호까지 잘못 옮겼습니다.
팀은 바뀌면 안 되는 정보는 코팅된 카드에 적고, 수량처럼 바뀌는 정보는 지울 수 있는 칸에 적기로 했습니다.
자주 쓰는 안내 문구와 가격 계산 방식은 작은 양식으로 만들어두니, 새 상품 카드가 늘어나도 무엇을 가져와 어디에 채워야 하는지 훨씬 분명해졌습니다.

## 한 줄 요약

ES6는 JavaScript를 더 짧고 읽기 좋게 쓰도록 도와준 큰 문법 업데이트입니다. 실무에서는 `let`, `const`, 화살표 함수, 구조 분해, 모듈, `Promise` 같은 기능을 자주 만납니다.

## 개념 설명

ES6는 JavaScript의 특정 라이브러리가 아니라 언어 표준입니다. 그래서 React, Vue, Node.js 같은 도구를 쓰지 않아도 JavaScript 코드 자체에서 사용할 수 있는 문법과 기능을 말합니다.

대표적인 변화는 변수 선언 방식입니다. 예전에는 `var`를 많이 썼지만, ES6 이후에는 보통 `const`를 먼저 쓰고, 값 재할당이 필요할 때만 `let`을 씁니다.

```js
const productName = "Keyboard";
let quantity = 1;

quantity = quantity + 1;
```

`const`는 변수에 다른 값을 다시 넣지 않겠다는 뜻입니다. 객체나 배열 안의 내용까지 완전히 얼리는 것은 아니므로, 그 차이는 주의해야 합니다.

```js
const cart = [];

cart.push("Keyboard"); // 가능
// cart = ["Mouse"];   // 불가능
```

화살표 함수는 짧은 함수를 만들 때 자주 씁니다. 특히 배열을 돌면서 값을 바꾸거나 걸러낼 때 코드가 간결해집니다.

```js
const prices = [10000, 20000, 30000];

const discountedPrices = prices.map((price) => price * 0.9);
```

구조 분해는 객체나 배열에서 필요한 값만 꺼낼 때 유용합니다. API 응답에서 `name`, `price`처럼 화면에 필요한 값만 바로 꺼내는 상황에서 자주 보입니다.

```js
const product = {
  id: 1,
  name: "Keyboard",
  price: 30000,
};

const { name, price } = product;

console.log(`${name} 가격은 ${price}원입니다.`);
```

템플릿 리터럴은 문자열 안에 값을 자연스럽게 넣을 수 있게 해줍니다. 문자열을 여러 번 `+`로 이어 붙이는 코드보다 읽기 쉽습니다.

```js
const message = `${name} 상품이 장바구니에 담겼습니다.`;
```

스프레드 문법은 배열이나 객체를 복사하면서 일부만 바꿀 때 자주 씁니다. 원본을 직접 바꾸지 않고 새 값을 만드는 흐름에 잘 맞습니다.

```js
const user = {
  name: "Min",
  grade: "basic",
};

const upgradedUser = {
  ...user,
  grade: "premium",
};
```

모듈 문법은 파일을 나누어 코드를 관리할 때 씁니다. 한 파일에서 내보낸 함수를 다른 파일에서 가져와 사용할 수 있습니다.

```js
// price.js
export function formatPrice(price) {
  return `${price.toLocaleString()}원`;
}

// product-card.js
import { formatPrice } from "./price.js";

console.log(formatPrice(30000));
```

`Promise`는 시간이 걸리는 작업을 다룰 때 사용합니다. 예를 들어 서버에서 상품 정보를 받아온 뒤, 성공하면 화면에 보여주고 실패하면 에러 메시지를 보여주는 흐름을 표현할 수 있습니다.

```js
fetch("/api/products/1")
  .then((response) => response.json())
  .then((product) => {
    console.log(`${product.name}을 화면에 보여줍니다.`);
  })
  .catch(() => {
    console.log("상품 정보를 불러오지 못했습니다.");
  });
```

`async`와 `await`도 ES6라고 함께 묶어 말하는 경우가 많지만, 정확히는 ES2017에 추가된 문법입니다. ES6를 공부할 때는 먼저 `Promise`의 흐름을 이해하면 이후 비동기 문법을 배우기 쉽습니다.

ES6의 핵심은 새 문법을 많이 외우는 것이 아닙니다. 바뀌지 않는 값은 `const`로 고정하고, 작은 함수를 읽기 쉽게 만들고, 필요한 데이터만 꺼내 쓰고, 파일을 나누어 관리하는 방식에 익숙해지는 것이 중요합니다.
