# Leetcode - Add Two Numbers

## Tags

- TAG
    - Algorithm
    - LeetCode

## Category

Study

## Content

### 들어가며

LeetCode 중급의 문제가 퀄이 좋다고 해서, 이직 전까지 중급 문제만 최대한 풀어보려고 LeetCode 를 시작했다. 확실히 예제가 많고, 틀린 값을 알려줘서 좋지만, 파이썬의 경우 O(n^2) 가 되는 BruteForce로 때리면 대부분 시간초과가 떠서 좀 억울하긴 하다.

[좌표](https://leetcode.com/problems/add-two-numbers/)

```python
# You are given two non-empty linked lists representing two non-negative integers.
# The digits are stored in reverse order, and each of their nodes contains a single digit. Add the two numbers and return the sum as a linked list.
# You may assume the two numbers do not contain any leading zero, except the number 0 itself.

# Definition for singly-linked list.
class ListNode:
    def __init__(self, val=0, next=None):
        self.val = val
        self.next = next

class Solution:
    def addTwoNumbers(self, l1: ListNode, l2: ListNode) -> ListNode:
        head = ListNode()
        result = head
        tail_sum = 0
        while l2 or l1 or tail_sum:
            sum = 0
            if not l1 and l2:
                sum = l2.val
            elif not l2 and l1:
                sum = l1.val
            elif l1 and l2:
                sum = l1.val + l2.val
            head_sum = sum % 10 + tail_sum if (sum % 10 + tail_sum) < 10 else (sum % 10 + tail_sum) % 10
            tail_sum = sum // 10 if (sum % 10 + tail_sum) < 10 else sum // 10 + (sum % 10 + tail_sum) // 10

            result.val = head_sum
            result.next = ListNode()

            if l1:
                l1 = l1.next
            if l2:
                l2 = l2.next

            if l1 is None and l2 is None and tail_sum is 0:
                result.next = None
            else:
                result = result.next

        return head
```

이 문제는 주어진 ListNode 를 뒤집고, 덧셈 후, 다시 뒤집어서 ListNode를 출력하는 문제이다. 여러 풀이들이 있을텐데 제일 쉬운 풀이는 ListNode를 for문으로 array로 형변환 한 뒤, 덧셈을 하고, array to ListNode로 하면 쉽게 풀릴거지만 그럴거면 ListNode가 아니라 그냥 배열을 넘겨주었을 것이다. ListNode만 가지고 풀어보자.

먼저 답으로 사용할 ListNode의 head를 만든다.

```python
head = ListNode()
result = head
```

그리고 result 를 만들어서 head를 지정해준다.

그 후, 반복문을 돌면서 

```python
head_sum = sum % 10 + tail_sum if (sum % 10 + tail_sum) < 10 else (sum % 10 + tail_sum) % 10
tail_sum = sum // 10 if (sum % 10 + tail_sum) < 10 else sum // 10 + (sum % 10 + tail_sum) // 10
```

덧셈 후, 10의 자리와 1의 자리를 분리해준다. 그 후 result의 val 에 head_sum 을 지정해주고 result.next 는 새로운 ListNode를 선언해준다.

```python
result.val = head_sum
result.next = ListNode()
```

이제 왜 head를 result에 재 할당 하였나면

```python
if l1 is None and l2 is None and tail_sum is 0:
	result.next = None
else:
	result = result.next
```

위와같이 result = result.next 로 선언해주면 head의 포인터에서 위에서 선언한 result.next = ListNode() 의 포인터로 할당되므로 마지막에 head만 return 해주면 모든 ListNode들이 따라서 들어가게 된다.