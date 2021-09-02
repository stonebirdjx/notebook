# 1、**HJ1** **字符串最后一个单词的长度**

```golang
// 描述
计算字符串最后一个单词的长度，单词以空格隔开，字符串长度小于5000。
// 输入描述：
输入一行，代表要计算的字符串，非空，长度小于5000
// 输出描述
输出一个整数，表示输入字符串最后一个单词的长度。
// 输入：
hello nowcoder
// 输出：
8
// 说明：
最后一个单词为nowcoder，长度为8

// code
package main

import (
	"bufio"
	"fmt"
	"os"
)

func main() {
	sanner := bufio.NewScanner(os.Stdin)
	input := ""
	ret := 0
	for sanner.Scan() {
		input = sanner.Text()
		break
	}

	strLength := len(input)
	for i := strLength - 1; i >= 0; i-- {
		if input[i] == 32 {
			break
		}
		ret++
	}
	fmt.Println(ret)
}


```

# 2、**HJ1** **字符串最后一个单词的长度**

```golang
//描述
写出一个程序，接受一个由字母、数字和空格组成的字符串，和一个字母，然后输出输入字符串中该字母的出现次数。不区分大小写，字符串长度小于500。
//输入描述：
第一行输入一个由字母和数字以及空格组成的字符串，第二行输入一个字母。
//输出描述：
输出输入字符串中含有该字符的个数。

// 示例1
// 输入：
ABCabc
A
//输出：
2

// code
package main

import (
	"bufio"
	"fmt"
	"os"
)

func main() {
	sanner := bufio.NewScanner(os.Stdin)
	input := ""
	flag := ""
	times := 0
	ret := 0
	for sanner.Scan() {
		if times == 0 {
			input = sanner.Text()
		} else if times == 1 {
			flag = sanner.Text()
			break
		}
		times++
	}
	flagByte := []byte(flag)[0]
	inputByte := []byte(input)
	switch {
	case flagByte >= 'A' && flagByte <= 'Z':
		for _, s := range inputByte {
			if s == flagByte {
				ret++
			}
			if s == flagByte +32{
				ret++
			}
		}

	case flagByte >= 'a' && flagByte <= 'z':
		for _, s := range inputByte {
			if s == flagByte {
				ret++
			}
			if s == flagByte-32{
				ret++
			}
		}
	case (flagByte >= '0' && flagByte <= '9') || flagByte == ' ':
		for _, s := range inputByte {
			if s == flagByte {
				ret++
			}
		}
	}
	fmt.Println(ret)
}
```

# 3、**HJ3** **明明的随机数**

```golang
//描述
明明想在学校中请一些同学一起做一项问卷调查，为了实验的客观性，他先用计算机生成了N个1到1000之间的随机整数（N≤1000），对于其中重复的数字，只保留一个，把其余相同的数去掉，不同的数对应着不同的学生的学号。然后再把这些数从小到大排序，按照排好的顺序去找同学做调查。请你协助明明完成“去重”与“排序”的工作(同一个测试用例里可能会有多组数据(用于不同的调查)，希望大家能正确处理)。
//注：测试用例保证输入参数的正确性，答题者无需验证。测试用例不止一组。
//当没有新的输入时，说明输入结束。
//输入描述：
注意：输入可能有多组数据(用于不同的调查)。每组数据都包括多行，第一行先输入随机整数的个数N，接下来的N行再输入相应个数的整数。具体格式请看下面的"示例"。

//输出描述：
返回多行，处理后的结果
//输入：
3
2
2
1
11
10
20
40
32
67
40
20
89
300
400
15
//输出：
1
2
10
15
20
32
40
67
89
300
400
//说明：
//输入解释：
第一个数字是3，也即这个小样例的N=3，说明用计算机生成了3个1到1000之间的随机整数，接下来每行一个随机数字，共3行，也即这3个随机数字为：
2
1
1
所以第一个小样例的输出为：
1
2
第二个小样例的第一个数字为11，也即...(类似上面的解释)...
所以第二个小样例的输出为：
10
15
20
32
40
67
89
300
400
所以示例1包含了两个小样例！！  

// code
package main

import (
	"bufio"
	"fmt"
	"os"
	"sort"
	"strconv"
)

func main() {
	sanner := bufio.NewScanner(os.Stdin)
	flag := 0
	var is []int
	for sanner.Scan() {
		input := sanner.Text()
		i, _ := strconv.Atoi(input)
		if flag == 0 {
			flag = i
		} else {
			is = append(is, i)
			if len(is) == flag {
				sort.Ints(is)
				mp := make(map[int]string)
				for _,key := range is{
					if _, ok := mp[key]; !ok {
						fmt.Println(key)
						mp[key] = "stb"
					}
				}
				flag = 0
				is = []int{}
				mp = map[int]string{}
			}
		}
	}
}
```

# 4、**HJ4** **字符串分隔**

