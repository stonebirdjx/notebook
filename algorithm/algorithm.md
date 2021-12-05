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
	line := 0
	var ss []string
	for sanner.Scan() {
		input := sanner.Text()
		if line == 0 {
			inputToint, _ := strconv.Atoi(input)
			line = inputToint
			continue
		}
		ss = append(ss, input)
		l := len(ss)
		if l == line {
			sort.Strings(ss)
			for i := 0; i < l; i++ {
				fmt.Println(ss[i])
			}
			line = 0
			ss = []string{}
		}
	}
}
```

# 15、**HJ15** **求int型正整数在内存中存储时1的个数**

```golang
// 描述
输入一个int型的正整数，计算出该int型数据在内存中存储时1的个数。
// 输入描述：
输入一个整数（int类型）
//输出描述：
这个数转换成2进制后，输出1的个数
// 输入：
5
// 输出：
2

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
		inputToInt, _ := strconv.Atoi(input)
		s := strconv.FormatInt(int64(inputToInt), 2)
		count := 0
		for _, i := range s {
			if i == '1' {
				count++
			}
		}
		fmt.Println(count)
		count = 0
	}
}
```

# 16、**HJ16** **购物单**（待实现）

```golang
// 分为两类：主件与附件，附件是从属于某个主件的，下表就是一些主件与附件的例子：
// 主件	附件
电脑	打印机，扫描仪
书柜	图书
书桌	台灯，文具
工作椅	无
	// 如果要买归类为附件的物品，必须先买该附件所属的主件。
每个主件可以有 0 个、 1 个或 2 个附件。附件不再有从属于自己的附件。王强想买的东西很多，为了不超出预算，他把每件物品规定了一个重要度，分为 5 等：用整数 1 ~ 5 表示，第 5 等最重要。他还从因特网上查到了每件物品的价格（都是 10 元的整数倍）。他希望在不超过 N 元（可以等于 N 元）的前提下，//使每件物品的价格与重要度的乘积的总和最大。
    设第 j 件物品的价格为 v[j] ，重要度为 w[j] ，共选中了 k 件物品，编号依次为 j 1 ， j 2 ，……， j k ，则所求的总和为：
v[j 1 ]*w[j 1 ]+v[j 2 ]*w[j 2 ]+ … +v[j k ]*w[j k ] 。（其中 * 为乘号）
    请你帮助王强设计一个满足要求的购物单。
 
// 输入描述：
输入的第 1 行，为两个正整数，用一个空格隔开：N m
（其中 N （ <32000 ）表示总钱数， m （ <60 ）为希望购买物品的个数。）
	//从第 2 行到第 m+1 行，第 j 行给出了编号为 j-1 的物品的基本数据，每行有 3 个非负整数 v p q
（其中 v 表示该物品的价格（ v<10000 ）， p 表示该物品的重要度（ 1 ~ 5 ）， q 表示该物品是主件还是附件。
	//如果 q=0 ，表示该物品为主件，如果 q>0 ，表示该物品为附件， q 是所属主件的编号）
 
// 输出描述：
 输出文件只有一个正整数，为不超过总钱数的物品的价格与重要度乘积的总和的最大值（ <200000 ）。
// 示例1
// 输入：
1000 5
800 2 0
400 5 1
300 5 1
400 3 0
500 2 0
// 输出：
2200

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
	line := 0
	tm, tn, ret := 0, 0, 0
	var is [][4]int
	for sanner.Scan() {
		input := sanner.Text()
		if line == 0 {
			inputs := strings.Split(input, " ")
			tm, _ = strconv.Atoi(inputs[0])
			tn, _ = strconv.Atoi(inputs[1])
			line++
			break
		}
		inputs := strings.Split(input, " ")
		v, _ := strconv.Atoi(inputs[0])
		p, _ := strconv.Atoi(inputs[1])
		q, _ := strconv.Atoi(inputs[2])
		s := v * p
		tmp := [4]int{v, p, q, s}
		is = append(is, tmp)
		l := len(is)
		if l == tn {
			// 待实现
			fmt.Println(ret)
		}
	}
}
```

# 17、**HJ17** **坐标移动**

```golang
// 描述
开发一个坐标计算工具， A表示向左移动，D表示向右移动，W表示向上移动，S表示向下移动。从（0,0）点开始移动，从输入字符串里面读取一些坐标，并将最终输入结果输出到输出文件里面。
// 输入：
合法坐标为A(或者D或者W或者S) + 数字（两位以内）
坐标之间以;分隔。
非法坐标点需要进行丢弃。如AA10;  A1A;  $%$;  YAD; 等。
下面是一个简单的例子 如：
A10;S20;W10;D30;X;A1A;B10A11;;A10;
//处理过程：
起点（0,0）
+   A10   =  （-10,0）
+   S20   =  (-10,-20)
+   W10  =  (-10,-10)
+   D30  =  (20,-10)
+   x    =  无效
+   A1A   =  无效
+   B10A11   =  无效
+  一个空 不影响
+   A10  =  (10,-10)
结果 （10， -10）
注意请处理多组输入输出
// 输入描述：
一行字符串
// 输出描述：
最终坐标，以逗号分隔
// 输入：
A10;S20;W10;D30;X;A1A;B10A11;;A10;
//输出：
10,-10

// code
package main

import (
	"bufio"
	"fmt"
	"os"
	"regexp"
	"strconv"
	"strings"
)

