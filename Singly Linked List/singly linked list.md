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

### 1.2 Cycle
- [141. Linked List Cycle](https://leetcode.com/problems/linked-list-cycle/)
  > Hash Table

  ```java
  public class Solution {
      public boolean hasCycle(ListNode head) {
          Set<ListNode> nodesSeen = new HashSet<>();

          while (head != null) {
              if (nodesSeen.contains(head)) return true;
              nodesSeen.add(head);
              head = head.next;
          }

          return false;
      }
  }
  ```
  
  - Time complexity : O(n). We visit each of the n elements in the list at most once. Adding a node to the hash table costs only O(1) time.
  - Space complexity: O(n). The space depends on the number of elements added to the hash table, which contains at most n elements.

- [142. Linked List Cycle II](https://leetcode.com/problems/linked-list-cycle-ii/)
  > Floyd's Tortoise and Hare

  ```java
  public class Solution {
    public ListNode getIntersect(ListNode head) {
        ListNode slow = head;
        ListNode fast = head;
        
        // stop at the end node
        while (fast != null && fast.next != null) {
            slow = slow.next;
            fast = fast.next.next;
            if (slow == fast) {
                return slow;
            }
        }
        
        return null;
    }
    
    public ListNode detectCycle(ListNode head) {
        if (head == null) return null;
        
        ListNode intersect = getIntersect(head);
        if (intersect == null) return null;
        
        ListNode ptr1 = head;
        ListNode ptr2 = intersect;
        
        while (ptr1 != ptr2) {
            ptr1 = ptr1.next;
            ptr2 = ptr2.next;
        }
        
        return ptr1;
    }
  }
  ```
  
  - Time complexity : O(n).
  - Space complexity : O(1). Floyd's Tortoise and Hare algorithm allocates only pointers, so it runs with constant overall memory usage.

### 1.3 Remove
- [83. Remove Duplicates from Sorted List](https://leetcode.com/problems/remove-duplicates-from-sorted-list/)
  > Straight-Forward  
  > **Notice: curr.next = curr.next.next;**

  ```java
  class Solution {
    public ListNode deleteDuplicates(ListNode head) {
        ListNode curr = head;
        
        while (curr != null && curr.next != null) {
            if (curr.val == curr.next.val) {
                curr.next = curr.next.next;
            }
            else {
                curr = curr.next;
            }
        }
        
        return head;
    }
  }
  ```
  
  - Time complexity : O(n). Because each node in the list is checked exactly once to determine if it is a duplicate or not, the total run time is O(n), where nn is the number of nodes in the list.
  - Space complexity : O(1). No additional space is used.

- [82. Remove Duplicates from Sorted List II](https://leetcode.com/problems/remove-duplicates-from-sorted-list-ii/)
  > Sentinel Head + Predecessor  
  > **三个Node: sentinel, prev, curr.**

  ```java
  class Solution {
    public ListNode deleteDuplicates(ListNode head) {
        ListNode sentinel = new ListNode(0, head);
        ListNode prev = sentinel, curr = head;
        
        while (curr != null) {
            if (curr.next != null && curr.val == curr.next.val) {
                while (curr.next != null && curr.val == curr.next.val) {
                    curr = curr.next;
                }
                prev.next = curr.next;
            }
            else {
                prev = prev.next;
            }
            curr = curr.next;
        }
        
        return sentinel.next;
    }
  }
  ```
  
  - Time complexity: O(N) since it's one pass along the input list.
  - Space complexity: O(1) because we don't allocate any additional data structure.


- [203. Remove Linked List Elements](https://leetcode.com/problems/remove-linked-list-elements/)
  > Sentinel Node  
  > Why use: 中间的好删除，两头不好删。  
  > **Notice: return sentinel.next;**  
  > Sentinel nodes are widely used in trees and linked lists as pseudo-heads, pseudo-tails, markers of level end, etc. They are purely functional, and usually does  not hold any data. Their main purpose is to standardize the situation, for example, make linked list to be never empty and never headless and hence simplify   insert and delete.

  ```java
  class Solution {
    public ListNode removeElements(ListNode head, int val) {
        ListNode sentinel = new ListNode(0);
        sentinel.next = head;
        
        ListNode prev = sentinel, curr = head;
        while (curr != null) {
            if (curr.val == val) {
                prev.next = curr.next;
            }
            else {
                prev = curr;
            }
            curr = curr.next;
        }
        
        return sentinel.next;
    }
  }
  ```
  
  - Time complexity: O(N), it's one pass solution.
  - Space complexity: O(1), it's a constant space solution.

- [237. Delete Node in a Linked List](https://leetcode.com/problems/delete-node-in-a-linked-list/)
  ```java
  class Solution {
    public void deleteNode(ListNode node) {
        node.val = node.next.val;
        node.next = node.next.next;
    }
  }
  ```
- [19. Remove Nth Node From End of List](https://leetcode.com/problems/remove-nth-node-from-end-of-list/)
  > Two pass algorithm  
  > 1. 算出链表长度。
  > 2. 删除倒数第n个节点。  
  > **Notice: length -= n; first = dummy; // 指向前一个节点可直接删除  
  > 循环条件: length > 0;**

  ```java
  class Solution {
    public ListNode removeNthFromEnd(ListNode head, int n) {
        ListNode dummy = new ListNode(0);
        dummy.next = head;
        int length = 0;
        
        ListNode first = head;
        while (first != null) {
            length++;
            first = first.next;
        }
        
        length -= n;
        first = dummy; // 指向前一个节点可直接删除
        while (length > 0) {
            first = first.next;
            length--;
        }
        first.next = first.next.next;
        return dummy.next;
    }
  }
  ```
  - Time complexity : O(L). The algorithm makes two traversal of the list, first to calculate list length L and second to find the (L−n) th node. There are 2L-n operations and time complexity is O(L).
  - Space complexity : O(1). We only used constant extra space.

### 1.4 Palindrome
- [234. Palindrome Linked List](https://leetcode.com/problems/palindrome-linked-list/)
  > Way1: Copy into Array List and then Use Two Pointer Technique

  ```java
  class Solution {
    public boolean isPalindrome(ListNode head) {
        List<Integer> vals = new ArrayList<>();
        
        ListNode curr = head;
        while (curr != null) {
            vals.add(curr.val);
            curr = curr.next;
        }
        
        int front = 0;
        int back = vals.size() - 1;
        
        while (front < back) {
            if (!vals.get(front).equals(vals.get(back))) {
                return false;
            }
            front++;
            back--;
        }
        
        return true;
        
    }
  }
  ```
  - Time complexity : O(n), where n is the number of nodes in the Linked List.  
    
    In the first part, we're copying a Linked List into an Array List. Iterating down a Linked List in sequential order is O(n), and each of the n writes to the ArrayList is O(1). Therefore, the overall cost is O(n).  
    
    In the second part, we're using the two pointer technique to check whether or not the Array List is a palindrome. Each of the n values in the Array list is accessed once, and a total of O(n/2) comparisons are done. Therefore, the overall cost is O(n). The Python trick (reverse and list comparison as a one liner) is also O(n).  
    
    This gives O(2n) = O(n) because we always drop the constants.
  - Space complexity : O(n), where nn is the number of nodes in the Linked List. We are making an Array List and adding n values to it.

  > <a id="no.234_way2"></a>Way2: Reverse Second Half In-place  
  > **Notice: 分前后半段循环条件：fast.next != null && fast.next.next != null; 返回的是前半段结尾。**

  ```java
  class Solution {

    public boolean isPalindrome(ListNode head) {

        if (head == null) return true;

        // Find the end of first half and reverse second half.
        ListNode firstHalfEnd = endOfFirstHalf(head);
        ListNode secondHalfStart = reverseList(firstHalfEnd.next);

        // Check whether or not there is a palindrome.
        ListNode p1 = head;
        ListNode p2 = secondHalfStart;
        boolean result = true;
        while (result && p2 != null) {
            if (p1.val != p2.val) result = false;
            p1 = p1.next;
            p2 = p2.next;
        }        

        // Restore the list and return the result.
        firstHalfEnd.next = reverseList(secondHalfStart);
        return result;
    }

    // Taken from https://leetcode.com/problems/reverse-linked-list/solution/
    private ListNode reverseList(ListNode head) {
        ListNode prev = null;
        ListNode curr = head;
        while (curr != null) {
            ListNode nextTemp = curr.next;
            curr.next = prev;
            prev = curr;
            curr = nextTemp;
        }
        return prev;
    }

    private ListNode endOfFirstHalf(ListNode head) {
        ListNode fast = head;
        ListNode slow = head;
        
        // 前半段的结尾
        while (fast.next != null && fast.next.next != null) {
            fast = fast.next.next;
            slow = slow.next;
        }
        return slow;
    }
  }
  ```
  - Time complexity : O(n), where n is the number of nodes in the Linked List.  
    Similar to the above approaches. Finding the middle is O(n), reversing a list in place is O(n), and then comparing the 2 resulting Linked Lists is also O(n).
  - Space complexity : O(1). We are changing the next pointers for half of the nodes. This was all memory that had already been allocated, so we are not using any extra memory and therefore it is O(1).  

### 1.5 Find Middle
- [876. Middle of the Linked List](https://leetcode.com/problems/middle-of-the-linked-list/)
  > Fast and Slow Pointer  
  > **Notice: 和[234题解法2](#no.234_way2)循环条件不同，此题是：fast != null && fast.next != null; 返回的是后半段开头。**

  ```java
  class Solution {
      public ListNode middleNode(ListNode head) {
          ListNode slow = head;
          ListNode fast = head;

          // 后半段的开头
          while (fast != null && fast.next != null) {
              slow = slow.next;
              fast = fast.next.next;
          }

          return slow;
      }
  }
  ```
  - Time Complexity: O(N), where N is the number of nodes in the given list.
  - Space Complexity: O(1)), the space used by slow and fast.

### 1.6 Reorder
- [143. Reorder List](https://leetcode.com/problems/reorder-list/)
  > Reverse the Second Part of the List and Merge Two Sorted Lists  
  > 1. Middle of the Linked List
  > 2. Reverse Linked List
  > 3. Merge Two Sorted Lists  
  > **Notice: 合并时循环条件为：second.next != null，循环截止时，second指向后半段最后一个节点，最终结果最后一个节点为前半段的最后一个节点。**

  ```java
  class Solution {
    public void reorderList(ListNode head) {
        if (head == null) return;
        
        // find middle
        ListNode slow = head;
        ListNode fast = head;
        while (fast != null && fast.next != null) {
            slow = slow.next;
            fast = fast.next.next;
        }
        
        // reverse second half
        ListNode prev = null;
        ListNode curr = slow;
        while (curr != null) {
            ListNode temp = curr.next;
            curr.next = prev;
            prev = curr;
            curr = temp;
        }
        
        // merge
        ListNode first = head;
        ListNode second = prev;
        // 循环截止时，second指向后半段最后一个节点，最终结果最后一个节点为前半段的最后一个节点
        while (second.next != null) {
            ListNode temp = first.next;
            first.next = second;
            first = temp;
            
            temp = second.next;
            second.next = first;
            second = temp;
        }
    }
  }
  ```
  - Time complexity: O(N). There are three steps here. To identify the middle node takes O(N) time. To reverse the second part of the list, one needs N/2 operations. The final step, to merge two lists, requires N/2 operations as well. In total, that results in O(N) time complexity.
  - Space complexity: O(1), since we do not allocate any additional data structures.

### 1.7 Sort
- [148. Sort List](https://leetcode.com/problems/sort-list/)
  > Top Down Merge Sort  
  > **Recursive**  
  > 1. getMid. 注意循环条件，slow fast开始位置不同，slow从head开始，fast从head后两个节点开始。**所求为mid前一个节点**
  > 2. sort recursively
  > 3. merge

  ```java
  class Solution {
    public ListNode getMid(ListNode head) {
        ListNode midPrev = null;
        // 所求为mid前一个节点
        while (head != null && head.next != null) {
            midPrev = (midPrev == null) ? head : midPrev.next;
            head = head.next.next;
        }
        ListNode mid = midPrev.next;
        midPrev.next = null;
        
        return mid;
    }
    
    public ListNode merge(ListNode l1, ListNode l2) {
        ListNode dummy = new ListNode();
        ListNode tail = dummy;
        
        while (l1 != null && l2 != null) {
            if (l1.val < l2.val) {
                tail.next = l1;
                l1 = l1.next;
                tail = tail.next;
            }
            else {
                tail.next = l2;
                l2 = l2.next;
                tail = tail.next;
            }
        }
        // l1空时指向l2
        tail.next = (l1 != null) ? l1 : l2;
        
        return dummy.next;
    }
    
    public ListNode sortList(ListNode head) {
        if (head == null || head.next == null) return head;
        
        ListNode mid = getMid(head);
        ListNode left = sortList(head);
        ListNode right = sortList(mid);
        
        return merge(left, right);
    }
  }
  ```
  - Time Complexity: O(nlogn), where n is the number of nodes in linked list. The algorithm can be split into 2 phases, Split and Merge.  
    
    **Split:** The recursion tree expands in form of a complete binary tree, splitting the list into two halves recursively. The number of levels in a complete binary tree is given by $\log_{2}n$.  
    
    **Merge:** At each level, we merge n nodes which takes O(n) time.  
  
  - Space Complexity: O(logn) , where n is the number of nodes in linked list. Since the problem is recursive, we need additional space to store the recursive call stack. The maximum depth of the recursion tree is logn.

- [147. Insertion Sort List](https://leetcode.com/problems/insertion-sort-list/)
  > Insertion Sort  
  > **Notice: 每次新循环都要把prev放到最开头，从第一个找起。**

  ```java
  class Solution {
      public ListNode insertionSortList(ListNode head) {
          ListNode dummy = new ListNode();
          ListNode curr = head;

          while (curr != null) {
              // 每次新循环都要把prev放到最开头
              ListNode prev = dummy;

              while (prev.next != null && prev.next.val < curr.val) {
                  prev = prev.next;
              }

              ListNode next = curr.next;

              curr.next = prev.next;
              prev.next = curr;

              curr = next;
          }

          return dummy.next;
      }
  }
  ```

- Time Complexity: ${O}(N^2)$  
  
  First of all, we run an iteration over the input list.
  
  At each iteration, we insert an element into the resulting list. In the worst case where the position to insert is the tail of the list, we have to walk through the entire resulting list.
  
- Space Complexity: O(1). We used some pointers within the algorithm. However, their memory consumption is constant regardless of the input.

  **Note, we did not create new nodes to hold the values of input list, but simply reorder the existing nodes.**
  
### 1.8 Partition/Split
- [86. Partition List](https://leetcode.com/problems/partition-list/)
  > Two Pointer Approach  
  > **Notice: 注意before_head, before, after_head, after四个节点指向哪里。**

  ```java
  class Solution {
    public ListNode partition(ListNode head, int x) {
        ListNode before_head = new ListNode();
        ListNode before = before_head;
        ListNode after_head = new ListNode();
        ListNode after = after_head;
        
        while (head != null) {
            if (head.val < x) {
                before.next = head;
                before = before.next;
            }
            else {
                after.next = head;
                after = after.next;
            }
            head = head.next;
        }
        
        after.next = null;
        
        before.next = after_head.next;
        
        return before_head.next;
    }
  }
  ```
  - Time Complexity: O(N), where N is the number of nodes in the original linked list and we iterate the original list.
  - Space Complexity: O(1), we have not utilized any extra space, the point to note is that we are reforming the original list, by moving the original nodes, we have not used any extra space as such.

- [725. Split Linked List in Parts](https://leetcode.com/problems/split-linked-list-in-parts/)
  > Split Input List  
  > If there are N nodes in the linked list root, then there are N / k items in each part, plus the first N % k parts have an extra item. We can count N with a simple loop.

  ```java
  class Solution {
    public ListNode[] splitListToParts(ListNode head, int k) {
        ListNode curr = head;
        int N = 0;
        while (curr != null) {
            N++;
            curr = curr.next;
        }
        
        int width = N / k, rem = N % k;
        
        ListNode[] ans = new ListNode[k];
        curr = head;
        for (int i = 0; i < k; i++) {
            // 新sublist的第一个节点
            ListNode root = curr;
            // 前k个part可以多放一个item。已经放了一个节点所以要-1
            for (int j = 0; j < width + (i < rem ? 1 : 0) -1; j++) {
                if (curr != null) {
                    curr = curr.next;
                }
            }
            if (curr != null) {
                ListNode prev = curr;
                curr = curr.next;
                prev.next = null;
            }
            ans[i] = root;
        }
        return ans;
    }
  }
  ```
  - Time Complexity: O(N + k), where N is the number of nodes in the given list. If k is large, it could still require creating many new empty lists.
  - Space Complexity: O(max(N, k)), the space used in writing the answer.

- [328. Odd Even Linked List](https://leetcode.com/problems/odd-even-linked-list/)
  > 跟86题很像

  ```java
  class Solution {
    public ListNode oddEvenList(ListNode head) {
        if (head == null) return null;
        
        ListNode odd = head, even = head.next, evenHead = even;
        // even在后面
        while (even != null && even.next != null) {
            odd.next = even.next;
            odd = odd.next;
            even.next = odd.next;
            even = even.next;
        }
        odd.next = evenHead;
        return head;
      }
  }
  ```
  - Time complexity : O(n). There are total n nodes and we visit each node once.
  - Space complexity : O(1). All we need is the four pointers.

### 1.9 Rotate
- [61. Rotate List](https://leetcode.com/problems/rotate-list/)
  > 1. 循环到链表尾行程环，同时求链表长度。
  > 2. **k = k % n, 包含k > len的情况。**

  ```java
  class Solution {
    public ListNode rotateRight(ListNode head, int k) {
        if (head == null) return null;
        if (head.next == null) return head;
        
        ListNode old_tail = head;
        int len;
        for (len = 1; old_tail.next != null; len++) {
            old_tail = old_tail.next;
        }
        old_tail.next = head;
        
        ListNode new_tail = head;
        // 包含k > len的情况
        k = k % len;
        for (int i = 0; i < len - k - 1; i++) {
            new_tail = new_tail.next;
        }
        ListNode new_head = new_tail.next;
        new_tail.next = null;
        
        return new_head;
    }
  }
  ```
  - Time complexity : O(N) where N is a number of elements in the list.
  - Space complexity : O(1) since it's a constant space solution.

### 1.10 Copy
- [138. Copy List with Random Pointer](https://leetcode.com/problems/copy-list-with-random-pointer/)
  > Iterative with O(N) Space  
  > 创建HashMap存储新旧node，旧node是key，新node是value。

  ```java
  class Solution {
    HashMap<Node, Node> visited = new HashMap<>();
    
    public Node getClonedNode(Node node) {
        if (node != null) {
            if (this.visited.containsKey(node)) {
                return this.visited.get(node);
            }
            else {
                this.visited.put(node, new Node(node.val, null, null));
                return this.visited.get(node);
            }
        }
        
        return null;
    }
    
    public Node copyRandomList(Node head) {
        if (head == null) return null;
        
        Node oldNode = head;
        
        Node newNode = new Node(oldNode.val);
        this.visited.put(oldNode, newNode);
        
        while (oldNode != null) {
            newNode.random = this.getClonedNode(oldNode.random);
            newNode.next = this.getClonedNode(oldNode.next);
            
            oldNode = oldNode.next;
            newNode = newNode.next;
        }
        
        return this.visited.get(head);
      }
  }
  ```
  - Time Complexity : O(N) because we make one pass over the original linked list.
  - Space Complexity : O(N) as we have a dictionary containing mapping from old list nodes to new list nodes. Since there are N nodes, we have O(N) space complexity.

### 1.11 Swap
- [24. Swap Nodes in Pairs](https://leetcode.com/problems/swap-nodes-in-pairs/)
  > Iterative  
  > Use dummy.

  ```java
  class Solution {
    public ListNode swapPairs(ListNode head) {
        ListNode dummy = new ListNode(-1);
        dummy.next = head;
        
        ListNode preNode = dummy;
        
        while (head != null && head.next != null) {
            ListNode firstNode = head;
            ListNode secondNode = head.next;
            
            preNode.next = secondNode;
            firstNode.next = secondNode.next;
            secondNode.next = firstNode;
            
            preNode = firstNode;
            head = firstNode.next;
        }
       
        return dummy.next;
      }
  }
  ```
  - Time Complexity : O(N) where N is the size of the linked list.
  - Space Complexity : O(1).

