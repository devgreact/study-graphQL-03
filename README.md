# GraphQL 서버로 부터 목록 받아와 표시하기 및 삭제

List.js

```js
import React from "react";

import { useQuery, gql, useMutation } from "@apollo/client";

//  기본형
const GET_TODOS = gql`
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
//  함수형
const GET_TODOS_FN = gql`
  query todos {
    todos {
      id
      title
      date
      complete
      weather
    }
  }
`;

// $는 변수이다.
const DELETE_TODO = gql`
  # 타입 적는 곳
  mutation deleteTodo($id: Int) {
    # 실제 우리가 전달할 변수 적는 곳
    deleteTodo(id: $id) {
      id
    }
  }
`;

const UPDATE_TODO = gql`
  mutation {
    addTodo(
      id: 100
      title: "수정이요"
      date: "2023-04-06"
      complete: true
      weather: 5
    ) {
      id
      title
      date
      complete
      weather
    }
  }
`;

const List = () => {
  const { data } = useQuery(GET_TODOS);

  const [deleteTodo] = useMutation(DELETE_TODO);
  const [updateTodo] = useMutation(UPDATE_TODO);

  const onClickDelete = async (_id) => {
    // const result = await deleteTodo();
    // console.log(result);
    // console.log(result.data.deleteTodo.id);
    console.log("삭제");
    const result = await deleteTodo({
      // variables 가 $ 역할을 한다.
      variables: {
        id: _id,
      },
      refetchQueries: [{ query: GET_TODOS }],
    });
  };

  const onClickEdit = async (_id) => {
    console.log("업데이트");
    const result = await updateTodo({
      variables: {
        id: _id,
      },
      refetchQueries: [{ query: GET_TODOS }],
    });
    console.log(result);
  };

  return (
    <div>
      <h1>Todo목록</h1>
      <hr />
      <ul>
        {data
          ? data.todos.map((item) => {
              return (
                <li
                  key={item.id}
                  style={{
                    display: "flex",
                    justifyContent: "space-between",
                    alignItems: "center",
                    borderBottom: "1px solid #000",
                    padding: "10px",
                  }}
                >
                  <h2>{item.title}</h2>
                  <div>
                    {item.weather}
                    <button onClick={() => onClickEdit(item.id)}>
                      수정하기
                    </button>
                    <button onClick={() => onClickDelete(item.id)}>
                      삭제하기
                    </button>{" "}
                    {item.date}
                  </div>
                </li>
              );
            })
          : "로딩중입니다."}
      </ul>
    </div>
  );
};

export default List;
```
