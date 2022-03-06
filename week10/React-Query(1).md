# React Query와 상태관리

## 상태 관리
**상태**란 **시간에 따라 변화**하는 문자열, 배열, 객체 등의 형태로 저장되는 데이터입니다.
모던 웹 프론트엔드 에서는 프로젝트의 규모가 커지면서 UI/UX가 많아지고 자연스럽게 **관리하는 상태가 많아졌고 관리 역시 어려워졌습니다.**
예를 들어 React 같은 경우 하위 컴포넌트로 props를 전달해 주기 위해 **Props Drilling 이슈**가 존재합니다.
```javascript
// 대략...
const App = () => {
    return <Component1 name={"동원"} />
}
const Component1 = ({name}) => {
    return <Component2 name={name}/>
}
const Component2 = ({name}) => {
    return <Component3 name={name}/>
}
const Component3 = ({name}) => {
    return <Component4 name={name}/>
}
const Component4 = ({name}) => {
    return <p>{name}</p>
}
```
이러한 이슈를 해결하기 위해 **redux**, **mobx**, **recoil** 과 같은 전역 상태 라이브러리가 만들어졌습니다.

## React Query를 사용해야 하는 이유
**기존 Store에서 Server 상태를 분리하고 효율적으로 관리**할 수 있다.

### 상태의 두 가지 종류

1. **Client 상태**
- **Client에서 소유하**는 상태. 
- meta 데이터와 같이 Client 내에서 전역으로 사용하는 상태들..

2. **Server 상태**
- **api 통신을 통해 얻는 DB 정보**와 같은 상태.
- **비동기 처리**가 필요한 상태
- 다른 사람들과 공유되는 상태 

### 가장 많이 사용했던 redux의 문제 
redux를 사용하면서 의아했던 점은 Client 상태 관리의 목적보단 Server 상태 관리를 더 많이 한다는 것입니다. 즉 **Client 상태보단 Server 쪽 상태를 관리하는데 더 많이 쓰이고 있습니다.**

```javascript
// 비동기 네트워크 통신을 위한 redux-saga의 경우 data외에 loading, error를 관리한다.
function* loginSaga(action: ReturnType<typeof login>) {
  yield put(startLoading(LOGIN));
  try {
    const { email, password } = action;
    const response: AxiosResponse = yield call(authApi.login, {
      email,
      password,
    });

    yield put({
      type: LOGIN_SUCCESS,
      payload: response.data,
    });
  } catch (e) {
    yield put({
      type: LOGIN_FAILURE,
      payload: e,
      error: true,
    });
  }
  yield put(finishLoding(LOGIN));
}
```

## React Query 예제

#### 세팅
**QueryClientProvider**
내부적으로 context를 사용하여 server 상태를 관리합니다. 전역으로 사용하기 위해 최상위 컴포넌트를 감싸줍니다.

```javascript
...
ReactDOM.render(
    <QueryClientProvider>
        <App />
    </QueryClientProvider>,
  document.getElementById('root')
);
```

### useQuery
서버 데이터 **GET (Read)** 목적인 함수
- Query key: 값을 관리하기 위한 key 값. 이 key를 이용해 다른 곳에서 결과를 가져올 수 있다.
- Query function: Promise를 반환하는 함수. 데이터 resolve 하거나 error 반환한다.

```javascript
function app(){
    // useQuery(Query key, Query function)
    const info = useQuery('todos', ()=> fetchOrder(orderNo), options)
}
```

**반환값 info**
>**data**: 마지막으로 성공한 resolve된 데이터 (response 값)
>**error**: 에러 발생할 때 반환
>**isFetching**: runtime 중일 때 발생하는 값
>**status**, **isLoading**, **isSuccess**: 현재 쿼리 상태
>**refetch**: 해당 조건에 re-fetch 할 수 있게 해주는 인터페이스
>**remove**: 쿼리 캐시 지우는 함수  

**options**
>**onSuccess**, **onError**, **onSettled**: query fetching 성공/실패/완료 시 실행할 side Effect 정의
>**enabled**: false로 지정하면 실행되지 않음
>**retry**: 쿼리가 동작을 실패했을 때 자동으로 Retry 하는
>**select**: 성공 시 가져온 data를 가공해서 전달
>**keepPreviousData**: 새롭게 fetching 시 이전 데이터 유지 여부
>**refetchInterval**: 주기적으로 refetch 할지 결정 ( 3초에 한 번씩 ~)

## useMutations
서버 데이터 **생성, 수정, 삭제(CREATE, UPDATE, DELETE)** 목적인 함수
```javascript
const postMutation = useMutation(newTodo => {
    return axios.post('/todos', newTodo)
}, options)

// 다양한 경우 처리
const options = {
    onSuccess: () => { ... },
    onError: () => { ... }},
    onMutate: () => { ... }
}

// 실행 함수
postMutation.mutate(data);
```

**반환값 mutation**
> **mutate**: mutation을 실행하는 함수.
> **reset**: 내부 상태 초기화

**options**
> **onMutate**: mutaion 동작 전에 먼저 실행되는 함수 (Optimistic update 기능)
> **onSuccess**: mutation이 성공했을 때 처리
> **onError**: mutation이 실패했을 때 처리