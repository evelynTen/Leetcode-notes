# Content
## 1 Basic Operations
### 1.1 Reverse
- [206. Reverse Linked List](https://leetcode.com/problems/reverse-linked-list/)
  > **Iterative:**  
  > 定义prev = null, head指向prev，之后prev是链表的第一个节点head, head是第二个节点。最后prev指向最后一个节点，head == null，return prev.  

  ```java  
  class Solution {
    public ListNode reverseList(ListNode head) {
        ListNode prev = null;
        ListNode curr = head; //1
        while (curr != null) {
            ListNode nextTemp = curr.next; //2
            curr.next = prev; //null <- 1 || 1 -> null
            prev = curr; // prev == 1
            curr = nextTemp; // curr == 2
        }
        return prev;
    }
  }
  ```

  - Iterative
    - Time complexity : O(n). Assume that n is the list's length, the time complexity is O(n).
    - Space complexity : O(1).
  - Recursive 
    - Time complexity : O(n). Assume that n is the list's length, the time complexity is O(n).
    - Space complexity : O(n). The extra space comes from implicit stack space due to recursion. The recursion could go up to n levels deep.
 
- [92. Reverse Linked List II](https://leetcode.com/problems/reverse-linked-list-ii/)
  > **Iterative:**
  > 1. 先找到左节点，注意循环条件left > 1，因为left = curr, prev要指向left前一个节点，只用移动left - 1次，每移动一次left--, right--。
  > 2. 指定con = prev, tail = curr。
  > 3. 开始翻转，循环条件right > 0，因为right--了和left一样的次数，right和left之间差值与之前相同，需要翻转right - left + 1次。到此prev == right。
  > 4. if (con != null) con.next = prev; con == null代表从头开始翻转，应head = prev。tail.next = curr。
  > 5. return head

  ```java
  class Solution {
    public ListNode reverseBetween(ListNode head, int left, int right) {
        if (head == null) return null;
        
        ListNode curr = head, prev = null;
        while (left > 1) {
            prev = curr;
            curr = curr.next;
            left--;
            right--;
        }
        
        ListNode con = prev, tail = curr;
        
        while (right > 0) {
            ListNode temp = curr.next;
            curr.next = prev;
            prev = curr;
            curr = temp;
            right--;
        }
        
        if (con != null) {
            con.next = prev;
        }
        else {
            head = prev;
        }
        
        tail.next = curr;
        
        return head;
    }
  }
  ```

  - Iterative
    - Time Complexity: O(N) considering the list consists of N nodes. We process each of the nodes at most once (we don't process the nodes after the n^{th} node from       the beginning.
    - Space Complexity: O(1) since we simply adjust some pointers in the original linked list and only use O(1) additional memory for achieving the final result.
  - Recursive 
    - Time Complexity: O(N) since we process all the nodes at-most twice. Once during the normal recursion process and once during the backtracking process. During the       backtracking process we only just swap half of the list if you think about it, but the overall complexity is O(N).
    - Space Complexity: O(N) in the worst case when we have to reverse the entire list. This is the space occupied by the recursion stack.
