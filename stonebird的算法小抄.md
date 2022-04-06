# stonebird的算法小抄

整理知识，传播智慧

好好学习，天天向上

gopher：石鸟路遇

email：1245863260@qq.com

Simple, Poetic, Pithy

文档为个人工作和收集积累，不涉及商用，仅供学习和参考，如有不正确的地方，请指正。



power by :

​		https://github.com/CyC2018/CS-Notes



据结构的存储方式只有两种：数组（顺序存储）和链表（链式存储）。

算法小抄由go和python实现

对于任何数据结构，其基本操作无非遍历 + 访问，再具体一点就是：增删查改。



常见排序复杂度

| **排序算法**     | **平均时间复杂度** | **最坏时间复杂度** | **最好时间复杂度** | **空间复杂度** | **稳定性** |
| ---------------- | ------------------ | ------------------ | ------------------ | -------------- | ---------- |
| **冒泡排序**     | O(n²)              | O(n²)              | O(n)               | O(1)           | 稳定       |
| **直接选择排序** | O(n²)              | O(n²)              | O(n)               | O(1)           | 不稳定     |
| **直接插入排序** | O(n²)              | O(n²)              | O(n)               | O(1)           | 稳定       |
| **快速排序**     | O(nlogn)           | O(n²)              | O(nlogn)           | O(nlogn)       | 不稳定     |
| **堆排序**       | O(nlogn)           | O(nlogn)           | O(nlogn)           | O(1)           | 不稳定     |
| **希尔排序**     | O(nlogn)           | O(ns)              | O(n)               | O(1)           | 不稳定     |
| **归并排序**     | O(nlogn)           | O(nlogn)           | O(nlogn)           | O(n)           | 稳定       |
| **计数排序**     | O(n+k)             | O(n+k)             | O(n+k)             | O(n+k)         | 稳定       |
| **基数排序**     | O(N*M)             | O(N*M)             | O(N*M)             | O(M)           | 稳定       |



用于求解 **Kth Element** 问题，也就是第 K 个元素的问题。可以使用快速排序的 partition() 进行实现。

用于求解 **TopK Elements** 问题，也就是 K 个最小元素的问题。使用最小堆来实现 TopK 问题，最小堆使用大顶堆来实现，大顶堆的堆顶元素为当前堆的最大元素。



当mid 是偶数时  mid+1=mid⊕1；

当 mid 是奇数时，mid−1=mid⊕1。



深度优先搜索和广度优先搜索广泛运用于树和图中，但是它们的应用远远不止如此。

广度优先搜索一层一层地进行遍历，每层遍历都是以上一层遍历的结果作为起点，遍历一个距离能访问到的所有节点。**需要注意的是，遍历过的节点不能再次被遍历**。

推导出一个结论：对于先遍历的节点 i 与后遍历的节点 j，有 di <= dj。利用这个结论，可以求解最短路径等 **最优解** 问题：第一次遍历到目的节点，其所经过的路径为最短路径。应该注意的是，使用 BFS 只能求解无权图的最短路径，无权图是指从一个节点到另一个节点的代价都记为 1。

在程序实现 BFS 时需要考虑以下问题：

- 队列：用来存储每一轮遍历得到的节点；
- 标记：对于遍历过的节点，应该将它标记，防止重复遍历。



如果只是要找到某一个结果是否存在，那么DFS会更高效。

但是BFS必须所有可能的情况同时尝试，在找到一种满足条件的结果的同时，也尝试了很多不必要的路径；



# 算法思想

## 双指针

#### [167. 两数之和 II - 输入有序数组](https://leetcode-cn.com/problems/two-sum-ii-input-array-is-sorted/)

难度中等724收藏分享切换为英文接收动态反馈

给你一个下标从 **1** 开始的整数数组 `numbers` ，该数组已按 **非递减顺序排列** ，请你从数组中找出满足相加之和等于目标数 `target` 的两个数。如果设这两个数分别是 `numbers[index1]` 和 `numbers[index2]` ，则 `1 <= index1 < index2 <= numbers.length` 。

以长度为 2 的整数数组 `[index1, index2]` 的形式返回这两个整数的下标 `index1` 和 `index2`。

你可以假设每个输入 **只对应唯一的答案** ，而且你 **不可以** 重复使用相同的元素。

你所设计的解决方案必须只使用常量级的额外空间。

**示例 1：**

```
输入：numbers = [2,7,11,15], target = 9
输出：[1,2]
解释：2 与 7 之和等于目标数 9 。因此 index1 = 1, index2 = 2 。返回 [1, 2] 。
```

**示例 2：**

```
输入：numbers = [2,3,4], target = 6
输出：[1,3]
解释：2 与 4 之和等于目标数 6 。因此 index1 = 1, index2 = 3 。返回 [1, 3] 。
```

**示例 3：**

```
输入：numbers = [-1,0], target = -1
输出：[1,2]
解释：-1 与 0 之和等于目标数 -1 。因此 index1 = 1, index2 = 2 。返回 [1, 2] 。
```

提示：

> 2 <= numbers.length <= 3 * 104
> -1000 <= numbers[i] <= 1000
> numbers 按 非递减顺序 排列
> -1000 <= target <= 1000
> 仅存在一个有效答案。



code-go

```go
func twoSum(numbers []int, target int) []int {
	left, right := 0, len(numbers)-1
	for left < right {
		sum := numbers[left] + numbers[right]
		switch {
		case sum > target:
			right--
		case sum < target:
			left++
		default:
			return []int{left + 1, right + 1}
		}
	}
	return nil
}
```

code-python

```python
class Solution:
    def twoSum(self, numbers: List[int], target: int) -> List[int]:
        left, right = 0, len(numbers) - 1
        while left < right:
            sum_val = numbers[left] + numbers[right]
            if sum_val < target:
                left += 1
            elif sum_val > target:
                right -= 1
            else:
                return [left + 1, right + 1]

        return []
```

#### [633. 平方数之和](https://leetcode-cn.com/problems/sum-of-square-numbers/)

难度中等332收藏分享切换为英文接收动态反馈

给定一个非负整数 `c` ，你要判断是否存在两个整数 `a` 和 `b`，使得 `a2 + b2 = c` 。

 

**示例 1：**

```
输入：c = 5
输出：true
解释：1 * 1 + 2 * 2 = 5
```

**示例 2：**

```
输入：c = 3
输出：false
```

 

**提示：**

> 0 <= c <= 2**31 - 1

code-go

```go
func judgeSquareSum(c int) bool {
	left, right := 0, int(math.Sqrt(float64(c)))
	for left <= right {
		sum := left * left + right * right
		switch {
		case sum > c:
			right--
		case sum < c:
			left++
		default:
			return true
		}
	}
	return false
}
```

code-python

```python
class Solution:
    def judgeSquareSum(self, c: int) -> bool:
        left, right = 0, int(math.sqrt(c))
        while left <= right:
            sum_val = left * left + right * right
            if sum_val > c:
                right -= 1
            elif sum_val < c:
                left += 1
            else:
                return True
        return False
```

#### [345. 反转字符串中的元音字母](https://leetcode-cn.com/problems/reverse-vowels-of-a-string/)

难度简单241收藏分享切换为英文接收动态反馈

给你一个字符串 `s` ，仅反转字符串中的所有元音字母，并返回结果字符串。

元音字母包括 `'a'`、`'e'`、`'i'`、`'o'`、`'u'`，且可能以大小写两种形式出现。

 

**示例 1：**

```
输入：s = "hello"
输出："holle"
```

**示例 2：**

```
输入：s = "leetcode"
输出："leotcede"
```

 

**提示：**

> 1 <= s.length <= 3 * 105
> s 由 可打印的 ASCII字符组成

code-go

```go
func reverseVowels(s string) string {
	aeiou := "aeiouAEIOU"
	string2Byte := []byte(s)
	left, right := 0, len(string2Byte)-1
	for left < right {
		for left < right && !strings.Contains(aeiou, string(string2Byte[left])) {
			left++
		}
		for left < right && !strings.Contains(aeiou, string(string2Byte[right])) {
			right--
		}
		string2Byte[left], string2Byte[right] = string2Byte[right], string2Byte[left]
		left++
		right--
	}

	return string(string2Byte)
}
```

