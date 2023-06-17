# LC-Python23

## 39.Combination Sum, 40.Combination Sum II, 131.Palindrome Partitioning

June 15, 2023  4h

Congratulations!\
This is the 23st day for leetcode python study. Today we will learn more about backtracking!\
The challenges today are about using backtracking as a kind of loop solution. For each loop, the number of possible choices of elements /size of one set defines the width of the tree, and the number of loops we need to have/k defines the depth of the tree, we use recursion to simplify the multiple loops. Don't forget the backtracking process followed!\
So for K elements, the tree's depth is k, because we choose one element in each layer.\
The palindrome is a hard question!

## 39.Combination Sum
[Reading link](https://github.com/youngyangyang04/leetcode-master/blob/master/problems/0039.%E7%BB%84%E5%90%88%E6%80%BB%E5%92%8C.md)\
[video](https://www.bilibili.com/video/BV1KT4y1M7HJ/?spm_id_from=pageDriver&vd_source=63f26efad0d35bcbb0de794512ac21f3)\
[leetcode](https://leetcode.com/problems/combination-sum/)\
Compared to question 216, here the number can be repeated used, so the statIndex in recursion starts at i rather than i+1.\
Here fo this question, we sorted the given list of branch cutting.
```python
# ways 1: recursion + branch cutting
class Solution:
    def backtracking(self, candidates, target, total, startIndex, path, result):
        if total == target:     #current sum has already equal to target
            result.append(path[:])
            return
        for i in range(startIndex, len(candidates)):
            if total + candidates[i] > target:      #cutting branch
                break
            total += candidates[i]
            path.append(candidates[i])
            self.backtracking(candidates, target, total, i, path, result)
            total -= candidates[i]    #backtracking
            path.pop()

    def combinationSum(self, candidates: List[int], target: int) -> List[List[int]]:
        result = []
        candidates.sort()   #sort the given list for branch cutting
        self.backtracking(candidates, target,
```


## 40.Combination Sum II
[leetcode](https://leetcode.com/problems/combination-sum-ii/)\
The set given has duplicated elements, but the result can't have duplicates.\
Here we **delete the duplicates in result while searching** rather than after searching, this can help us to save time!\
By drawing a tree, we can remove the duplicate in the tree layer, which means emit the same elements in the for loop/width of the tree. But in the depth of tree/recursion process, the same elements can be used, for example [1,2,2] can be a solution.\
Here the startIndex is used to emit the same elements in the tree layer/width of tree/for loop.\
The array *used* We define it to mark the elements we have used before, so we won't use the same one again. It is very important to help us differentiate the duplicates between layers/width/don't want duplicates, and the depth/which can have duplicates.
```python
# ways 1: recursion and backcutting + remove duplicates in layer/width by used array
class Solution:
    def backtracking(self, candidates, target, total, startIndex, used, path,result):
        if total == target:     #current sum equals to the target, mission complete!
            result.append(path[:])
            return
        
        for i in range(startIndex, len(candidates)):
            #delete duplicates in the layer/width when array used i-1 is 0
            if i>0 and candidates[i]==candidates[i-1] and not used[i-1]:
                continue    #skip the rest code and go on to the next loop
            
            if total + candidates[i] > target:
                break       #end ALL the code!branch cutting
            
            total += candidates[i]
            path.append(candidates[i])
            used[i] = True      #mark the used elements as 1, which means we have used it.
            self.backtracking(candidates, target, total, i+1, used, path,result)
            used[i] = False     #backtracking!!
            total -= candidates[i]
            path.pop()

    def combinationSum2(self, candidates: List[int], target: int) -> List[List[int]]:
        used = [False] * len(candidates)    #initialize the array used
        result = []
        candidates.sort()
        self.backtracking(candidates, target, 0, 0, used, [],result)
        return result
```


## 131.Palindrome Partitioning
[leetcode](https://leetcode.com/problems/palindrome-partitioning/)\
This is a hard question, and there are more similar ones in the following chapters.\
Here the boundary for partitioning the string is actually the startIndex! \
The logic of deciding whether a partition is palindrome or not is in the recursion process. If the partition is not palindrome, we will not process the next recursion, and this partition will not be added to the result.
```python
class Solution:
    def partition(self, s: str) -> List[List[str]]:
        result = []
        self.backtracking(s, 0, [], result)
        return result

    def backtracking(self, s, start_index, path, result):
        #Base case/termination condition
        if start_index == len(s):
            result.append(path[:])
            return
        
        for i in range(start_index, len(s)):
            if self.is_palindrome(s, start_index, i):
                path.append(s[start_index : i+1])
                self.backtracking(s, i+1, path, result)
                path.pop()

    def is_palindrome(self, s:str, start:int, end:int)->bool:
        i: int=start
        j: int=end
        while i < j:
            if s[i] != s[j]:
                return False
            i += 1
            j -= 1
        return True
```























