## Jest 的基本用法

### 1. 匹配器 Matcher

Jest使用“匹配器”以不同的方式测试值。比较典型的如下：

- toBe(),数值比较，用'==='实现。
```
test('two plus two is four', () => {
  expect(2 + 2).toBe(4);
});
```

- toEqual()，对象比较，递归地检查对象或数组的每个字段。
```
test('object assignment', () => {
  const data = {one: 1};
  data['two'] = 2;
  expect(data).toEqual({one: 1, two: 2});
});
```

- toBeNull(),测试是否为null
- toBeUndefined()，测试是否undefined
- toBeDefined (), toBeUndefined的相反操作
- toBeTruthy()，判断是否为true
- toBeFalsy(),是否false