```golang
// 描述
•连续输入字符串，请按长度为8拆分每个字符串后输出到新的字符串数组；
•长度不是8整数倍的字符串请在后面补数字0，空字符串不处理。
// 输入描述：
连续输入字符串(输入多次,每个字符串长度小于100)
// 输出描述：
输出到长度为8的新字符串数组
// 输入：
abc
123456789
// 输出：
abc00000
12345678
90000000

// code
package main

import (
	"bufio"
	"fmt"
	"os"
)

func main() {
	sanner := bufio.NewScanner(os.Stdin)
	for sanner.Scan() {
		input := sanner.Text()
		ret := ""
		for _, s := range input {
			ret = ret + string(s)
			if len(ret) == 8 {
				fmt.Println(ret)
				ret = ""
			}
		}
		if ret != "" {
			b := 8 - len(ret)
			for i := 0; i < b; i++ {
				ret = ret + "0"
			}
			fmt.Println(ret)
		}
	}
}
```

# 5、**HJ5** **进制转换**

```golang
// 描述
写出一个程序，接受一个十六进制的数，输出该数值的十进制表示。
//输入描述：
输入一个十六进制的数值字符串。注意：一个用例会同时有多组输入数据，请参考帖子https://www.nowcoder.com/discuss/276处理多组输入的问题。

//输出描述：
输出该数值的十进制字符串。不同组的测试用例用\n隔开。
//输入：
0xA
0xAA
//输出：
10
170
// code
package main

import (
	"bufio"
	"fmt"
	"os"
	"strconv"
)

func main() {
	sanner := bufio.NewScanner(os.Stdin)
	for sanner.Scan() {
		input := sanner.Text()
		input = input[2:]
		i, _ := strconv.ParseUint(input, 16, 64)
		fmt.Println(i)
	}
}

```

# 6、**HJ6** **质数因子**

```golang
// 描述
功能:输入一个正整数，按照从小到大的顺序输出它的所有质因子（重复的也要列举）（如180的质因子为2 2 3 3 5 ）
最后一个数后面也要有空格
// 输入描述：
输入一个long型整数
// 输出描述：
按照从小到大的顺序输出它的所有质数的因子，以空格隔开。最后一个数后面也要有空格。

// 输入：
180
// 输出：
2 2 3 3 5

// code
package main

import (
	"bufio"
	"fmt"
	"os"
	"strconv"
)

func main() {
	sanner := bufio.NewScanner(os.Stdin)
	for sanner.Scan() {
		input := sanner.Text()
		toInt, _ := strconv.Atoi(input)
		ret := ""
		for i := 2; i*i <= toInt; i++ {
			for toInt%i == 0 {
				toInt = toInt / i
				r := strconv.Itoa(i)
				ret = ret + r + " "
			}
		}
		if toInt > 1 {
			r := strconv.Itoa(toInt)
			ret = ret + r + " "
		}
		fmt.Println(ret)
	}
}
```

# 7、**HJ7** **取近似值**

```golang
// 描述
写出一个程序，接受一个正浮点数值，输出该数值的近似整数值。如果小数点后数值大于等于5,向上取整；小于5，则向下取整。
// 输入描述：
输入一个正浮点数值
// 输出描述：
输出该数值的近似整数值

// 输入：
5.5
// 输出：
6
// code
package main

import (
	"bufio"
	"fmt"
	"os"
	"strconv"
	"strings"
)

func main() {
	sanner := bufio.NewScanner(os.Stdin)
	for sanner.Scan() {
		input := sanner.Text()
		z := strings.Split(input,".")[0]
		x := strings.Split(input,".")[1]
		zi,_ := strconv.Atoi(z)
		x = string(x[0])
		x0i,_ := strconv.Atoi(x)
		if x0i >= 5 {
			zi++
		}
		fmt.Println(zi)
	}
}
```

# 8、**HJ8** **合并表记录**

```golang
// 描述
数据表记录包含表索引和数值（int范围的正整数），请对表索引相同的记录进行合并，即将相同索引的数值进行求和运算，输出按照key值升序进行输出。
// 输入描述：
先输入键值对的个数
然后输入成对的index和value值，以空格隔开
// 输出描述：
输出合并后的键值对（多行）

//输入：
4
0 1
0 2
1 2
3 4

//输出：
0 3
1 2
3 4

// code
package main

import (
	"bufio"
	"fmt"
	"os"
	"sort"
	"strconv"
	"strings"
)

func main() {
	sanner := bufio.NewScanner(os.Stdin)
	line := 0
	flag := 0
	mp := make(map[int]int)
	var is []int
	for sanner.Scan() {
		if line == 0 {
			input := sanner.Text()
			inputToint, _ := strconv.Atoi(input)
			line = inputToint
			continue
		}
		input := sanner.Text()
		key := strings.Split(input, " ")[0]
		keyToint, _ := strconv.Atoi(key)
		value := strings.Split(input, " ")[1]
		valueToint, _ := strconv.Atoi(value)
		if _, ok := mp[keyToint]; ok {
			mp[keyToint] = mp[keyToint] + valueToint
		} else {
			mp[keyToint] = valueToint
		}
		flag++
		if flag == line {
			for k := range mp {
				is = append(is, k)
			}
			sort.Ints(is)
			for _, i := range is {
				fmt.Println(i, mp[i])
			}
			line = 0
			flag = 0
			mp = map[int]int{}
			is = []int{}
		}
	}
}
```