func main() {
	sanner := bufio.NewScanner(os.Stdin)
	x, y := 0, 0
	for sanner.Scan() {
		input := sanner.Text()
		is := strings.Split(input, ";")
		reg, err := regexp.Compile(`^[A|D|S|W][0-9]{1,2}$`)
		if err != nil {
			continue
		}
		for _, s := range is {
			ss := reg.FindAllString(s, -1)
			if len(ss) != 1 {
				continue
			}
			flag := ss[0]
			f := string(flag[0])
			tmp := ""
			for _, i := range flag[1:] {
				tmp = tmp + string(i)
			}
			num, _ := strconv.Atoi(tmp)
			switch f {
			case "A":
				x = x - num
			case "D":
				x = x + num
			case "S":
				y = y - num
			case "W":
				y = y + num
			}
		}
		fmt.Printf("%d,%d\n", x, y)
		x, y = 0, 0
	}
}

```

# 18、**HJ18** **识别有效的IP地址和掩码并进行分类统计**（待实现）

```golang
// 描述
请解析IP地址和对应的掩码，进行分类识别。要求按照A/B/C/D/E类地址归类，不合法的地址和掩码单独归类。
所有的IP地址划分为 A,B,C,D,E五类
A类地址1.0.0.0~126.255.255.255;
B类地址128.0.0.0~191.255.255.255;
C类地址192.0.0.0~223.255.255.255;
D类地址224.0.0.0~239.255.255.255；
E类地址240.0.0.0~255.255.255.255

// 私网IP范围是：
10.0.0.0～10.255.255.255
172.16.0.0～172.31.255.255
192.168.0.0～192.168.255.255
子网掩码为二进制下前面是连续的1，然后全是0。（例如：255.255.255.32就是一个非法的掩码）
注意二进制下全是1或者全是0均为非法

注意：
1. 类似于【0.*.*.*】和【127.*.*.*】的IP地址不属于上述输入的任意一类，也不属于不合法ip地址，计数时可以忽略
2. 私有IP地址和A,B,C,D,E类地址是不冲突的

输入描述：
多行字符串。每行一个IP地址和掩码，用~隔开。

输出描述：
统计A、B、C、D、E、错误IP地址或错误掩码、私有IP的个数，之间以空格隔开。

示例1
输入：
10.70.44.68~255.254.255.0
1.0.0.1~255.0.0.0
192.168.0.2~255.255.255.0
19..0.~255.255.255.0
复制
输出：
1 0 1 0 0 2 1

```

# 19、**HJ19** **简单错误记录**

```golang
// 描述
开发一个简单错误记录功能小模块，能够记录出错的代码所在的文件名称和行号。
// 处理：
1、 记录最多8条错误记录，循环记录，最后只用输出最后出现的八条错误记录。对相同的错误记录只记录一条，但是错误计数增加。最后一个斜杠后面的带后缀名的部分（保留最后16位）和行号完全匹配的记录才做算是”相同“的错误记录。
2、 超过16个字符的文件名称，只记录文件的最后有效16个字符；
3、 输入的文件可能带路径，记录文件名称不能带路径。
4、循环记录时，只以第一次出现的顺序为准，后面重复的不会更新它的出现时间，仍以第一次为准
// 输入描述：
每组只包含一个测试用例。一个测试用例包含一行或多行字符串。每行包括带路径文件名称，行号，以空格隔开。
// 输出描述：
将所有的记录统计并将结果输出，格式：文件名 代码行数 数目，一个空格隔开，如：

// 输入：
D:\zwtymj\xccb\ljj\cqzlyaszjvlsjmkwoqijggmybr 645
E:\je\rzuwnjvnuz 633
C:\km\tgjwpb\gy\atl 637
F:\weioj\hadd\connsh\rwyfvzsopsuiqjnr 647
E:\ns\mfwj\wqkoki\eez 648
D:\cfmwafhhgeyawnool 649
E:\czt\opwip\osnll\c 637
G:\nt\f 633
F:\fop\ywzqaop 631
F:\yay\jc\ywzqaop 631
// 输出：
rzuwnjvnuz 633 1
atl 637 1
rwyfvzsopsuiqjnr 647 1
eez 648 1
fmwafhhgeyawnool 649 1
c 637 1
f 633 1
ywzqaop 631 2

// code
package main

import (
	"bufio"
	"fmt"
	"os"
	"strings"
	"time"
)

var ss []string
var mp = make(map[string]int)

