# Bless up.
<!-- Describe your first thoughts on how to solve this problem. -->
Trying a new question makes you "question" your approach and knowledge and it is completely fine to have fear when facing something new. But with practice, things start becoming intuitive. And although I just started doing leetcode problems, this one seemed easy n interesting enough that I'd document my thoughts and notes n findings from the net for my future self.

# Approach
<!-- Describe your approach to solving the problem. -->
This is one of those problems that seems like Dynamic Programming at first ("find a subsequence"), but the optimal solution is actually Greedy.

The Brute force approach would have been to try every triplet...which has a time complexity of O(n³). So not quite.

The approach that we use is :
While scanning, we know which is the smallest element till now and which is the second smallest till now. Let's find a number which is greater than both.

I know it sounds stupid but trust me this is exactly what **greedy approach** is.

Mr. Greedy always asks:
"Can I make the best local decision right now without worrying about the future?"
Here our local decisions are
Can I reduce the variable `first`?
If yes then replace it.
Or
Can I reduce the variable `second`?
If yes then replace it.

Why?
Because smaller candidates make future success easier.
- We never reconsider.
- We never backtrack.
- We never store alternatives.
- We simply make the locally best replacement.

**That's exactly greedy.**

So, the approach is to scan the array and keep the smallest element seen so far (first) and the smallest possible second element of an increasing pair (second). Whenever we find a smaller candidate for either, we greedily replace it because smaller values make it easier to complete an increasing triplet later. If we ever encounter a number larger than second, we've found an increasing triplet.

--- 

## 🧪 Dry Run
first  = ∞
second = ∞

**Input:** `nums = [2, 1, 5, 0, 4, 6]`

| `num` | Condition | `first` | `second` | Action |
|:-----:|-----------|:-------:|:--------:|--------|
| `2` | `2 <= first` | `2` | `∞` | Update `first` |
| `1` | `1 <= first` | `1` | `∞` | Update `first` |
| `5` | `5 <= second` | `1` | `5` | Update `second` |
| `0` | `0 <= first` | `0` | `5` | Update `first` |
| `4` | `4 <= second` | `0` | `4` | Update `second` |
| `6` | `6 > second` | `0` | `4` | 🎯 Triplet found |

**Result:** `true`

**Triplet:** `0 < 4 < 6`

---



# Complexity
- Time complexity: `O(n)` — single pass through the array
<!-- Add your time complexity here, e.g. $$O(n)$$ -->

- Space complexity: `O(1)` — only `first` and `second` are stored
<!-- Add your space complexity here, e.g. $$O(n)$$ -->

# Code
```cpp []
class Solution {
public:
    bool increasingTriplet(vector<int>& nums) {
        int first = INT_MAX;
        int second = INT_MAX;

        for(auto& x : nums){
            if(x<=first){
                first = x;
            }
            else if(x<=second){
                second = x;
            }
            else{ //x>second>first
                return true;
            }
        }
        return false;
    }
};
```





---
---
# 🧠 Mental Checklist to Identify the Pattern
```
Whenever you read a new problem, don't jump into coding immediately. Instead, ask yourself these questions in order.
```

## 1️⃣ Does the answer involve a **contiguous** range?

Examples:
- Subarray
- Substring
- Continuous segment

### ✅ Yes
Think:
- Sliding Window
- Prefix Sum
- Monotonic Queue

Examples:
- Longest Substring Without Repeating Characters → Sliding Window
- Maximum Sum Subarray → Kadane's Algorithm

### ❌ No
Continue to Question 2.

Example:
- Increasing Triplet Subsequence → Not contiguous → Continue



## 2️⃣ Is the array already sorted, or would sorting simplify the problem?

### ✅ Yes
Think:
- Two Pointers
- Binary Search
- Greedy after Sorting
- Interval Problems

Examples:
- Two Sum II
- Merge Intervals
- Meeting Rooms

### ❌ No
Continue.



## 3️⃣ Are you optimizing something (minimum / maximum / earliest / cheapest)?

Examples:
- Minimum cost
- Maximum profit
- Largest value
- Earliest completion

### Ask yourself:

### ❓Can making the best local choice never hurt the final answer?

✅ Yes → Greedy

