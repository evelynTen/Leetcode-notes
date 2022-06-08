# Content
## 1 Basic Operations
### 1.1 Reverse
- [206. Reverse Linked List](https://leetcode.com/problems/reverse-linked-list/)
  - Iterative
    - Time complexity : O(n). Assume that n is the list's length, the time complexity is O(n).
    - Space complexity : O(1).
  - Recursive TC: O(n) SC: O(n)
    - Time complexity : O(n). Assume that n is the list's length, the time complexity is O(n).
    - Space complexity : O(n). The extra space comes from implicit stack space due to recursion. The recursion could go up to n levels deep.
  > **Iterative:**  
  > 定义prev = null, head指向prev，之后prev是链表的第一个节点head, head是第二个节点。最后prev指向最后一个节点，head == null，return prev.
 
- [92. Reverse Linked List II](https://leetcode.com/problems/reverse-linked-list-ii/)
  
