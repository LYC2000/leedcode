# Leedcode Hot100 刷题总结

## 两数之和

[题目链接](https://leetcode.cn/problems/add-two-numbers/)

一开始用的是很笨的两个for循环找到目标值，但时间复杂度为0（n^2^），性能较差，看完题解，有人用字典的方式（其他语言中使用的是map方法）解决了。思路为：将nums中的每个值都用字典存储对应，然后在字典中找出等于target-nums的值并返回，时间复杂度可到达O（n^2^）。

```
class Solution:
    def twoSum(self, nums: List[int], target: int) -> List[int]:
        hashmap = {}
        List_ans = []
        for ind, num in enumerate(nums):
            hashmap[num] = ind
        for i, num in enumerate(nums):
            j = target - num
            if j in hashmap.keys() and i != hashmap.get(j):
                return [i, hashmap.get(j)]
```



## 两数相加

[题目链接](https://leetcode.cn/problems/add-two-numbers/)

要考虑进位问题，采用的方法是用一个变量carry存储进位，下一轮按位相加时加上carry，最后也别忘了进位，思路清晰，主要是链表的理解不到位。

```
# Definition for singly-linked list.
"""class ListNode:
     def __init__(self, val=0, next=None):
         self.val = val
         self.next = next"""
         
class Solution:
    def addTwoNumbers(self, l1: Optional[ListNode], l2: Optional[ListNode]) -> Optional[ListNode]:
        result = cur = ListNode()
        carry = 0
        while l1 or l2:
            x = l1.val if l1 else 0
            y = l2.val if l2 else 0
            total = x + y + carry
            carry = int(total / 10)
            cur.next = ListNode(total % 10)
            if l1: l1 = l1.next
            if l2: l2 = l2.next
            cur = cur.next
        if carry:
            cur.next = ListNode(carry)
        return result.next
```

## 有效的括号

[题目链接](https://leetcode.cn/problems/valid-parentheses/)

首先使用字典的方式存储对应的括号，接着使用栈先进后出的特性进行配对。

```
def isValid(self, s: str) -> bool:
        dic = {'{': '}',  '[': ']', '(': ')', '?': '?'}
        stack = ['?']
        for i in range(len(s)):
            if s[i] in dic: 
                stack.append(s[i])
            elif dic[stack.pop()] != s[i]:
                return False
        return len(stack) == 1
```

## 合并两个有序链表

[题目描述](https://leetcode.cn/problems/merge-two-sorted-lists/)

仅在此记录自己的方法，其他的方法需要理解后续找工作时可进阶学习。我用的是while循环两个链表，若其中一个为空则直接返回另一个链表。若均不为空，则移动指针比对两个链表各节点的值，小的插入新链表。

```
#class ListNode:
#    def __init__(self, val=0, next=None):
#        self.val = val
#        self.next = next


class Solution:
    def mergeTwoLists(self, list1: Optional[ListNode], list2: Optional[ListNode]) -> Optional[ListNode]:
        result = cur = ListNode()
        temp = 0
        while list1 or list2:
            if list1 == None:
                cur.next = list2
                return result.next
            elif list2 == None:
                cur.next = list1
                return result.next
            elif list1.val >= list2.val:
                cur.next = ListNode(list2.val)
                list2 = list2.next
                cur = cur.next
            else:
                cur.next = ListNode(list1.val)
                list1 = list1.next
                cur = cur.next
```



## 只出现一次的数字

#### 1.自己想的字典方法

```
def singleNumber(self, nums: List[int]) -> int:
    d = {}
    for i in range(len(nums)):
        if str(nums[i]) not in d:
           d[str(nums[i])] = 0
         else:
           d[str(nums[i])] = 1
    for key in d:
        if d[key] == 0:
           return int(key)
```

使用字典的基本方法，虽然时间复杂度为线性，但还是创建了额外空间

学会了一条新语句，原来在python字典中可以通过如下语句循环字典，并且获得每一步的key，求value时都非常方便

```
for key in d:
    if d[key] == 0:
       return int(key)
```

#### 2.神仙一行代码

```
return sum(set(nums))*2-sum(nums)
```
