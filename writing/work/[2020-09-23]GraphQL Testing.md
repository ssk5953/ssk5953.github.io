# GraphQL(Apollo Server) 테스트
아폴로 서버에서 간단하게 테스트를 구성할 수 있는 패키지를 제공한다.
패키지를 불러온 후에 아폴로 서버를 만들면 바로 테스트를 할 수 있게 해준다.

## 패키지 설치
아폴로 서버 테스트 패키지 `apollo-server-testing`을 설치한다. 테스트 진행은 `jest`로 한다.
타입스크립트는 `jest`의 타입을 모르기 때문에 `@types/jest`도 함께 설치해준다.

### 패키지 설치
```bash
npm install --save-dev apollo-server-testing jest @types/jest
```

### 스키마 작성
```typescript
// schema.ts
type query {
    test(id: String): Boolean
}
```

### 리졸버(해석기) 작성
```typescript
// resolver.ts
const userResolver: IResolver = {
    Query: {
        test: async (parent, { id }) => {
            return true;
        }
    }
}
```

### test.ts(테스트 스크립트)
```typescript
import { ApolloServer, gql } from "apollo-server-express";
import { createTestClient } from "apollo-server-testing";
import schema from "../schema";
import resolver from "../resolver;

const server = new ApolloServer({
    schema,
    resolver
});

const { query, mutate } = createTestClient(server);

test("test", async () => {
    const test1 = gql`
        query {
            test(id: "sdf")
        }
    `;

    const {
        data: { test }
    } = await query({ query: test1 });

    expect(test).toEqual(true);
});
```

### package.json
```json
{
    "devDependencies": {
        "@types/jest": "^26.0.14",
        "jest": "^26.4.2",
        "apollo-server-testing": "^2.18.0",
        ...
    },
    "script": {
        "test": "jest"
    }
}
```

### 실행
```bash
npm run test
```