code-python

```python
class Solution:
    def reverseVowels(self, s: str) -> str:
        aeiou = "aeiouAEIOU"
        s_list = list(s)
        left, right = 0, len(s_list) - 1
        while left < right:
            while left < right and s_list[left] not in aeiou:
                left += 1
            while left < right and s_list[right] not in aeiou:
                right -= 1
            s_list[left], s_list[right] = s_list[right], s_list[left]
            left += 1
            right -= 1

        return ''.join(s_list)
```

#### [680. 验证回文字符串 Ⅱ](https://leetcode-cn.com/problems/valid-palindrome-ii/)

难度简单463收藏分享切换为英文接收动态反馈

给定一个非空字符串 `s`，**最多**删除一个字符。判断是否能成为回文字符串。

 

**示例 1:**

```
输入: s = "aba"
输出: true
```

**示例 2:**

```
输入: s = "abca"
输出: true
解释: 你可以删除c字符。
```

**示例 3:**

```
输入: s = "abc"
输出: false
```

 

**提示:**

> 1 <= s.length <= 105
> s由小写英文字母组成



code-go

```go
func validPalindrome(s string) bool {
	length := len(s)
	if length == 1 {
		return true
	}

	left, right := 0, length-1
	for left < right {
		if s[left] != s[right] {
			return base(s, left+1, right) || base(s, left, right-1)
		}
		left++
		right--
	}
	return true
}

func base(str string, left, right int) bool {
	for left < right {
		if str[left] != str[right] {
			return false
		}
		left++
		right--
	}
	return true
}
```

code-python

```python
class Solution:
    def validPalindrome(self, s: str) -> bool:
        length = len(s)
        if length == 1:
            return True
        self.s = s
        left, right = 0, length - 1
        while left < right:
            if s[left] != s[right]:
                return self.base(left + 1, right) or self.base(left, right - 1)
            left += 1
            right -= 1

        return True

    def base(self, left: int, right: int):
        while left < right:
            if self.s[left] != self.s[right]:
                return False
            left += 1
            right -= 1
        return True
```

#### [88. 合并两个有序数组](https://leetcode-cn.com/problems/merge-sorted-array/)

难度简单1337收藏分享切换为英文接收动态反馈

给你两个按 **非递减顺序** 排列的整数数组 `nums1` 和 `nums2`，另有两个整数 `m` 和 `n` ，分别表示 `nums1` 和 `nums2` 中的元素数目。

请你 **合并** `nums2` 到 `nums1` 中，使合并后的数组同样按 **非递减顺序** 排列。

**注意：**最终，合并后数组不应由函数返回，而是存储在数组 `nums1` 中。为了应对这种情况，`nums1` 的初始长度为 `m + n`，其中前 `m` 个元素表示应合并的元素，后 `n` 个元素为 `0` ，应忽略。`nums2` 的长度为 `n` 。

 

**示例 1：**

```
输入：nums1 = [1,2,3,0,0,0], m = 3, nums2 = [2,5,6], n = 3
输出：[1,2,2,3,5,6]
解释：需要合并 [1,2,3] 和 [2,5,6] 。
合并结果是 [1,2,2,3,5,6] ，其中斜体加粗标注的为 nums1 中的元素。
```

**示例 2：**

```
输入：nums1 = [1], m = 1, nums2 = [], n = 0
输出：[1]
解释：需要合并 [1] 和 [] 。
合并结果是 [1] 。
```

**示例 3：**

```
输入：nums1 = [0], m = 0, nums2 = [1], n = 1
输出：[1]
解释：需要合并的数组是 [] 和 [1] 。
合并结果是 [1] 。
注意，因为 m = 0 ，所以 nums1 中没有元素。nums1 中仅存的 0 仅仅是为了确保合并结果可以顺利存放到 nums1 中。
```

 

**提示：**

> nums1.length == m + n
> nums2.length == n
> 0 <= m, n <= 200
> 1 <= m + n <= 200
> -109 <= nums1[i], nums2[j] <= 109

code-go

```go
func merge(nums1 []int, m int, nums2 []int, n int) {
	tmpSlice := make([]int, 0, m+n)
	p1, p2 := 0, 0
	for {

		if p1 == m {
			tmpSlice = append(tmpSlice,nums2[p2:n]...)
			break
		}

		if p2 == n {
			tmpSlice = append(tmpSlice,nums1[p1:m]...)
			break
		}

		if nums1[p1] <= nums2[p2] {
			tmpSlice = append(tmpSlice,nums1[p1])
			p1++
		}else {
			tmpSlice = append(tmpSlice,nums2[p2])
			p2++
		}
	}

	copy(nums1,tmpSlice)
}
```

code-python

```python
class Solution:
    def merge(self, nums1: List[int], m: int, nums2: List[int], n: int) -> None:
        """
        Do not return anything, modify nums1 in-place instead.
        """
        tmp_list = []
        p1, p2 = 0, 0
        while True:
            if p1 == m:
                tmp_list.extend(nums2[p2:n])
                break

            if p2 == n:
                tmp_list.extend(nums1[p1:m])
                break

            if nums1[p1] <= nums2[p2]:
                tmp_list.append(nums1[p1])
                p1 += 1
            else:
                tmp_list.append(nums2[p2])
                p2 += 1
        nums1.clear()
        nums1.extend(tmp_list)
```

#### [141. 环形链表](https://leetcode-cn.com/problems/linked-list-cycle/)

难度简单1405收藏分享切换为英文接收动态反馈

给你一个链表的头节点 `head` ，判断链表中是否有环。

如果链表中有某个节点，可以通过连续跟踪 `next` 指针再次到达，则链表中存在环。 为了表示给定链表中的环，评测系统内部使用整数 `pos` 来表示链表尾连接到链表中的位置（索引从 0 开始）。**注意：`pos` 不作为参数进行传递** 。仅仅是为了标识链表的实际情况。

*如果链表中存在环* ，则返回 `true` 。 否则，返回 `false` 。

 

**示例 1：**

![img](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2018/12/07/circularlinkedlist.png)

```
输入：head = [3,2,0,-4], pos = 1
输出：true
解释：链表中有一个环，其尾部连接到第二个节点。
```

**示例 2：**

![img](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2018/12/07/circularlinkedlist_test2.png)

```
输入：head = [1,2], pos = 0
输出：true
解释：链表中有一个环，其尾部连接到第一个节点。
```

**示例 3：**

![img](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2018/12/07/circularlinkedlist_test3.png)

```
输入：head = [1], pos = -1
输出：false
解释：链表中没有环。
```

 

**提示：**

> 链表中节点的数目范围是 [0, 104]
> -105 <= Node.val <= 105
> pos 为 -1 或者链表中的一个 有效索引。

code-go

```go
 /**
 * Definition for singly-linked list.
 * type ListNode struct {
 *     Val int
 *     Next *ListNode
 * }
 */
func hasCycle(head *ListNode) bool {
	if head == nil || head.Next == nil {
		return false
	}
	slow, fast := head, head.Next
	for fast != slow {
		if fast == nil || fast.Next == nil {
			return false
		}
		slow = slow.Next
		fast = fast.Next.Next
	}

	return true
}
```

code_python

```python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, x):
#         self.val = x
#         self.next = None

class Solution:
    def hasCycle(self, head: Optional[ListNode]) -> bool:
        if head is None or head.next is None:
            return False

        slow, fast = head, head.next

        while slow is not fast:
            if fast is None or fast.next is None:
                return False

            slow = slow.next
            fast = fast.next.next

        return True
```

#### [524. 通过删除字母匹配到字典里最长单词](https://leetcode-cn.com/problems/longest-word-in-dictionary-through-deleting/)

难度中等292收藏分享切换为英文接收动态反馈

给你一个字符串 `s` 和一个字符串数组 `dictionary` ，找出并返回 `dictionary` 中最长的字符串，该字符串可以通过删除 `s` 中的某些字符得到。

如果答案不止一个，返回长度最长且字母序最小的字符串。如果答案不存在，则返回空字符串。

 

