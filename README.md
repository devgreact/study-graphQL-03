# React 프로젝트
- Apollo 클라이언트(https://www.apollographql.com/docs/react/get-started/)
```js
npx create-react-app ./

// 아폴로 클라이언트 모듈 적용
npm install @apollo/client graphql
```

## 1. 아폴로 클라언트 모듈 임포트

index.js

- client : 	GraphQL 서버로와 정보를 주고받을 ApolloClient 객체
- uri : 	GraphQL 서버의 주소
- cache	: InMemoryCache를 통한 캐시 관리(https://www.apollographql.com/docs/react/caching/cache-configuration/)

```js
import { ApolloProvider} from '@apollo/client';
import { ApolloClient, InMemoryCache } from '@apollo/client'

const client = new ApolloClient({
  uri: 'http://localhost:4000',
  cache: new InMemoryCache()
});

```
### 내부 요소들을 ApolloProvider 로 감싸준다.
index.js

```js
const root = ReactDOM.createRoot(document.getElementById('root'));
root.render(
  <ApolloProvider client={client}>
    <App />
  </ApolloProvider>
);
```

index.js 전체 코드
```js
import React from 'react';
import ReactDOM from 'react-dom/client';
import './index.css';
import App from './App';
import reportWebVitals from './reportWebVitals';
import { ApolloProvider} from '@apollo/client';
import { ApolloClient, InMemoryCache } from '@apollo/client'

const client = new ApolloClient({
  uri: 'http://localhost:4000',
  cache: new InMemoryCache()
});

const root = ReactDOM.createRoot(document.getElementById('root'));
root.render(
  <ApolloProvider client={client}>
    <App />
  </ApolloProvider>
);

// If you want to start measuring performance in your app, pass a function
// to log results (for example: reportWebVitals(console.log))
// or send to an analytics endpoint. Learn more: https://bit.ly/CRA-vitals
reportWebVitals();

```