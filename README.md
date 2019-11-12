# code_daily
To implement the demo for single modality requirement from Prof.Biswal, I deleted t2.nii.gz t1ce.nii.gz and flair.nii.gz and copy t1.nii.gz to be t2 t1ce and flair.
```
import os
import glob
import shutil
from tqdm import *

mods = ['t1ce','t2','flair']
print('deleting...')
for mod in mods:
    for file in tqdm(glob.glob(os.path.abspath('../data/preprocessed_val_data/*/*/'+mod+'*'))):
        if os.path.exists(file):
            os.remove(file)
        else:
            print(file)
print('copying...')
for t1_file in tqdm(glob.glob(os.path.abspath('../data/preprocessed_val_data/*/*/t1*'))):
    for mod in mods:
        shutil.copy(t1_file,'/'.join(t1_file.split('/')[:-1])+'/'+mod+'.nii.gz')
print('Done')
```
----------------
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