**示例 1：**

```
输入：s = "abpcplea", dictionary = ["ale","apple","monkey","plea"]
输出："apple"
```

**示例 2：**

```
输入：s = "abpcplea", dictionary = ["a","b","c"]
输出："a"
```

 

**提示：**

> 1 <= s.length <= 1000
> 1 <= dictionary.length <= 1000
> 1 <= dictionary[i].length <= 1000
> s和 dictionary[i] 仅由小写英文字母组成

code-go

```go
func findLongestWord(s string, dictionary []string) string {
	var res string
	for _, element := range dictionary {
		p1 := 0
		for p2 := range s {
			if s[p2] == element[p1] {
				p1++
				if p1 == len(element) {
					if len(element) > len(res) || (len(element) == len(res) && element < res) {
						res = element
					}
					break
				}
			}
		}
	}
	return res
}
```

code-python

```python
class Solution:
    def findLongestWord(self, s: str, dictionary: List[str]) -> str:
        res = ""
        for element in dictionary:
            p1 = 0
            for letter in s:
                if letter == element[p1]:
                    p1 += 1
                    if p1 == len(element):
                        if len(element) > len(res) or (len(element) == len(res) and element < res):
                            res = element
                        break
        return res
```

#### [392. 判断子序列](https://leetcode-cn.com/problems/is-subsequence/)

难度简单615收藏分享切换为英文接收动态反馈

给定字符串 **s** 和 **t** ，判断 **s** 是否为 **t** 的子序列。

字符串的一个子序列是原始字符串删除一些（也可以不删除）字符而不改变剩余字符相对位置形成的新字符串。（例如，`"ace"`是`"abcde"`的一个子序列，而`"aec"`不是）。

**进阶：**

如果有大量输入的 S，称作 S1, S2, ... , Sk 其中 k >= 10亿，你需要依次检查它们是否为 T 的子序列。在这种情况下，你会怎样改变代码？

**致谢：**

