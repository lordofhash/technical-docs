# Some Thoughts Before Coming to the Solution
<!-- Describe your first thoughts on how to solve this problem. -->
I am documenting this because one day I will forget how I solved this problem.
At first glance, this problem looks like another "find pairs" question. In reality, it is testing whether you can simplify an equation and recognize that a brute-force comparison is unnecessary.
The concepts hidden inside this problem are much more important than the problem itself:
- Reversing an integer without using built-in functions.
- Understanding how a hashmap (unordered_map) stores frequencies.
- Basic algebraic manipulation.
- Counting combinations instead of generating pairs.
- Preventing integer overflow.
- Using modulo arithmetic correctly.

This problem reminded me that a little mathematics can eliminate a lot of computation.

# Approach
<!-- Describe your approach to solving the problem. -->
Initially, the condition for a nice pair is
```
nums[i] + rev(nums[j]) = nums[j] + rev(nums[i])
```
A brute-force solution would compare every pair.
```
for every i
    for every j > i
        check condition
```

This gives

> **Time Complexity = O(n²)**

which is too slow.

FAAAAAAAAAH

However, just by using lil bit of maths the equation becomes : 
```
nums[i] - rev(nums[i]) = nums[j] - rev(nums[j]) 
```
This changes the entire problem.

Instead of comparing every pair, we only need to find numbers having the same difference.
For every number,

```cpp
difference = rev(number) - number;
```
(or equivalently `number - rev(number)` as long as you're consistent.)

Now the problem becomes:

> Count how many numbers have the same difference.

This is exactly what a hashmap is good at.

---

## Understanding `unordered_map`

Declaration

```cpp
unordered_map<int, int> mp;
```

means

```text
Key   -> int
Value -> int
```

### Example

Suppose we have

| Number | Reverse | Difference (`rev(num)-num`) |
|---------|----------|----------------------------|
| 42 | 24 | -18 |
| 97 | 79 | -18 |
| 13 | 31 | 18 |
| 20 | 2 | -18 |

Our hashmap becomes

```text
Key      Value

-18 ---> 3
18  ---> 1
```

Meaning

```text
Difference -18 appeared three times.

Difference 18 appeared once.
```

Notice that **the key is NOT the number itself.**

The key is

```cpp
rev(number) - number
```

The value is simply

```text
frequency
```

---

## What does this line do?

```cpp
revmap[diff]++;
```

Suppose the map is empty.

```text
{}
```

First difference

```cpp
diff = 18;

revmap[18]++;
```

Map becomes

```text
18 -> 1
```

Another number gives

```cpp
diff = 18;
```

Again

```cpp
revmap[18]++;
```

Map becomes

```text
18 -> 2
```

Again

```text
18 -> 3
```

No need to check if the key already exists.

`unordered_map` automatically inserts a new key with value `0` if it doesn't exist.


---

# Reversing an Integer

A very common interview pattern.

Suppose

```text
x = 1234
```

We repeatedly remove the last digit.

### Iteration 1

```cpp
digit = x % 10;
```

```text
digit = 4
```

Append it

```cpp
reversed = reversed * 10 + digit;
```

Initially

```text
reversed = 0

0 * 10 + 4 = 4
```

Remove the last digit

```cpp
x /= 10;
```

```text
x = 123
```

---

### Iteration 2

```text
digit = 3

reversed = 4 * 10 + 3 = 43

x = 12
```

---

### Iteration 3

```text
digit = 2

reversed = 43 * 10 + 2 = 432

x = 1
```

---

### Iteration 4

```text
digit = 1

reversed = 432 * 10 + 1 = 4321

x = 0
```

Loop ends.

### Final Code

```cpp
int rev(int x)
{
    int reversed = 0;

    while(x > 0)
    {
        reversed = reversed * 10 + (x % 10);
        x /= 10;
    }

    return reversed;
}
```

---

# Preventing Overflow

Imagine

```text
count = 100000
```

Then

```cpp
count * (count - 1)
```

is approximately

```
10^10
```

Maximum value of an `int`

```
2,147,483,647
≈ 2.1 × 10^9
```

Since

```
10^10 > 2.1 × 10^9
```

an `int` will overflow.

Instead use

```cpp
long long count;
long long pairs;
long long result;
```

A `long long` can safely store values up to approximately

```
9 × 10^18
```

---

# Why Modulo?

LeetCode usually asks for the answer modulo

```cpp
1e9 + 7
```

because the actual answer may become extremely large.

Instead of storing

```
123456789123456789...
```

we only store its remainder after division by `1e9+7`.

Always apply modulo during accumulation.

```cpp
result = (result + pairs) % MOD;
```

instead of waiting until the very end.

This keeps numbers small throughout the computation and avoids unnecessary overflow.

---

# Complexity

### Time Complexity

```
O(n)
```

- One pass to compute the difference of every number.
- One pass through the hashmap.

---

### Space Complexity

```
O(n)
```

In the worst case, every difference is unique and stored in the hashmap.

---

# Code

```cpp []
class Solution {
public:

    int rev(int x){
        int reversed = 0;
        while(x>0){
            reversed = reversed*10 + (x%10);
            x/=10;
        }
        return reversed;
    }

    long long countNicePairs(vector<int>& nums) {

        long long result = 0;
        const long long MOD = 1e9 + 7;

        unordered_map<int,int> revmap;

        for(int i=0;i<nums.size();i++){

            int diff = rev(nums[i]) - nums[i];
            revmap[diff]++;
        }

        for(auto &x : revmap){

            long long count = x.second;

            long long pairs = count * (count - 1) / 2;

            result = (result + pairs) % MOD;
        }

        return result;
    }
};
```

> Every solution is just another log on a fire built by countless curious minds. If this adds even a little warmth to your own journey, then it was worth leaving behind.

