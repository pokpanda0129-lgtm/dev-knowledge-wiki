# JS 얕은복사와 깊은복사

## 단어 뜻

- `복사`: 기존 값을 바탕으로 새 값을 만드는 것.
- `얕은복사`: 겉부분만 새로 만들고, 안쪽 객체는 기존 것을 같이 바라보는 복사.
- `깊은복사`: 안쪽 객체까지 새로 만들어서 기존 값과 분리하는 복사.
- `참조`: 객체가 실제로 있는 위치를 가리키는 연결 정보.

## 예시 스토리

친구에게 여행 계획표를 복사해서 줬다고 생각해보자.

겉표지는 각자 한 장씩 가지고 있는데, 안에 끼워 둔 숙소 예약 종이는 같은 원본을 같이 보고 있다. 그래서 친구가 숙소 예약 종이에 메모를 하면 내 계획표에서도 그 메모가 보인다. 이게 얕은복사 느낌이다.

반대로 표지뿐 아니라 안에 있는 숙소 예약 종이까지 전부 새로 복사해서 나눠 가지면, 친구가 자기 종이에 메모해도 내 종이는 바뀌지 않는다. 이게 깊은복사 느낌이다.

## 한 줄 요약

얕은복사는 바깥 껍데기만 새로 만들고 안쪽 객체는 공유한다. 깊은복사는 안쪽 객체까지 새로 만들어서 원본과 복사본이 서로 영향을 주지 않게 한다.

## 개념 설명

JavaScript에서 숫자, 문자열, boolean 같은 원시값은 값 자체가 복사된다.

```js
let a = 10;
let b = a;

b = 20;

console.log(a); // 10
console.log(b); // 20
```

하지만 객체와 배열은 값 전체가 변수 안에 직접 들어 있는 것이 아니라, 객체가 있는 위치를 가리키는 참조를 통해 다룬다.

```js
const user1 = { name: "Mina" };
const user2 = user1;

user2.name = "Jisoo";

console.log(user1.name); // Jisoo
```

`user1`과 `user2`는 이름만 다를 뿐 같은 객체를 바라보고 있다. 그래서 `user2`를 바꿨는데 `user1`도 바뀐 것처럼 보인다.

### 얕은복사

얕은복사는 객체의 첫 번째 층만 새로 만든다.

```js
const original = {
  name: "Mina",
  address: {
    city: "Seoul",
  },
};

const copied = { ...original };

copied.name = "Jisoo";
copied.address.city = "Busan";

console.log(original.name); // Mina
console.log(original.address.city); // Busan
```

`name`은 첫 번째 층에 있는 값이라 복사본에서 바꿔도 원본이 바뀌지 않는다.

하지만 `address`는 안쪽 객체다. 얕은복사는 `address` 객체 자체를 새로 만들지 않고 같은 객체를 공유한다. 그래서 `copied.address.city`를 바꾸면 `original.address.city`도 같이 바뀐다.

JavaScript에서 자주 쓰는 얕은복사 방법은 다음과 같다.

```js
const copiedObject = { ...original };
const copiedArray = [...items];
const copiedByAssign = Object.assign({}, original);
```

### 깊은복사

깊은복사는 안쪽 객체까지 새로 만든다.

```js
const original = {
  name: "Mina",
  address: {
    city: "Seoul",
  },
};

const copied = structuredClone(original);

copied.address.city = "Busan";

console.log(original.address.city); // Seoul
console.log(copied.address.city); // Busan
```

`structuredClone`을 사용하면 중첩된 객체도 새로 복사된다. 그래서 복사본의 안쪽 값을 바꿔도 원본은 그대로 남는다.

다만 깊은복사가 모든 상황에서 좋은 선택은 아니다. 데이터가 크면 복사 비용이 커질 수 있고, 함수나 특정 클래스 인스턴스처럼 그대로 복사하기 어려운 값도 있다. 실제 프로젝트에서는 데이터 구조와 브라우저 지원 범위를 확인해야 한다.

### JSON으로 깊은복사하기

예전 코드에서는 다음 방식도 자주 보인다.

```js
const copied = JSON.parse(JSON.stringify(original));
```

이 방식은 간단하지만 한계가 있다.

- 함수는 사라진다.
- `undefined` 값은 사라질 수 있다.
- `Date`는 문자열로 바뀐다.
- `Map`, `Set` 같은 값은 제대로 복원되지 않는다.

그래서 요즘은 가능하면 `structuredClone`을 먼저 고려하고, 복잡한 객체는 상황에 맞는 라이브러리나 직접 복사 로직을 사용한다.

### React나 Vue에서 왜 중요할까?

프론트엔드에서는 상태를 직접 바꾸지 않고 새 값으로 교체해야 하는 상황이 많다.

예를 들어 React에서 상태 안쪽 객체를 그대로 바꾸면 화면 갱신이 기대한 대로 일어나지 않거나, 이전 상태와 새 상태를 비교하기 어려워질 수 있다.

```js
const nextUser = {
  ...user,
  address: {
    ...user.address,
    city: "Busan",
  },
};
```

이 코드는 바깥 `user`도 새로 만들고, 안쪽 `address`도 새로 만든다. 즉 필요한 깊이까지만 직접 복사하는 방식이다.

## 그래서 뭐가 좋은데?

- 객체 안에 중첩 객체가 없고 간단히 새 배열이나 객체만 필요하다면 얕은복사가 충분하다.
- 중첩 객체를 수정해야 하고 원본이 바뀌면 안 된다면 깊은복사나 필요한 깊이까지의 복사가 필요하다.
- 상태 관리에서는 전체를 깊은복사하기보다, 바뀌는 경로만 새로 만드는 방식을 많이 사용한다.
- 외부 API 응답처럼 원본 데이터를 보존해야 한다면 복사 방식이 원본을 건드리지 않는지 확인해야 한다.

## 비교표

| 구분 | 얕은복사 | 깊은복사 |
| --- | --- | --- |
| 복사 범위 | 첫 번째 층만 새로 복사 | 안쪽 객체까지 새로 복사 |
| 중첩 객체 | 원본과 공유될 수 있음 | 원본과 분리됨 |
| 속도와 비용 | 비교적 가벼움 | 데이터가 크면 비용이 커짐 |
| 대표 방법 | spread, `Object.assign` | `structuredClone`, 라이브러리 |
| 주의점 | 안쪽 값을 바꾸면 원본도 바뀔 수 있음 | 복사할 수 없는 값이나 성능을 확인해야 함 |

## 요약

- 객체와 배열은 참조를 통해 다룬다.
- 얕은복사는 바깥만 새로 만들고 안쪽 객체는 공유할 수 있다.
- 깊은복사는 안쪽 객체까지 새로 만들어 원본과 분리한다.
- 프론트엔드 상태 변경에서는 바뀌는 경로만 새로 복사하는 패턴을 자주 쓴다.
- 복사 방법을 고를 때는 데이터 깊이, 성능, 복사할 값의 종류를 함께 봐야 한다.
