# 回溯题目

返回所有组合

## 17.电话号码的字母组合

给定一个仅包含数字 `2-9` 的字符串，返回所有它能表示的字母组合。

给出数字到字母的映射如下（与电话按键相同）。注意 1 不对应任何字母。

![img](/Users/zhengxinzhi/typora_img/LeetCode%E9%A2%98%E7%9B%AE%E6%80%BB%E7%BB%93/200px-Telephone-keypad2.svg.png)

**示例:**

```
输入："23"
输出：["ad", "ae", "af", "bd", "be", "bf", "cd", "ce", "cf"].
```

**说明:**
尽管上面的答案是按字典序排列的，但是你可以任意选择答案输出的顺序。

### 解法

#### 解法一 

**Complexity Analysis**

- Time complexity : O(3N×4M)where `N` is the number of digits in the input that maps to 3 letters (*e.g.* `2, 3, 4, 5, 6, 8`) and `M` is the number of digits in the input that maps to 4 letters (*e.g.* `7, 9`), and `N+M` is the total number digits in the input.
- Space complexity : O(3N×4M) since one has to keep 3N×4Msolutions.

```Python
class Solution:
    def letterCombinations(self, digits: str) -> List[str]:
        
        def generate(i, item, nums, result, phone):
            if len(nums) == len(item):
                result.append(item)
            else:
                for c in phone[digits[i]]:
                    generate(i+1, item + c, nums, result, phone)

           
        # 边界条件
        if digits == "":
            return []
        phone = {'2': ['a', 'b', 'c'],
         '3': ['d', 'e', 'f'],
         '4': ['g', 'h', 'i'],
         '5': ['j', 'k', 'l'],
         '6': ['m', 'n', 'o'],
         '7': ['p', 'q', 'r', 's'],
         '8': ['t', 'u', 'v'],
         '9': ['w', 'x', 'y', 'z']}
        
        item = ""
        result = []
        nums = digits
        generate(0, item, nums, result, phone)
        return result
        
        
        
                
```

## 78.子集

### 题目描述

给定一组不含重复元素的整数数组 nums，返回该数组所有可能的子集（幂集）。

说明：解集不能包含重复的子集。

示例:

输入: nums = [1,2,3]
输出:
[
  [3],
  [1],
  [2],
  [1,2,3],
  [1,3],
  [2,3],
  [1,2],
  []
]

### 解法 

首先 ，这个题是NP问题，没有多项式时间内的算法，只能用搜索解决的问题

选择用DFS-backtracking 的递归方式解决

#### 解法一 回溯

```python
class Solution:
    def subsets(self, nums):
        def generate(i, nums, item, result):
            result.append(list(item))
            for j in range(i, len(nums)):
                item.append(nums[j])
                generate(j+1, nums, item, result)
                item.pop()

        result = []
        generate(0, nums, [], result)
        return result
```

#### 解法二 位运算

> & | ^ ~ << >>

本题使用到 & and <<

集合的每个元素，都有可以选或不选，对每一位为1的表示这一位的数字存在，用二进制和位运算，可以很好的表示。(具体细节见题目总结文档)

```java
    public static List<List<Integer>> binaryBit(int[] nums) {
        List<List<Integer>> res = new ArrayList<List<Integer>>();
        for (int i = 0; i < (1 << nums.length); i++) {
            List<Integer> sub = new ArrayList<Integer>();
            for (int j = 0; j < nums.length; j++)
                // 1 << 0位001，1 << 1为010，1 << 2为100
                if (i & (1 << j)) sub.add(nums[j]);
            res.add(sub);
        }
        return res;
    }
```

## 90 子集II

### 题目描述

给定一个可能包含重复元素的整数数组 **\*nums***，返回该数组所有可能的子集（幂集）。

**说明：**解集不能包含重复的子集。

**示例:**