# 9、**HJ9** **提取不重复的整数**

```golang
// 描述
输入一个int型整数，按照从右向左的阅读顺序，返回一个不含重复数字的新的整数。
保证输入的整数最后一位不是0。
// 输入描述：
输入一个int型整数
// 输出描述：
按照从右向左的阅读顺序，返回一个不含重复数字的新的整数

// 示例1
// 输入：
9876673
// 输出：
37689

// code
package main

import (
	"bufio"
	"fmt"
	"os"
)

func main() {
	sanner := bufio.NewScanner(os.Stdin)
	for sanner.Scan() {
		input := sanner.Text()
		ret := ""
		mp := make(map[string]string)
		l := len(input)
		for i := l - 1; i >= 0; i-- {
			if _, ok := mp[string(input[i])]; ok {
				continue
			} else {
				ret = ret + string(input[i])
				mp[string(input[i])] = "stb"
			}
		}
		fmt.Println(ret)
	}
}

```

# 10、**HJ10** **字符个数统计**

```golang
// 描述
编写一个函数，计算字符串中含有的不同字符的个数。字符在ASCII码范围内(0~127，包括0和127)，换行表示结束符，不算在字符里。不在范围内的不作统计。多个相同的字符只计算一次
例如，对于字符串abaca而言，有a、b、c三种不同的字符，因此输出3。
// 输入描述：
输入一行没有空格的字符串。

// 输出描述：
输出 输入字符串 中范围在(0~127，包括0和127)字符的种数。
示例1
// 输入：
abc
// 输出：
3

// code
package main

import (
	"bufio"
	"fmt"
	"os"
)

func main() {
	sanner := bufio.NewScanner(os.Stdin)
	for sanner.Scan() {
		input := sanner.Text()
		mp := make(map[string]string)
		for _, s := range input {
			if _, ok := mp[string(s)]; ok {
				continue
			} else {
				mp[string(s)] = "stb"
			}
		}
		fmt.Println(len(mp))

	}
}
```

# 11、**HJ11** **数字颠倒**

```golang
// 描述
输入一个整数，将这个整数以字符串的形式逆序输出
程序不考虑负数的情况，若数字含有0，则逆序形式也含有0，如输入为100，则输出为001
// 输入描述：
输入一个int整数
// 输出描述：
将这个整数以字符串的形式逆序输出

// 输入：
1516000
// 输出：
0006151

// code
package main

import (
	"bufio"
	"fmt"
	"os"
)

func main() {
	sanner := bufio.NewScanner(os.Stdin)
	for sanner.Scan() {
		input := sanner.Text()
		l := len(input)
		ret := ""
		for i := l - 1; i >= 0; i-- {
			ret = ret + string(input[i])
		}
		fmt.Println(ret)
	}
}

```

# 12、**HJ12** **字符串反转**

```golang
// 描述
接受一个只包含小写字母的字符串，然后输出该字符串反转后的字符串。（字符串长度不超过1000）

// 输入描述：
输入一行，为一个只包含小写字母的字符串。

// 输出描述：
输出该字符串反转后的字符串。

// 输入：
abcd
// 输出：
dcba

// code 
package main

import (
	"bufio"
	"fmt"
	"os"
)

func main() {
	sanner := bufio.NewScanner(os.Stdin)
	for sanner.Scan() {
		input := sanner.Text()
		l := len(input)
		ret := ""
		for i := l - 1; i >= 0; i-- {
			ret = ret + string(input[i])
		}
		fmt.Println(ret)
	}
}
```

# 13 、**HJ13** **句子逆序**

```golang
// 描述
将一个英文语句以单词为单位逆序排放。例如“I am a boy”，逆序排放后为“boy a am I”
所有单词之间用一个空格隔开，语句中除了英文字母外，不再包含其他字符

// 输入描述：
输入一个英文语句，每个单词用空格隔开。保证输入只包含空格和字母。

// 输出描述：
得到逆序的句子

// 输入：
I am a boy
// 输出：
boy a am I

// code
package main

import (
	"bufio"
	"fmt"
	"os"
	"strings"
)

func main() {
	sanner := bufio.NewScanner(os.Stdin)
	for sanner.Scan() {
		input := sanner.Text()
		s := strings.Split(input, " ")
		l := len(s)
		ret := ""
		for i := l - 1; i >= 0; i-- {
			if i == 0{
				ret = ret + s[i]
			}else {
				ret = ret + s[i]+" "
			}
		}
		fmt.Println(ret)
	}
}
```

# 14、**HJ14** **字符串排序**

```golang
// 描述
给定n个字符串，请对n个字符串按照字典序排列。
// 输入描述：
输入第一行为一个正整数n(1≤n≤1000),下面n行为n个字符串(字符串长度≤100),字符串中只含有大小写字母。
// 输出描述：
数据输出n行，输出结果为按照字典序排列的字符串。

//输入：
9
cap
to
cat
card
two
too
up
boat
boot

//输出：
boat
boot
cap
card
cat
to
too
two
up

```





