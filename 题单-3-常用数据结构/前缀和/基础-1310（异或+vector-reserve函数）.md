# 基础-1310（子数组异或查询）

> 核心思想:**前缀异或 = 把"前缀和相减"换成"前缀异或相消",预处理 O(n),区间查询 O(1)**

## 1. 题意概括

给出数组 `arr` 与若干查询 `queries[i] = [Li, Ri]`。
对每个查询返回闭区间 `[Li, Ri]` 的连续异或值:`arr[Li] ^ arr[Li+1] ^ ... ^ arr[Ri]`。

## 2. 核心思路

区间连续运算 → 用前缀思想,把重复计算的公共部分提前算好。

### 2.1 异或的性质(XOR 的逆运算是它自己)

- `0 ^ x = x`(任何数异或 0 不变)
- `x ^ x = 0`(同一数出现两次即抵消)

> 类比:**加法**对应"前缀和相减";**异或**对应"前缀异或相消"。

### 2.2 构建前缀异或数组(O(n))

```cpp
prefix[i + 1] = prefix[i] ^ arr[i];   // = arr[0..i] 的异或
```

长度 `n + 1`,且 `prefix[0] = 0`。

### 2.3 区间查询公式(O(1))

**`arr[L..R]` 的异或 = `prefix[R + 1] ^ prefix[L]`**

原理:`prefix[R+1]` 含 `arr[0..R]`,`prefix[L]` 含 `arr[0..L-1]`;两者异或后,
`arr[0..L-1]` 每个元素出现两次归零,恰好剩下 `arr[L..R]`。

> 记忆口诀:**右端点 +1 异或左端点**(同前缀和一致)

## 3. 代码展示

```cpp
class Solution {
public:
    vector<int> xorQueries(vector<int>& arr, vector<vector<int>>& queries) {
        int n = arr.size();
        vector<int> prefix(n + 1, 0);
        for (int i = 0; i < n; ++i)
            prefix[i + 1] = prefix[i] ^ arr[i];        // 构建前缀异或

        vector<int> ans;
        ans.reserve(queries.size());                   // 预留容量,避免频繁扩容
        for (auto& q : queries) {
            int L = q[0], R = q[1];
            ans.push_back(prefix[R + 1] ^ prefix[L]);  // 区间异或
        }
        return ans;
    }
};
```

## 4. 易错点分析

| 易错点 | 正确做法 |
| --- | --- |
| prefix 数组长度 | `n + 1`,不是 `n` |
| 区间公式下标 | `prefix[R+1] ^ prefix[L]`,不是 `prefix[R] ^ prefix[L-1]` |
| 忘记 `prefix[0] = 0` | 否则 `L = 0` 时公式失效 |
| `reserve` 只扩容量 | `ans.size()` 仍为 0,**必须 `push_back`**,`ans[i] = ...` 下标赋值会越界 |
| 溢出 | `arr[i] ≤ 1e9 < 2^30`,XOR 结果 int 足够,无需 long long |

## 5. 复杂度分析

- **时间 O(n + q):** 构建 prefix 需 O(n),每个查询 O(1),q 个查询共 O(q)
- **空间 O(n):** 额外 prefix 数组长度 `n + 1`(不计返回值 ans)

## 6. 示例

```text
arr    = {1, 3, 4, 8}
prefix = {0, 1, 2, 6, 14}

查询 [1, 2]: prefix[3] ^ prefix[1] = 6 ^ 1 = 7   (即 3 ^ 4 = 7)      ✓
查询 [3, 3]: prefix[4] ^ prefix[3] = 14 ^ 6 = 8  (即单个 8)          ✓
查询 [0, 3]: prefix[4] ^ prefix[0] = 14 ^ 0 = 14 (1^3^4^8)           ✓
```