Examples:
- Increasing Triplet
- Jump Game
- Gas Station
- Assign Cookies



### ❓Do I need to compare many previous possibilities?

✅ Yes → Dynamic Programming

Examples:
- Coin Change
- House Robber
- Longest Increasing Subsequence
- Edit Distance



## 4️⃣ Can everything you've seen so far be summarized using only a few variables?

### ✅ Yes
Usually:
- Greedy
- One-pass Scan
- Running Minimum/Maximum

Examples:
- Best Time to Buy & Sell Stock
- Increasing Triplet
- Maximum Difference

Increasing Triplet only stores:

- `first`
- `second`

Nothing else.

### ❌ No
Continue.



## 5️⃣ Am I repeatedly querying ranges?

Examples:
- Sum of elements
- Minimum in a range
- Maximum in a range
- Frequency in a range

Think:

- Prefix Sum
- Fenwick Tree (BIT)
- Segment Tree
- Sparse Table



## 6️⃣ Are decisions affecting future decisions?

Examples:
- Number of ways
- Maximum profit
- Minimum cost
- Longest sequence

### Ask:

Do different choices lead to overlapping subproblems?

### ✅ Yes
Dynamic Programming

Common clues:

- "Maximum..."
- "Minimum..."
- "Count ways..."
- "Longest..."
- "Can I reach..."



## 7️⃣ Am I exploring multiple possibilities or paths?

### Tree traversal
→ DFS / BFS

### Graph traversal
→ DFS / BFS

### All subsets or permutations
→ Backtracking

### Shortest path
→ Dijkstra
→ BFS (unweighted)
→ Bellman-Ford

### Dependency ordering
→ Topological Sort



## 8️⃣ Do I need fast lookup of values or frequencies?

Think:

- Hash Map
- Hash Set

Often combined with:

- Sliding Window
- Prefix Sum

Examples:

- Two Sum
- Subarray Sum Equals K
- Longest Consecutive Sequence



## 9️⃣ Is there a monotonic property?

Ask yourself:

> If one answer works, will every larger (or smaller) answer also work?

### ✅ Yes
Binary Search on the Answer

Examples:

- Koko Eating Bananas
- Capacity To Ship Packages Within D Days
- Split Array Largest Sum

Typical clue:

Find the **minimum/maximum** value satisfying a condition.



## 🔟 Are elements continuously entering and leaving while you need the minimum or maximum?

Think:

- Monotonic Stack
- Monotonic Queue
- Heap / Priority Queue

Examples:

- Sliding Window Maximum
- Daily Temperatures
- Next Greater Element



## 1️⃣1️⃣ Does the input describe connections?

Examples:

- Cities
- Friends
- Roads
- Courses
- Computers

Think:

- Graph Algorithms
- Tree Algorithms
- Union-Find (DSU)

Examples:

- Number of Islands
- Course Schedule
- Network Delay Time



## 1️⃣2️⃣ Am I repeatedly merging intervals or groups?

Think:

- Merge Intervals
- Union-Find
- Sweep Line

Examples:

- Merge Intervals
- Accounts Merge
- Employee Free Time



# 🐨🫧🍓 Quick Recognition Table

| If you see... | Think... |
|---------------|----------|
| Contiguous subarray/substring | Sliding Window / Prefix Sum |
| Sorted array | Two Pointers / Binary Search |
| Local optimal decisions | Greedy |
| Overlapping subproblems | Dynamic Programming |
| Fast lookup | Hash Map / Hash Set |
| Tree or graph traversal | DFS / BFS |
| Dependencies | Topological Sort |
| Shortest path | Dijkstra / BFS |
| Dynamic min/max | Heap / Monotonic Queue |
| Next Greater/Smaller | Monotonic Stack |
| Connectivity | Union-Find |
| Repeated range queries | Segment Tree / Fenwick Tree |
| Monotonic feasibility | Binary Search on Answer |

---


Tbh before writing a single line of code, spend 2–3 minutes classifying the problem.

> Once you identify the correct pattern, the implementation often becomes straightforward.

---
---
> We inherit understanding from strangers we'll never meet, and leave our own for those we'll never know. May this small contribution honor that quiet tradition.