func main() {
	sanner := bufio.NewScanner(os.Stdin)
	go func() {
		for sanner.Scan() {
			input := sanner.Text()
			p := strings.Split(input, " ")[0]
			pSplit := strings.Split(p,`\`)
			pb := pSplit[len(pSplit)-1]
			l := len(pb)
			if l > 16 {
				pb = pb[l-16:]
			}
			line := strings.Split(input, " ")[1]
			key := pb + " " + line
			if _, ok := mp[key]; ok {
				mp[key]++
			} else {
				mp[key] = 1
				ss = append(ss, key)
			}
		}
	}()
	select {
	case <-time.After(time.Millisecond):
	}
	ls := len(ss)
	if ls > 8 {
		ss = ss[ls-8:]
	}
	for _, key := range ss {
		fmt.Printf("%s %d\n",key,mp[key])
	}
}
```

# 20、HJ20 **密码验证合格程序**

```golang
// 描述
// 密码要求:
1.长度超过8位
2.包括大小写字母.数字.其它符号,以上四种至少三种
3.不能有相同长度大于2的子串重复
// 输入描述：
一组或多组长度超过2的字符串。每组占一行
// 输出描述：
如果符合要求输出：OK，否则输出NG
// 输入：
021Abc9000
021Abc9Abc1
021ABC9000
021$bc9000
// 输出：
OK
NG
NG
OK

// code
package main

import (
	"bufio"
	"fmt"
	"os"
	"regexp"
	"strings"
)

func main() {
	sanner := bufio.NewScanner(os.Stdin)
	for sanner.Scan() {
		input := sanner.Text()
		if len(input) < 9 {
			fmt.Println("NG")
			continue
		}
		ret := reg(input)
		if ret < 3 {
			fmt.Println("NG")
			continue
		}
		if noRepeat(input) {
			fmt.Println("OK")
		}else {
			fmt.Println("NG")
		}
	}
}

func noRepeat(str string) bool {
	c := ""
	for _, i := range str {
		if len(c) < 3 {
			c = c + string(i)
		}
		if len(c)  == 3 {
			if strings.Count(str,c) > 1 {
				return false
			}
			c = string(c[1])+string(c[2])
		}
	}
	return true
}

func reg(str string) int {
	ret := 0
	reg := regexp.MustCompile(`[A-Z]`)
	if reg.FindString(str) != "" {
		ret++
	}
	reg = regexp.MustCompile(`[a-z]`)
	if reg.FindString(str) != "" {
		ret++
	}
	reg = regexp.MustCompile(`[0-9]`)
	if reg.FindString(str) != "" {
		ret++
	}
	reg = regexp.MustCompile(`[^A-Za-z0-9]`)
	if reg.FindString(str) != "" {
		ret++
	}
	return ret
}
```

# 21、**HJ21** **简单密码**

```golang
// 描述
密码是我们生活中非常重要的东东，我们的那么一点不能说的秘密就全靠它了。哇哈哈. 接下来渊子要在密码之上再加一套密码，虽然简单但也安全。
假设渊子原来一个BBS上的密码为zvbo9441987,为了方便记忆，他通过一种算法把这个密码变换成YUANzhi1987，这个密码是他的名字和出生年份，怎么忘都忘不了，而且可以明目张胆地放在显眼的地方而不被别人知道真正的密码。
他是这么变换的，大家都知道手机上的字母： 1--1， abc--2, def--3, ghi--4, jkl--5, mno--6, pqrs--7, tuv--8 wxyz--9, 0--0,就这么简单，渊子把密码中出现的小写字母都变成对应的数字，数字和其他的符号都不做变换，
声明：密码中没有空格，而密码中出现的大写字母则变成小写之后往后移一位，如：X，先变成小写，再往后移一位，不就是y了嘛，简单吧。记住，z往后移是a哦。

// 输入描述：
输入包括多个测试数据。输入是一个明文，密码长度不超过100个字符，输入直到文件结尾

// 输出描述：
输出渊子真正的密文

// 输入：
YUANzhi1987

// 输出：
zvbo9441987

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
		for _, i := range input {
			switch {
			case i >= 'A' && i < 'Z':
				i = i + 33
				ret = ret + string(i)
			case i == 'Z':
				ret = ret + "a"
			case i >= 'a' && i <= 'c':
				ret = ret + "2"
			case i >= 'd' && i <= 'f':
				ret = ret + "3"
			case i >= 'g' && i <= 'i':
				ret = ret + "4"
			case i >= 'j' && i <= 'l':
				ret = ret + "5"
			case i >= 'm' && i <= 'o':
				ret = ret + "6"
			case i >= 'p' && i <= 's':
				ret = ret + "7"
			case i >= 't' && i <= 'v':
				ret = ret + "8"
			case i >= 'w' && i <= 'z':
				ret = ret + "9"
			default:
				ret = ret + string(i)
			}
		}
		fmt.Println(ret)
	}
}
```

# 22、**HJ22** **汽水瓶**

```golang
// 描述
有这样一道智力题：“某商店规定：三个空汽水瓶可以换一瓶汽水。小张手上有十个空汽水瓶，她最多可以换多少瓶汽水喝？”答案是5瓶，方法如下：先用9个空瓶子换3瓶汽水，喝掉3瓶满的，喝完以后4个空瓶子，用3个再换一瓶，喝掉这瓶满的，这时候剩2个空瓶子。然后你让老板先借给你一瓶汽水，喝掉这瓶满的，喝完以后用3个空瓶子换一瓶满的还给老板。如果小张手上有n个空汽水瓶，最多可以换多少瓶汽水喝？
// 输入描述：
输入文件最多包含10组测试数据，每个数据占一行，仅包含一个正整数n（1<=n<=100），表示小张手上的空汽水瓶数。n=0表示输入结束，你的程序不应当处理这一行。

// 输出描述：
对于每组测试数据，输出一行，表示最多可以喝的汽水瓶数。如果一瓶也喝不到，输出0。

// 输入：
3
10
81
0 

// 输出：
1
5
40
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
		i, _ := strconv.Atoi(input)
		if i == 0 {
			continue
		}
		fmt.Println(buy(i))

	}
}

func buy(i int) int {
	ret := i / 3
	s := i%3 + ret
	if s > 2 {
		ret = ret + buy(s)
	}
	if s == 2 {
		ret = ret + 1
	}
	return ret
}
```

# 23、括号

```golang
// 括号。设计一种算法，打印n对括号的所有合法的（例如，开闭一一对应）组合。
说明：解集不能包含重复的子集。
例如，给出 n = 3，生成结果为：
["((()))", "(()())","(())()","()(())","()()()"]

// code
package main

import (
	"bufio"
	"fmt"
	"os"
	"strconv"
)

var ret []string
func main() {
	bs := bufio.NewScanner(os.Stdin)
	for bs.Scan() {
		input := bs.Text()
		i, _ := strconv.Atoi(input)
		ret = []string{}
		dfs(i,i,i,"")
		fmt.Println(ret)
	}
}

func dfs(n, l, r int, group string) {
	if len(group) == n*2 {
		ret = append(ret, group)
		return
	}
	if l > 0 {
		dfs(n, l-1, r, group+"(")
	}
	if l < r && r > 0 {
		dfs(n, l, r-1, group+")")
	}
}
```

# 24、Go实现二叉树(重要)

```golang
// 树本身就具有有序性，所以你不必另外对其进行排序。只要插入到了正确的位置，那么树就能保持有序。
// code
package main

import "fmt"

type tree struct {
	l *tree
	r *tree
	v int
}

func main() {
	t := new(tree)
	for i := 0; i < 50; i++ {
		t = insert(t,i)
	}
	view(t)
}

func insert(t *tree, v int) *tree {
	if t == nil {
		return &tree{nil, nil, v}
	}
	if v == t.v {
		return t
	}
	if v < t.v {
		t.l = insert(t.l, v)
		return t
	}
	t.r = insert(t.r, v)
	return t

}

func view(t *tree) {
	if t == nil {
		return
	}
	view(t.l)
	fmt.Println(t.v)
	view(t.r)
}
```

# 25、Go实现Hash表(重要)

```golang
// 大量数据查找的应用
// code
package main

import "fmt"

type hash struct {
	v    int
	next *hash
}

type hashTable struct {
	table map[int]*hash
	size  int
}

func hashFunc(i, size int) int {
	return (i % size)
}

func insert(ht *hashTable, value int) int {
	index := hashFunc(value, ht.size)
	element := hash{v: value, next: ht.table[index]}
	ht.table[index] = &element
	return index
}

func view(ht *hashTable){
	for k := range ht.table {
		if ht.table[k] != nil {
			t := ht.table[k]
			for t != nil {
				fmt.Printf("%d -> ", t.v)
				t = t.next
			}
		}
		fmt.Println()
	}
}

func main() {
	Size := 15
	table := make(map[int]*hash, Size)
	hash := &hashTable{table: table, size: Size}
	fmt.Println("Numbder of spaces:", hash.size)
	for i := 0; i < 120; i++ {
		insert(hash, i)
	}
	view(hash)
}

```

# 26、Go语言实现链表(重要)

```go
// 链表的优点
链表不仅可以对数据排序，还可以在插入和删除元素之后保持数据的有序性。//如果频繁删除数据，那么相对于哈希表和二叉树，使用链表是一个更好的选择。
package main

import "fmt"

type Node struct {
	Value int
	Next  *Node
}

// 双向链表
//type Node struct {
//    Value    int
//    Previous *Node
//    Next     *Node
//}

var root = new(Node)

func addNode(t *Node, v int) int {
	if root == nil {
		t = &Node{v, nil}
		root = t
		return 0
	}

	if v == t.Value {
		fmt.Println("Node already exists:", v)
		return -1
	}

	if t.Next == nil {
		t.Next = &Node{v, nil}
		return -2
	}

	return addNode(t.Next, v)
}

func lookupNode(t *Node, v int) bool {
	if root == nil {
		t = &Node{v, nil}
		root = t
		return false
	}

	if v == t.Value {
		return true
	}

	if t.Next == nil {
		return false
	}

	return lookupNode(t.Next, v)
}

func size(t *Node) int {
	if t == nil {
		fmt.Println("-> Empty list!")
		return 0
	}

	i := 0
	for t != nil {
		i++
		t = t.Next
	}
	return i
}
func traverse(t *Node) {
	if t == nil {
		fmt.Println("-> Empty list!")
		return
	}

	for t != nil {
		fmt.Printf("%d -> ", t.Value)
		t = t.Next
	}
	fmt.Println()
}
func main() {
	fmt.Println(root)
	root = nil
	traverse(root)
	addNode(root, 1)
	addNode(root, -1)
	traverse(root)
	addNode(root, 10)
	addNode(root, 5)
	addNode(root, 45)
	addNode(root, 5)
	addNode(root, 5)
	traverse(root)
	addNode(root, 100)
	traverse(root)

	if lookupNode(root, 100) {
		fmt.Println("Node exists!")
	} else {
		fmt.Println("Node does not exist!")
	}

	if lookupNode(root, -100) {
		fmt.Println("Node exists!")
	} else {
		fmt.Println("Node does not exist!")
	}
}

```

# 27、Go语音实现队列(重要)

```go
package main

import "fmt"

type Node struct {
	Value int
	Next  *Node
}

var size = 0
var queue = new(Node)

func Push(t *Node, v int) bool {
	if queue == nil {
		queue = &Node{v, nil}
		size++
		return true
	}

	t = &Node{v, nil}
	t.Next = queue
	queue = t
	size++

	return true
}

func Pop(t *Node) (int, bool) {
	if size == 0 {
		return 0, false
	}

	if size == 1 {
		queue = nil
		size--
		return t.Value, true
	}

	temp := t
	for (t.Next) != nil {
		temp = t
		t = t.Next
	}

	v := (temp.Next).Value
	temp.Next = nil

	size--
	return v, true
}

func traverse(t *Node) {
	if size == 0 {
		fmt.Println("Empty Queue!")
		return
	}

	for t != nil {
		fmt.Printf("%d -> ", t.Value)
		t = t.Next
	}
	fmt.Println()
}

func main() {
	queue = nil
	Push(queue, 10)
	fmt.Println("Size:", size)
	traverse(queue)

	v, b := Pop(queue)
	if b {
		fmt.Println("Pop:", v)
	}
	fmt.Println("Size:", size)

	for i := 0; i < 5; i++ {
		Push(queue, i)
	}
	traverse(queue)
	fmt.Println("Size:", size)

	v, b = Pop(queue)
	if b {
		fmt.Println("Pop:", v)
	}
	fmt.Println("Size:", size)

	v, b = Pop(queue)
	if b {
		fmt.Println("Pop:", v)
	}
	fmt.Println("Size:", size)
	traverse(queue)
}
```

# 28、Go语音实现栈(重要)

```go
package main

import "fmt"

type Node struct {
	Value int
	Next  *Node
}

var size = 0
var stack = new(Node)

func Push(v int) bool {
	if stack == nil {
		stack = &Node{v, nil}
		size = 1
		return true
	}

	temp := &Node{v, nil}
	temp.Next = stack
	stack = temp
	size++
	return true
}

func Pop(t *Node) (int, bool) {
	if size == 0 {
		return 0, false
	}

	if size == 1 {
		size = 0
		stack = nil
		return t.Value, true
	}

	stack = stack.Next
	size--
	return t.Value, true
}

func traverse(t *Node) {
	if size == 0 {
		fmt.Println("Empty Stack!")
		return
	}

	for t != nil {
		fmt.Printf("%d -> ", t.Value)
		t = t.Next
	}
	fmt.Println()
}


func main() {
	stack = nil
	v, b := Pop(stack)
	if b {
		fmt.Print(v, " ")
	} else {
		fmt.Println("Pop() failed!")
	}
	Push(100)
	traverse(stack)
	Push(200)
	traverse(stack)

	for i := 0; i < 10; i++ {
		Push(i)
	}

	for i := 0; i < 15; i++ {
		v, b := Pop(stack)
		if b {
			fmt.Print(v, " ")
		} else {
			break
		}
	}
	fmt.Println()
	traverse(stack)
}
```

# 29、 **HJ23** **删除字符串中出现次数最少的字符**

```go
// 描述
实现删除字符串中出现次数最少的字符，若多个字符出现次数一样，则都删除。输出删除这些单词后的字符串，字符串中其它字符保持原来的顺序。
注意每个输入文件有多组输入，即多个字符串用回车隔开
// 输入描述：
字符串只包含小写英文字母, 不考虑非法输入，输入的字符串长度小于等于20个字节。
// 输出描述：
删除字符串中出现次数最少的字符后的字符串。
// 输入：
abcdd
aabcddd

// 输出：
dd
aaddd

// code
package main

import (
	"bufio"
	"fmt"
	"os"
	"sort"
	"strings"
)

func main() {
	bs := bufio.NewScanner(os.Stdin)
	for bs.Scan() {
		input := bs.Text()
		ret := ""
		mp := make(map[string]int)
		for _, i := range input {
			if _, ok := mp[string(i)]; ok {
				continue
			}
			n := strings.Count(input, string(i))
			mp[string(i)] = n
		}
		//tmp
		tmp := make(map[int]string)
		for k, v := range mp {
			if _, ok := tmp[v]; ok {
				tmp[v] = tmp[v] + "|" + k
			} else {
				tmp[v] = k
			}
		}
		// slice 排序
		var s []int
		for k := range tmp {
			s = append(s, k)
		}
		l := len(s)
		if l > 0 {
			sort.Ints(s)
			less := s[0]
			str := tmp[less]
			strs := strings.Split(str, "|")
			for _, i := range input {
				flag := false
				for _, s := range strs {
					if s == string(i) {
						flag = true
						continue
					}
				}
				if flag {
					continue
				}
				ret = ret +string(i)
			}
		}
		fmt.Println(ret)
	}
}
```

# 30、**HJ26** **字符串排序**（待实现）

```golang
// 描述
编写一个程序，将输入字符串中的字符按如下规则排序。
规则 1 ：英文字母从 A 到 Z 排列，不区分大小写。
如，输入： Type 输出： epTy
规则 2 ：同一个英文字母的大小写同时存在时，按照输入顺序排列。
如，输入： BabA 输出： aABb
规则 3 ：非英文字母的其它字符保持原来的位置。
如，输入： By?e 输出： Be?y
注意有多组测试数据，即输入有多行，每一行单独处理（换行符隔开的表示不同行）

// 输入描述：
输入字符串
// 输出描述：
输出字符串
输入：
A Famous Saying: Much Ado About Nothing (2012/8).
输出：
A aaAAbc dFgghh: iimM nNn oooos Sttuuuy (2012/8).

// code


```

# 31、**HJ29** **字符串加解密**

```golang
// 描述
1、对输入的字符串进行加解密，并输出。
2、加密方法为：
当内容是英文字母时则用该英文字母的后一个字母替换，同时字母变换大小写,如字母a时则替换为B；字母Z时则替换为a；
当内容是数字时则把该数字加1，如0替换1，1替换2，9替换0；
其他字符不做变化。
3、解密方法为加密的逆过程。
本题含有多组样例输入。
//输入说明
输入一串要加密的密码
输入一串加过密的密码
// 输出说明
输出加密后的字符
输出解密后的字符
输入：
abcdefg
BCDEFGH
输出：
BCDEFGH
abcdefg


// code
package main

import (
	"bufio"
	"fmt"
	"os"
)

func main() {
	bs := bufio.NewScanner(os.Stdin)
	isAdd := true
	for bs.Scan() {
		input := bs.Text()
		if isAdd {
			ret := add(input)
			fmt.Println(ret)
			isAdd = false
			continue
		}
		ret := down(input)
		fmt.Println(ret)
		isAdd = true
	}
}

func down(str string) string {
	s := ""
	for _, i := range str {
		switch{
		case i > 'a' && i <= 'z':
			s = s + string(i-33)
		case i == 'a':
			s = s + "Z"
		case i > 'A' && i <='Z':
			s = s + string(i+31)
		case i == 'A':
			s = s + "z"

		case i > '0' && i <= '9':
			s = s + string(i-1)
		case i == '0':
			s = s + "9"
		}
	}
	return s
}

func add(str string) string {
	s := ""
	for _, i := range str {
		switch {
		case i >= 'a' && i < 'z':
			s = s + string(i-31)
		case i == 'z':
			s = s + "A"
		case i >= 'A' && i < 'Z':
			s = s + string(i+33)
		case i == 'Z':
			s = s + "a"
		case i >= '0' && i < '9':
			s = s + string(i+1)
		case i == '9':
			s = s + "0"
		}
	}
	return s
}
```

# 31、**HJ108** **求最小公倍数**

```go
// 描述
正整数A和正整数B 的最小公倍数是指 能被A和B整除的最小的正整数值，设计一个算法，求输入A和B的最小公倍数。
// 输入描述：
输入两个正整数A和B。
// 输出描述：
输出A和B的最小公倍数。
// 输入：
5 7
// 输出：
35
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
	bs := bufio.NewScanner(os.Stdin)
	for bs.Scan() {
		input := bs.Text()
		s := strings.Split(input, " ")
		be, _ := strconv.Atoi(s[0])
		af, _ := strconv.Atoi(s[1])
		min := be * af
		for i := 1; i <= be * af; i ++ {
			if i %  be == 0 && i % af == 0  && i < min{
				min = i

			}
		}
		fmt.Println(min)
	}
}
```

# 32、**NC19** **子数组的最大累加和问题**

```go
给定一个数组arr，返回子数组的最大累加和
例如，arr = [1, -2, 3, 5, -2, 6, -1]，所有子数组中，[3, 5, -2, 6]可以累加出最大的和12，所以返回12.
题目保证没有全为负数的数据
// [要求]
时间复杂度为O(n)O(n)，空间复杂度为O(1)O(1)
// 输入：
[1, -2, 3, 5, -2, 6, -1]
// 返回值：
12

// code
package main

import "fmt"

/**
 * max sum of the subarray
 * @param arr int整型一维数组 the array
 * @return int整型
 */

func main() {
	arr := []int{1, -2, 3, 5, -2, 6, -1}
	fmt.Println(maxsumofSubarray(arr))

}

func maxsumofSubarray(arr []int) int {
	// write code here
	dp := make([]int, len(arr))
	ret := arr[0]
	dp[0] = ret
	l := len(arr)
	for i := 1; i < l; i++ {
		if dp[i-1] > 0 {
			dp[i] = dp[i-1] + arr[i]
		} else {
			dp[i] = arr[i]
		}
		ret = max(ret,dp[i])
	}
	return ret
}

func max(a, b int) int {
	if a > b {
		return a
	}
	return b
}
```

# 33、NC59 矩阵的最小路径和

```golang
// 描述
给定一个 n * m 的矩阵 a，从左上角开始每次只能向右或者向下走，最后到达右下角的位置，路径上所有的数字累加起来就是路径和，输出所有的路径中最小的路径和。
示例1
// 输入：
[[1,3,5,9],[8,1,3,4],[5,0,6,1],[8,8,4,0]]
// 返回值：
12

// code
package main

import (
	"fmt"
)

/**
 *
 * @param matrix int整型二维数组 the matrix
 * @return int整型
 */

var res = 0

func main() {
	matrix := [][]int{
		{1, 3, 5, 9},
		{8, 1, 3, 4},
		{5, 0, 6, 1},
		{8, 8, 4, 0},
	}
	fmt.Println(minPathSum(matrix))
}

func minPathSum(matrix [][]int) int {
	// write code here
	dp(0, 0, matrix, 0)
	return res
}

func dp(w, h int, matrix [][]int, ret int) {
	ret = ret + matrix[w][h]
	if w == len(matrix)-1 && h == len(matrix[0])-1 {
		if res == 0 {
			res = ret
		}else if ret < res {
			res = ret
		}
	}
	if w < len(matrix)-1 {
		dp(w+1, h, matrix, ret)
	}
	if h < len(matrix[0])-1 {
		dp(w, h+1, matrix, ret)
	}
}

```

# 34、**HJ31** **单词倒排**

```golang
// 描述
对字符串中的所有单词进行倒排。
说明：
1、构成单词的字符只有26个大写或小写英文字母；
2、非构成单词的字符均视为单词间隔符；
3、要求倒排后的单词间隔符以一个空格表示；如果原字符串中相邻单词间有多个间隔符时，倒排转换后也只允许出现一个空格间隔符；
4、每个单词最长20个字母；

// 输入描述：
输入一行以空格来分隔的句子
// 输出描述：
输出句子的逆序

// 输入：
I am a student
// 输出：
student a am I

 // code
package main

import (
	"bufio"
	"fmt"
	"os"
	"strings"
)

func main() {
	bs := bufio.NewScanner(os.Stdin)
	for bs.Scan() {
		input := bs.Text()
		ret := ""
		for _, i := range input {
			switch {
			case i >= 'A' && i <= 'Z':
				ret = ret + string(i)
			case i >= 'a' && i <= 'z':
				ret = ret + string(i)
			default:
				ret = ret + "|"
			}
		}
		rets := strings.Split(ret, "|")
		l := len(rets)
		res := ""
		for i := l - 1; i >= 0; i-- {
			if rets[i] == "" {
				continue
			} else {
				if res == "" {
					res = rets[i]
				} else {
					res = res + " " + rets[i]
				}
			}

		}
		fmt.Println(res)
	}
}
```

# 35、**HJ81** **字符串字符匹配**

```golang
// 描述
判断短字符串中的所有字符是否在长字符串中全部出现。
请注意本题有多组样例输入。

// 输入描述：
输入两个字符串。第一个为短字符串，第二个为长字符串。两个字符串均由小写字母组成。

// 输出描述：
如果短字符串的所有字符均在长字符串中出现过，则输出true。否则输出false。

// 输入：
bc
abc

?// 输出：
true

// code
package main

import (
	"bufio"
	"fmt"
	"os"
)

func main() {
	bs := bufio.NewScanner(os.Stdin)
	before := ""
	after := ""
	for bs.Scan() {
		input := bs.Text()
		if before == "" {
			before = input
			continue
		}
		after = input
		for _, i := range before {
			flag := false
			for _, j := range after {
				if i == j {
					flag = true
					break
				}
			}
			if !flag {
				fmt.Println(false)
				goto loop
			}
		}
		fmt.Println(true)
	loop:
		before = ""
		after = ""
	}
}
```

# 36 、**HJ85** **最长回文子串**

```golang
// 描述
给定一个仅包含小写字母的字符串，求它的最长回文子串的长度。
所谓回文串，指左右对称的字符串。
所谓子串，指一个字符串删掉其部分前缀和后缀（也可以不删）的字符串
（注意：记得加上while处理多个测试用例）

// 输入描述：
输入一个仅包含小写字母的字符串
// 输出描述：
返回最长回文子串的长度

//输入：
cdabbacc
// 输出：
4
// 说明：
abba为最长的回文子串 

// code
package main

import (
	"bufio"
	"fmt"
	"os"
)

func main() {
	bs := bufio.NewScanner(os.Stdin)
	for bs.Scan() {
		input := bs.Text()
		if len(input) < 2 {
			fmt.Println(len(input))
			continue
		}
		length := len(input)

		dp := make([][]bool, length)
		for i := 0; i < length; i++ {
			dp[i] = make([]bool, length)
		}
		maxGap := 0
		var ans string
		//子串的长度（从0到length不断扩张）
		for gap := 0; gap < length; gap++ {
			//子串开始位置，从第一个位置开始滑动窗口比较
			for start := 0; start + gap < length; start++ {
				//子串结束位置
				end := start + gap
				flag := false
				if gap == 0 {
					//子串长度为1，即单个字符肯定为回文子串
					dp[start][end] = true
					flag = true
				} else if gap == 1 {
					//子串长度为2，只需比较两个字符是否相等即可，相等为回文子串
					if input[start] == input[end] {
						dp[start][end] = true
						flag = true
					}
				} else {
					//子串长度>2，则利用状态转移方程求解
					if dp[start+1][end-1] && input[start] == input[end] {
						dp[start][end] = true
						flag = true
					}
				}
				if flag {
					//记录最大距离
					if end - start + 1 > maxGap {
						maxGap = start - end + 1
						ans = input[start:end+1]
					}
				}
			}
		}
		fmt.Println(len(ans))
	}
}
```

# 37、**HJ86** **求最大连续bit数**

```go
// 描述
求一个byte数字对应的二进制数字中1的最大连续数，例如3的二进制为00000011，最大连续2个1
本题含有多组样例输入。
// 输入描述：
输入一个byte数字
// 输出描述：
输出转成二进制之后连续1的个数
示例1
// 输入：
3
5
// 输出：
2
1
// 说明：
3的二进制表示是11，最多有2个连续的1。
5的二进制表示是101，最多只有1个连续的1。 
// code
package main

import (
	"bufio"
	"fmt"
	"os"
	"strconv"
)

func main() {
	bs := bufio.NewScanner(os.Stdin)
	for bs.Scan() {
		input := bs.Text()
		i, _ := strconv.ParseInt(input, 10, 64)
		s := strconv.FormatInt(i, 2)
		ret := 0
		tmp := 0
		for _, b := range s {
			if b == '1' {
				tmp++
			}
			if b == '0' {
				if ret < tmp {
					ret = tmp
				}
				tmp = 0
			}
		}
		if ret < tmp {
			ret = tmp
		}
		tmp = 0
		fmt.Println(ret)
	}
}
```

 # 38、时效缓存

```golang
// 实现一个时效缓存系统
// 存入时：
//		当key 值存在时，直接更新value
//		当key不存在时，容量未满时直接存在
//		当key不存在时，容量满时删除冷门数据再存入

// 取出时：
// key存在直接取，key不存在返回-1
package main

import "fmt"

type value struct {
	val int
	hot int
}

type cache struct {
	capacity int
	mp       map[string]*value
}

func main() {
	c := newCache(2)
	c.put("1",1)
	c.put("1",0)
	c.put("2",2)
	c.put("3",3)
	fmt.Println(c.get("1"))
	fmt.Println(c.get("2"))
	fmt.Println(c.get("3"))
}

func (c *cache) get(key string) int {
	if _, ok := c.mp[key]; ok {
		c.mp[key].hot++
		return c.mp[key].val
	}
	return -1
}

func (c *cache) put(key string, val int) {
	// key 在map 里，直接更新val ，hot++
	if _, ok := c.mp[key]; ok {
		c.mp[key].val = val
		c.mp[key].hot++
		return
	}
	if len(c.mp) < c.capacity {
		// 容量未满时，直接插入
		v := new(value)
		v.val = val
		c.mp[key] = v
	} else {
		// 容量满时
		tmpKey := ""
		tmpHot := -1
		for k, c := range c.mp {
			if tmpHot == -1 && tmpKey == "" {
				tmpKey = k
				tmpHot = c.hot
				continue
			}
			if c.hot < tmpHot {
				tmpKey = k
				tmpHot = c.hot
			}
		}
		delete(c.mp, tmpKey)
		v := new(value)
		v.val = val
		c.mp[key] = v
	}
}

func newCache(i int) *cache {
	c := new(cache)
	mp := make(map[string]*value)
	c.capacity = i
	c.mp = mp
	return c
}
```

# 39、括号匹配

```golang
package main

import "fmt"

/*
给定一个字符串，里边可能包含“()”, “[]”, “{}”括号，请编写程序检查该字符串的括号是否闭合。例如，
1.	"()" yes；
2.	")(" no;
3.	"(()" no;
4.	"{}()" yes;
5.	"{()}" yes;
6.	"{(})" no.
*/

func main() {
	s := "{()}"
	var tmp []string
	for _, i := range s {
		switch {
		case i == '{' || i == '[' || i == '(':
			tmp = append(tmp, string(i))
		case i == '}':
			l := len(tmp)
			if l == 0 {
				fmt.Println("no")
				return
			}
			if tmp[l-1] != "{" {
				fmt.Println("no")
				return
			}
			tmp = tmp[:l-1]
		case i == ']':
			l := len(tmp)
			if l == 0 {
				fmt.Println("no")
				return
			}
			if tmp[l-1] != "[" {
				fmt.Println("no")
				return
			}
			tmp = tmp[:l-1]
		case i == ')':
			l := len(tmp)
			if l == 0 {
				fmt.Println("no")
				return
			}
			if tmp[l-1] != "(" {
				fmt.Println("no")
				return
			}
			tmp = tmp[:l-1]
		}
	}
	fmt.Println("yes")
}
```

# 40、挑7

```golang
// 描述
输出7有关数字的个数，包括7的倍数，还有包含7的数字（如17，27，37...70，71，72，73...）的个数（一组测试用例里可能有多组数据，请注意处理）

// 输入描述：
一个正整数N。(N不大于30000)

// 输出描述：
不大于N的与7有关的数字个数，例如输入20，与7有关的数字包括7,14,17.

输入：
20
10
输出：
3
1
// code
package main

import (
	"bufio"
	"fmt"
	"os"
	"strconv"
)

func main() {
	bs := bufio.NewScanner(os.Stdin)
	for bs.Scan() {
		input := bs.Text()
		ret := 0
		inputInt, _ := strconv.Atoi(input)
		for i := 1; i <= inputInt; i++ {
			if i%7 == 0 {
				ret++
			} else {
				str := strconv.Itoa(i)
				loop:for _,stri := range str{
					switch stri {
					case '7' :
						ret++
						break loop
					}
				}
			}
		}
		fmt.Println(ret)
	}
}
```

# 41、高精度加法

```golang
// 描述
输入两个用字符串表示的整数，求它们所表示的数之和。
字符串的长度不超过10000。
本题含有多组样例输入。

// 输入描述：
输入两个字符串。保证字符串只含有'0'~'9'字符

// 输出描述：
输出求和后的结果

示例1
输入：
9876543210
1234567890
输出：
11111111100

// code
package main

import (
	"bufio"
	"fmt"
	"math/big"
	"os"
)

func main() {
	bs := bufio.NewScanner(os.Stdin)
	flag := true
	l1 := new(big.Int)
	l2 := new(big.Int)
	for bs.Scan() {
		input := bs.Text()
		if flag {
			l1.SetString(input,10)
			flag = false
			continue
		}
		l2.SetString(input,10)
		s := l1.Add(l1,l2)
		fmt.Println(s.String())
		flag = true
		l1 = new(big.Int)
		l2 = new(big.Int)
	}
}s
```









