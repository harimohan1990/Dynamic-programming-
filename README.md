# Dynamic-programming 

Here’s a **clean and structured summary** of the **entire GeeksforGeeks Dynamic Programming (DP)** tutorial you shared — ideal for **interviews and revision**:

---

### 🧠 **What is Dynamic Programming (DP)?**

A method to solve problems by breaking them into overlapping subproblems, solving each once, and storing their results (to avoid recomputation).

---

### 🧩 **When to Use DP**

1. **Optimal Substructure**: Problem can be broken down into subproblems.
2. **Overlapping Subproblems**: Subproblems repeat over recursion.

---

### 🧱 **DP Approaches**

1. **Top-Down (Memoization)**: Recursion + caching (store results in map/array).
2. **Bottom-Up (Tabulation)**: Iterative solution with DP table.
3. **Space Optimized**: Only store required previous values.

---

### 📌 **Steps to Solve a DP Problem**

1. Identify subproblems.
2. Write the recurrence relation.
3. Choose between memoization/tabulation.
4. Initialize base cases.
5. Solve in the right order.

---

### ⚡ **Fibonacci Example**

#### Recursive (Brute Force)

`O(2^n)` exponential time

```js
function fib(n) {
  if (n <= 1) return n;
  return fib(n - 1) + fib(n - 2);
}
```

#### Memoization

`O(n)` time, `O(n)` space

```js
function fibRec(n, memo = new Array(n+1).fill(-1)) {
  if (n <= 1) return n;
  if (memo[n] !== -1) return memo[n];
  return memo[n] = fibRec(n - 1, memo) + fibRec(n - 2, memo);
}
```

#### Tabulation

`O(n)` time, `O(n)` space

```js
function fib(n) {
  const dp = [0, 1];
  for (let i = 2; i <= n; i++)
    dp[i] = dp[i-1] + dp[i-2];
  return dp[n];
}
```

#### Space Optimized

`O(n)` time, `O(1)` space

```js
function fib(n) {
  let a = 0, b = 1;
  for (let i = 2; i <= n; i++) [a, b] = [b, a + b];
  return n ? b : a;
}
```

---

### 📚 **Popular DP Problems**

* 0/1 Knapsack
* Unbounded Knapsack
* Subset Sum
* Longest Common Subsequence (LCS)
* Longest Increasing Subsequence (LIS)
* Coin Change
* Word Break
* Egg Dropping Puzzle
* Matrix Chain Multiplication
* Palindrome Partitioning
* Edit Distance
* Bellman-Ford, Floyd-Warshall
* Digit DP, Bitmask DP, DP on Trees/Graphs/Grids

---

### ✅ **Benefits of DP**

* Avoids redundant computations.
* Guarantees optimal solutions.
* Converts exponential time to polynomial time in many cases.

---

Would you like a **cheat sheet image** or a **LinkedIn post-ready summary** for this?


Here's the **TypeScript implementation** of **Leetcode 70: Climbing Stairs** using all three DP approaches:

---

### ✅ 1. **Top-Down (Memoization)**

```ts
function climbStairsMemo(n: number, memo: number[] = Array(n + 1).fill(-1)): number {
  if (n <= 2) return n;
  if (memo[n] !== -1) return memo[n];
  memo[n] = climbStairsMemo(n - 1, memo) + climbStairsMemo(n - 2, memo);
  return memo[n];
}
```

---

### ✅ 2. **Bottom-Up (Tabulation)**

```ts
function climbStairsTab(n: number): number {
  if (n <= 2) return n;
  const dp: number[] = new Array(n + 1);
  dp[1] = 1;
  dp[2] = 2;
  for (let i = 3; i <= n; i++) {
    dp[i] = dp[i - 1] + dp[i - 2];
  }
  return dp[n];
}
```

---

### ✅ 3. **Space Optimized**

```ts
function climbStairsOptimized(n: number): number {
  if (n <= 2) return n;
  let prev1 = 1, prev2 = 2;
  for (let i = 3; i <= n; i++) {
    const curr = prev1 + prev2;
    prev1 = prev2;
    prev2 = curr;
  }
  return prev2;
}
```