特别感谢 [@pbrother ](https://leetcode.com/pbrother/)添加此问题并且创建所有测试用例。

 

**示例 1：**

```
输入：s = "abc", t = "ahbgdc"
输出：true
```

**示例 2：**

```
输入：s = "axc", t = "ahbgdc"
输出：false
```

 

**提示：**

> 0 <= s.length <= 100
> 0 <= t.length <= 10^4
> 两个字符串都只由小写字符组成。

code-go

```go
func isSubsequence(s string, t string) bool {
	sLength := len(s)
	tLength := len(t)
	i, j := 0, 0
	for i < sLength && j < tLength {
		if s[i] == t[j] {
			i++
		}
		j++
	}
	
	if sLength != i {
		return false
	}
	
	return true
}
```

code-python

```python
class Solution:
    def isSubsequence(self, s: str, t: str) -> bool:
        s_length = len(s)
        t_length = len(t)

        i, j = 0, 0
        while i < s_length and j < t_length:
            if s[i] == t[j]:
                i += 1
            j += 1

        if i != s_length:
            return False

        return True

```





## 排序(待完善)

#### [215. 数组中的第K个最大元素](https://leetcode-cn.com/problems/kth-largest-element-in-an-array/)

难度中等1535收藏分享切换为英文接收动态反馈

给定整数数组 `nums` 和整数 `k`，请返回数组中第 `**k**` 个最大的元素。

请注意，你需要找的是数组排序后的第 `k` 个最大的元素，而不是第 `k` 个不同的元素。

 

**示例 1:**

```
输入: [3,2,1,5,6,4] 和 k = 2
输出: 5
```

**示例 2:**

```
输入: [3,2,3,1,2,4,5,5,6] 和 k = 4
输出: 4
```

 

**提示：**

> 1 <= k <= nums.length <= 104
> -104 <= nums[i] <= 104

code-go

```go
func findKthLargest(nums []int, k int) int {
	sort.Ints(nums)
	return nums[len(nums)-k]
}
```

code-python

```python
class Solution:
    def findKthLargest(self, nums: List[int], k: int) -> int:
        nums.sort()
        return nums[-k]
```

#### [347. 前 K 个高频元素](https://leetcode-cn.com/problems/top-k-frequent-elements/)

难度中等1070收藏分享切换为英文接收动态反馈

给你一个整数数组 `nums` 和一个整数 `k` ，请你返回其中出现频率前 `k` 高的元素。你可以按 **任意顺序** 返回答案。

 

**示例 1:**

```
输入: nums = [1,1,1,2,2,3], k = 2
输出: [1,2]
```

**示例 2:**

```
输入: nums = [1], k = 1
输出: [1]
```

 

**提示：**

> 1 <= nums.length <= 105
> k 的取值范围是 [1, 数组中不相同的元素的个数]
> 题目数据保证答案唯一，换句话说，数组中前 `k` 个高频元素的集合是唯一的



## 贪心思想

#### [455. 分发饼干](https://leetcode-cn.com/problems/assign-cookies/)

难度简单462收藏分享切换为英文接收动态反馈

假设你是一位很棒的家长，想要给你的孩子们一些小饼干。但是，每个孩子最多只能给一块饼干。

对每个孩子 `i`，都有一个胃口值 `g[i]`，这是能让孩子们满足胃口的饼干的最小尺寸；并且每块饼干 `j`，都有一个尺寸 `s[j]` 。如果 `s[j] >= g[i]`，我们可以将这个饼干 `j` 分配给孩子 `i` ，这个孩子会得到满足。你的目标是尽可能满足越多数量的孩子，并输出这个最大数值。

**示例 1:**

```
输入: g = [1,2,3], s = [1,1]
输出: 1
解释: 
你有三个孩子和两块小饼干，3个孩子的胃口值分别是：1,2,3。
虽然你有两块小饼干，由于他们的尺寸都是1，你只能让胃口值是1的孩子满足。
所以你应该输出1。
```

**示例 2:**

```
输入: g = [1,2], s = [1,2,3]
输出: 2
解释: 
你有两个孩子和三块小饼干，2个孩子的胃口值分别是1,2。
你拥有的饼干数量和尺寸都足以让所有孩子满足。
所以你应该输出2.
```

 

**提示：**

> 1 <= g.length <= 3 * 104
> 0 <= s.length <= 3 * 104
> 1 <= g[i], s[j] <= 231 - 1



code-go

```go
func findContentChildren(g []int, s []int) int {
	var res int

	sort.Ints(g)
	sort.Ints(s)

	gLength := len(g)
	sLength := len(s)

	for i, j := 0, 0; i < gLength && j < sLength; i++ {
		for j < sLength && g[i] > s[j] {
			j++
		}
		if j < sLength{
			res++
			j++
		}

	}

	return res
}
```

code-python

```go
class Solution:
    def findContentChildren(self, g: List[int], s: List[int]) -> int:
        res = 0

        g.sort()
        s.sort()

        g_length = len(g)
        s_length = len(s)

        j = 0
        for i in range(g_length):
            while j < s_length and g[i] > s[j]:
                j += 1

            if j < s_length:
                res += 1
                j += 1

        return res
```

#### [435. 无重叠区间](https://leetcode-cn.com/problems/non-overlapping-intervals/)

难度中等631收藏分享切换为英文接收动态反馈

给定一个区间的集合 `intervals` ，其中 `intervals[i] = [starti, endi]` 。返回 *需要移除区间的最小数量，使剩余区间互不重叠* 。

 

**示例 1:**

```
输入: intervals = [[1,2],[2,3],[3,4],[1,3]]
输出: 1
解释: 移除 [1,3] 后，剩下的区间没有重叠。
```

**示例 2:**

```
输入: intervals = [ [1,2], [1,2], [1,2] ]
输出: 2
解释: 你需要移除两个 [1,2] 来使剩下的区间没有重叠。
```

**示例 3:**

```
输入: intervals = [ [1,2], [2,3] ]
输出: 0
解释: 你不需要移除任何区间，因为它们已经是无重叠的了。
```

 

**提示:**

> 1 <= intervals.length <= 105
> intervals[i].length == 2
> -5 * 104 <= starti < endi <= 5 * 104

code-go

```go
func eraseOverlapIntervals(intervals [][]int) int {
	// 对 intervals 排序
	sort.Slice(intervals, func(i, j int) bool {
		return intervals[i][1] < intervals[j][1]
	})

	// 贪心思想，计算无重叠区间的个数
	res, right := 1, intervals[0][1]

	for _, element := range intervals[1:] {
		if element[0] >= right {
			res++
			right = element[1]
		}
	}

	// 去掉无重叠区间的个数，得到需要删除的个数
	return len(intervals) - res
}
```

code-python

```python
class Solution:
    def eraseOverlapIntervals(self, intervals: List[List[int]]) -> int:
        intervals.sort(key=lambda element: element[1])
        
        res, right = 1, intervals[0][1]

        for element in intervals[1:]:
            if element[0] >= right:
                res += 1
                right = element[1]

        return len(intervals) - res
```

#### [452. 用最少数量的箭引爆气球](https://leetcode-cn.com/problems/minimum-number-of-arrows-to-burst-balloons/)

难度中等542收藏分享切换为英文接收动态反馈

在二维空间中有许多球形的气球。对于每个气球，提供的输入是水平方向上，气球直径的开始和结束坐标。由于它是水平的，所以纵坐标并不重要，因此只要知道开始和结束的横坐标就足够了。开始坐标总是小于结束坐标。

一支弓箭可以沿着 x 轴从不同点完全垂直地射出。在坐标 x 处射出一支箭，若有一个气球的直径的开始和结束坐标为 `x``start`，`x``end`， 且满足  `xstart ≤ x ≤ x``end`，则该气球会被引爆。可以射出的弓箭的数量没有限制。 弓箭一旦被射出之后，可以无限地前进。我们想找到使得所有气球全部被引爆，所需的弓箭的最小数量。

给你一个数组 `points` ，其中 `points [i] = [xstart,xend]` ，返回引爆所有气球所必须射出的最小弓箭数。

**示例 1：**

```
输入：points = [[10,16],[2,8],[1,6],[7,12]]
输出：2
解释：对于该样例，x = 6 可以射爆 [2,8],[1,6] 两个气球，以及 x = 11 射爆另外两个气球
```

**示例 2：**

```
输入：points = [[1,2],[3,4],[5,6],[7,8]]
输出：4
```

**示例 3：**

```
输入：points = [[1,2],[2,3],[3,4],[4,5]]
输出：2
```

**示例 4：**

```
输入：points = [[1,2]]
输出：1
```

**示例 5：**

```
输入：points = [[2,3],[2,3]]
输出：1
```

 

**提示：**

> 1 <= points.length <= 104
> points[i].length == 2
> -231 <= xstart < xend <= 231 - 1

code-go

```go
// 排序加贪心
func findMinArrowShots(points [][]int) int {
	// 对points 排序
	sort.Slice(points, func(i, j int) bool {
		return points[i][1] < points[j][1]
	})

	res, right := 1, points[0][1]

	for _, element := range points[1:] {
		if element[0] > right {
			res++
			right = element[1]
		}
	}
	
	return res
}
```

code-python

```python
class Solution:
    def findMinArrowShots(self, points: List[List[int]]) -> int:
        points.sort(key=lambda element: element[1])

        res, right = 1, points[0][1]

        for element in points[1:]:
            if element[0] > right:
                res += 1
                right = element[1]

        return res
```

#### [406. 根据身高重建队列](https://leetcode-cn.com/problems/queue-reconstruction-by-height/)

难度中等1168收藏分享切换为英文接收动态反馈

假设有打乱顺序的一群人站成一个队列，数组 `people` 表示队列中一些人的属性（不一定按顺序）。每个 `people[i] = [hi, ki]` 表示第 `i` 个人的身高为 `hi` ，前面 **正好** 有 `ki` 个身高大于或等于 `hi` 的人。

请你重新构造并返回输入数组 `people` 所表示的队列。返回的队列应该格式化为数组 `queue` ，其中 `queue[j] = [hj, kj]` 是队列中第 `j` 个人的属性（`queue[0]` 是排在队列前面的人）。

**示例 1：**

```
输入：people = [[7,0],[4,4],[7,1],[5,0],[6,1],[5,2]]
输出：[[5,0],[7,0],[5,2],[6,1],[4,4],[7,1]]
解释：
编号为 0 的人身高为 5 ，没有身高更高或者相同的人排在他前面。
编号为 1 的人身高为 7 ，没有身高更高或者相同的人排在他前面。
编号为 2 的人身高为 5 ，有 2 个身高更高或者相同的人排在他前面，即编号为 0 和 1 的人。
编号为 3 的人身高为 6 ，有 1 个身高更高或者相同的人排在他前面，即编号为 1 的人。
编号为 4 的人身高为 4 ，有 4 个身高更高或者相同的人排在他前面，即编号为 0、1、2、3 的人。
编号为 5 的人身高为 7 ，有 1 个身高更高或者相同的人排在他前面，即编号为 1 的人。
因此 [[5,0],[7,0],[5,2],[6,1],[4,4],[7,1]] 是重新构造后的队列。
```

**示例 2：**

```
输入：people = [[6,0],[5,0],[4,0],[3,2],[2,2],[1,4]]
输出：[[4,0],[5,0],[2,2],[3,2],[1,4],[6,0]]
```

 

**提示：**

> 1 <= people.length <= 2000
> 0 <= hi <= 106
> 0 <= ki < people.length
> 题目数据确保队列可以被重建

code-go

```go
// 排序后插队
func reconstructQueue(people [][]int) [][]int {
	// 对people按从小到大的顺序排序，
	sort.Slice(people, func(i, j int) bool {
		return people[i][0] < people[j][0] || (people[i][0] == people[j][0] && people[i][1] > people[j][1])
	})

	res := make([][]int, len(people))
	for _, element := range people {
		// 成员位置,前面加当前+1
		space := element[1] + 1
		for index := range res {
			if res[index] == nil {
				space--
				if space == 0 {
					res[index] = element
					break
				}
			}
		}
	}
	
	return res
}
```

code-python

```python
class Solution:
    def reconstructQueue(self, people: List[List[int]]) -> List[List[int]]:
        people.sort(key=lambda element: (element[0], -element[1]))

        res = [None] * len(people)
        for element in people:
            # 记录位置
            space = element[1] + 1
            for index in range(len(res)):
                if res[index] is None:
                    space -= 1
                    if space == 0:
                        res[index] = element
                        break

        return res
```

#### [121. 买卖股票的最佳时机](https://leetcode-cn.com/problems/best-time-to-buy-and-sell-stock/)

难度简单2207收藏分享切换为英文接收动态反馈

给定一个数组 `prices` ，它的第 `i` 个元素 `prices[i]` 表示一支给定股票第 `i` 天的价格。

你只能选择 **某一天** 买入这只股票，并选择在 **未来的某一个不同的日子** 卖出该股票。设计一个算法来计算你所能获取的最大利润。

返回你可以从这笔交易中获取的最大利润。如果你不能获取任何利润，返回 `0` 。

 

**示例 1：**

```
输入：[7,1,5,3,6,4]
输出：5
解释：在第 2 天（股票价格 = 1）的时候买入，在第 5 天（股票价格 = 6）的时候卖出，最大利润 = 6-1 = 5 。
     注意利润不能是 7-1 = 6, 因为卖出价格需要大于买入价格；同时，你不能在买入前卖出股票。
```

**示例 2：**

```
输入：prices = [7,6,4,3,1]
输出：0
解释：在这种情况下, 没有交易完成, 所以最大利润为 0。
```

 

**提示：**

> 1 <= prices.length <= 105
> 0 <= prices[i] <= 104

code-go

```go
// 使用贪心算法，每次都找出最小的价格，用当前价格减去最小的价格
func maxProfit(prices []int) int {
	res, minPrice := 0, prices[0]
	length := len(prices)
	
	for i := 1; i < length; i++ {
		if prices[i] < minPrice {
			minPrice = prices[i]
		}

		tmp := prices[i] - minPrice
		if tmp > res {
			res = tmp
		}
	}
	
	return res
}
```

code-python

```python
class Solution:
    def maxProfit(self, prices: List[int]) -> int:
        res, min_price = 0, prices[0]

        for i in range(1, len(prices)):
            if prices[i] < min_price:
                min_price = prices[i]

            tmp = prices[i] - min_price
            if res < tmp:
                res = tmp

        return res
```

#### [122. 买卖股票的最佳时机 II](https://leetcode-cn.com/problems/best-time-to-buy-and-sell-stock-ii/)

难度中等1605收藏分享切换为英文接收动态反馈

给定一个数组 `prices` ，其中 `prices[i]` 表示股票第 `i` 天的价格。

在每一天，你可能会决定购买和/或出售股票。你在任何时候 **最多** 只能持有 **一股** 股票。你也可以购买它，然后在 **同一天** 出售。
返回 *你能获得的 **最大** 利润* 。

 

**示例 1:**

```
输入: prices = [7,1,5,3,6,4]
输出: 7
解释: 在第 2 天（股票价格 = 1）的时候买入，在第 3 天（股票价格 = 5）的时候卖出, 这笔交易所能获得利润 = 5-1 = 4 。
     随后，在第 4 天（股票价格 = 3）的时候买入，在第 5 天（股票价格 = 6）的时候卖出, 这笔交易所能获得利润 = 6-3 = 3 。
```

**示例 2:**

```
输入: prices = [1,2,3,4,5]
输出: 4
解释: 在第 1 天（股票价格 = 1）的时候买入，在第 5 天 （股票价格 = 5）的时候卖出, 这笔交易所能获得利润 = 5-1 = 4 。
     注意你不能在第 1 天和第 2 天接连购买股票，之后再将它们卖出。因为这样属于同时参与了多笔交易，你必须在再次购买前出售掉之前的股票。
```

**示例 3:**

```
输入: prices = [7,6,4,3,1]
输出: 0
解释: 在这种情况下, 没有交易完成, 所以最大利润为 0。
```

 

**提示：**

> 1 <= prices.length <= 3 * 104
> 0 <= prices[i] <= 104

code-go

```go
func maxProfit(prices []int) int {
	var res int
	length := len(prices)
	for i := 1; i < length; i++ {
		res += max(0,prices[i]-prices[i-1])

	}
	return res
}

func max(a, b int) int {
	if a > b {
		return a
	}
	return b
}
```

code-python

```go
class Solution:
    def maxProfit(self, prices: List[int]) -> int:
        res = 0
        for i in range(1, len(prices)):
            res += max(0, prices[i] - prices[i - 1])
            
        return res

```

#### [605. 种花问题](https://leetcode-cn.com/problems/can-place-flowers/)

难度简单440收藏分享切换为英文接收动态反馈

假设有一个很长的花坛，一部分地块种植了花，另一部分却没有。可是，花不能种植在相邻的地块上，它们会争夺水源，两者都会死去。

给你一个整数数组 `flowerbed` 表示花坛，由若干 `0` 和 `1` 组成，其中 `0` 表示没种植花，`1` 表示种植了花。另有一个数 `n` ，能否在不打破种植规则的情况下种入 `n` 朵花？能则返回 `true` ，不能则返回 `false`。

 

**示例 1：**

```
输入：flowerbed = [1,0,0,0,1], n = 1
输出：true
```

**示例 2：**

```
输入：flowerbed = [1,0,0,0,1], n = 2
输出：false
```

 

**提示：**

> 1 <= flowerbed.length <= 2 * 104
> flowerbed[i]为 0 或 1
> flowerbed 中不存在相邻的两朵花
> 0 <= n <= flowerbed.length

code-go

```go
func canPlaceFlowers(flowerbed []int, n int) bool {
	var res int
	length := len(flowerbed)

	// 设置一个哨兵，判断前面是0还是1
	sentinel := 0
	loop:for i := 0; i < length; {
		switch {
		case i == length - 1:
			if flowerbed[i] == 0 && sentinel == 0 {
				res++
			}
			break loop
		case flowerbed[i] == 0 && flowerbed[i+1] == 0 && sentinel == 0 :
			res++
			i += 2
		default:
			sentinel = flowerbed[i]
			i++
		}
	}

	if res < n {
		return false
	}

	return true
}
```

code-python

```python
class Solution:
    def canPlaceFlowers(self, flowerbed: List[int], n: int) -> bool:
        res = 0
        length = len(flowerbed)
        sentinel = 0
        i = 0
        while i < length:
            if i == length - 1:
                if sentinel == 0 and flowerbed[i] == 0:
                    res += 1
                break
            elif sentinel == 0 and flowerbed[i] == 0 and flowerbed[i + 1] == 0:
                res += 1
                i += 2
            else:
                sentinel = flowerbed[i]
                i += 1

        if res < n:
            return False

        return True
```

#### [665. 非递减数列](https://leetcode-cn.com/problems/non-decreasing-array/)

难度中等651收藏分享切换为英文接收动态反馈

给你一个长度为 `n` 的整数数组 `nums` ，请你判断在 **最多** 改变 `1` 个元素的情况下，该数组能否变成一个非递减数列。

我们是这样定义一个非递减数列的： 对于数组中任意的 `i` `(0 <= i <= n-2)`，总满足 `nums[i] <= nums[i + 1]`。

 

**示例 1:**

```
输入: nums = [4,2,3]
输出: true
解释: 你可以通过把第一个 4 变成 1 来使得它成为一个非递减数列。
```

**示例 2:**

```
输入: nums = [4,2,1]
输出: false
解释: 你不能在只改变一个元素的情况下将其变为非递减数列。
```

 

**提示：**

> n == nums.length
> 1 <= n <= 104
> -105 <= nums[i] <= 105

code-go

```go
func checkPossibility(nums []int) bool {
	// 是否更改
	isChange := false
	length := len(nums)

	for i := 1; i < length; i++ {
		if nums[i] < nums[i-1] {
			if isChange {
				return false
			}
			isChange = true

			// 队首 或者 [3,4,2,3]
			if i == 1 || (i >1 && nums[i-2] < nums[i]) {
				nums[i-1] = nums[i]
			}

			if i > 1 && nums[i - 2] > nums[i] {
				nums[i] = nums[i - 1]
			}
		}

	}

	return true
}
```

code-python

```python
class Solution:
    def checkPossibility(self, nums: List[int]) -> bool:
        is_change = False

        for i in range(1, len(nums)):
            if nums[i] < nums[i - 1]:
                if is_change:
                    return False

                is_change = True

                if i == 1 or (i > 1 and nums[i - 2] < nums[i]):
                    nums[i - 1] = nums[i]

                if i > 1 and nums[i - 2] > nums[i]:
                    nums[i] = nums[i - 1]

        return True
```

#### [53. 最大子数组和](https://leetcode-cn.com/problems/maximum-subarray/)

难度简单4597收藏分享切换为英文接收动态反馈

给你一个整数数组 `nums` ，请你找出一个具有最大和的连续子数组（子数组最少包含一个元素），返回其最大和。

**子数组** 是数组中的一个连续部分。

 

**示例 1：**

```
输入：nums = [-2,1,-3,4,-1,2,1,-5,4]
输出：6
解释：连续子数组 [4,-1,2,1] 的和最大，为 6 。
```

**示例 2：**

```
输入：nums = [1]
输出：1
```

**示例 3：**

```
输入：nums = [5,4,-1,7,8]
输出：23
```

 

**提示：**

> 1 <= nums.length <= 105
> -104 <= nums[i] <= 104

code-go

```go
// 贪心思想: 动态数组
// 前者与后者相加,如果大于后者，和赋值给后者
func maxSubArray(nums []int) int {
	max := nums[0]
	length := len(nums)
	
	for i := 1; i < length; i++ {
		if nums[i-1] > 0 {
			nums[i] += nums[i-1]
		}
		
		if nums[i] > max {
			max = nums[i]
		}
	}
	
	return max
}
```

code-python

```python
class Solution:
    def maxSubArray(self, nums: List[int]) -> int:
        max_val = nums[0]

        for i in range(1, len(nums)):
            if nums[i - 1] > 0:
                nums[i] += nums[i - 1]

            if nums[i] > max_val:
                max_val = nums[i]

        return max_val
```

#### [763. 划分字母区间](https://leetcode-cn.com/problems/partition-labels/)

难度中等684收藏分享切换为英文接收动态反馈

字符串 `S` 由小写字母组成。我们要把这个字符串划分为尽可能多的片段，同一字母最多出现在一个片段中。返回一个表示每个字符串片段的长度的列表。

 

**示例：**

```
输入：S = "ababcbacadefegdehijhklij"
输出：[9,7,8]
解释：
划分结果为 "ababcbaca", "defegde", "hijhklij"。
每个字母最多出现在一个片段中。
像 "ababcbacadefegde", "hijhklij" 的划分是错误的，因为划分的片段数较少。
```

 

**提示：**

> S的长度在[1, 500]之间。
> S只包含小写字母 'a'到 'z' 。

code-go

```go
func partitionLabels(s string) []int {
	var res []int
	// 记录最后一个索引值
	note := make(map[uint8]int)

	length := len(s)
	for i := 0; i < length; i++ {
		note[s[i]] = i
	}

	start, end := 0, 0
	for i := 0; i < length; i++ {
		if note[s[i]] > end {
			end = note[s[i]]
		}
		if i == end {
			res = append(res, end-start+1)
			start = end + 1
		}
	}

	return res
}
```

code-python

```python
class Solution:
    def partitionLabels(self, s: str) -> List[int]:
        res = []
        note = {}

        for i, c in enumerate(s):
            note[c] = i

        start, end = 0, 0
        for i in range(len(s)):
            if note[s[i]] > end:
                end = note[s[i]]

            if end == i:
                res.append(end - start + 1)
                start = end + 1

        return res
```



## 二分查找

#### [69. x 的平方根 ](https://leetcode-cn.com/problems/sqrtx/)

难度简单940收藏分享切换为英文接收动态反馈

给你一个非负整数 `x` ，计算并返回 `x` 的 **算术平方根** 。

由于返回类型是整数，结果只保留 **整数部分** ，小数部分将被 **舍去 。**

**注意：**不允许使用任何内置指数函数和算符，例如 `pow(x, 0.5)` 或者 `x ** 0.5` 。

 

**示例 1：**

```
输入：x = 4
输出：2
```

**示例 2：**

```
输入：x = 8
输出：2
解释：8 的算术平方根是 2.82842..., 由于返回类型是整数，小数部分将被舍去。
```

 

**提示：**

> 0 <= x <= 231 - 1

code-go

```go
func mySqrt(x int) int {
	if x < 2 {
		return x
	}

	left, right := 1, x

	for left < right {
		mid := left + (right-left+1)/2
		if mid*mid <= x {
			left = mid
		} else {
			right = mid - 1
		}
	}

	return left
}
```

code-python

```python
class Solution:
    def mySqrt(self, x: int) -> int:
        if x < 2:
            return x

        left, right = 1, x // 2

        while left < right:
            mid = left + (right - left + 1) // 2
            if mid * mid <= x:
                left = mid
            else:
                right = mid - 1

        return left

```

#### [744. 寻找比目标字母大的最小字母](https://leetcode-cn.com/problems/find-smallest-letter-greater-than-target/)

难度简单146收藏分享切换为英文接收动态反馈

给你一个排序后的字符列表 `letters` ，列表中只包含小写英文字母。另给出一个目标字母 `target`，请你寻找在这一有序列表里比目标字母大的最小字母。

在比较时，字母是依序循环出现的。举个例子：

- 如果目标字母 `target = 'z'` 并且字符列表为 `letters = ['a', 'b']`，则答案返回 `'a'`

 

**示例 1：**

```
输入: letters = ["c", "f", "j"]，target = "a"
输出: "c"
```

**示例 2:**

```
输入: letters = ["c","f","j"], target = "c"
输出: "f"
```

**示例 3:**

```
输入: letters = ["c","f","j"], target = "d"
输出: "f"
```

 

**提示：**

> 2 <= letters.length <= 104
> letters[i]是一个小写字母
> letters按非递减顺序排序
> letters最少包含两个不同的字母
> target是一个小写字母

code-go

```go
// 给定一个有序的字符数组 letters 和一个字符 target，要求找出 letters 中大于 target 的最小字符，如果找不到就返回第 1 个字符。
func nextGreatestLetter(letters []byte, target byte) byte {
	left, right := 0, len(letters)-1
	if letters[right] <= target {
		return letters[left]
	}

	for left <= right {
		mid := left + (right-left)/2
		if letters[mid] > target {
			right = mid - 1
		} else {
			left = mid + 1
		}
	}
	return letters[left]

}
```

code-python

```python
class Solution:
    def nextGreatestLetter(self, letters: List[str], target: str) -> str:
        left, right = 0, len(letters) - 1

        if letters[right] <= target:
            return letters[left]

        while left <= right:
            mid = left + (right - left) // 2
            if letters[mid] > target:
                right = mid - 1
            else:
                left = mid + 1

        return letters[left]
```

#### [540. 有序数组中的单一元素](https://leetcode-cn.com/problems/single-element-in-a-sorted-array/)

难度中等485收藏分享切换为英文接收动态反馈

给你一个仅由整数组成的有序数组，其中每个元素都会出现两次，唯有一个数只会出现一次。

请你找出并返回只出现一次的那个数。

你设计的解决方案必须满足 `O(log n)` 时间复杂度和 `O(1)` 空间复杂度。

 

**示例 1:**

```
输入: nums = [1,1,2,3,3,4,4,8,8]
输出: 2
```

**示例 2:**

```
输入: nums =  [3,3,7,7,10,11,11]
输出: 10
```

 

**提示:**

> 1 <= nums.length <= 105
> 0 <= nums[i] <= 105

code-go

```go
func singleNonDuplicate(nums []int) int {
	left, right := 0, len(nums)-1

	for left < right {
		mid := left +(right-left) / 2
		// 保证 left/right/mid 都在偶数位，使得查找区间大小一直都是奇数
		if mid % 2 == 1 {
			mid--
		}
		
		if nums[mid] == nums[mid+1] {
			left = mid + 2
		}else {
			right = mid
		}
	}
	return nums[left]
}
```

code-python

```python
class Solution:
    def singleNonDuplicate(self, nums: List[int]) -> int:
        left, right = 0, len(nums) - 1

        while left < right:
            mid = left + (right - left) // 2
            if mid % 2 == 1:
                mid -= 1

            if nums[mid] == nums[mid + 1]:
                left = mid + 2
            else:
                right = mid

        return nums[left]
```

#### [278. 第一个错误的版本](https://leetcode-cn.com/problems/first-bad-version/)

难度简单637收藏分享切换为英文接收动态反馈

你是产品经理，目前正在带领一个团队开发新的产品。不幸的是，你的产品的最新版本没有通过质量检测。由于每个版本都是基于之前的版本开发的，所以错误的版本之后的所有版本都是错的。

假设你有 `n` 个版本 `[1, 2, ..., n]`，你想找出导致之后所有版本出错的第一个错误的版本。

你可以通过调用 `bool isBadVersion(version)` 接口来判断版本号 `version` 是否在单元测试中出错。实现一个函数来查找第一个错误的版本。你应该尽量减少对调用 API 的次数。

**示例 1：**

```
输入：n = 5, bad = 4
输出：4
解释：
调用 isBadVersion(3) -> false 
调用 isBadVersion(5) -> true 
调用 isBadVersion(4) -> true
所以，4 是第一个错误的版本。
```

**示例 2：**

```
输入：n = 1, bad = 1
输出：1
```

 

**提示：**

> 1 <= bad <= n <= 231 - 1

code-go

```go
/** 
 * Forward declaration of isBadVersion API.
 * @param   version   your guess about first bad version
 * @return 	 	      true if current version is bad 
 *			          false if current version is good
 * func isBadVersion(version int) bool;
 */

func firstBadVersion(n int) int {
	left, right := 1, n

	for left < right {
		mid := left + (right-left)/2
		if isBadVersion(mid) {
			right = mid
		} else {
			left = mid + 1
		}
	}

	return left
}
```

code-python

```python
# The isBadVersion API is already defined for you.
# @param version, an integer
# @return an integer
# def isBadVersion(version):

class Solution:
    def firstBadVersion(self, n):
        """
        :type n: int
        :rtype: int
        """
        left, right = 0, n

        while left < right:
            mid = left + (right - left) // 2
            if isBadVersion(mid):
                right = mid
            else:
                left = mid + 1

        return left
```

#### [153. 寻找旋转排序数组中的最小值](https://leetcode-cn.com/problems/find-minimum-in-rotated-sorted-array/)

难度中等703收藏分享切换为英文接收动态反馈

已知一个长度为 `n` 的数组，预先按照升序排列，经由 `1` 到 `n` 次 **旋转** 后，得到输入数组。例如，原数组 `nums = [0,1,2,4,5,6,7]` 在变化后可能得到：

- 若旋转 `4` 次，则可以得到 `[4,5,6,7,0,1,2]`
- 若旋转 `7` 次，则可以得到 `[0,1,2,4,5,6,7]`

注意，数组 `[a[0], a[1], a[2], ..., a[n-1]]` **旋转一次** 的结果为数组 `[a[n-1], a[0], a[1], a[2], ..., a[n-2]]` 。

给你一个元素值 **互不相同** 的数组 `nums` ，它原来是一个升序排列的数组，并按上述情形进行了多次旋转。请你找出并返回数组中的 **最小元素** 。

你必须设计一个时间复杂度为 `O(log n)` 的算法解决此问题。

 

**示例 1：**

```
输入：nums = [3,4,5,1,2]
输出：1
解释：原数组为 [1,2,3,4,5] ，旋转 3 次得到输入数组。
```

**示例 2：**

```
输入：nums = [4,5,6,7,0,1,2]
输出：0
解释：原数组为 [0,1,2,4,5,6,7] ，旋转 4 次得到输入数组。
```

**示例 3：**

```
输入：nums = [11,13,15,17]
输出：11
解释：原数组为 [11,13,15,17] ，旋转 4 次得到输入数组。
```

 

**提示：**

> n == nums.length
> 1 <= n <= 5000
> -5000 <= nums[i] <= 5000
> nums 中的所有整数 互不相同
> nums原来是一个升序排序的数组，并进行了 1 至 n次旋转

code-go

```go
// 二分查找最小值
func findMin(nums []int) int {
	left, right := 0, len(nums)-1

	for left < right {
		mid := left + (right-left)/2
		if nums[mid] > nums[right] {
			left = mid + 1
		} else {
			right = mid
		}
	}

	return nums[left]
}

```

code-python

```python
class Solution:
    def findMin(self, nums: List[int]) -> int:
        left, right = 0, len(nums) - 1

        while left < right:
            mid = left + (right - left) // 2
            if nums[mid] > nums[right] :
                left = mid + 1
            else:
                right = mid

        return nums[left]
```



#### [34. 在排序数组中查找元素的第一个和最后一个位置](https://leetcode-cn.com/problems/find-first-and-last-position-of-element-in-sorted-array/)

难度中等1559收藏分享切换为英文接收动态反馈

给定一个按照升序排列的整数数组 `nums`，和一个目标值 `target`。找出给定目标值在数组中的开始位置和结束位置。

如果数组中不存在目标值 `target`，返回 `[-1, -1]`。

**进阶：**

- 你可以设计并实现时间复杂度为 `O(log n)` 的算法解决此问题吗？

 

**示例 1：**

```
输入：nums = [5,7,7,8,8,10], target = 8
输出：[3,4]
```

**示例 2：**

```
输入：nums = [5,7,7,8,8,10], target = 6
输出：[-1,-1]
```

**示例 3：**

```
输入：nums = [], target = 0
输出：[-1,-1]
```

 

**提示：**

> 0 <= nums.length <= 105
> -109 <= nums[i] <= 109
> nums是一个非递减数组
> -109 <= target <= 109

code-go

```go
func searchRange(nums []int, target int) []int {
    leftmost := sort.SearchInts(nums, target)
    if leftmost == len(nums) || nums[leftmost] != target {
        return []int{-1, -1}
    }
    rightmost := sort.SearchInts(nums, target + 1) - 1
    return []int{leftmost, rightmost}
}
```

code-python

```python
class Solution:
    def searchRange(self, nums: List[int], target: int) -> List[int]:
        try:
            left = nums.index(target)
            right = left
            for i in range(left+1,len(nums)):
                if nums[i] == target:
                    right = i
            return [left,right]

        except:
            return [-1, -1]
```

## 分治

#### [241. 为运算表达式设计优先级](https://leetcode-cn.com/problems/different-ways-to-add-parentheses/)

难度中等531收藏分享切换为英文接收动态反馈

给你一个由数字和运算符组成的字符串 `expression` ，按不同优先级组合数字和运算符，计算并返回所有可能组合的结果。你可以 **按任意顺序** 返回答案。

 

**示例 1：**

```
输入：expression = "2-1-1"
输出：[0,2]
解释：
((2-1)-1) = 0 
(2-(1-1)) = 2
```

**示例 2：**

```
输入：expression = "2*3-4*5"
输出：[-34,-14,-10,-10,10]
解释：
(2*(3-(4*5))) = -34 
((2*3)-(4*5)) = -14 
((2*(3-4))*5) = -10 
(2*((3-4)*5)) = -10 
(((2*3)-4)*5) = 10
```

 

**提示：**

> 1 <= expression.length <= 20
> expression由数字和算符 '+'、'-'和 '*' 组成。
> 输入表达式中的所有整数值在范围 [0, 99]

code-go

```go
func diffWaysToCompute(expression string) []int {
	if str2int, err := strconv.Atoi(expression); err != nil {
		return []int{str2int}
	}
	var res []int
	for i := range expression {
		if expression[i] == '+' || expression[i] == '-' || expression[i] == '*' {
			left := diffWaysToCompute(expression[:i])
			right := diffWaysToCompute(expression[i+1:])
			for _, lInt := range left {
				for _, rInt := range right {
					switch expression[i] {
					case '+':
						res = append(res, lInt+rInt)
					case '-':
						res = append(res, lInt-rInt)
					case '*':
						res = append(res, lInt*rInt)
					default:
					}
				}
			}
		}
	}
	return res
}
```

code-python

```python
class Solution:
    def diffWaysToCompute(self, expression: str) -> List[int]:
        # 整型直接返回
        if expression.isdigit():
            return [int(expression)]

        res = []

        for i in range(len(expression)):
            if expression[i] == "+" or expression[i] == "-" or expression[i] == "*":
                left = self.diffWaysToCompute(expression[:i])
                right = self.diffWaysToCompute(expression[i + 1:])
                for l_int in left :
                    for r_int in right:
                        if expression[i] == "+":
                            res.append(l_int+r_int)
                        elif expression[i] == "-":
                            res.append(l_int - r_int)
                        else:
                            res.append(l_int * r_int)
        return res
```

#### [95. 不同的二叉搜索树 II](https://leetcode-cn.com/problems/unique-binary-search-trees-ii/)

难度中等1174收藏分享切换为英文接收动态反馈

给你一个整数 `n` ，请你生成并返回所有由 `n` 个节点组成且节点值从 `1` 到 `n` 互不相同的不同 **二叉搜索树** 。可以按 **任意顺序** 返回答案。

 

**示例 1：**

![img](https://assets.leetcode.com/uploads/2021/01/18/uniquebstn3.jpg)

```
输入：n = 3
输出：[[1,null,2,null,3],[1,null,3,2],[2,1,3],[3,1,null,null,2],[3,2,null,1]]
```

**示例 2：**

```
输入：n = 1
输出：[[1]]
```

 

**提示：**

> 1 <= n <= 8

code-go

```go
/**
 * Definition for a binary tree node.
 * type TreeNode struct {
 *     Val int
 *     Left *TreeNode
 *     Right *TreeNode
 * }
 */
func generateTrees(n int) []*TreeNode {
	if n <= 0 {
		return nil
	}

	return dfs(1, n)
}

func dfs(start, end int) []*TreeNode {
	if start > end {
		return []*TreeNode{nil}
	}
	var all []*TreeNode
	// 枚举可行根节点
	for i := start; i <= end; i++ {
		// 获得所有可行的左子树集合
		lefts := dfs(start, i - 1)
		// 获得所有可行的右子树集合
		rights := dfs(i + 1, end)
		// 从左子树集合中选出一棵左子树，从右子树集合中选出一棵右子树，拼接到根节点上
		for _, left := range lefts {
			for _, right := range rights {
				currTree := &TreeNode{i, left, right}
				all = append(all, currTree)
			}
		}
	}
	return all
}
```

code-python

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def generateTrees(self, n: int) -> List[TreeNode]:
        if n <= 0:
            return None
        
        return self.dfs(1,n)

    def dfs(self, start: int, end: int) -> List[TreeNode]:
        if start > end:
            return [None]

        all = []

        for i in range(start, end + 1):
            lefts = self.dfs(start, i - 1)
            rights = self.dfs(i + 1, end)
            
            for left in lefts:
                for right in rights:
                    node = TreeNode(val=i,left=left,right=right)
                    all.append(node)
        return all
```

## 搜索

深度优先搜索和广度优先搜索广泛运用于树和图中，但是它们的应用远远不止如此。

### BFS

#### [1091. 二进制矩阵中的最短路径](https://leetcode-cn.com/problems/shortest-path-in-binary-matrix/)

难度中等181收藏分享切换为英文接收动态反馈

给你一个 `n x n` 的二进制矩阵 `grid` 中，返回矩阵中最短 **畅通路径** 的长度。如果不存在这样的路径，返回 `-1` 。

二进制矩阵中的 畅通路径 是一条从 **左上角** 单元格（即，`(0, 0)`）到 右下角 单元格（即，`(n - 1, n - 1)`）的路径，该路径同时满足下述要求：

- 路径途经的所有单元格都的值都是 `0` 。
- 路径中所有相邻的单元格应当在 **8 个方向之一** 上连通（即，相邻两单元之间彼此不同且共享一条边或者一个角）。

**畅通路径的长度** 是该路径途经的单元格总数。

 

**示例 1：**

![img](https://assets.leetcode.com/uploads/2021/02/18/example1_1.png)

```
输入：grid = [[0,1],[1,0]]
输出：2
```

**示例 2：**

![img](https://assets.leetcode.com/uploads/2021/02/18/example2_1.png)

```
输入：grid = [[0,0,0],[1,1,0],[1,1,0]]
输出：4
```

**示例 3：**

```
输入：grid = [[1,0,0],[1,1,0],[1,1,0]]
输出：-1
```

 

**提示：**

> n == grid.length
> n == grid[i].length
> 1 <= n <= 100
> `grid[i][j]` 为 0或 1

code-go

```go
func shortestPathBinaryMatrix(grid [][]int) int {
	rows := len(grid)
	if grid == nil || rows == 0 || grid[0][0] == 1 || grid[rows-1][rows-1] == 1 {
		return -1
	}
	if len(grid) == 1 && grid[0][0] == 0 {
		return 1
	}
	direction := []int{-1, 0, 1}
	grid[0][0] = 1
	//途经的每一个点都记录从起点到次的长度
	que := make([]int, 0)
	que = append(que, 0)
	//用que记录当前点的坐标，判断有没有下一个节点

	var x, y, nx, ny int

	for len(que) > 0 {
		x, y = que[0]/rows, que[0]%rows
		que = que[1:]
		for _, i := range direction {
			for _, j := range direction {
				if i == j && i == 0 {
					continue
				}
				nx, ny = x+i, y+j
				if nx < rows && ny < rows && nx >= 0 && ny >= 0 && grid[nx][ny] == 0 {
					que = append(que, nx*rows+ny)
					grid[nx][ny] = grid[x][y] + 1
					if nx == rows-1 && ny == rows-1 {
						return grid[nx][ny]
					}
				}
			}
		}
	}
	return -1
}
```

code-python

```python
class Solution:
    def shortestPathBinaryMatrix(self, grid: List[List[int]]) -> int:
        rows = len(grid)
        if grid[0][0] == 1 or grid[rows - 1][rows - 1] == 1:
            return -1

        if rows == 1 and grid[0][0] == 0:
            return 1

        direction = [1, -1, 0]
        grid[0][0] = 1
        queue = [0]

        x, y, nx, ny = 0, 0, 0, 0

        while queue:
            x, y = queue[0] // rows, queue[0] % rows
            queue = queue[1:]
            for i in direction:
                for j in direction:
                    if i == 0 and i == j:
                        continue
                    nx, ny = x + i, y + j
                    if 0 <= nx < rows and 0 <= ny < rows and grid[nx][ny] == 0:
                        grid[nx][ny] = grid[x][y] + 1
                        queue.append(nx * rows + ny)
                        if nx == rows - 1 and ny == nx:
                            return grid[nx][ny]

        return -1
```

#### [127. 单词接龙](https://leetcode-cn.com/problems/word-ladder/)

难度困难991收藏分享切换为英文接收动态反馈

字典 `wordList` 中从单词 `beginWord` 和 `endWord` 的 **转换序列** 是一个按下述规格形成的序列 `beginWord -> s1 -> s2 -> ... -> sk`：

- 每一对相邻的单词只差一个字母。
-  对于 `1 <= i <= k` 时，每个 `si` 都在 `wordList` 中。注意， `beginWord` 不需要在 `wordList` 中。
- `sk == endWord`

给你两个单词 `beginWord` 和 `endWord` 和一个字典 `wordList` ，返回 *从 `beginWord` 到 `endWord` 的 **最短转换序列** 中的 **单词数目*** 。如果不存在这样的转换序列，返回 `0` 。

**示例 1：**

```
输入：beginWord = "hit", endWord = "cog", wordList = ["hot","dot","dog","lot","log","cog"]
输出：5
解释：一个最短转换序列是 "hit" -> "hot" -> "dot" -> "dog" -> "cog", 返回它的长度 5。
```

**示例 2：**

```
输入：beginWord = "hit", endWord = "cog", wordList = ["hot","dot","dog","lot","log"]
输出：0
解释：endWord "cog" 不在字典中，所以无法进行转换。
```

 

**提示：**

> 1 <= beginWord.length <= 10
> endWord.length == beginWord.length
> 1 <= wordList.length <= 5000
> wordList[i].length == beginWord.length
> beginWord、endWord 和 wordList[i] 由小写英文字母组成
> beginWord != endWord
> wordList中的所有字符串 **互不相同**



























## 动态规划

#### [279. 完全平方数](https://leetcode-cn.com/problems/perfect-squares/)

难度中等1305收藏分享切换为英文接收动态反馈

给你一个整数 `n` ，返回 *和为 `n` 的完全平方数的最少数量* 。

**完全平方数** 是一个整数，其值等于另一个整数的平方；换句话说，其值等于一个整数自乘的积。例如，`1`、`4`、`9` 和 `16` 都是完全平方数，而 `3` 和 `11` 不是。

 

**示例 1：**

```
输入：n = 12
输出：3 
解释：12 = 4 + 4 + 4
```

**示例 2：**

```
输入：n = 13
输出：2
解释：13 = 4 + 9
```

**提示：**

> 1 <= n <= 104

code-go

```go
func numSquares(n int) int {
	noteList := make([]int, n+1)
	for i := 1; i <= n; i++ {
		// 1 <= n <= 104
		minValue := 10000
		for j := 1; j*j <= i; j++ {
			minValue = min(minValue, noteList[i-j*j])
		}
		noteList[i] = minValue + 1
	}
	return noteList[n]
}

func min(a, b int) int {
	if a > b {
		return b
	}
	return a
}
```

code-python

```python
class Solution:
    def numSquares(self, n: int) -> int:
        note_list = [0] * (n + 1)
        for i in range(1, n + 1):
            min_value = 10000
            j = 1
            while j * j <= i:
                min_value = min(min_value, note_list[i - j * j])
                j += 1

            note_list[i] = min_value + 1
        return note_list[n]
```







数学

# 数据结构相关

链表

树

栈和队列

哈希表

字符串

数组和矩阵

图

位运算



