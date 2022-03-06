# React Query와 상태 관리(2)

React Query의 **invalidation**, **caching**, **synchronization** 동작 방식

## Querty Invalidation
오래된 데이터를 **무효화**하기 위해선 **Querty Invalidation**를 사용할 수 있습니다.
간단히 **query client**를 통해 **invalidateQueries({key}) 메서드**를 호출하면 됩니다.
이때 fetch 할 때 지정한 key를 가진 state는 stale 취급되고 refetch 됩니다.

```javascript
// key 존재할 때
queryClient.invalidateQueries('todos');

// key 존재하지 않을 때
queryClient.invalidateQueries();
```

## Caching, Synchronization
RFC 5861의 `Cache-Control: max-age=600, stale-while-revalidate=30` 의미는 다음과 같습니다.
캐시 데이터를 600초 동안 유효하다고 보고, 600초가 지난 630초 까지는 최신 데이터가 올 때까지(백그라운드에서 refetch 될 때까지) stale 데이터를 보여주겠다.

React Query는 이 컨셉을 이용해 **메모리 캐시**에도 적용합니다. 다음 옵션들은 **Caching**, **Synchronization** 과 관련 있는 useQuery의 option들입니다.

**Caching options**
- **cacheTime**: 메모리에 얼마만큼 있을지. 시간이 지나면 GC가 삭제한다. (default: 5 * 60 * 1000)
- **staleTime**: 얼마나 지나야 데이터를 stale로 판단할지. 그전까지는 stale한 데이터가 아님을 말함. default(0) 으로 설정하면 불러옴과 동시에 stale 데이터가 된다.

**Synchronization options** (default 모두 true)
- **refetchMount**: mount 시 데이터가 stale라면 refetch
- **refetchOnWindowFocus**: 해당 window에 포커싱이 될 때 데이터가 stale이라면 refetch
- **refetchOnReconnect**: recconect 시에 데이터가 stale라면 refetch

### Query 상태 흐름
#### 전체 흐름
1. **fetching** 상태
    첫 data fetching 한 시점
2. **fresh** 상태
    data fetch 후 staleTime이 지나지 않은 data 일 때의 상태 (staleTime이 0이라면 바로 stale)
3. **stale** 상태
    data fetch 후 staleTime이 지난 data 일 때 상태
4. **inactive** 상태
    inactive는 **화면 속에서 사용 안 될 때** (윈도우 포커싱이 바뀌거나, 컴포넌트가 사라진다거나..)
    cacheTime이 만료되기 전까지의 상태
5. **delelte** 상태
    cacheTime이 만료되면 GC에 의해 delete되는 상태

#### active 상태의 흐름
아래의 흐름은 **delete 되기 전에 inactive 상태였다가 active 됐을 때 상태의 흐름입니다.** 즉 컴포넌트가 다시 mount 되거나(**refethchMount**), window 포커싱이 되거나(**refetchOnWindowFocus**), 다시 연결되는 경우(**refetchOnReconnect**)를 말합니다. 이 경우는 refetch 되므로 **fetching, fresh, stale 상태가 반복**되게 됩니다.
1. **fetching** 상태
    data fetching 한 시점
2. **fresh** 상태
    data fetch 후 staleTime이 지나지 않은 data 일 때의 상태 
3. **stale** 상태
    data fetch 후 staleTime이 지난 data 일 때 상태

### 기본 예제의 Caching, Synchronization 분석
```javascript
function Example(){
    const { data } = useQuery('todos', ()=> fetchTodos());
}
```
위의 코드는 모든 options가 default므로 다음과 같은 값을 가집니다.
>cacheTime: 5분
staleTime: 0
refetchMount: true
refetchOnWindowFocus: true
refetchOnReconnect: true

따라서 fetch 되는 todos data는 다음과 같은 흐름으로 상태가 변합니다.
1. **staleTime이 0이므로 fetch 됨과 동시에 stale 데이터**가 된다. (항상 stale data)
2. 이 상태로 5분이 지나기 전까지 **(cacheTime가 만료되기 전까지)** 메모리에 캐시 데이터가 남는다.
    - 5분이 지나면 GC에 의해 제거된다.
3. 제거되기 전 mount, focus, reconnect가 되면 **refetch가 되고 동시에 stale 데이터**가 된다.
4. 1,2,3,4 과정 반복




