# Tanstack Query
### 사용이유
서버의 상태는 클라이언트 상태와 본질적으로 다르다. 클라이언트의 통제를 받지않고 원격에서 API를 통해 접근/수정된다. 잘 관리하지않으면 stale한 상태가 될 수 있다. 

이전에는 상태 관리 라이브러리를 사용하여  서버데이터를 함께 관리했지만 클라이언트 상태와 서버 상태가 완전히 다르기 때문에 적합하지않다.

이러한 서버의 상태를 클라이언트와 쉽게 동기화 할 수 있도록 도와준다. (페칭, 캐싱, 동기화, 업데이트 등) 

- 서버 상태
    - 클라이언트에서 통제하거나 소유할 수 없는 위치에서 원격으로 유지
    - 가져오거나 업데이트를 위해 비동기 API가 필요
    - 주의를 기울이지 않으면 최신화되지 않은 상태를 가질 수 있다.
- 클라이언트 상태
	- 클라이언트에 의해서 제어되는 상태
	- [[React의 상태 (State)]], [[전역 상태 라이브러리 비교]]

### `staleTime` vs `gcTime`

각각의 설정을 통해 데이터를 더 효율적으로 관리하고, 불필요한 네트워크 요청을 줄이면서도 최신 데이터를 가져올 수 있도록 합니다.
###### `staleTime`
- 데이터가 신선한 상태로 간주하는 시간 (fresh에서 stale로 전환될 때까지의 기간)
- 쿼리가 fresh한 한, 데이터는 항상 캐시에서만 읽힙니다 (네트워크 요청은 발생x)
- 디폴트: 0 (받은 시점에서 이 데이터는 stale 하다고 간주)
###### `gcTime`
- **쿼리가 사용되지 않게 된 후에도 캐시에 유지될** 시간을 정하는 것이다
- `staleTime`이 지나면 데이터는 '오래된' 상태가 되지만, 여전히 캐시된 데이터는 사용이 가능합니다. ===이 캐시 데이터는 새로운 요청을 보내 신선한 데이터를 받아오기 이전에 임시로 기존 데이터를 표시하는 데 활용됩니다.===
- `gcTime`은 쿼리가 신선한 상태일때는 있을 때는 아무것도 하지 않는다. `gcTime`은 비활성화된 데이터를 gc가 언제 수집할지를 결정하는 시간이다.
- `gcTime`은 staleTime과 관계없이, 무조건 `inactive` 된 시점을 기준으로 캐시 데이터 삭제를 결정한다.
- **캐시된 데이터를 stale 상태에서도 유지**하여, 네트워크 요청이 진행되는 동안 **캐시 데이터를 즉시 렌더링할 수 있게 만드는 역할**
- 이렇게 
- 디폴트: 5분
###### `cacheTime` 이 `staleTime`보다 커야한다 ?  

> 만약 `staleTime`이 `cacheTime`보다 크면, 데이터가 
> - **아직 신선해야 하는데**
> - 이미 캐시에서 삭제되어버려서
> - 새로 요청을 보내야 하는 이상한 상황이 생김.
> - 즉, "신선함"을 판단하려면 데이터가 캐시에 남아 있어야 하기 때문에 `cacheTime` 이 `staleTime`보다 커야한다는 주장이 있다. **"staleTime ≤ cacheTime"**

[TkDodo의 reply](https://github.com/TanStack/query/discussions/1685#discussioncomment-1876723)에 따르면 TkDodo는 이 의견에 동의하지 않는다고 한다. ==구성 요소가 마운트되어 있는 한, `cacheTime`는 중요하지 않다. (관계없음)== `cacheTime`은 **쿼리가 비활성화된 후(마지막 구독자가 사라진 후)에만 영향을 미친다**
예컨대, staleTime이 60분일지라도 유저가 자주 사용하지 않는 데이터라면 굳이 gcTime을 60분 이상으로 설정하여 메모리를 낭비할 필요가 없다. 

###### 최적의 staleTime, gcTime ?
리액트 쿼리를 기본 설정으로 사용하는 경우 네트워크 요청 횟수가 더 많아진다. staleTime에 어떠한 설정도 하지 않으면 해당 쿼리를 사용하는 컴포넌트(Observer)가 mount 됐을 때 매번 다시 API를 요청한다.

TkDodo는 `cacheTime`을 변경하는 경우는 거의 없었고, time 설정 중 하나를 조정하는 경우에는 보통 `staleTime`을 조정하여 해결한다고 한다.

- 변경이 자주 일어나지 않는 데이터: `staleTime`을 조정하여 불필요한 네트워크 요청 횟수 감소
- 자주 변경되는 데이터: 기본 설정을 바꾸지 않는 걸 추천 (st: 0, ct: 5m)

### 쿼리키 관리
- [tkdodo의 블로그](https://tkdodo.eu/blog/effective-react-query-keys)에서 권장한 방법을 참고하여 쿼리키 팩토리를 통해 추후 변경에 용이하게 관리했습니다.
- 쿼리키는 일반적인 것부터 구체적인 것까지 순차적으로 구성하였습니다. 계층적 쿼리키를 사용하면 특정 범위의 데이터를 손쉽게 무효화(Invalidate)할 수 있어 관리가 용이합니다.

```typescript
const JobCatalogsKeys = {
  all: () => ['JobCatalogs'],
  root: () => [...JobCatalogsKeys.all(), 'root'],
  children: (parentId: string) => [...JobCatalogsKeys.root(), parentId],
};
```

### Request Waterfall
하나의 컴포넌트에서 여러 개의 Suspense Query를 호출할 경우 [^2]Request Waterfall이 발생한다. 
```tsx
function Component() {
	const user = useSuspenseQuery(['user'], fetchUser); 
	const posts = useSuspenseQuery(['posts'], fetchPosts);
} 
```

- 방법1: useSuspenseQuery를 사용하면 직렬로 요청하여 Request Waterfall이 발생한다. -> useSuspenseQueries를 사용하면 원하던 병렬로 요청
- 방법2: Prefetching하거나 페이지 로드 또는 탐색 시 라우터에서 요청들을 모두 Prefetching


>[!tip]
>v5에서 useQuery의  onSuccess, onError, [^1]onSettled 콜백들은 이제 사용되지 않는다.
>제거한 이유:
>- 예측 가능하고 일관성있는 useQuery
>- 상태 동기화를 목적으로 사용했을 때 발생하는 추가 렌더 사이클. 예) onSuccess 콜백에 로컬 또는 전역 상태 업데이트
>- 콜백이 호출되지 않을 여지 예) staleTime 설정으로 query function이 호출되지 않아 의도한 콜백이 실행하지 않을 경우
>
>따라서 v5이후로는 다음 방법으로 콜백을 다룰 것을 제안하였다.
>- 전역 콜백 처리 (queryClient)
>- Error boundary로 에러 처리
>- statud, isError 등으로 컴포넌트에서 처리



[^1]: 쿼리가 성공하든 실패하든 무조건 호출됨

[^2]: 앞선 요청이 완료되어야지 다음 요청을 진행할 수 있는 것