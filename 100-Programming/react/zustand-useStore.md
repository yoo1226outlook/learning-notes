# Zustand useStore()

## Summary

Zustand 상태를 구독하는 hook.

## Key Points

- selector 사용
- 필요한 state만 subscribe

## Example

```javascript
const count = useStore(state => state.count)
```

## Common Mistakes

selector 없이 사용하면 전체 상태 구독 발생

## Related
