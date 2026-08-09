# Some Thoughts before coming to the solution
<!-- Describe your first thoughts on how to solve this problem. -->
I recently started doing Leetcode problems and solving one problem in more than one ways has helped me more than solving multiple problems instead.
One day I am going to forget how I arrived at this solution and so I am lowkey making this note for myself. An artisan knows there are more than one ways to do the same thing but choose the most elegant route.

``` NOTE []
Just a tip from my side : If you cannot solve any linkedlist problem directly, simply convert it into vector.
```
However, that diminishes the actual advantage and learning curve that can be gained via LinkedList problems.

> PS: Rn I am only writing in C++ but after building good foundation I will rewrite all these problems in Java.

# Approach(es)
<!-- Describe your approach to solving the problem. -->
I solved this problem using two approaches as of now.
- Using Vector
- Using Fast and slow pointer

# Approach 1 (Using Vector)

This is a layman's approach imo but works nicely and can be used as a fallback if no other solution is coming to your mind in that moment.

## Complexity
- Time complexity: O(n)
<!-- Add your time complexity here, e.g. $$O(n)$$ -->

- Space complexity:O(n)
<!-- Add your space complexity here, e.g. $$O(n)$$ -->

## Code I

```cpp []
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode() : val(0), next(nullptr) {}
 *     ListNode(int x) : val(x), next(nullptr) {}
 *     ListNode(int x, ListNode *next) : val(x), next(next) {}
 * };
 */
class Solution {
public:
    bool isPalindrome(ListNode* head) {
        vector<int> listvals;
        while(head){
            listvals.push_back(head->val);
            head = head -> next;
        }
        int i = 0;
        int j = listvals.size()-1;
        while(i<j){
            
            if(listvals[i]!=listvals[j]){
                return false;
            }
            i++;
            j--;
        }
        return true;
    }
};
```

# Approach 2 (Using Fast and Slow Pointer)

Honestly, I just learned about fast and slow pointer approach and wanted to try it out and this seemed like the perfect opportunity for that and boy was I right. Not only was I able to practice this approach, I also practiced reversing a linked list.

Why this approach is considered optimal, according to google :)

1. It relies on three classic linked-list techniques:

2. Fast and slow pointers to find the midpoint in one traversal.
In-place reversal of the second half, avoiding extra memory.
Linear comparison of corresponding nodes.

3. Together, these achieve the best possible asymptotic complexity for this problem: O(n) time and O(1) auxiliary space, without needing random access or additional data structures.

What does "extra space" mean?

It refers to additional memory used by the algorithm, excluding the input itself.

``` RULE_OF_THUMB []

Ask yourself:

If the input size doubles, does my algorithm need more additional memory?

No → O(1) extra space
Yes, proportional to n → O(n) extra space
Yes, proportional to log n → O(log n) extra space (e.g., recursion depth in balanced trees)

That's why this palindrome solution is considered optimal: it processes the list in linear time while using only a constant amount of additional memory.

```
## Common misconception

Some people think:
```
"But we're changing the linked list, so aren't we using more memory?"
```
No. You're reusing the existing nodes.

For example:

Before reversing:
```
1 → 2 → 3
```
After reversing:
```
3 → 2 → 1
```
You didn't create any new nodes. You only changed the next pointers of the existing ones. Reusing existing memory does not count as extra space.


## Complexity
- Time complexity: O(n)
<!-- Add your time complexity here, e.g. $$O(n)$$ -->

- Space complexity:O(1)
<!-- Add your space complexity here, e.g. $$O(n)$$ -->

## Code II
```cpp []
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode() : val(0), next(nullptr) {}
 *     ListNode(int x) : val(x), next(nullptr) {}
 *     ListNode(int x, ListNode *next) : val(x), next(next) {}
 * };
 */
class Solution {
public:
    ListNode* reverse(ListNode* head){
        ListNode* prev = NULL;
        ListNode* next = NULL;
        ListNode* curr = head;
        while(curr!=NULL){
            next = curr->next;
            curr->next = prev;
            prev = curr;
            curr = next;
        }
        return prev;
    }
    bool isPalindrome(ListNode* head) {
        ListNode* slow = head;
        ListNode* fast = head;

        while(fast != NULL && fast->next !=NULL){
            slow = slow->next;
            fast = fast->next->next;
        }

        ListNode* rev = reverse(slow);

        while(rev != NULL){
            if(head->val != rev->val){
                return false;
            }
            head = head->next;
            rev = rev->next;
        }
        return true;

    }
};
```

> *I hope this note find a fellow traveler on the path of learning. If they light even a small part of your journey, they've served their purpose. I'm still learning myself, and it brings me joy to know our paths crossed, if only for a moment, in the pursuit of understanding.*




