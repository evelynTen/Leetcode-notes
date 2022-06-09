# Content
## 1 Basic Operation
### 1.1 Merge
- [21. Merge Two Sorted Lists](https://leetcode.com/problems/merge-two-sorted-lists/)
  > Recursion

  ```java
  class Solution {
    public ListNode mergeTwoLists(ListNode list1, ListNode list2) {
        
        if (list1 == null) {
            return list2;
        }
        else if (list2 == null) {
            return list1;
        }
        else if (list1.val <= list2.val) {
            list1.next = mergeTwoLists(list1.next, list2);
            return list1;
        }
        else {
            list2.next = mergeTwoLists(list2.next, list1);
            return list2;
        }
        
    }
  }
  ```
  - Time complexity : O(n + m). Because each recursive call increments the pointer to l1 or l2 by one (approaching the dangling null at the end of each list), 
there will be exactly one call to mergeTwoLists per element in each list. Therefore, the time complexity is linear in the combined size of the lists.
  - Space complexity : O(n + m). The first call to mergeTwoLists does not return until the ends of both l1 and l2 have been reached, so n + mstack frames consume O(n + m) space.
  
### 1.2 Intersection
- [160. Intersection of Two Linked Lists](https://leetcode.com/problems/intersection-of-two-linked-lists/)
  > Hash Table

  ```java
  public class Solution {
    public ListNode getIntersectionNode(ListNode headA, ListNode headB) {
        Set<ListNode> nodesInB = new HashSet<ListNode>();
        
        while (headB != null) {
            nodesInB.add(headB);
            headB = headB.next;
        }
        
        while (headA != null) {
            if (nodesInB.contains(headA)) {
                return headA;
            }
            headA = headA.next;
        }
        
        return null;
    }
  }
  ```
  - Time complexity : O(N + M).
  - Space complexity : O(M).

### 1.3 Add
- [2. Add Two Numbers](https://leetcode.com/problems/add-two-numbers/)
  > sum / 10 = 十位数  
  > sum % 10 = 个位数

  ```java
  class Solution {
    public ListNode addTwoNumbers(ListNode l1, ListNode l2) {
        ListNode dummyHead = new ListNode(0);
        ListNode p = l1, q = l2, curr = dummyHead;
        int carry = 0;
        while (p != null || q != null) {
            int x = (p != null) ? p.val : 0;
            int y = (q != null) ? q.val : 0;
            int sum = carry + x + y;
            // 十位数加到后面
            carry = sum / 10;
            // 个位数存到节点
            curr.next = new ListNode(sum % 10);
            curr = curr.next;
            if (p != null) p = p.next;
            if (q != null) q = q.next;
        }
        // 最后carry还大于0，创建新节点存储
        if (carry > 0) {
            curr.next = new ListNode(carry);
        }
        return dummyHead.next;
        }
  }
  ```
  - Time complexity : O(max(m,n)). Assume that m and n represents the length of l1 and l2 respectively, the algorithm above iterates at most max(m,n) times.
  - Space complexity : O(max(m,n)). The length of the new list is at most max(m,n)+1.