---

Let’s now explore **Word Break**, **Coin Change**, and **Longest Increasing Subsequence (LIS)** using **Memoization**, **Tabulation**, and **Space Optimization (if applicable)**.

---

### ✅ **3. Word Break** (Leetcode 139 - Medium)

#### 📌 Problem:

Check if a string `s` can be segmented into space-separated words from a dictionary.

---

#### 🧠 Memoization:

```js
function wordBreak(s, wordDict) {
  const wordSet = new Set(wordDict);
  const memo = new Map();

  function dfs(start) {
    if (start === s.length) return true;
    if (memo.has(start)) return memo.get(start);

    for (let end = start + 1; end <= s.length; end++) {
      if (wordSet.has(s.slice(start, end)) && dfs(end)) {
        memo.set(start, true);
        return true;
      }
    }
    memo.set(start, false);
    return false;
  }

  return dfs(0);
}
```

#### 🧱 Tabulation:

```js
function wordBreak(s, wordDict) {
  const wordSet = new Set(wordDict);
  const dp = new Array(s.length + 1).fill(false);
  dp[0] = true;

  for (let i = 1; i <= s.length; i++) {
    for (let j = 0; j < i; j++) {
      if (dp[j] && wordSet.has(s.slice(j, i))) {
        dp[i] = true;
        break;
      }
    }
  }

  return dp[s.length];
}
```

🔁 No meaningful space optimization here—DP array is needed.

---

### ✅ **4. Coin Change** (Leetcode 322 - Medium)

#### 📌 Problem:

Given coins and an amount, return minimum number of coins to make the amount.

---

#### 🧠 Memoization:

```js
function coinChange(coins, amount) {
  const memo = new Array(amount + 1).fill(-1);

  function dp(rem) {
    if (rem < 0) return Infinity;
    if (rem === 0) return 0;
    if (memo[rem] !== -1) return memo[rem];

    let min = Infinity;
    for (let coin of coins) {
      min = Math.min(min, dp(rem - coin) + 1);
    }
    return memo[rem] = min;
  }

  const res = dp(amount);
  return res === Infinity ? -1 : res;
}
```

#### 🧱 Tabulation:

```js
function coinChange(coins, amount) {
  const dp = new Array(amount + 1).fill(Infinity);
  dp[0] = 0;

  for (let coin of coins) {
    for (let i = coin; i <= amount; i++) {
      dp[i] = Math.min(dp[i], dp[i - coin] + 1);
    }
  }

  return dp[amount] === Infinity ? -1 : dp[amount];
}
```

⚠️ Space optimization not feasible here without losing subproblem data.

---

### ✅ **5. Longest Increasing Subsequence (LIS)** (Leetcode 300 - Medium)

#### 📌 Problem:

Find the length of the longest strictly increasing subsequence.

---

#### 🧠 Memoization (with recursion + caching):

```js
function lengthOfLIS(nums) {
  const memo = {};
  function dp(i, prev) {
    const key = `${i}-${prev}`;
    if (key in memo) return memo[key];
    if (i === nums.length) return 0;

    let taken = 0;
    if (prev === -1 || nums[i] > nums[prev]) {
      taken = 1 + dp(i + 1, i);
    }
    const notTaken = dp(i + 1, prev);
    return memo[key] = Math.max(taken, notTaken);
  }
  return dp(0, -1);
}
```

#### 🧱 Tabulation (with binary search — O(n log n)):

```js
function lengthOfLIS(nums) {
  const sub = [];
  for (let num of nums) {
    let left = 0, right = sub.length;
    while (left < right) {
      const mid = (left + right) >> 1;
      if (sub[mid] < num) left = mid + 1;
      else right = mid;
    }
    sub[left] = num;
  }
  return sub.length;
}
```

✅ This is the most optimized version using a greedy + binary search strategy.

---

Let me know if you want **DP patterns** or **a comparison cheat sheet for interviews**.


Here’s a full **TypeScript solution for Leetcode 198: House Robber** using all **three DP approaches**:

---