```
输入: [1,2,2]
输出:
[
  [2],
  [1],
  [1,2,2],
  [2,2],
  [1,2],
  []
]

```

### 解法

#### 解法一 回溯

```python
class Solution:
    def subsetsWithDup(self, nums: List[int]) -> List[List[int]]:
        """
            和子集I区别在于有重复
            重复分两种 一种是集合相同但位置不同 可通过排序解决
                      一种是集合相同位置也相同 可通过结果用set保存
            set 直接存list 返回 type error: unhashable type: list
            
        """
        def generate(i, item, nums,  result):
            result.add(tuple(item))
            for j in range(i, len(nums)):
                item.append(nums[j])
                generate(j+1, item, nums,  result)
                item.pop() 

        # 做排序 防止第一种重复
        item = []
        result = set()
        nums.sort()
        generate(0, item, nums,  result)
        # 如何对列表中的列表去重 或者程序中如何做判断防止相同的列表进入
        result = [list(item) for item in result]
        return result
        
```

#### 解法二 递归

```python
class Solution:
    def subsetsWithDup(self, nums: List[int]) -> List[List[int]]:
        if not nums:
            return [[]]
        nums = sorted(nums)
        num = nums[0]
        subsets = self.subsetsWithDup(nums[1:])
        subsets = subsets + [i+[num] for i in subsets]
        subsets = [list(j) for j in set([tuple(item) for item in subsets])]
        return subsets
```



## 39.组合总和

### 题目描述

给定一个**无重复元素**的数组 `candidates` 和一个目标数 `target` ，找出 `candidates` 中所有可以使数字和为 `target` 的组合。

`candidates` 中的数字可以无限制重复被选取。

**说明：**

- 所有数字（包括 `target`）都是正整数。
- 解集不能包含重复的组合。 

**示例 1:**

```
输入: candidates = [2,3,6,7], target = 7,
所求解集为:
[
  [7],
  [2,2,3]
]
```

**示例 2:**

```
输入: candidates = [2,3,5], target = 8,
所求解集为:
[
  [2,2,2,2],
  [2,3,3],
  [3,5]
]
```

### 解法

#### 解法一 回溯

```python
class Solution:
    def combinationSum(self, candidates: List[int], target: int) -> List[List[int]]:

        def generate(i, item, nums,  result, target):
            if target == 0:
                result.append(list(item))
            elif target < 0:
                return 
            else:
                for j in range(i, len(nums)):
                    item.append(nums[j])
                    # key: candidates 中的数字可以无限制重复被选取。所以传入j
                    generate(j, item, nums,  result, target - nums[j])
                    item.pop()

        item = []
        result = []
        nums = sorted(candidates)
        generate(0, item, nums,  result, target)
        return result
```

## 40 组合总和II

### 题目描述

给定一个数组 `candidates` 和一个目标数 `target` ，找出 `candidates` 中所有可以使数字和为 `target` 的组合。

`candidates` 中的每个数字在每个组合中只能使用一次。

**说明：**

- 所有数字（包括目标数）都是正整数。
- 解集不能包含重复的组合。 

**示例 1:**

```
输入: candidates = [10,1,2,7,6,1,5], target = 8,
所求解集为:
[
  [1, 7],
  [1, 2, 5],
  [2, 6],
  [1, 1, 6]
]
```

**示例 2:**

```
输入: candidates = [2,5,2,1,2], target = 5,
所求解集为:
[
  [1,2,2],
  [5]
]
```

### 解法

#### 解法一 回溯 （推荐）

