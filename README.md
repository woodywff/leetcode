# code_daily
---
## KMP
```
def kmp_find(pattern, text):
    pdb.set_trace()
    prefix_arr = get_prefix_arr(pattern)
    initial_point = []
    p_text = p_pattern = 0
    while p_text < len(text):
        if text[p_text] == pattern[p_pattern]:
            p_text += 1
            p_pattern += 1
            if p_pattern == len(pattern):
                initial_point.append(p_text-p_pattern)
                p_pattern = prefix_arr[p_pattern-1]
        elif p_pattern == 0:
            p_text += 1
        else:
            p_pattern = prefix_arr[p_pattern-1]
        
    return initial_point

def get_prefix_arr(pattern):
    prefix_arr = [0] * len(pattern)
    n = 0
    m = 1
    while m < len(pattern):
        if pattern[m] == pattern[n]:
            n += 1
            prefix_arr[m] = n
            m += 1
        elif n == 0:
            prefix_arr[m] = 0
            m += 1
        else:
            n = prefix_arr[n-1]
    return prefix_arr
```
---
## Leetcode: Find K-th Smallest Pair Distance
```
class Solution:
    def smallestDistancePair(self, nums: List[int], k: int) -> int:
        def possible(guess):
            #Is there k or more pairs with distance <= guess?
            count = left = 0
            for right, x in enumerate(nums):
                while x - nums[left] > guess:
                    left += 1
                count += right - left
            return count >= k

        nums.sort()
        lo = 0
        hi = nums[-1] - nums[0]
        while lo < hi:
            mi = (lo + hi) / 2
            if possible(mi):
                hi = mi
            else:
                lo = mi + 1

        return lo
```
----
## Leetcode: Split Array Largest Sum

```
class Solution:
    def splitArray(self, nums: List[int], m: int) -> int:
        left=max(nums) 
        right=sum(nums) 
        while left < right: 
            mid=left+(right-left)//2 
            if self.canSplit(nums,m,mid): 
                right = mid 
            else: 
                left=mid+1 
        return left 
    
    def canSplit(self,nums,m,sum_ceil): 
        cnt=1 
        curSum=0 
        for i in range(len(nums)): 
            curSum+=nums[i] 
            if curSum>sum_ceil: 
                curSum=nums[i] 
                cnt+=1 
                if cnt>m: 
                    return False 
        return True
```
---
## Word Search II

```
import collections

class Node:
    def __init__(self):
        self.children = collections.defaultdict(Node)
        # self.is_word = False
        self.value = ''

class Trie:
    def __init__(self):
        self.root = Node()
        
    def insert(self,word):
        curr = self.root
        for w in word:
            curr = curr.children[w]
        curr.is_word = True
        curr.value = word


class Solution:
    def findWords(self, board: List[List[str]], words: List[str]) -> List[str]:
        trie = Trie()
        for word in words:
            trie.insert(word)
        initial = [word[0] for word in words]
        res = []

        def find_next(curr_node, curr_x, curr_y):
            if curr_node.value:
                res.append(curr_node.value)
                curr_node.value = ''
            
            if curr_x < 0 or curr_x >= len(board) or curr_y < 0 or curr_y >= len(board[0]):
                return
            next_node = curr_node.children.get(board[curr_x][curr_y])
            if next_node is None:
                return
            tmp, board[curr_x][curr_y] = board[curr_x][curr_y], '*'
            for dx,dy in zip([1,-1,0,0],[0,0,1,-1]):
                find_next(next_node, curr_x+dx, curr_y+dy)
            board[curr_x][curr_y] = tmp
                    
            
         
        for x in range(len(board)):
            for y in range(len(board[0])):
                if board[x][y] in initial:
                    find_next(trie.root, x, y)
        return res
                    
```
---
## Palindrome Pairs
```
class Solution:
    def is_palindrome(self, word):
        return word == word[::-1]
    
    def palindromePairs(self, words: List[str]) -> List[List[int]]:
        dic = {word:i for i,word in enumerate(words)}
        res = []
        for i, word in enumerate(words):
            for j in range(len(word)+1):
                prefix, suffix = word[:j],word[j:]
                if self.is_palindrome(prefix) and suffix[::-1] in dic and dic[suffix[::-1]] != i:
                    res.append([dic[suffix[::-1]],i])
                if suffix and self.is_palindrome(suffix) and prefix[::-1] in dic and dic[prefix[::-1]] != i:
                    res.append([i,dic[prefix[::-1]]])
        return res
```  
---
## Intersection of Two Linked Lists
```
# Definition for singly-linked list.
# class ListNode(object):
#     def __init__(self, x):
#         self.val = x
#         self.next = None

class Solution(object):
    def getIntersectionNode(self, headA, headB):
        """
        :type head1, head1: ListNode
        :rtype: ListNode
        """
        if headA is None or headB is None:
            return None
        pa = headA
        pb = headB
        while pa != pb:
            pa = headB if pa is None else pa.next
            pb = headA if pb is None else pb.next
        return pa
```
---
## Reverse Linked List
### Solution 1: Head to origin
```
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, x):
#         self.val = x
#         self.next = None

class Solution:
    def reverseList(self, head: ListNode) -> ListNode:
        if head is None or head.next is None:
            return head
        prev = head
        origin = head
        head = head.next
        while head is not None:
            prev.next = head.next
            head.next = origin
            origin = head
            head = prev.next
        return origin
```
### Solution 2: Recursion
```
class Solution:
    def reverseList(self, head: ListNode) -> ListNode:
        if head is None or head.next is None:
            return head
        new_head = self.reverseList(head.next)
        head.next.next = head
        head.next = None
        return new_head
```
### Solution 3: Reverse one by one
```
class Solution:
    def reverseList(self, head: ListNode) -> ListNode:
        if head is None or head.next is None:
            return head
        prev = head
        head = head.next
        prev.next = None
        while head.next is not None:
            ahead = head.next
            head.next = prev
            prev,head = head,ahead
        head.next = prev
        return head
```
---
## Merge Two Sorted Lists
```
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, x):
#         self.val = x
#         self.next = None

class Solution:
    def mergeTwoLists(self, l1: ListNode, l2: ListNode) -> ListNode:
        if l1 is None:
            return l2
        if l2 is None:
            return l1
        if l1.val < l2.val:
            l1.next = self.mergeTwoLists(l1.next, l2)
            return l1
        else:
            l2.next = self.mergeTwoLists(l2.next, l1)
            return l2
```
---
## Add Two Numbers
```
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, x):
#         self.val = x
#         self.next = None

class Solution:
    def addTwoNumbers(self, l1: ListNode, l2: ListNode) -> ListNode:
        def to_int(head):
            if head is None:
                return 0
            return head.val + 10 * to_int(head.next)
        
        def to_list(num):
            head = ListNode(num % 10)
            if num > 9:                
                head.next = to_list(num//10)
            return head
        
        return to_list(to_int(l1) + to_int(l2))
```
---
## Find Duplicate Subtrees
```
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None

class Solution:
    def findDuplicateSubtrees(self, root: TreeNode) -> List[TreeNode]:
        import collections
        count = collections.Counter()
        ans = []
        
        def dfs(root):
            if root is None:
                return '#'
            serial = '{},{},{}'.format(root.val, dfs(root.left), dfs(root.right))
            count[serial] += 1
            if count[serial] == 2:
                ans.append(root)
            return serial
        
        dfs(root)
        return ans
```
---
