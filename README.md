### [과제] 숙련주차 과제 답


# Q1. 추가하기 버튼을 클릭해도 추가한 아이템이 화면에 표시되지 않음.

```
import { useDispatch } from "react-redux";
const dispatch = useDispatch();//    

// addTodo 액션을 디스패치하여 새로운 할 일을 추가
    dispatch(addTodo({ ...todo, id }));
```
dispatch 함수를 사용하여 Redux 스토어에 액션을 전달합니다.
addTodo 액션 생성자를 호출하고, 그 안에는 새로운 할 일을 나타내는 객체를 전달합니다.
 1번 문제는 dispatch를 사용하고 있지 않아서  Redux에서 상태의 변경이 이루어지지 않기 때문에 버튼을 클릭해도 추가한 아이템이 화면에 표시되지 않았습니다.

# Q2. 추가하기 버튼 클릭 후 기존에 존재하던 아이템들이 사라짐.
```
const todos = (state = initialState, action) => {
  switch (action.type) {
    case ADD_TODO:
      return {
        ...state,
        todos: [...state.todos, action.payload], //action.payload
      };
```
수정전 코드는 기존배열에서 변경을 하는 코드입니다 ... 전개연산자를 사용하여 기존의 배열을 변경하지 않고 리턴하도록 수정하였습니다

# Q3. 삭제 기능이 동작하지 않음.
```
  case DELETE_TODO:
        return {
          ...state,
          todos: state.todos.filter((todo) => todo.id !== action.payload),
        };
```

todo.js에 DELETE_TODO 액션을 처리하는 Redux 리듀서 부분이 없어서 동작하지 않았습니다.
특정 id와 일치하지 않는 todo만을 남기고 나머지는 필터링하여 새로운 배열을 생성합니다


# Q4. 상세 페이지에 진입하였을 때 데이터가 업데이트 되지 않음.
```
 useEffect(() => {
    dispatch(getTodoByID(id));
  }, [dispatch, id]); 
```
useEffect 훅이 없어서 
페이지가 처음 로딩될 때 해당 id에 대한 데이터를 가져오지 않게 되어
데이터가 업데이트 되지 않았습니다


# Q5. 완료된 카드의 상세 페이지에 진입하였을 때 올바른 데이터를 불러오지 못함.
```
    {todos.map((todo, index) => {
          if (todo.isDone) {
            return (
              <StTodoContainer key={todo.id}>
                 <StLink to={`/${index}`} key={todo.id}>
                  <div>상세보기</div>
                </StLink> //기존코드 
 ```            
상세 페이지에 전달되는 파라미터가 index 여서 , getTodoByID에서 해당 id를 찾을 때 문제가 발생 했습니다
```
    {todos.map((todo, ) => {
      if (todo.isDone) {
         return (
           <StTodoContainer key={todo.id}>
            
            <StLink to={`/${todo.id}`} key={todo.id}>
              <div>상세보기</div>
            </StLink> //수정한 코드
```

# Q6. 취소 버튼 클릭시 기능이 작동하지 않음.

```
 onClick={onToggleStatusTodo}//기존코드
 onClick={() => onToggleStatusTodo(todo.id)} //바꾼코드

 ```
함수를 새로 생성해서 전달하면 함수 호출이 즉시 발생하는 것을 방지하고
클릭 이벤트가 발생했을 때 함수가 실행되도록 할 수 있습니다.