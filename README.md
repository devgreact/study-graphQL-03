# GraphQL 서버로 부터 목록 받아와 표시하기
App.js
``` js
import './App.css';
import List from './List';

function App() {
  return (
    <div className="App">
      <List/>
    </div>
  );
}

export default App;

```

List.js
```js
import React from 'react'

import { useQuery, gql } from '@apollo/client';

const GET_TODO = gql`
query {
  todos {
    id
    title
    date
    complete
    weather
  }
}
`;

const List = () => {   
    const { loading, error, data } = useQuery(GET_TODO);
    if (loading) return <p className="loading">Loading...</p>
    if (error) return <p className="error">Error :(</p>
  return (
    <div>
        <h1>Todo목록</h1>
        <hr />
        <ul>
        {data.todos.map((item) => {
          return (
            <li key={item.id} style={{display:"flex", justifyContent:"space-between", alignItems:"center", borderBottom: "1px solid #000", padding: "10px"}}>
                <h2>{item.title}</h2>
                <div>
                   {item.weather} <button>수정하기</button><button>삭제하기</button> {item.date} 
                </div>
            </li>
          )
        })}
      </ul>
    </div>
  )
}

export default List
```