### 🔁 **Recurrence Relation:**

Let `dp[i] = max money from house 0 to i`

`dp[i] = max(dp[i - 1], dp[i - 2] + nums[i])`

---

### ✅ 1. **Top-Down (Memoization)**

```ts
function robMemo(nums: number[]): number {
  const memo: number[] = Array(nums.length).fill(-1);

  const dp = (i: number): number => {
    if (i < 0) return 0;
    if (memo[i] !== -1) return memo[i];
    return memo[i] = Math.max(dp(i - 1), dp(i - 2) + nums[i]);
  };

  return dp(nums.length - 1);
}
```

---

### ✅ 2. **Bottom-Up (Tabulation)**

```ts
function robTab(nums: number[]): number {
  const n = nums.length;
  if (n === 0) return 0;
  if (n === 1) return nums[0];

  const dp: number[] = new Array(n);
  dp[0] = nums[0];
  dp[1] = Math.max(nums[0], nums[1]);

  for (let i = 2; i < n; i++) {
    dp[i] = Math.max(dp[i - 1], dp[i - 2] + nums[i]);
  }

  return dp[n - 1];
}
```

---

### ✅ 3. **Space Optimized**

```ts
function robOptimized(nums: number[]): number {
  let prev1 = 0, prev2 = 0;

  for (const num of nums) {
    const temp = Math.max(prev1, prev2 + num);
    prev2 = prev1;
    prev1 = temp;
  }

  return prev1;
}
```

---

### 📌 Summary:

| Approach    | Time | Space | Notes                        |
| ----------- | ---- | ----- | ---------------------------- |
| Memoization | O(n) | O(n)  | Recursive with caching       |
| Tabulation  | O(n) | O(n)  | Iterative DP array           |
| Optimized   | O(n) | O(1)  | Best for interviews, compact |

Let me know if you want **dry-run examples** or a **visual explanation**.



Let’s explain **Leetcode 139: Word Break** like a school kid and then give you the **TypeScript code** using **memoization and tabulation**.

---

### 🧠 Simple Idea:

You are given a **string** (like a big word)
and a **dictionary of smaller words**.

You need to check:
💬 “Can I cut the big word into **pieces from the dictionary**?”

---

### ✂️ Example:

`s = "leetcode"`
`wordDict = ["leet", "code"]`

Can you break "leetcode" into words from the dictionary?
👉 Yes: `"leet"` + `"code"` → ✅ return `true`

---

### 🪄 Strategy (Memoization):

Try every possible **cut** of the word from `start` to `end`.
If left part is in dictionary and right part **can be broken**, return true.

---

### ✅ TypeScript: Memoization (Top-Down)

```ts
function wordBreak(s: string, wordDict: string[]): boolean {
  const wordSet = new Set(wordDict);
  const memo = new Map<number, boolean>();

  function canBreak(start: number): boolean {
    if (start === s.length) return true;
    if (memo.has(start)) return memo.get(start)!;

    for (let end = start + 1; end <= s.length; end++) {
      const word = s.substring(start, end);
      if (wordSet.has(word) && canBreak(end)) {
        memo.set(start, true);
        return true;
      }
    }

    memo.set(start, false);
    return false;
  }

  return canBreak(0);
}
```

---

### ✅ TypeScript: Tabulation (Bottom-Up)

```ts
function wordBreak(s: string, wordDict: string[]): boolean {
  const wordSet = new Set(wordDict);
  const dp: boolean[] = new Array(s.length + 1).fill(false);
  dp[0] = true;

  for (let i = 1; i <= s.length; i++) {
    for (let j = 0; j < i; j++) {
      if (dp[j] && wordSet.has(s.substring(j, i))) {
        dp[i] = true;
        break;
      }
    }
  }

  return dp[s.length];
}
```

---

### 🧒 TL;DR:

Break the string at every spot.
Check if left part is a word 🧩 and right part can also be split 🧩.
If yes → ✅ return true.

Let me know if you want visual steps or dry run for `"applepenapple"`!



Let’s explain **Leetcode 322: Coin Change** like a school kid, then give **TypeScript code** with **memoization and tabulation**.