```python
import copy
class Solution:
    def combinationSum2(self, candidates: List[int], target: int) -> List[List[int]]:
				"""
				            本题创新点: 如何剪枝
            思考: 当子集累计值大于 >= target时 剪枝(因为所有数字都是正整数)
            防止不能包含重复的组合:
                两种重复:
                1.集合中元素位置不同 值相同(先排序)
                2.集合中元素相同(二重列表去重 或防止重复集合进入结果集)
        """
        def generate(i, item, nums, result, target):
            if target == 0:
                result.add(tuple(item))
            elif target < 0:
                return
            else:   
                for j in range(i, len(nums)):
										# 剪枝 跳过重复
                    if j > i and nums[j] == nums[j-1]:
                        continue
										# 前面做了nums排序
                    if nums[j] > target:
                        break
                    generate(j+1, item + [nums[j]], nums, result, target - nums[j])

        item = []
        result = set()
        nums = sorted(candidates)
        generate(0, item, nums, result, target)
        result = [list(item) for item in result]
        return result
        
```



## 216. 组合总和 III

### 题目描述

找出所有相加之和为 ***n*** 的 **k** 个数的组合**。**组合中只允许含有 1 - 9 的正整数，并且每种组合中不存在重复的数字。

**说明：**

- 所有数字都是正整数。
- 解集不能包含重复的组合。 

**示例 1:**

```
输入: k = 3, n = 7
输出: [[1,2,4]]
```

**示例 2:**

```
输入: k = 3, n = 9
输出: [[1,2,6], [1,3,5], [2,3,4]]
```

### 解法

#### 解法一

```python
class Solution:
    def combinationSum3(self, k: int, n: int) -> List[List[int]]:
        
        def generate(i, item, nums, result, n):
            if n < 0 or len(item) > k:
                return
            if len(item) == k and n == 0:
                result.append(list(item))
            else:
                for j in range(i, len(nums)):
                    generate(j+1, item + [nums[j]], nums, result, n - nums[j])

        nums = list(range(1, min(n + 1, 10)))
        item = []
        result = []
        generate(0, item, nums, result, n)
        return result
```

##### 

## 377. 组合总和 Ⅳ DP

### 题目描述

给定一个由正整数组成且不存在重复数字的数组，找出和为给定目标正整数的组合的个数。

示例:

nums = [1, 2, 3]
target = 4

所有可能的组合为：
(1, 1, 1, 1)
(1, 1, 2)
(1, 2, 1)
(1, 3)
(2, 1, 1)
(2, 2)
(3, 1)

请注意，顺序不同的序列被视作不同的组合。

因此输出为 7。
进阶：
如果给定的数组中含有负数会怎么样？
问题会产生什么变化？
我们需要在题目中添加什么限制来允许负数的出现？

### 解法

### 解法一 DP

```java
class Solution:
    def combinationSum4(self, nums: List[int], target: int) -> int:            
        dp = [0] * (target+1)
        dp[0] = 1
        for i in range(1, target+1):
            for num in nums:
                if i >= num:
                    dp[i] += dp[i-num]
        return dp[target]
```

```java
class Solution {
    public int combinationSum4(int[] nums, int target) {
        // state是target
        int[] dp = new int[target + 1];
        dp[0] = 1;
        for(int i = 1; i <= target; i++){
            for(int j = 0; j < nums.length; j++){
                if(i >= nums[j]){
                    dp[i] += dp[i-nums[j]];
                }
            } 
        }
        return dp[target];
    }
}
```

## 46.全排列

### 题目描述

给定一个**没有重复**数字的序列，返回其所有可能的全排列。

进阶：有重复数字

**示例:**

```
输入: [1,2,3]
输出:
[
  [1,2,3],
  [1,3,2],
  [2,1,3],
  [2,3,1],
  [3,1,2],
  [3,2,1]
]
```

### 解法

#### 解法一

```python
class Solution:
    def permute(self, nums: List[int]) -> List[List[int]]:
        
        def generate(item, nums, result):
            if len(item) == len(nums):
                result.append(list(item))
            else:
                for j in range(len(nums)):
                    if nums[j] not in item:
                        # 不使用set的原因是set无序
                        item.append(nums[j])
                        generate(item, nums, result)
                        item.pop()
    
        item = []
        result = []
        generate(item, nums, result)
        return result
```

