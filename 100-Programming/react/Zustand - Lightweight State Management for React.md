## Summary

리액트 환경에서 가장 가볍고 직관적으로 전역 상태를 관리할 수 있는 라이브러리입니다. Flux 패턴을 따르면서도 Redux 대비 보일러플레이트가 거의 없어 현대적인 리액트 프로젝트에서 선호되는 방식입니다.

## Key Concepts

- **Store**: 전역 데이터와 데이터를 변경하는 함수(Action)를 모아둔 중앙 저장소입니다.
    
- **set**: 스토어 내부의 상태를 업데이트하는 함수로, 얕은 병합(Shallow Merge)을 지원합니다.
    
- **get**: 액션 함수 내부에서 현재 상태값을 읽어와야 할 때(예: 다음 offset 계산 등) 사용합니다.
    
- **Selectors**: 컴포넌트에서 필요한 상태 조각만 선택하여 구독함으로써 불필요한 리렌더링을 방지합니다.
    

## Example

### Zustand Store 생성 및 사용

```javascript
import { create } from 'zustand';

// 1. 스토어 정의 (데이터 + 액션)
const useStore = create((set, get) => ({
  count: 0,
  increase: () => set((state) => ({ count: state.count + 1 })),
  asyncFetch: async () => {
    const res = await fetch('/api/data');
    set({ data: await res.json() });
  }
}));

// 2. 컴포넌트에서 선택자(Selector)를 통해 사용
function Counter() {
  const count = useStore((state) => state.count);
  const increase = useStore((state) => state.increase);
  
  return <button onClick={increase}>{count}</button>;
}
```

## Notes

- **성능 최적화**: `const { count } = useStore()` 방식은 스토어의 다른 값이 바뀔 때도 리렌더링되므로, `state => state.count` 처럼 선택자를 쓰는 것이 권장됩니다.
    
- **간결함**: Redux와 달리 `<Provider>`로 앱을 감쌀 필요가 없어 설정이 매우 단순합니다.
    
- **비동기 지원**: 액션 함수 내부에서 `async/await`를 사용하여 API 호출을 직접 처리할 수 있습니다.
    

## Related

- [[React Hooks]]
    
- [[Redux]]
    
- [[State Management]]
    
- [[Context API]]
    
- [[Shallow Comparison]]