---

### 💰 Problem:

You’re given coins like `[1, 2, 5]`
and a total amount like `11`.

🔍 **Goal:** Find the **fewest coins** to make `11`.
If not possible → return `-1`.

---

### 🎯 Example:

Coins = `[1, 2, 5]`
Amount = `11`

👉 Best way: `5 + 5 + 1 = 3 coins`
✅ Answer: `3`

---

### 🧠 Idea:

Try every coin.
At each step:

* Take the coin
* Subtract from amount
* Recurse for the rest

Store answers (memo) so you don't repeat!

---

### ✅ TypeScript: Memoization (Top-Down)

```ts
function coinChange(coins: number[], amount: number): number {
  const memo: number[] = new Array(amount + 1).fill(-1);

  const dp = (rem: number): number => {
    if (rem < 0) return Infinity;
    if (rem === 0) return 0;
    if (memo[rem] !== -1) return memo[rem];

    let min = Infinity;
    for (let coin of coins) {
      min = Math.min(min, dp(rem - coin) + 1);
    }
    memo[rem] = min;
    return min;
  };

  const result = dp(amount);
  return result === Infinity ? -1 : result;
}
```

---

### ✅ TypeScript: Tabulation (Bottom-Up)

```ts
function coinChange(coins: number[], amount: number): number {
  const dp: number[] = new Array(amount + 1).fill(Infinity);
  dp[0] = 0;

  for (let coin of coins) {
    for (let i = coin; i <= amount; i++) {
      dp[i] = Math.min(dp[i], dp[i - coin] + 1);
    }
  }

  return dp[amount] === Infinity ? -1 : dp[amount];
}
```

---

### 👦 Like a Kid:

You’re building money like LEGO blocks.
Try each coin and save the **smallest number of coins** to reach every amount.

Let me know if you want an animation or step-by-step dry run for `amount = 11`.


Let’s explain **Leetcode 300: Longest Increasing Subsequence (LIS)** like a school kid 🧒 and then show **TypeScript solutions** using **memoization** and **O(n log n)** optimization.

---

### 📚 Problem:

You are given a list of numbers.
Find the **longest sequence** where each number is **bigger than the one before**.

You can **skip** numbers to form this sequence.

---

### 🎯 Example:

```ts
nums = [10, 9, 2, 5, 3, 7, 101, 18]
```

🔢 One increasing subsequence: \[2, 3, 7, 101] → Length = 4
✅ Answer: `4`

---

### 🧠 Memoization (Top-Down Recursion)

Try picking or skipping each number, store results to avoid repeats.

```ts
function lengthOfLIS(nums: number[]): number {
  const memo = new Map<string, number>();

  const dp = (i: number, prev: number): number => {
    const key = `${i}-${prev}`;
    if (memo.has(key)) return memo.get(key)!;
    if (i === nums.length) return 0;

    let take = 0;
    if (prev === -1 || nums[i] > nums[prev]) {
      take = 1 + dp(i + 1, i);
    }
    const skip = dp(i + 1, prev);

    const res = Math.max(take, skip);
    memo.set(key, res);
    return res;
  };

  return dp(0, -1);
}
```

* ✅ Time: O(n²)
* ✅ Space: O(n²)

---

### ⚡ Optimized Tabulation with Binary Search (O(n log n))

```ts
function lengthOfLIS(nums: number[]): number {
  const sub: number[] = [];

  for (let num of nums) {
    let left = 0, right = sub.length;
    while (left < right) {
      const mid = Math.floor((left + right) / 2);
      if (sub[mid] < num) left = mid + 1;
      else right = mid;
    }
    sub[left] = num;
  }

  return sub.length;
}
```

* ⚡ Time: O(n log n)
* 🧠 Space: O(n)
* ✔️ Most efficient solution

---

### 🧒 Like a Kid:

You’re stacking blocks 📦.
Each block must be taller 📏 than the last.
Find the **longest tower** you can build by skipping or picking blocks wisely.

Want a visual dry run or explanation of the O(n log n) trick?
