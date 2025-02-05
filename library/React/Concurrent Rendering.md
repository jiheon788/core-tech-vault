# Concurrent Rendering
리액트의 동시성 기능은 작업을 작은 단위로 나누고, 우선순위에 따라 처리하는 방식으로, 중요한 작업을 먼저 수행하고 덜 중요한 작업은 중단하거나 나중에 처리할 수 있도록 합니다.

디바운싱과 쓰로틀링은 고정된 시간을 기다린 후 실행되지만, 동시성 기능은 작업의 우선순위와 시스템 상태에 맞춰 작업을 조정한다. 중요한 작업은 즉시 처리하고, 덜 중요한 작업은 필요에 따라 지연시켜 더 부드럽고 최적화된 사용자 경험을 제공한다.

>[!tip]
>인풋에 입력한 값을 기반하는 검색을 하는 경우, 만약 [[Debouncing과 Throttling#디바운싱(debouncing)|디바운싱]]을 적용한다면 디바운스 타임에 의존이 생겨 무조건 특정 시간이후에 입력값을 바꿔 연속적인 입력을 모두 검색하는 것을 막는다. 동시성모드는 트랜지션을 사용해 우선순위를 조정하기에 유연하다.


Concurrent Mode로 설정하기 위해서는 기존의 render 대신 createRoot를 사용하면 된다. Concurrent Mode로 설정하면 개선된 기능(Automatic Batching)들과 동시처리를 위한 startTransition, useTransition, useDeferredValue 훅들을 사용할 수 있다.

