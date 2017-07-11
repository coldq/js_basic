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

- toMatch()，检查字符串是否与正则表达式匹配。

```
test('string test', () => {
  expect('test').not.toMatch('test);
});


test('there is no I in team', () => {
  expect('team').not.toMatch(/I/);
});

test('but there is a "stop" in Christoph', () => {
  expect('Christoph').toMatch(/stop/);
});
```

- toContain(item) 检查项目是否在列表中时，使用===，严格的等式检查。

```
//getAllFlavors() returns a list of flavors
test('the flavor list contains lime', () => {
  expect(getAllFlavors()).toContain('lime');
});
```

### 2. 异步的检验

#### 不能用回调函数

下面代码，一旦fetchData完成，测试将在调用回调之前完成。
```

// Don't do this!
test('the data is peanut butter', () => {
  function callback(data) {
    expect(data).toBe('peanut butter');
  }

  fetchData(callback);
});
```
有一种替代形式的测试来解决这个问题。不要将测试置于带有空参数的函数中，而是使用一个名为done的参数。 Jest将在完成测试之前等待完成回调。
```
test('the data is peanut butter', done => {
  function callback(data) {
    expect(data).toBe('peanut butter');
    done();
  }

  fetchData(callback);
});
```

#### Promise 

