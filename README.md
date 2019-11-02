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
