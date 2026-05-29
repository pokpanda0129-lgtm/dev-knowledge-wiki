# Nitro

## 단어 뜻

- `Nuxt 2 서버`: Nuxt 2 Universal Mode에서 SSR HTML을 만들고 Vue 2 앱을 실행하는 Node.js 기반 서버입니다. `serverMiddleware`나 커스텀 서버를 붙이면 API 형태의 엔드포인트도 만들 수 있습니다.
- `Nitro`: Nuxt 3의 서버 엔진입니다. SSR HTML 생성, server route 실행, middleware/plugin 처리, 배포 결과물 생성을 맡습니다.
- `server route`: Nuxt 프로젝트의 `server/api`나 `server/routes` 아래에 두는 서버에서 실행되는 endpoint입니다. `server/api`는 보통 `/api` 경로로 호출하고, `server/routes`는 `/api` prefix 없이 직접 경로로 등록합니다.
- `.output`: `nuxt build` 후 만들어지는 배포 결과물입니다. 운영 환경에서 실행할 서버 코드와 정적 파일이 들어갑니다.
- `routeRules`: URL 패턴별로 prerender, redirect, cache, headers 같은 동작을 정하는 규칙입니다.

## 예시 스토리

오래된 커머스 매장에는 한 건물 안에 붙박이 작업실이 있습니다. 이 작업실은 상품 설명판을 만들고 가격표를 붙여 손님에게 보여줍니다. 지금 쓰는 Nuxt 2 Universal Mode의 서버는 이런 붙박이 작업실에 가깝습니다.

새 매장으로 바꾸면서 회사는 이동식 표준 작업실을 들여옵니다. 이 작업실은 같은 방식으로 상품 설명판을 만들지만, 일반 매장 건물에도 놓을 수 있고 작은 팝업 매장이나 빠른 임시 매장에도 맞춰 설치할 수 있습니다. Nuxt 3의 Nitro는 이런 이동식 표준 작업실에 가깝습니다.

또 이 작업실에는 규칙표가 붙어 있습니다. “이 안내판은 미리 만들어두기”, “오래된 번호는 새 번호로 안내하기”, “자주 묻는 안내는 잠깐 보관하기” 같은 규칙을 작업실 안에서 통일해 처리합니다. 그래서 Nitro는 단순히 SSR만 하는 서버라기보다, Nuxt 3 앱을 여러 운영 환경에 맞게 실행하고 포장하는 서버 엔진입니다.

## 한 줄 요약

Nitro는 Nuxt 3의 서버 엔진입니다. Nuxt 2의 Node.js SSR 서버처럼 HTML을 만들 수 있고, 여기에 server route, middleware, routeRules, 여러 배포 환경용 `.output` 생성을 Nuxt 3의 표준 서버 흐름으로 묶어줍니다.

## 개념 설명

Nuxt 2 Universal Mode도 서버에서 Vue 컴포넌트를 실행해 HTML을 먼저 만들 수 있습니다. 다만 이때의 서버는 Nuxt 2의 Node.js SSR 서버입니다. Nitro는 Nuxt 3로 넘어오면서 서버 쪽 실행을 담당하는 새 엔진입니다.

차이는 “서버가 있냐 없냐”나 “API 비슷한 엔드포인트를 만들 수 있냐 없냐”가 아닙니다. Nuxt 2의 Node.js 서버도 `serverMiddleware`나 커스텀 서버를 통해 API 형태의 요청을 처리할 수 있습니다. 다만 Nuxt 3처럼 `server/api` 파일을 자동으로 서버 API로 등록하는 표준 구조는 Nitro 쪽에 가깝습니다. 차이는 Nuxt 3에서는 그 서버 역할이 Nitro라는 독립적인 엔진으로 정리되면서, SSR, server route, middleware, routeRules, 배포 결과물 생성이 한 흐름으로 묶였다는 점입니다.

Nitro가 있다고 해서 Node 서버나 WAS가 사라지는 것도 아닙니다. Node는 Nitro가 실행될 수 있는 런타임 중 하나이고, WAS는 주문 생성, 결제, 회원 권한, 재고 차감처럼 도메인 규칙이 중요한 일을 계속 맡을 수 있습니다. Nitro는 브라우저와 WAS 사이에서 화면 렌더링, 프론트 친화 API 조합, 캐시나 redirect 같은 웹 계층의 처리를 담당하는 쪽에 가깝습니다.

예를 들어 Nuxt 페이지가 상품 상세를 SSR로 보여줘야 한다면 Nitro는 서버에서 페이지를 실행하고 HTML을 만듭니다. `server/api/products/[id]` 같은 경로 패턴으로 server route를 두면 브라우저가 `/api/products/123`처럼 호출할 수 있고, 서버 렌더링 중에는 내부 handler 호출처럼 동작해 불필요한 왕복을 줄일 수 있습니다.

`routeRules`는 URL별 운영 규칙을 붙이는 장치입니다. 특정 페이지를 prerender하거나, 오래된 경로를 redirect하거나, 일부 응답에 cache/header 정책을 줄 수 있습니다. 다만 사용자별 데이터나 권한이 섞인 응답을 공유 캐시에 넣으면 위험하므로, 캐시는 공개 데이터와 짧은 TTL부터 신중하게 적용하는 편이 안전합니다.

참고:
- [Nuxt Server](https://nuxt.com/docs/getting-started/server)
- [Nuxt Server Engine](https://nuxt.com/docs/guide/concepts/server-engine)
- [Nuxt Server Directory](https://nuxt.com/docs/guide/directory-structure/server)
- [Nuxt routeRules](https://nuxt.com/docs/guide/concepts/rendering#route-rules)
