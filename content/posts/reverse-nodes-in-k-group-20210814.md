+++
title="Reverse Nodes in k-Group"
description="A detailed step-by-step breakdown of the LeetCode Hard 'Reverse Nodes in k-Group' problem, with a clear explanation and a C++ implementation"
date=2021-08-14

[taxonomies]
categories = ["Programming"]
tags = ["programming", "algorithms", "leetcode"]
+++


So, after a very good amount of struggle, finally I was able to solve this problem`[1]`.
The main intuition behind the solution is that, for each group of k nodes, we need to reverse the links between them, which is exactly `(k-1)` links.


### Example:

Consider the following linked list:

```
1 -> 2 -> 3 -> 4 -> 5 -> NULL
```

And let `k = 3`.

for this example, we've only one group of 3 nodes, (which is `1 -> 2 -> 3`). So, we'll reverse both the links in the group one by one

- **After 1st reversal:**
  ```
  2 -> 1 -> 3 -> 4 -> 5 -> NULL
  ```

- **After 2nd reversal:**
  ```
  3 -> 2 -> 1 -> 4 -> 5 -> NULL
  ```

Now, let's dive into how we can reverse each link in the group smartly.


### Visualization

Here is a sketch to help visualize the process of reversing a single link in the group:

![image](https://raw.githubusercontent.com/argon17/SDESheet/refs/heads/master/LeetcodeDiscussions/imgs/reverseNodesKGroup.png)

The following three lines of code reverse a single link without affecting the others. Let's break it down line by line:

1. **Before reversing** the `b -> c` link, we hold the `c`'s next using `b`'s next, so we don’t lose it.
2. Now, **reverse** the `b -> c` link and point `c`'s next to `a`'s next (which is currently `b`).
3. Finally, **point** `a`'s next to `c` to complete the reversal.

These three magical lines reverses one link, without affecting the other links. Let's break it down line by line.

1. **Before reversing** the `b -> c` link, we hold the `c`'s next using `b`'s next, so we don’t lose it.
2. Now, **reverse** the `b -> c` link by pointing `c`'s next to `a`'s next (which is currently `b`).
3. Finally, **point** `a`'s next to `c` to complete the reversal.

Now let's have a look at the code.

### The Code

Here’s the code that implements the solution:

```cpp
class Solution {
public:
    ListNode* reverseKGroup(ListNode* head, int k) {
        ListNode* dummy = new ListNode();
        dummy->next = head;
        ListNode* pre = dummy, *cur, *nex;
        int cnt = length(head);
        // continue till more than k groups left
        while(cnt>=k){
            // maintain the order [ pre->cur->nex ] 
            cur = pre->next;
            nex = cur->next;
            // reverse (k-1) links in the group
            for(int i = 1; i < k; ++i)
                reverseSingleLink(pre, cur, nex);
            cnt -= k;
            // to move to next group, pre will now be cur
            pre = cur;
        }
        return dummy->next;
    }
private:
    void reverseSingleLink(ListNode* &pre, ListNode* &cur, ListNode* &nex){
        // to move forward in the group
        nex = cur->next;
        // those three magical lines
        cur->next = nex->next;
        nex->next = pre->next;
        pre->next = nex;
    }
    // to find the length of the list
    int length(ListNode* head){
        int cnt = 0;
        while(head){
            ++cnt;
            head = head->next;
        }
        return cnt;
    }
};
```

### Reference
`[1]` [Leetcode 25 - Reverse Nodes in k-Group](https://leetcode.com/problems/reverse-nodes-in-k-group/)