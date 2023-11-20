
[572. Subtree of Another Tree](https://leetcode.com/problems/subtree-of-another-tree/)

- dfs 
- recursion

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def test(self, root: Optional[TreeNode], subRoot: Optional[TreeNode]):
        if subRoot == None and root == None:
            return True
        if root == None or subRoot == None or root.val != subRoot.val:
            return False
        return self.test(root.left, subRoot.left) and self.test(root.right, subRoot.right)

    def isSubtree(self, root: Optional[TreeNode], subRoot: Optional[TreeNode]) -> bool:
        if subRoot == None and root == None:
            return True
        if root == None or subRoot == None:
            return False
        ans = False
        if root.val == subRoot.val:
            ans = (self.test(root.left, subRoot.left) and self.test(root.right, subRoot.right)) 

        return ans or self.isSubtree(root.left, subRoot) or self.isSubtree(root.right, subRoot)

```

