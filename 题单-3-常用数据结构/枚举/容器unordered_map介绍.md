一句话概括

  基于哈希表的键值对容器，查找/插入/删除都是平均 O(1)，最坏 O(n)。

  ---
  核心操作速查

  unordered_map<int, string> m;

  // --- 插入 ---
  m[1] = "one";                          // 最简便，键存在则覆盖
  m.insert({2, "two"});                  // 键存在则不覆盖
  m.emplace(3, "three");                 // 原地构造，性能更好

  // --- 查找 ---
  ==m.count(1);        // 1 或 0（键是否存在）==
  m.find(1);         // 返回迭代器，没找到返回 m.end()
  m.contains(1);     // C++20，true/false
  string v = m[1];   // ==小心：键不存在会自动插入默认值并返回==

  // --- 删除 ---
  m.erase(1);        // 删指定键
  m.clear();         // 清空

  // --- 遍历 ---
  for (auto& [key, val] : m) { }         // C++17 结构化绑定
  for (auto& kv : m) { kv.first, kv.second; }

  ---
  ==关键陷阱（碰到最多的 bug）==

  1. ==operator[] 会插入不存在的键==

  ==if (m[5] == "hello") { }  // 如果 5 不存在，会插入 {5, ""}==

  ==正确做法：用 find 或 at 做只读查找：==
  ==if (m.find(5) != m.end()) { string v = m[5]; }  // 安全==
  ==string v = m.at(5);  // 键不存在时抛 std::out_of_range==

  2. 先插入还是先检查（你刚遇到的）

  找互补元素的场景，必须先检查历史，找不到再插入自己，否则可能自己配对自己。

  3. 遍历时不要修改结构

  for (auto& [k, v] : m) {
      m.erase(k);  // 迭代器失效，UB！
  }

  ---
  效率要点

  rehash 是最耗的操作，提前知道数据量调用 reserve：

  m.reserve(预估元素数 / 0.7);  // 负载因子默认 0.75
  // 或
  m.reserve(n);  // n ≥ 预期元素数即可

  其他技巧：
  - emplace 优于 insert（原地构造，少一次拷贝）
  - 不需要有序时用 unordered_map，需要有序用 map（红黑树，O(log n)）
  - 键用整数优于 string（hash 更便宜）

  ---
  常见用法模式

  计数/频率统计：
  for (int x : nums) umap[x]++;  // 利用 operator[] 自动初始化 0

  找互补/配对（你的场景）：
  if (umap[target] > 0) { /* 用掉一个 */ }
  else umap[nums[i]]++;

  缓存/memo：
  unordered_map<int, int> memo;
  function<int(int)> fib = [&](int n) {
      if (n <= 1) return n;
      if (memo.count(n)) return memo[n];
      return memo[n] = fib(n-1) + fib(n-2);
  };

  ---
  选型对比

  ┌────────┬────────────────┬────────────────┐
  │        │ unordered_map  │      map       │
  ├────────┼────────────────┼────────────────┤
  │ 底层   │ 哈希表         │ 红黑树         │
  ├────────┼────────────────┼────────────────┤
  │ 查找   │ O(1) 平均      │ O(log n)       │
  ├────────┼────────────────┼────────────────┤
  │ 有序   │ 否             │ 是（按键排序） │
  ├────────┼────────────────┼────────────────┤
  │ 键要求 │ 需要 hash 函数 │ 需要 operator< │
  ├────────┼────────────────┼────────────────┤
  │ 场景   │ 绝大多数       │ 需要按序遍历键 │
  └────────┴────────────────┴────────────────┘

  日常刷题和开发，无脑选 unordered_map 就行。