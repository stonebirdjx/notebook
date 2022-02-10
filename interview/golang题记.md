# 其他

关于go vendor，下面说法正确的是（）
A.基本思路是将引用的外部包的源代码放在当前工程的vendor目录下面
B.编译go代码会优先从vendor目录先寻找依赖包
C.可以指定引用某个特定版本的外部包
D.有了vendor目录后，打包当前的工程代码到其他机器的$GOPATH/src下都可以通过编译

> ABD

`无法精确的引用外部包进行版本控制,一旦外部包升级,vendor下的代码不会跟着升级，
而且vendor下面并没有元文件记录引用包的版本信息`

------

关于go vet，下面说法正确的是（）
A.go vet是golang自带工具go tool vet的封装
B.当执行go vet database时，可以对database所在目录下的所有子文件夹进行递归检测
C.go vet可以使用绝对路径、相对路径或相对GOPATH的路径指定待检测的包
D.go vet可以检测出死代码

> ACD

`go tool vet package1 package2 ； go tool vet 才可以递归`

------

go test要求测试函数的前缀必须命名为 Test

------

GO语言中处理错误，一般用哪个接口实现？ error

------

假设源文件的命名为slice.go，则测试文件的命名为 slice_test.go

------

在go语言中，flag是bool型变量，下面if表达式符合编码规范的是（）
A.if flag == 1
B.if flag
C.if flag == false
D.if !flag

> BCD



# 算法

## 括号生成

数字 `n` 代表生成括号的对数，请你设计一个函数，用于能够生成所有可能的并且 **有效的** 括号组合。

示例 1：

输入：n = 3
输出：["((()))","(()())","(())()","()(())","()()()"]
示例 2：

输入：n = 1
输出：["()"]


提示：

1 <= n <= 8

```go
func generateParenthesis(n int) []string {
	if n < 1 || n > 8 {
		return nil
	}
	var res []string
	var dfs func(left, right int, item string)
	dfs = func(left, right int, item string) {
		if left == right && left == n {
			res = append(res, item)
			return
		}
		if right > left || left > n || right > n {
			return
		}
		dfs(left+1, right, item+"(")
		dfs(left, right+1, item+")")
	}

	dfs(0, 0, "")
	return res

}
```

## 供暖器

冬季已经来临。 你的任务是设计一个有固定加热半径的供暖器向所有房屋供暖。

在加热器的加热半径范围内的每个房屋都可以获得供暖。

现在，给出位于一条水平线上的房屋 houses 和供暖器 heaters 的位置，请你找出并返回可以覆盖所有房屋的最小加热半径。

说明：所有供暖器都遵循你的半径标准，加热的半径也一样。

 

示例 1:

输入: houses = [1,2,3], heaters = [2]
输出: 1
解释: 仅在位置2上有一个供暖器。如果我们将加热半径设为1，那么所有房屋就都能得到供暖。
示例 2:

输入: houses = [1,2,3,4], heaters = [1,4]
输出: 1
解释: 在位置1, 4上有两个供暖器。我们需要将加热半径设为1，这样所有房屋就都能得到供暖。
示例 3：

输入：houses = [1,5], heaters = [2]
输出：3

```go
func findRadius(houses []int, heaters []int) int {
	var ans int
	sort.Ints(heaters)
	for _, house := range houses {
		j := sort.SearchInts(heaters, house+1)
		minDis := math.MaxInt32
		if j < len(heaters) {
			minDis = heaters[j] - house
		}
		i := j - 1
		if i >= 0 && house-heaters[i] < minDis {
			minDis = house - heaters[i]
		}
		if minDis > ans {
			ans = minDis
		}
	}
	return ans
}
```

## 数组中的第K个最大元素

难度中等1423收藏分享切换为英文接收动态反馈

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

 ```go
 func findKthLargest(nums []int, k int) int {
 	sort.Slice(nums, func(i, j int) bool {
 		if nums[i] > nums[j] {
 			return true
 		}
 		return false
 	})
 	ret := nums[0]
 	f := 0
 	for _, val := range nums {
 		if val <= ret {
 			f++
 			ret = val
 		}
 		if f >= k {
 			break
 		}
 	}
 	return ret
 }
 ```

