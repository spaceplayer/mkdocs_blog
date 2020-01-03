## 知识点

### [算法] DFS原理及模板 stack

判出口（终点、越界）->剪枝->扩展->标记->递归->还原

```c++

void dfs(int 当前状态)
	{
	      if(当前状态为边界状态)
	      {
	        记录或输出
	        return;
	      }
	      for(i=0;i<n;i++)		//横向遍历解答树所有子节点
	     {
	           //扩展出一个子状态。
	           修改了全局变量
	           if(子状态满足约束条件)
	            {
	              dfs(子状态)
	           }
	            恢复全局变量//回溯部分
	        }
void dfs(...) 
{
    // 结束递归的条件
    if (...) {
        ..... // 把“当前结果” 加入 “结果集容器” 中
        return;
    }

    // 继续递归，里面可能有回溯，也可能没有
    if (...) {

        ... // 在容器中保存当前数据
        dfs() 
        ... // 在容器中删除上面保存的数据（注：这种情况下就称为回溯，很明显它是dfs的一个步骤）
    }
}
void dfs()//参数用来表示状态  
{  
    if(到达终点状态)  
    {  
        ...//根据题意添加  
        return;  
    }  
    if(越界或者是不合法状态)  
        return;  
    if(特殊状态)//剪枝
        return ;
    for(扩展方式)  
    {  
        if(扩展方式所达到状态合法)  
        {  
            修改操作;//根据题意来添加  
            标记；  
            dfs（）；  
            (还原标记)；  
            //是否还原标记根据题意  
            //如果加上（还原标记）就是 回溯法  
        }  

    }  
}  



```

### [算法] BFS原理及模板 queue

### [算法] 回溯

![image-20190303125851748](/Users/zhengxinzhi/typora_img/LeetCode题目总结/image-20190303125851748.png)



**关于回溯时间的思考，由于回溯多次调用递归，导致我曾经以为很多函数并行执行，其实仍是单核执行，时间复杂度很高**

### [算法] 计数排序

当输入的元素是![n](https://wikimedia.org/api/rest_v1/media/math/render/svg/a601995d55609f2d9f5e233e36fbe9ea26011b3b)个![{\displaystyle  0 }](https://wikimedia.org/api/rest_v1/media/math/render/svg/2aae8864a3c1fec9585261791a809ddec1489950)到![ k ](https://wikimedia.org/api/rest_v1/media/math/render/svg/c3c9a2c7b599b37105512c5d570edc034056dd40)之间的整数时，它的运行时间是![{\displaystyle \Theta (n+k)}](https://wikimedia.org/api/rest_v1/media/math/render/svg/9048502a7c90cb3c2896b72bbebca46233a02408)。计数排序不是[比较排序](https://www.wikiwand.com/zh-hans/%E6%AF%94%E8%BE%83%E6%8E%92%E5%BA%8F)，排序的速度快于任何比较排序算法.

1. 找出待排序的数组中最大和最小的元素
2. 统计数组中每个值为![i](https://wikimedia.org/api/rest_v1/media/math/render/svg/add78d8608ad86e54951b8c8bd6c8d8416533d20)的元素出现的次数，存入数组![ C ](https://wikimedia.org/api/rest_v1/media/math/render/svg/4fc55753007cd3c18576f7933f6f089196732029)的第![i](https://wikimedia.org/api/rest_v1/media/math/render/svg/add78d8608ad86e54951b8c8bd6c8d8416533d20)项
3. 对所有的计数累加（从![ C ](https://wikimedia.org/api/rest_v1/media/math/render/svg/4fc55753007cd3c18576f7933f6f089196732029)中的第一个元素开始，每一项和前一项相加）
4. 反向填充目标数组：将每个元素![i](https://wikimedia.org/api/rest_v1/media/math/render/svg/add78d8608ad86e54951b8c8bd6c8d8416533d20)放在新数组的第![{\displaystyle C[i]}](https://wikimedia.org/api/rest_v1/media/math/render/svg/d9e670bd48c5b92b5228727d2a35097581cc85a6)项，每放一个元素就将![{\displaystyle C[i]}](https://wikimedia.org/api/rest_v1/media/math/render/svg/d9e670bd48c5b92b5228727d2a35097581cc85a6)减去1

```java
	public static int[] countSort(int []a){
		int b[] = new int[a.length];
		int max = a[0], min = a[0];
		for(int i : a){
			if(i > max){
				max = i;
			}
			if(i < min){
				min = i;
			}
		}
		//这里k的大小是要排序的数组中，元素大小的极值差+1
		int k = max - min + 1;
		int c[] = new int[k];
		for(int i = 0; i < a.length; ++i){
			c[a[i]-min] += 1;//优化过的地方，减小了数组c的大小
		}
		for(int i = 1; i < c.length; ++i){
			c[i] = c[i] + c[i-1];
		}
		for(int i = a.length-1; i >= 0; --i){
			b[--c[a[i]-min]] = a[i];//按存取的方式取出c的元素
		}
		return b;
	}
```

### [算法] 排序排序

```python
# This function takes last element as pivot, places 
# the pivot element at its correct position in sorted 
# array, and places all smaller (smaller than pivot) 
# to left of pivot and all greater elements to right 
# of pivot 
def partition(arr,low,high): 
    i = ( low-1 )         # index of smaller element 
    pivot = arr[high]     # pivot 
    for j in range(low , high): 
        # If current element is smaller than or 
        # equal to pivot 
        if   arr[j] <= pivot: 
            # increment index of smaller element 
            i = i+1 
            arr[i],arr[j] = arr[j],arr[i]
    arr[i+1],arr[high] = arr[high],arr[i+1] 
    return ( i+1 ) 

# key: 降低最坏情况出现的概率
def partition_r(arr[], low, hig)
    r = Random Number from low to high
    Swap arr[r] and arr[hi]
    return partition(arr, low, high)

# The main function that implements QuickSort 
# arr[] --> Array to be sorted, 
# low  --> Starting index, 
# high  --> Ending index 
  
# Function to do Quick sort 
def quickSort(arr,low,high): 
    if low < high:
        # pi is partitioning index, arr[p] is now 
        # at right place 
        pi = partition(arr,low,high) 
        # Separately sort elements before 
        # partition and after partition 
        quickSort(arr, low, pi-1) 
        quickSort(arr, pi+1, high) 
```

### [算法] 最大团问题 todo

![image-20190313154630103](/Users/zhengxinzhi/typora_img/LeetCode题目总结/image-20190313154630103.png)

### [DATA STRUCTURE] 二叉搜索树

**二叉搜索树**、**有序二叉树**（ordered binary tree）或**排序二叉树**

二叉查找树相比于其他数据结构的优势在于查找、插入的时间复杂度较低。为![O(\log n)](https://wikimedia.org/api/rest_v1/media/math/render/svg/aae0f22048ba6b7c05dbae17b056bfa16e21807d)。二叉查找树是基础性数据结构，用于构建更为抽象的数据结构，如[集合](https://www.wikiwand.com/zh-hans/%E9%9B%86%E5%90%88_(%E8%AE%A1%E7%AE%97%E6%9C%BA%E7%A7%91%E5%AD%A6))、[多重集](https://www.wikiwand.com/zh-hans/%E5%A4%9A%E9%87%8D%E9%9B%86)、[关联数组](https://www.wikiwand.com/zh-hans/%E5%85%B3%E8%81%94%E6%95%B0%E7%BB%84)等。

虽然二叉查找树的最坏效率是![O(n)](https://wikimedia.org/api/rest_v1/media/math/render/svg/34109fe397fdcff370079185bfdb65826cb5565a)，但它支持动态查询，且有很多改进版的二叉查找树可以使树高为![O(\log n)](https://wikimedia.org/api/rest_v1/media/math/render/svg/aae0f22048ba6b7c05dbae17b056bfa16e21807d)，从而将最坏效率降至![O(\log n)](https://wikimedia.org/api/rest_v1/media/math/render/svg/aae0f22048ba6b7c05dbae17b056bfa16e21807d)，如[AVL树](https://www.wikiwand.com/zh-hans/AVL%E6%A0%91)、[红黑树](https://www.wikiwand.com/zh-hans/%E7%BA%A2%E9%BB%91%E6%A0%91)等。

1. 若任意节点的左子树不空，则左子树上所有节点的值均小于它的根节点的值；
2. 若任意节点的右子树不空，则右子树上所有节点的值均大于它的根节点的值；
3. 任意节点的左、右子树也分别为二叉查找树；
4. 没有键值相等的节点。

### [DATA STRUCTURE] 前序遍历 中序遍历 后序遍历

有两种通用的遍历树的策略：

宽度优先搜索（BFS）

我们按照高度顺序一层一层的访问整棵树，高层次的节点将会比低层次的节点先被访问到。

深度优先搜索（DFS）

在这个策略中，我们采用深度作为优先级，以便从跟开始一直到达某个确定的叶子，然后再返回根到达另一个分支。

深度优先搜索策略又可以根据根节点、左孩子和右孩子的相对顺序被细分为前序遍历，中序遍历和后序遍历。

下图中的顶点按照访问的顺序编号，按照 1-2-3-4-5 的顺序来比较不同的策略。

![102.png](/Users/zhengxinzhi/typora_img/LeetCode题目总结/b61ff2d47852e4264f5dfe0a5b00101bdeca2b0ba216aa83ca3cb6fac42ebb84-102.png)

### [DATA STRUCTURE] 前缀树 

在[计算机科学](https://www.wikiwand.com/zh-hans/%E8%AE%A1%E7%AE%97%E6%9C%BA%E7%A7%91%E5%AD%A6)中，**trie**，又称**前缀树**或**字典树**，是一种有序[树](https://www.wikiwand.com/zh-hans/%E6%A0%91_(%E6%95%B0%E6%8D%AE%E7%BB%93%E6%9E%84))，用于保存[关联数组](https://www.wikiwand.com/zh-hans/%E5%85%B3%E8%81%94%E6%95%B0%E7%BB%84)，其中的键通常是[字符串](https://www.wikiwand.com/zh-hans/%E5%AD%97%E7%AC%A6%E4%B8%B2)。与[二叉查找树](https://www.wikiwand.com/zh-hans/%E4%BA%8C%E5%8F%89%E6%9F%A5%E6%89%BE%E6%A0%91)不同，键不是直接保存在节点中，而是由节点在树中的位置决定。一个节点的所有子孙都有相同的[前缀](https://www.wikiwand.com/zh-hans/%E5%89%8D%E7%BC%80)，也就是这个节点对应的字符串，而根节点对应[空字符串](https://www.wikiwand.com/zh-hans/%E7%A9%BA%E5%AD%97%E7%AC%A6%E4%B8%B2)。一般情况下，不是所有的节点都有对应的值，只有叶子节点和部分内部节点所对应的键才有相关的值。

Trie这个术语来自于re**trie**val。根据[词源学](https://www.wikiwand.com/zh-hans/%E8%AF%8D%E6%BA%90%E5%AD%A6)，trie的发明者Edward Fredkin把它读作[/ˈtriː/](https://www.wikiwand.com/zh-hans/Help:%E8%8B%B1%E8%AA%9E%E5%9C%8B%E9%9A%9B%E9%9F%B3%E6%A8%99)"tree"。[[1\]](https://www.wikiwand.com/zh-hans/Trie#citenoteDADS1)[[2\]](https://www.wikiwand.com/zh-hans/Trie#citenoteLiang19832)但是，其他作者把它读作[/ˈtraɪ/](https://www.wikiwand.com/zh-hans/Help:%E8%8B%B1%E8%AA%9E%E5%9C%8B%E9%9A%9B%E9%9F%B3%E6%A8%99) "try"。[[1\]](https://www.wikiwand.com/zh-hans/Trie#citenoteDADS1)[[2\]](https://www.wikiwand.com/zh-hans/Trie#citenoteLiang19832)[[3\]](https://www.wikiwand.com/zh-hans/Trie#citenoteKnuthVol33)

在图示中，键标注在节点中，值标注在节点之下。每一个完整的英文单词对应一个特定的整数。Trie可以看作是一个[确定有限状态自动机](https://www.wikiwand.com/zh-hans/%E7%A1%AE%E5%AE%9A%E6%9C%89%E9%99%90%E7%8A%B6%E6%80%81%E8%87%AA%E5%8A%A8%E6%9C%BA)，尽管边上的符号一般是隐含在分支的顺序中的。

键不需要被显式地保存在节点中。图示中标注出完整的单词，只是为了演示trie的原理。

trie中的键通常是字符串，但也可以是其它的结构。trie的算法可以很容易地修改为处理其它结构的有序序列，比如一串数字或者形状的排列。比如，**bitwise trie**中的键是一串位元，可以用于表示整数或者内存地址。

![ä¸ä¸ªä¿å­äº8ä¸ªé®çtrieç"æï¼"A", "to", "tea", "ted", "ten", "i", "in", and "inn".](https://upload.wikimedia.org/wikipedia/commons/thumb/b/be/Trie_example.svg/500px-Trie_example.svg.png)

#### 应用

trie树常用于搜索提示。如当输入一个网址，可以自动搜索出可能的选择。当没有完全匹配的搜索结果，可以返回前缀最相似的可能。[[4\]](https://www.wikiwand.com/zh-hans/Trie#citenote4)

#### Summary

This article is for intermediate level users. It introduces the following ideas: The data structure Trie (Prefix tree) and most common operations with it.

##### Solution

#### Applications

Trie (we pronounce "try") or prefix tree is a tree data structure, which is used for retrieval of a key in a dataset of strings. There are various applications of this very efficient data structure such as :

##### 1. [Autocomplete](https://en.wikipedia.org/wiki/Autocomplete)

![Google Suggest](https://leetcode.com/media/original_images/208_GoogleSuggest.png)

*Figure 1. Google Suggest in action.*

##### 2. [Spell checker](https://en.wikipedia.org/wiki/Spell_checker)

![Spell Checker](https://leetcode.com/media/original_images/208_SpellCheck.png)

*Figure 2. A spell checker used in word processor.*

##### 3. [IP routing (Longest prefix matching)](https://en.wikipedia.org/wiki/Longest_prefix_match)

![IP Routing](https://leetcode.com/media/original_images/208_IPRouting.gif)

*Figure 3. Longest prefix matching algorithm uses Tries in Internet Protocol (IP) routing to select an entry from a forwarding table.*

##### 4. [T9 predictive text](https://en.wikipedia.org/wiki/T9_(predictive_text))

![T9 Predictive Text](https://leetcode.com/media/original_images/208_T9.jpg)

*Figure 4. T9 which stands for Text on 9 keys, was used on phones to input texts during the late 1990s.*

##### 5. [Solving word games](https://en.wikipedia.org/wiki/Boggle)

![Boggle](https://leetcode.com/media/original_images/208_Boggle.png)

*Figure 5. Tries is used to solve Boggle efficiently by pruning the search space.*

There are several other data structures, like balanced trees and hash tables, which give us the possibility to search for a word in a dataset of strings. Then why do we need trie? Although hash table has O(1)*O*(1) time complexity for looking for a key, it is not efficient in the following operations :

- Finding all keys with a common prefix.
- Enumerating a dataset of strings in lexicographical order.

Another reason why trie outperforms hash table, is that as hash table increases in size, there are lots of hash collisions and the search time complexity could deteriorate to O(n)*O*(*n*), where n*n* is the number of keys inserted. Trie could use less space compared to Hash Table when storing many keys with the same prefix. In this case using trie has only O(m)*O*(*m*) time complexity, where m*m* is the key length. Searching for a key in a balanced tree costs O(m \log n)*O*(*m*log*n*) time complexity.

#### Trie node structure

Trie is a rooted tree. Its nodes have the following fields:

- Maximum of R*R* links to its children, where each link corresponds to one of R*R* character values from dataset alphabet. In this article we assume that R*R* is 26, the number of lowercase latin letters.
- Boolean field which specifies whether the node corresponds to the end of the key, or is just a key prefix.



### [API] collections.Counter 计数

https://docs.python.org/3.6/library/collections.html#collections.Counter

A [`Counter`](https://docs.python.org/3.6/library/collections.html#collections.Counter) is a [`dict`](https://docs.python.org/3.6/library/stdtypes.html#dict) subclass for counting hashable objects. It is an unordered collection where elements are stored as dictionary keys and their counts are stored as dictionary values. Counts are allowed to be any integer value including zero or negative counts. The [`Counter`](https://docs.python.org/3.6/library/collections.html#collections.Counter) class is similar to bags or multisets in other languages.

`elements`()

Return an iterator over elements repeating each as many times as its count. Elements are returned in arbitrary order. If an element’s count is less than one, [`elements()`](https://docs.python.org/3.6/library/collections.html#collections.Counter.elements) will ignore it.

`most_common`([*n*])

Return a list of the *n* most common elements and their counts from the most common to the least. If *n* is omitted or `None`, [`most_common()`](https://docs.python.org/3.6/library/collections.html#collections.Counter.most_common) returns *all* elements in the counter. Elements with equal counts are ordered arbitrarily:

### [API] 短路逻辑

表达式从左至右运算，若 or 的左侧逻辑值为 True ，则短路 or 后所有的表达式（不管是 and 还是 or），直接输出 or 左侧表达式 。

表达式从左至右运算，若 and 的左侧逻辑值为 False ，则短路其后所有 and 表达式，直到有 or 出现，输出 and 左侧表达式到 or 的左侧，参与接下来的逻辑运算。

若 or 的左侧为 False ，或者 and 的左侧为 True 则不能使用短路逻辑。

### [API] Map Filter Reduce

##### Map

Map` applies a function to all the items in an input_list. Here is the blueprint:

**Blueprint**

```
map(function_to_apply, list_of_inputs)
```

```
items = [1, 2, 3, 4, 5]
squared = []
for i in items:
    squared.append(i**2)
```

`Map` allows us to implement this in a much simpler and nicer way. Here you go:

```
items = [1, 2, 3, 4, 5]
squared = list(map(lambda x: x**2, items))
```

Most of the times we use lambdas with `map` so I did the same. Instead of a list of inputs we can even have a list of functions!

```
def multiply(x):
    return (x*x)
def add(x):
    return (x+x)

funcs = [multiply, add]
for i in range(5):
    value = list(map(lambda x: x(i), funcs))
    print(value)

# Output:
# [0, 0]
# [1, 2]
# [4, 4]
# [9, 6]
# [16, 8]
```

#### Filter

As the name suggests, `filter` creates a list of elements for which a function returns true. Here is a short and concise example:

```
number_list = range(-5, 5)
less_than_zero = list(filter(lambda x: x < 0, number_list))
print(less_than_zero)

# Output: [-5, -4, -3, -2, -1]
```

The filter resembles a for loop but it is a builtin function and faster.

**Note:** If map & filter do not appear beautiful to you then you can read about `list/dict/tuple`comprehensions.

#### Reduce

`Reduce` is a really useful function for performing some computation on a list and returning the result. It applies a rolling computation to sequential pairs of values in a list. For example, if you wanted to compute the product of a list of integers.

So the normal way you might go about doing this task in python is using a basic for loop:

```
product = 1
list = [1, 2, 3, 4]
for num in list:
    product = product * num

# product = 24
```

Now let’s try it with reduce:

```
from functools import reduce
product = reduce((lambda x, y: x * y), [1, 2, 3, 4])

# Output: 24
```

### [头条] 常考题型-全排列

### [API] 字符数字转化 ascii

#### 字符转数字 

**ord('a')**

#### 数字转字符

chr(97)

### [常用位运算]

####  1. 原码

原码就是符号位加上真值的绝对值, 即用第一位表示符号, 其余位表示值. 比如如果是8位二进制:

> [+1]原 = 0000 0001
>
> [-1]原 = 1000 0001

第一位是符号位. 因为第一位是符号位, 所以8位二进制数的取值范围就是:

> [1111 1111 , 0111 1111]

即

> [-127 , 127]

原码是人脑最容易理解和计算的表示方式.

#### 2. 反码

反码的表示方法是:

正数的反码是其本身

负数的反码是在其原码的基础上, 符号位不变，其余各个位取反.

> [+1] = [00000001]原 = [00000001]反
>
> [-1] = [10000001]原 = [11111110]反

可见如果一个反码表示的是负数, 人脑无法直观的看出来它的数值. 通常要将其转换成原码再计算.

#### 3. 补码

补码的表示方法是:

正数的补码就是其本身

负数的补码是在其原码的基础上, 符号位不变, 其余各位取反, 最后+1. (即在反码的基础上+1)

> [+1] = [00000001]原 = [00000001]反 = [00000001]补
>
> [-1] = [10000001]原 = [11111110]反 = [11111111]补

对于负数, 补码表示方式也是人脑无法直观看出其数值的. 通常也需要转换成原码在计算其数值.

a ^ a = 0

整数的补码不变

#### 4.为何要使用原码, 反码和补码

#### 在开始深入学习前, 我的学习建议是先"死记硬背"上面的原码, 反码和补码的表示方式以及计算方法.

现在我们知道了计算机可以有三种编码方式表示一个数. 对于正数因为三种编码方式的结果都相同:

> [+1] = [00000001]原 = [00000001]反 = [00000001]补

所以不需要过多解释. 但是对于负数:

> [-1] = [10000001]原 = [11111110]反 = [11111111]补

可见原码, 反码和补码是完全不同的. 既然原码才是被人脑直接识别并用于计算表示方式, 为何还会有反码和补码呢?

首先, 因为人脑可以知道第一位是符号位, 在计算的时候我们会根据符号位, 选择对真值区域的加减. (真值的概念在本文最开头). 但是对于计算机, 加减乘数已经是最基础的运算, 要设计的尽量简单. 计算机辨别"符号位"显然会让计算机的基础电路设计变得十分复杂! 于是人们想出了将符号位也参与运算的方法. 我们知道, 根据运算法则减去一个正数等于加上一个负数, 即: 1-1 = 1 + (-1) = 0 , 所以机器可以只有加法而没有减法, 这样计算机运算的设计就更简单了.

于是人们开始探索 将符号位参与运算, 并且只保留加法的方法. 首先来看原码:

计算十进制的表达式: 1-1=0

> 1 - 1 = 1 + (-1) = [00000001]原 + [10000001]原 = [10000010]原 = -2

如果用原码表示, 让符号位也参与计算, 显然对于减法来说, 结果是不正确的.这也就是为何计算机内部不使用原码表示一个数.

为了解决原码做减法的问题, 出现了反码:

计算十进制的表达式: 1-1=0

> 1 - 1 = 1 + (-1) = [0000 0001]原 + [1000 0001]原= [0000 0001]反 + [1111 1110]反 = [1111 1111]反 = [1000 0000]原 = -0

发现用反码计算减法, 结果的真值部分是正确的. 而唯一的问题其实就出现在"0"这个特殊的数值上. 虽然人们理解上+0和-0是一样的, 但是0带符号是没有任何意义的. 而且会有[0000 0000]原和[1000 0000]原两个编码表示0.

于是补码的出现, 解决了0的符号以及两个编码的问题:

> 1-1 = 1 + (-1) = [0000 0001]原 + [1000 0001]原 = [0000 0001]补 + [1111 1111]补 = [0000 0000]补=[0000 0000]原

这样0用[0000 0000]表示, 而以前出现问题的-0则不存在了.而且可以用[1000 0000]表示-128:

> (-1) + (-127) = [1000 0001]原 + [1111 1111]原 = [1111 1111]补 + [1000 0001]补 = [1000 0000]补

-1-127的结果应该是-128, 在用补码运算的结果中, [1000 0000]补 就是-128. 但是注意因为实际上是使用以前的-0的补码来表示-128, 所以-128并没有原码和反码表示.(对-128的补码表示[1000 0000]补算出来的原码是[0000 0000]原, 这是不正确的)

使用补码, 不仅仅修复了0的符号以及存在两个编码的问题, 而且还能够多表示一个最低数. 这就是为什么8位二进制, 使用原码或反码表示的范围为[-127, +127], 而使用补码表示的范围为[-128, 127].

因为机器使用补码, 所以对于编程中常用到的32位int类型, 可以表示范围是: [-231, 231-1] 因为第一位表示的是符号位.而使用补码表示时又可以多保存一个最小值.

#### 5.lowbit(x) = x & (-x) == x & (~x + 1)

**lowbit 要的是你从末尾开始第1个 1(其他位置都是0) 所代表的值**

#### 6.x ^ x = 0

### 异或XOR的特点

#### 按位异或的3个特点:

##### (1) 0^0=0,0^1=1  0异或任何数＝任何数

##### (2) 1^0=1,1^1=0  1异或任何数－任何数取反

##### (3) 任何数异或自己＝把自己置0

#### 按位异或的几个常见用途:

##### (1) 使某些特定的位翻转

​    例如对数10100001的第2位和第3位翻转，则可以将该数与00000110进行按位异或运算。
　　　　　  10100001^00000110 = 10100111

##### (2) 实现两个值的交换，而不必使用临时变量。

​    例如交换两个整数a=10100001，b=00000110的值，可通过下列语句实现：
　　　　a = a^b； 　　//a=10100111
　　　　b = b^a； 　　//b=10100001
　　　　a = a^b； 　　//a=00000110

##### (3) 异或压缩奇偶性信息

二进制数中的1的个数的奇偶性: 如果一个二进制数中的1的个数为奇数，那么每一位数累次xor返回1；如果每一位数累次xor为偶数，那么返回0。

##### (4) 两数a和b的xor运算结果（二进制表示）中1的个数的奇偶性

         * (a的1s - b的1s) & 1 == 1 --> 结果中1s为奇数
         * (a的1s - b的1s) & 1 == 0 --> 结果中1s为偶数

### 字典序

```
设想一本英语字典里的单词，哪个在前哪个在后？

显然的做法是先按照第一个字母、以 a、b、c……z 的顺序排列；如果第一个字母一样，那么比较第二个、第三个乃至后面的字母。如果比到最后两个单词不一样长（比如，sigh 和 sight），那么把短者排在前。

---

形式定义
给定两个偏序集A和B,(a,b)和(a′,b′)属于笛卡尔积 A × B，则字典序定义为

(a,b) ≤ (a′,b′) 当且仅当 a < a′ 或 (a = a′ 且 b ≤ b′).
结果是偏序。如果A和B是全序, 那么结果也是全序。
```

### 多元素按字典序排序

```python
sorted(intervals, key=lambda x: (x[0], x[1]))
```

回文问题 manacher算法

```PYTHON
        def manacher(S):
            """
                回文问题三种解法：
                    1.中心扩散法
                    2.DP
                    3.manacher算法: O(n)
                本函数返回最大回文子字符串
            """
            # 1.解决长度奇偶性带来的对称轴位置问题
            #   其中首尾的作用是中心扩散的时候防止索引越界
            A = '^#' + '#'.join(S) + '#$'
            
            # 2.辅助数组LEN，存储的是以 i 为中心的最长回文的半径(**不包括i这个元素)
            #   LEN数组性质: LEN[i]就是该回文子串在原字符串S中的长度
            LEN = [0] * len(A)
            
            # 3.设置两个变量，right 和 center 。right 代表以 center 为中心的最长回文的右边界，
            #   也就是right = center + LEN[center] - 1。
            center = right = 0
            
            # 4.去掉首尾依次遍历A数组元素
            for i in range(1, len(A) - 1):
                    # 5.i <= right, j和i关于位置center对称, 以j为中心的回文串一定在以center为中心的回文串的内部,
                    #               LEN[i]=LEN[j]
                    #   i > right, 以i为中心的回文串在以center为中心的回文串的外部，
                    #   而大于right的部分我们还没有进行匹配，所以要从right+1位置开始一个一个进行匹配，直到发生失配，
                    #   从而更新P和对应的po以及Len[i]。
                if i < right:
                    LEN[i] = min(right - i, LEN[2 * center - i])
                else:
                    LEN[i] = 0
                # 6.奇数的中心扩散法
                while A[i + LEN[i] + 1] == A[i - LEN[i] - 1]:
                    LEN[i] += 1
                # 7..如果i的右边界 > 原来的右边界，更新
                if i + LEN[i] > right:
                    center, right = i, i + LEN[i]
            
            # print('修改后的字符串', A)
            # print('辅助半径数组', LEN)
            maxLen, centerIndex = max((n, i) for i, n in enumerate(LEN))
            print((maxLen, centerIndex))
            # return maxLen  求最大回文子串长度
            # return s[(centerIndex  - maxLen)//2: (centerIndex  + maxLen)//2] 求最大回文子串
            # return sum([(1+x)//2 for x in LEN]) 求回文子串数量
            return sum([(1+x)//2 for x in LEN])
```

### Python中set值会自动排序但并不保证 最好不要利用（索引无序） C++也是

```
顺序并不保证，这是具体实现的问题，无论如何，在使用set的时候，请把它认为是数学上的集合，无序，不要在实现set的时候假定其有顺序，可能会错
```

### 欧拉路径 欧拉回路

**欧拉路径是一个在图中所有边均只访问一次的路径**

**欧拉回路是起始点和终止点是一个的欧拉路径**

性质：
	如果已知图存在欧拉回路，那么你可以从任何一点开始欧拉回路

![image-20190801145102677](/Users/zhengxinzhi/typora_img/LeetCode题目总结/image-20190801145102677.png)

##### Java python 负数除法  

Java:        -1 / 2 == 0; -3 / 2 == -1

python3: -1 // 2 == -1; -3 // 2 == -2 

### 由数据范围反推算法复杂度以及算法内容

一般ACM或者笔试题的时间限制是1秒或2秒。
在这种情况下，C++代码中的操作次数控制在 $10^7$ 为最佳。

下面给出在不同数据范围下，代码的时间复杂度和算法该如何选择：

$n \le 30$, 指数级别, dfs+剪枝，状态压缩dp
$n \le 100$ => $O(n^3)$，floyd，dp
$n \le 1000$ => $O(n^2)$，$O(n^2logn)$，dp，二分
$n \le 10000$ => $O(n * \sqrt n)$，块状链表
$n \le 100000$ => $O(nlogn)$ => 各种sort，线段树、树状数组、set/map、heap、dijkstra+heap、spfa、求凸包、求半平面交、二分
$n \le 1000000$ => $O(n)$, 以及常数较小的 $O(nlogn)$ 算法 => hash、双指针扫描、kmp、AC自动机，常数比较小的 $O(nlogn)$ 的做法：sort、树状数组、heap、dijkstra、spfa
$n \le 10000000$ => $O(n)$，双指针扫描、kmp、AC自动机、线性筛素数
$n \le 10^9$ => $O(\sqrt n)$，判断质数
$n \le 10^{18}$ => $O(logn)$，最大公约数



## 数学

### 1. 两数之和

#### 1.1 题目描述

给定一个整数数组 `nums` 和一个目标值 `target`，请你在该数组中找出和为目标值的那 **两个** 整数，并返回他们的数组下标。

你可以假设每种输入只会对应一个答案。但是，你不能重复利用这个数组中同样的元素。

**示例:**

```
给定 nums = [2, 7, 11, 15], target = 9

因为 nums[0] + nums[1] = 2 + 7 = 9
所以返回 [0, 1]
```

#### 1.2 解法 Hash

```python
class Solution(object):
    def twoSum(self, nums, target):
        """
        :type nums: List[int]
        :type target: int
        :rtype: List[int]
        
        Hash
        """
        dic = dict()
        for i, num in enumerate(nums):
            diff = target - num
            if diff in dic:
                return [dic[diff], i]
            else:
                dic[num] = i
```



### 7.整数反转

#### 7.1.题目描述

给出一个 32 位的有符号整数，你需要将这个整数中每位上的数字进行反转。

**示例 1:**

```
输入: 123
输出: 321
```

 **示例 2:**

```
输入: -123
输出: -321
```

**示例 3:**

```
输入: 120
输出: 21
```

**注意:**

假设我们的环境只能存储得下 32 位的有符号整数，则其数值范围为 [−231,  231 − 1]。请根据这个假设，如果反转后整数溢出那么就返回 0。

#### 7.2.解法

注意：-2147483648的相反数溢出了

##### 7.2.1 方法一

```c++
#include <limits.h>
class Solution {
public:
    int reverse(int x) {
        int ans = 0;
        int sign = x < 0 ? - 1: 1;
        
        if(x == INT_MIN){
            // -2147483648 反转溢出
            return 0;
        }
        x = abs(x);
        while(x > 0){
            // 2**31(2 << 30): 2147483648
            if(ans == INT_MAX / 10){
                if(x % 10 > 7){
                    return 0;
                }
            }else if(ans > INT_MAX / 10){
                return 0;
            }
            ans = ans * 10 + x % 10;
            x /= 10;
        }
        return ans * sign;
    }
};
```

```c++
class Solution {
public:
    int reverse(int x) {
        int sign = x < 0 ? - 1: 1;
        if(x == INT_MIN){
            // -2147483648 反转溢出
            return 0;
        }
        x = abs(x);
        int res = 0;
        while(x > 0){
            if(res > INT_MAX / 10 || res * 10 > (INT_MAX - x % 10)){
                return 0;
            }
            res = res * 10 + x % 10;
            x /= 10;
        }
        return sign * res;
    }
};

```



##### 7.2.2 方法二 不推荐

```java
class Solution {
    public int reverse(int x) {
        int ans = 0;
        while(x != 0){
            int tail = x % 10;
            int newAns = ans * 10 + tail;
            // If overflow exists, the new result will not equal previous one.
            if((newAns - tail) / 10 != ans){
                return 0;
            }
            ans = newAns;
            x = x / 10;
        }
        return ans;
    }
}

```

### 29.两数相除

#### 题目描述

给定两个整数，被除数 dividend 和除数 divisor。将两数相除，要求不使用乘法、除法和 mod 运算符。

返回被除数 dividend 除以除数 divisor 得到的商。

示例 1:

输入: dividend = 10, divisor = 3
输出: 3
示例 2:

输入: dividend = 7, divisor = -3
输出: -2
说明:

被除数和除数均为 32 位有符号整数。
除数不为 0。
假设我们的环境只能存储 32 位有符号整数，其数值范围是 [−231,  231 − 1]。本题中，如果除法结果溢出，则返回 231 − 1。


#### 解法

##### 解法一 未全部考虑溢出
```python
class Solution(object):

    def divide(self, dividend, divisor):
        """
            奇思妙想：把最小负数转为整数需要判断溢出，返回来全部转为负数处理规避开这个问题
        """   
        INT_MIN = (-1 << 31)
        INT_MAX = ~((-1 << 31) + 1) + 1
        if dividend == INT_MIN and divisor == -1:
            return INT_MAX
        
        
        sign = -1 if (dividend > 0) ^ (divisor > 0) else 1
        # 全部转为负数不用在这判断溢出
        dvd = 0 - dividend if dividend > 0 else dividend
        dvs = 0 - divisor if divisor > 0 else divisor
        
        ans = 0
        while dvd <= dvs:
            temp = dvs
            cnt = 1
            # todo:没判断中间过程溢出
            while (temp << 1) >= dvd:
                temp <<= 1
                cnt <<= 1
            dvd -= temp
            ans += cnt
        return ans * sign
        
```

##### 解法二
```python

```

### 50.pow(x,n)

#### 题目描述

实现 pow(x, n) ，即计算 x 的 n 次幂函数。

示例 1:

输入: 2.00000, 10
输出: 1024.00000
示例 2:

输入: 2.10000, 3
输出: 9.26100
示例 3:

输入: 2.00000, -2
输出: 0.25000
解释: 2-2 = 1/22 = 1/4 = 0.25
说明:

-100.0 < x < 100.0
n 是 32 位有符号整数，其数值范围是 [−231, 231 − 1] 。


#### 解法

##### 解法一 递归

可选memo的工具:

from functools import lru_cache
@lru_cache(None)
def generate(i, ah, bh, res):

```python
class Solution:
    def myPow(self, x: float, n: int) -> float:
        """
            分治法 + 记忆化
            边界 负数
        """
        memo = dict()
        def helper(x, n):
            if n == 1:
                return x
            if (x, n) in memo:
                return memo[(x, n)]

            if n & 1:
                memo[(x, n)] = helper(x, n // 2) * helper(x, n // 2) * x
                return memo[(x, n)]
            else:
                memo[(x, n)] = helper(x, n // 2) * helper(x, n // 2) 
                return memo[(x, n)]
            
        if n == 0:
            return 1
        if x == 0:
            return 0
            
        if n < 0:
            n = -n
            x = 1 / x
        return helper(x, n)
```

##### 解法二 循环法todo
```python

```

### 69. x 的平方根

#### 69.1 题目描述

实现 `int sqrt(int x)` 函数。

计算并返回 *x* 的平方根，其中 *x* 是非负整数。

由于返回类型是整数，结果只保留整数的部分，小数部分将被舍去。

**示例 1:**

```
输入: 4
输出: 2
```

**示例 2:**

```
输入: 8
输出: 2
说明: 8 的平方根是 2.82842..., 
     由于返回类型是整数，小数部分将被舍去。
```

#### 69.2 解法

##### 69.2.1 方法一 找规律[二分]

**关键:** 如何找到一个x的整数平方根, 特点: y * y <= x < (y + 1)*(y + 1)

```python
class Solution:
    def mySqrt(self, x: int) -> int:
        """二分查找
            保留整数 假设结果为y
            y * y <= x < (y + 1) * (y + 1)
        """
        low, high = 0, x//2 + 1
        while low <= high:
            mid = (low + high) >> 1
            if mid * mid == x:
                return mid
            elif mid * mid < x:
                if (mid + 1) * (mid + 1) > x:
                    return mid
                else:
                    low = mid + 1
            else:
                high = mid - 1
```

69.2.2 方法二 牛顿迭代法

![image-20190312200259083](/Users/zhengxinzhi/typora_img/LeetCode%E9%A2%98%E7%9B%AE%E6%80%BB%E7%BB%93/image-20190312200259083.png)

```python
def mySqrt(self, n: int) -> int:
    r = n
    while r*r > n:
        r = (r + n//r) // 2
    return r
```



#### 69.3 右移的用法 x // 2 == x >> 1 [not] x >> 2 

### 166.分数到小数

#### 题目描述

给定两个整数，分别表示分数的分子 numerator 和分母 denominator，以字符串形式返回小数。

如果小数部分为循环小数，则将循环的部分括在括号内。

示例 1:

输入: numerator = 1, denominator = 2
输出: "0.5"
示例 2:

输入: numerator = 2, denominator = 1
输出: "2"
示例 3:

输入: numerator = 2, denominator = 3
输出: "0.(6)


#### 解法

##### 解法一  长除法
![image-20190718204610748](/Users/zhengxinzhi/typora_img/LeetCode题目总结/image-20190718204610748.png)

```java
class Solution {
     // 这题没意思
     public String fractionToDecimal(int numerator, int denominator) {
        if (numerator == 0) {
            return "0";
        }
        StringBuilder fraction = new StringBuilder();
        // If either one is negative (not both)
        if (numerator < 0 ^ denominator < 0) {
            fraction.append("-");
        }
        // Convert to Long or else abs(-2147483648) overflows
        long dividend = Math.abs(Long.valueOf(numerator));
        long divisor = Math.abs(Long.valueOf(denominator));
        fraction.append(String.valueOf(dividend / divisor));
        long remainder = dividend % divisor;
        if (remainder == 0) {
            return fraction.toString();
        }
        fraction.append(".");
        Map<Long, Integer> map = new HashMap<>();
        while (remainder != 0) {
            if (map.containsKey(remainder)) {
                fraction.insert(map.get(remainder), "(");
                fraction.append(")");
                break;
            }
            map.put(remainder, fraction.length());
            remainder *= 10;
            fraction.append(String.valueOf(remainder / divisor));
            remainder %= divisor;
        }
        return fraction.toString();
    }
}

```

##### 解法二
```python

```

### 204.计算质数

#### 204.1.题目描述

统计所有小于非负整数 *n* 的质数的数量。

**示例:**

```
输入: 10
输出: 4
解释: 小于 10 的质数一共有 4 个, 它们是 2, 3, 5, 7 。
```

#### 204.2.解法
##### 204.2.1 方法一 定义法

```java
class Solution {
    
    public int countPrimes(int n) {
        int ans = 0;
        for(int i = 2; i < n; i++){
            ans += isPrime(i) ? 1 : 0;
        }
        return ans;
    }
    
    public boolean isPrime(int n){
        if(n == 2 || n == 3){
            return true;
        }
        for(int i = 2; i <= Math.sqrt(n); i++){
            if(n % i == 0){
                return false;
            }
        }
        return true;
    }
}
```



##### 204.2.2 方法二

这题搜到一个非常牛逼的算法,叫做厄拉多塞筛法. 比如说求20以内质数的个数,首先0,1不是质数.2是第一个质数,然后把20以内所有2的倍数划去.2后面紧跟的数即为下一个质数3,然后把3所有的倍数划去.3后面紧跟的数即为下一个质数5,再把5所有的倍数划去.以此类推.

代码的实现上用了非常好的技巧:

在上面遍历索引的时候用到了一个非常好的技巧. 即i是从(2,int(n**0.5)+1)而非(2,n).这个技巧是可以验证的,比如说求9以内的质数个数,那么只要划掉sqrt(9)以内的质数倍数,剩下的即全为质数. 所以在划去倍数的时候也是从i*i开始划掉,而不是i+i.

```java
class Solution {
    
    public int countPrimes(int n) {
        if(n <= 2){
            return 0;
        }else{
            // 生成个值全部为1的列表
            int[] arr = new int[n+1];
            Arrays.fill(arr, 1);
            arr[0] = 0;
            arr[1] = 0;
            //因为0和1不是质数,所以列表的前两个位置赋值为0 output[0],output[1] = 0,0
            // 此时从index = 2开始遍历,output[2]==1,即表明第一个质数为2,然后将2的倍数对应的索引
            //全部赋值为0. 此时output[3] == 1,即表明下一个质数为3,同样划去3的倍数.以此类推.
            for(int i = 2; i <= Math.sqrt(n); i++){
                if(arr[i] == 1){
                    for(int j = i * i; j < n; j += i){
                        arr[j] = 0;
                    }
                }
            }
            int sum = 0;
            for(int i = 0; i < n; i++){
                if(arr[i] == 1){
                    sum++;
                }
            }
            return sum;
        }
    }
}
```

```java
class Solution {
    
    public int countPrimes(int n) {
        boolean[] notPrime = new boolean[n];
        int count = 0;
        for(int i = 2; i < n; i++){
            if(notPrime[i] == false){
                count++;
                for(int j = 2; j * i < n; j++){
                    notPrime[i*j] = true;
                }
            }
        }
        return count;
    }
}
```

### 241. 为运算表达式设计优先级

#### 241.1 题目描述

给定一个含有数字和运算符的字符串，为表达式添加括号，改变其运算优先级以求出不同的结果。你需要给出所有可能的组合的结果。有效的运算符号包含 `+`, `-` 以及 `*` 。

**示例 1:**

```
输入: "2-1-1"
输出: [0, 2]
解释: 
((2-1)-1) = 0 
(2-(1-1)) = 2
```

**示例 2:**

```
输入: "2*3-4*5"
输出: [-34, -14, -10, -10, 10]
解释: 
(2*(3-(4*5))) = -34 
((2*3)-(4*5)) = -14 
((2*(3-4))*5) = -10 
(2*((3-4)*5)) = -10 
(((2*3)-4)*5) = 10
```

##### 241.2 解法 分而治之

##### 241.2.1 方法一 分而治之

```python
class Solution(object):
    def diffWaysToCompute(self, input):
        """
        :type input: str
        :rtype: List[int]
        Divide and Conquer
        Divide Conquer Combine*
        """
        # 边界条件
        if input.isnumeric():
            return [int(input)]
        result = []
        for i, item in enumerate(input):
            left_res = []
            right_res = []
            if item in set(['*', '+', '-']):
                left = input[:i]
                right = input[i+1:]
                if left.isnumeric():
                    left_res.append(int(left))
                else:
                    left_res.extend(self.diffWaysToCompute(left))
                if right.isnumeric():
                    right_res.append(int(right))
                else:
                    right_res.extend(self.diffWaysToCompute(right))
            
            for l in left_res:
                for r in right_res:
                    if '*' == item:
                        result.append(l * r)
                    elif '+' == item:
                        result.append(l + r)
                    elif '-' == item:
                        result.append(l - r)
        return result
```

##### 241.2.2 方法二  or 短路逻辑替换条件分支

```python
def diffWaysToCompute(self, input):
    # 下面的or利用了短路逻辑
    return [a+b if c == '+' else a-b if c == '-' else a*b
            for i, c in enumerate(input) if c in '+-*'
            for a in self.diffWaysToCompute(input[:i])
            for b in self.diffWaysToCompute(input[i+1:])] or [int(input)]
```

##### 241.2.3 方法三 eval: 把字符串当做表达式执行

```python
def diffWaysToCompute(self, input):
    return [eval(`a`+c+`b`)
            for i, c in enumerate(input) if c in '+-*'
            for a in self.diffWaysToCompute(input[:i])
            for b in self.diffWaysToCompute(input[i+1:])] or [int(input)]
```

### [归纳公式]258.各位相加

#### 1.题目描述

给定一个非负整数 `num`，反复将各个位上的数字相加，直到结果为一位数。

**示例:**

```
输入: 38
输出: 2 
解释: 各位相加的过程为：3 + 8 = 11, 1 + 1 = 2。 由于 2 是一位数，所以返回 2。
```

**进阶:**
你可以不使用循环或者递归，且在 O(1) 时间复杂度内解决这个问题吗？

#### 2.解法
##### 2.2 方法二 公式 不通用

![{\displaystyle \operatorname {dr} (/Users/zhengxinzhi/typora_img/LeetCode题目总结/8aabde69a31053c2a3a769183ef1fea68b7ee49d)={\begin{cases}0&{\mbox{if))\ n=0,\\9&{\mbox{if))\ n\neq 0,\ n\ \equiv 0{\pmod {9)),\\n\ {\rm {mod))\ 9&{\mbox{if))\ n\not \equiv 0{\pmod {9))\end{cases))}](https://wikimedia.org/api/rest_v1/media/math/render/svg/8aabde69a31053c2a3a769183ef1fea68b7ee49d)

对于基数b（十进制情况b = 10），整数的数字根是：

如果n == 0，则dr（n）= 0
dr（n）=（b -1）如果n！= 0且n％（b -1）== 0
dr（n）= n mod（b -1）如果n％（b -1）！= 0
要么

dr（n）= 1 +（n - 1）％9
注意，当n = 0时，由于（n -1）％9 = -1，返回值为零（正确）。

从公式中，我们可以发现这个问题的结果是周期性的，周期（b -1）。

小数的输出顺序（b = 10）：

〜输入：0 1 2 3 4 ... 
输出：0 1 2 3 4 5 6 7 8 9 1 2 3 4 5 6 7 8 9 1 2 3 ....

从此以后，我们可以编写以下代码，其时间和空间复杂度都是O（1）。

```python
  class Solution(object):
  def addDigits(self, num):
    """
    :type num: int
    :rtype: int
    """
    while(num >= 10):
        temp = 0
        while(num > 0):
            temp += num % 10
            num /= 10
        num = temp
    return num
```

##### 2.1 方法一 迭代法 【推荐】

```python
class Solution(object):
    def addDigits(self, num):
        """
        :type num: int
        :rtype: int
        """
        while(num >= 10):
            sum_val = 0
            while(num > 0):
                sum_val += num % 10
                num /= 10
            num = sum_val
        return num
```

### 263.丑数

#### 1.题目描述

编写一个程序判断给定的数是否为丑数。

丑数就是只包含质因数 `2, 3, 5` 的**正整数**。

**示例 1:**

```
输入: 6
输出: true
解释: 6 = 2 × 3
```

**示例 2:**

```
输入: 8
输出: true
解释: 8 = 2 × 2 × 2
```

**示例 3:**

```
输入: 14
输出: false 
解释: 14 不是丑数，因为它包含了另外一个质因数 7。
```

**说明：**

1. `1` 是丑数。
2. 输入不会超过 32 位有符号整数的范围: [−231,  231 − 1]。

#### 2.解法
##### 2.1 方法一

```java
class Solution {
    public boolean isUgly(int num) {
        if(num == 1){
            return true;
        }
        
        while(num != 0 && num % 2 == 0){
            num /= 2;
        }
        while(num != 0 && num % 3 == 0){
            num /= 3;
        }
        while(num != 0 && num % 5 == 0){
            num /= 5;
        }
        return num == 1;
    }
}
```

```java
if (num > 0)
    for (int i=2; i<6; i++)
        while (num % i == 0)
            num /= i;
return num == 1;
```

### 264.丑数II

#### 题目描述

编写一个程序，找出第 n 个丑数。

丑数就是只包含质因数 2, 3, 5 的正整数。

示例:

输入: n = 10
输出: 12
解释: 1, 2, 3, 4, 5, 6, 8, 9, 10, 12 是前 10 个丑数。
说明:  

1 是丑数。
n 不超过1690。


#### 解法

##### 解法一 三指针法 O(n)

We have an array *k* of first n ugly number. We only know, at the beginning, the first one, which is 1. Then

k[1] = min( k[0]x2, k[0]x3, k[0]x5). The answer is k[0]x2. So we move 2's pointer to 1. Then we test:

k[2] = min( k[1]x2, k[0]x3, k[0]x5). And so on. Be careful about the cases such as 6, in which we need to forward both pointers of 2 and 3.

x here is multiplication.

```python
class Solution:
    def nthUglyNumber(self, n: int) -> int:
        """
            三指针法
        """
        res = [1]
        idx2 = 0
        idx3 = 0
        idx5 = 0
        for i in range(n - 1):
            res.append(min(res[idx2] * 2, res[idx3] * 3, res[idx5] * 5))
            # 当命中下一个丑数时，说明该指针指向的丑数 乘以对应权重所得积最小。此时，指针应该指向下一个丑数。
            if res[-1] == res[idx2]*2:
                idx2 += 1
            if res[-1] == res[idx3]*3:
                idx3 += 1
            if res[-1] == res[idx5]*5:
                idx5 += 1
        return res[n - 1]
```

##### 解法二 堆 O(n**2)

res[0] = 1
然后用res[0]乘以{2,3,5}，放进最小堆中，从堆中取出堆顶，即为res[1]。

res[1] = 2
然后用res[1]乘以{2,3,5}，放进最小堆中，从堆中取出堆顶，即为res[2]。

循环到 n 即可。

```python
public int nthUglyNumber(int n) {
    if(n==1) return 1;
    PriorityQueue<Long> q = new PriorityQueue();
    q.add(1l);
    
    for(long i=1; i<n; i++) {
        long tmp = q.poll();
        while(!q.isEmpty() && q.peek()==tmp) tmp = q.poll();
        
        q.add(tmp*2);
        q.add(tmp*3);
        q.add(tmp*5);
    }
    return q.poll().intValue();
```

### 279.完全平方数

#### 题目描述

给定正整数 n，找到若干个完全平方数（比如 1, 4, 9, 16, ...）使得它们的和等于 n。你需要让组成和的完全平方数的个数最少。

示例 1:

输入: n = 12
输出: 3 
解释: 12 = 4 + 4 + 4.
示例 2:

输入: n = 13
输出: 2
解释: 13 = 4 + 9.


#### 解法

##### 解法一 DP 会超时
```python
class Solution:      
    def numSquares(self, n: int) -> int:
        dp = [float('inf')] * (n + 1)
        dp[0] = 0
        
        for i in range(1, n + 1):
            j = 1
            while j * j <= i:
                dp[i] = min(dp[i], dp[i - j * j] + 1) 
                j += 1
        return dp[n]

```

##### 解法二 BFS

题目变形

![image-20190709141227932](/Users/zhengxinzhi/typora_img/LeetCode题目总结/image-20190709141227932.png)

```python
import queue
class Solution:      
    def numSquares(self, n):
        # 1.终止条件
        if n < 2:
            return n
        
         # 2.完全平方数列表
        square_lst = [] 
        i = 1
        while i * i <= n:
            square_lst.append(i * i)
            i += 1
            
        # 3.BFS 以n为根 以每个 （n - 完全平方数）为子结点 不断向下遍历 直到某子结点为完全平方数
        que = queue.Queue()
        que.put((n, 0))
        while que:
            num, depth = que.get()
            for square in square_lst:
                if square < num:
                    que.put((num - square, depth + 1))
                elif square == num:
                    return depth + 1

```

##### 解法三 四平方和定理 todo:讲解

```c++
class Solution 
{  
private:  
    int is_square(int n)
    {  
        int sqrt_n = (int)(sqrt(n));  
        return (sqrt_n*sqrt_n == n);  
    }
    
public:
    // Based on Lagrange's Four Square theorem, there 
    // are only 4 possible results: 1, 2, 3, 4.
    int numSquares(int n) 
    {  
        // If n is a perfect square, return 1.
        if(is_square(n)) 
        {
            return 1;  
        }
        
        // The result is 4 if and only if n can be written in the 
        // form of 4^k*(8*m + 7). Please refer to 
        // Legendre's three-square theorem.
        while ((n & 3) == 0) // n%4 == 0  
        {
            n >>= 2;  
        }
        if ((n & 7) == 7) // n%8 == 7
        {
            return 4;
        }
        
        // Check whether 2 is the result.
        int sqrt_n = (int)(sqrt(n)); 
        for(int i = 1; i <= sqrt_n; i++)
        {  
            if (is_square(n - i*i)) 
            {
                return 2;  
            }
        }  
        
        return 3;  
    }  
}; 
```

### 338.比特位计数

#### 题目描述

给定一个非负整数 num。对于 0 ≤ i ≤ num 范围中的每个数字 i ，计算其二进制数中的 1 的数目并将它们作为数组返回。

示例 1:

输入: 2
输出: [0,1,1]
示例 2:

输入: 5
输出: [0,1,1,2,1,2]
进阶:

给出时间复杂度为O(n*sizeof(integer))的解答非常容易。但你可以在线性时间O(n)内用一趟扫描做到吗？
要求算法的空间复杂度为O(n)。
你能进一步完善解法吗？要求在C++或任何其他语言中不使用任何内置函数（如 C++ 中的 __builtin_popcount）来执行此操作。


#### 解法

##### 解法一
```python
class Solution:
    def countBits(self, num: int) -> List[int]:
        """
            找规律 偶数初二结果相等 奇数等于前一个偶数结果+1
        """
        dp = [0]
        for i in range(1, num + 1):
            if i % 2:
                dp.append(dp[i - 1] + 1)
            else:
                dp.append(dp[i // 2])
                
        return dp
```

##### 解法二
```python

```

### 504. 七进制数

#### 504.1 题目描述

给定一个整数，将其转化为7进制，并以字符串形式输出。

**示例 1:**

```
输入: 100
输出: "202"
```

**示例 2:**

```
输入: -7
输出: "-10"
```

**注意:** 输入范围是 [-1e7, 1e7] 。

#### 504.2 解法

##### 504.2.1 方法一 迭代法

```python
def convertTo7(self, num):
    if num == 0: return '0'
    n, res = abs(num), ''
    while n:
      res = str(n % 7) + res
      n //= 7
    return res if num > 0 else '-' + res
```

##### 504.2.2 方法二 递归法

```python
def convertTo7(self, num):
    if num < 0: return '-' + self.convertTo7(-num)
    # 终止条件
    if num < 7: return str(num)
    return self.convertTo7(num // 7) + str(num % 7)
```

### 866. 回文素数

#### 866.1.题目描述

求出大于或等于 `N` 的最小回文素数。

回顾一下，如果一个数大于 1，且其因数只有 1 和它自身，那么这个数是*素数*。

例如，2，3，5，7，11 以及 13 是素数。

回顾一下，如果一个数从左往右读与从右往左读是一样的，那么这个数是*回文数。*

例如，12321 是回文数。

 

**示例 1：**

```
输入：6
输出：7
```

**示例 2：**

```
输入：8
输出：11
```

**示例 3：**

```
输入：13
输出：101
```

 

**提示：**

- `1 <= N <= 10^8`
- 答案肯定存在，且小于 `2 * 10^8`。

#### 866.2.解法
##### 866.2.1 方法一

```java
class Solution {
    public int primePalindrome(int N) {
        if(N % 2 == 0 && N > 2){
            N += 1;
        }
        while (true) {
            if (N == reverse(N) && isPrime(N))
                return N;
            if(N > 2){
                N++;
            }
            N++;
            if (10_000_000 < N && N < 100_000_000)
                N = 100_000_001;
        }
    }
    
    public boolean isPrime(int N) {
        if (N < 2) return false;
        int R = (int) Math.sqrt(N);
        for (int d = 2; d <= R; ++d)
            if (N % d == 0) return false;
        return true;
    }

    public int reverse(int N) {
        int ans = 0;
        while (N > 0) {
            ans = 10 * ans + (N % 10);
            N /= 10;
        }
        return ans;
    }
    
}
```

###1276.不浪费原料的汉堡制作方案

#### 题目描述

圣诞活动预热开始啦，汉堡店推出了全新的汉堡套餐。为了避免浪费原料，请你帮他们制定合适的制作计划。

给你两个整数 `tomatoSlices` 和 `cheeseSlices`，分别表示番茄片和奶酪片的数目。不同汉堡的原料搭配如下：

- **巨无霸汉堡：**4 片番茄和 1 片奶酪
- **小皇堡：**2 片番茄和 1 片奶酪

请你以 `[total_jumbo, total_small]`（[巨无霸汉堡总数，小皇堡总数]）的格式返回恰当的制作方案，使得剩下的番茄片 `tomatoSlices` 和奶酪片 `cheeseSlices` 的数量都是 `0`。

如果无法使剩下的番茄片 `tomatoSlices` 和奶酪片 `cheeseSlices` 的数量为 `0`，就请返回 `[]`。

 

**示例 1：**

```
输入：tomatoSlices = 16, cheeseSlices = 7
输出：[1,6]
解释：制作 1 个巨无霸汉堡和 6 个小皇堡需要 4*1 + 2*6 = 16 片番茄和 1 + 6 = 7 片奶酪。不会剩下原料。
```

**示例 2：**

```
输入：tomatoSlices = 17, cheeseSlices = 4
输出：[]
解释：只制作小皇堡和巨无霸汉堡无法用光全部原料。
```

**示例 3：**

```
输入：tomatoSlices = 4, cheeseSlices = 17
输出：[]
解释：制作 1 个巨无霸汉堡会剩下 16 片奶酪，制作 2 个小皇堡会剩下 15 片奶酪。
```

**示例 4：**

```
输入：tomatoSlices = 0, cheeseSlices = 0
输出：[0,0]
```

**示例 5：**

```
输入：tomatoSlices = 2, cheeseSlices = 1
输出：[0,1]
```

 

**提示：**

- `0 <= tomatoSlices <= 10^7`
- `0 <= cheeseSlices <= 10^7`


#### 解法

##### 解法一 鸡兔同笼问题
```python
class Solution:
    def numOfBurgers(self, tomatoSlices: int, cheeseSlices: int) -> List[int]:
        t, c = tomatoSlices, cheeseSlices
        if t < 2 * c or t > 4 * c:
            return []
        if (t - 2 * c) & 1:
            return []
        tn = (t - 2 * c) >> 1
        cn = c - tn
        return [tn, cn]
```

##### 解法二
```python

```

## 字符串

### 思路（类似数组）

**分治法 双指针**

### 3.无重复字符串的最长子串

#### 3.1.题目描述

给定一个字符串，请你找出其中不含有重复字符的 **最长子串** 的长度。

**示例 1:**

```
输入: "abcabcbb"
输出: 3 
解释: 因为无重复字符的最长子串是 "abc"，所以其长度为 3。
```

**示例 2:**

```
输入: "bbbbb"
输出: 1
解释: 因为无重复字符的最长子串是 "b"，所以其长度为 1。
```

**示例 3:**

```
输入: "pwwkew"
输出: 3
解释: 因为无重复字符的最长子串是 "wke"，所以其长度为 3。
     请注意，你的答案必须是 子串 的长度，"pwke" 是一个子序列，不是子串。
```

#### 3.2.解法
##### 3.2.1 方法一 O(n ^ 2)

固定首字符 bruce

```python
class Solution:
    def lengthOfLongestSubstring(self, s: str) -> int:
        max_l = 0
        for i in range(len(s)):
            c_set = set()
            for j, c in enumerate(s[i:]):
                if c in c_set:
                    max_l = max(max_l, j)
                    break
                else:
                    c_set.add(c)
                    max_l = max(max_l, j+1)
        return max_l
```
##### 3.2.2 方法二 Bruce O(n ^ 3)

```java
public class Solution {
    public int lengthOfLongestSubstring(String s) {
        int n = s.length();
        int ans = 0;
        for (int i = 0; i < n; i++)
            for (int j = i + 1; j <= n; j++)
                if (allUnique(s, i, j)) ans = Math.max(ans, j - i);
        return ans;
    }

    public boolean allUnique(String s, int start, int end) {
        Set<Character> set = new HashSet<>();
        for (int i = start; i < end; i++) {
            Character ch = s.charAt(i);
            if (set.contains(ch)) return false;
            set.add(ch);
        }
        return true;
    }
}
```

##### 3.2.3 方法三 滑动窗口 O(n)

暴力法非常简单。但它太慢了。那么我们该如何优化它呢？

在暴力法中，我们会反复检查一个子字符串是否含有有重复的字符，但这是没有必要的。如果从索引 i*i* 到 j - 1*j*−1 之间的子字符串 s_{ij}*s**i**j* 已经被检查为没有重复字符。我们只需要检查 s[j]*s*[*j*] 对应的字符是否已经存在于子字符串 s_{ij}*s**i**j* 中。

要检查一个字符是否已经在子字符串中，我们可以检查整个子字符串，这将产生一个复杂度为 O(n^2)*O*(*n*2) 的算法，但我们可以做得更好。

通过使用 HashSet 作为滑动窗口，我们可以用 O(1)*O*(1) 的时间来完成对字符是否在当前的子字符串中的检查。

滑动窗口是数组/字符串问题中常用的抽象概念。 窗口通常是在数组/字符串中由开始和结束索引定义的一系列元素的集合，即 [i, j)[*i*,*j*)（左闭，右开）。而滑动窗口是可以将两个边界向某一方向“滑动”的窗口。例如，我们将 [*i*,*j*) 向右滑动 1个元素，则它将变为 [i+1, j+1)（左闭，右开）。

回到我们的问题，我们使用 HashSet 将字符存储在当前窗口 [i, j)[*i*,*j*)（最初 j = i*j*=*i*）中。 然后我们向右侧滑动索引 j*j*，如果它不在 HashSet 中，我们会继续滑动 j*j*。直到 s[j] 已经存在于 HashSet 中。此时，我们找到的没有重复字符的最长子字符串将会以索引 i*i* 开头。如果我们对所有的 i*i* 这样做，就可以得到答案。

```python
    def lengthOfLongestSubstring(self, s: str) -> int:
        cset = set()
        i, j = 0, 0
        res = 0
        while j < len(s):
            if s[j] in cset:
                res = max(res, j - i)
                cset.remove(s[i])
                i += 1
            else:
                cset.add(s[j])
                j += 1
                
        res = max(res, j - i)
        return res
```



```java
import java.lang.Math;
import java.util.HashSet;
class Solution {
    public int lengthOfLongestSubstring(String s) {
        if(s == null || "".equals(s)){
            return 0;
        }
        int i = 0, j = 0, ans = 0;
        HashSet<Character> set = new HashSet<>();
        while(j < s.length()){
            if(!set.contains(s.charAt(j))){
                set.add(s.charAt(j++));
                ans = Math.max(ans, j - i);
            }else{
                set.remove(s.charAt(i++));
            }
        }
        return ans;
    }
}
```

##### 3.2.4 方法四 改进的滑动窗口 O(n)

上述的方法最多需要执行 2n 个步骤。事实上，它可以被进一步优化为仅需要 n 个步骤。我们可以定义字符到索引的映射，而不是使用集合来判断一个字符是否存在。 当我们找到重复的字符时，我们可以立即跳过该窗口。

也就是说，如果 s[j] 在 [i, j 范围内有与 j' 重复的字符，我们不需要逐渐增加 i 。 我们可以直接跳过 [i，j'][*i*，*j*′] 范围内的所有元素，并将 i变为 j' + 1。

```python
class Solution:
    def lengthOfLongestSubstring(self, s: str) -> int:
        dic = dict()
        i, j = 0, 0
        res = 0
        while j < len(s):
            if s[j] in dic:
                i = max(i, dic[s[j]])
            res = max(res, j - i + 1)
            dic[s[j]] = j + 1
            j += 1
        return res
```

```python
import java.lang.Math;
import java.util.HashMap;
class Solution {
    public int lengthOfLongestSubstring(String s) {
        if(s == null || "".equals(s)){
            return 0;
        }
        int i = 0, j = 0, ans = 0;
        HashMap<Character, Integer> map = new HashMap<>();
        while(j < s.length()){
            if(map.containsKey(s.charAt(j)))
          			# key: i一定是往后走
                i = Math.max(map.get(s.charAt(j)), i);
            ans = Math.max(ans, j - i + 1);
            map.put(s.charAt(j), ++j);
        }
        return ans;
    }
}
```

##### 

### 5.最长回文子串

#### 5.1.题目描述

给定一个字符串 `s`，找到 `s` 中最长的回文子串。你可以假设 `s` 的最大长度为 1000。

**示例 1：**

```
输入: "babad"
输出: "bab"
注意: "aba" 也是一个有效答案。
```

**示例 2：**

```
输入: "cbbd"
输出: "bb"
```

#### 5.2.解法

##### 5.2.1 方法一 DP O(n**2)

![image-20190713175743436](/Users/zhengxinzhi/typora_img/LeetCode题目总结/image-20190713175743436.png)

```java
import java.util.Arrays;
class Solution {
    public String longestPalindrome(String s) {
        if(s.length() <= 1){
            return s;
        }
        int left = 0, right = 0, len = 0;
        boolean[][] dp = new boolean[s.length()][s.length()];
        // i从后往前为了j-i从小往大
        for(int i = s.length() - 1; i >= 0; i--){
            for(int j = i; j < s.length(); j++){
                // 若s.charAt(i) == s.charAt(j) && （j - i <= 2）一定回文
                // 否则就要判断dp[i+1][j-1]
                dp[i][j] = s.charAt(i) == s.charAt(j) && (j - i <= 2 || dp[i+1][j-1]);
                if(dp[i][j] && j - i > right - left){
                    left = i;
                    right = j;
                }
            }
        }
        return s.substring(left, right+1);
    }
}

```

##### 5.2.2 方法二 中心扩散法 O(n**2)

事实上，只需使用恒定的空间，我们就可以在 O*(*n*2) 的时间内解决这个问题。

我们观察到回文中心的两侧互为镜像。因此，回文可以从它的中心展开，并且只有2*n*−1 个这样的中心。

你可能会问，为什么会是 2*n*−1 个，而不是 n 个中心？原因在于所含字母数为偶数的回文的中心可以处于两字母之间（例如 “abba” 的中心在两个 ‘b’ 之间）。

显然所有的回文串都是对称的。长度为奇数回文串以最中间字符的位置为对称轴左右对称，而长度为偶数的回文串的对称轴在中间两个字符之间的空隙。可否利用这种对称性来提高算法效率呢？答案是肯定的。我们知道整个字符串中的所有字符，以及字符间的空隙，都可能是某个回文子串的对称轴位置。可以遍历这些位置，在每个位置上同时向左和向右扩展，直到左右两边的字符不同，或者达到边界。对于一个长度为n的字符串，这样的位置一共有n+n-1=2n-1个，在每个位置上平均大约要进行n/4次字符比较，于是此算法的时间复杂度是O(n^2)。

```python
class Solution:
    def longestPalindrome(self, s: str) -> str:
        """
            边界条件
            中心扩散法
            马拉车法
        """
        def getLen(s, i, j):
            res = 0
            while i >= 0 and j < len(s) and s[i] == s[j]:
                res = max(res, j - i + 1)
                i -= 1
                j += 1
            return res
            
        l, r = 0, 0
        for i in range(len(s)):
            len1 = getLen(s, i, i)
            if len1 > r - l + 1:
                l, r = i - len1 // 2, i + len1 // 2
            len2 = getLen(s, i, i + 1)
            if len2 > r - l + 1:
                l, r = i - (len2 - 1) // 2, i + len2 // 2
        return s[l: r + 1]
```



```java
class Solution {
    public String longestPalindrome(String s) {
        if(s.length() <= 1){
            return s;
        }
        int start = 0, end = 0;
        for(int i = 0; i < s.length(); i++){
            // 单独的中心点
            int len1 = expandAroundCenter(s, i, i);
            // 两个相同地中心点
            int len2 = expandAroundCenter(s, i, i + 1);
            int len = Math.max(len1, len2);
            if(len >= end - start + 1){
                start = i - (len1 - 1) / 2;
                end = i + len / 2;
            }
        }
        return s.substring(start, end + 1);
    }
    
    private int expandAroundCenter(String s, int left, int right) {
        // 回文可以是一个中心或者两个相同地中心点
        int L = left, R = right;
        while (L >= 0 && R < s.length() && s.charAt(L) == s.charAt(R)) {
            L--;
            R++;
        }
        return R - L - 1;
    }
}


```

##### 5.2.3 Manacher 算法 TO(n) 马拉车算法

```python
class Solution:
    def longestPalindrome(self, s: str) -> str:
        """
            边界条件
            中心扩散法 O(n**2)
            马拉车法
        """
        # 1.以#填充间隔 解决回文子串奇偶性问题
        A = '^#' + '#'.join(s) + '#&'
        
        # 2.构建辅助数组Len 存储以i为核心的子串半径(不包括i)
        Len = [0] * len(A)
        
        center, right = 0, 0
        for i in range(1, len(A) - 1):
            if i < right:
                Len[i] = min(right - i, Len[2 * center - i])
            while A[i + Len[i] + 1] == A[i - Len[i] - 1]:
                Len[i] += 1
            if i + Len[i] > right:
                center, right = i, i + Len[i]
                
        # key: 求子串长度 记住吧
        maxLen, centerIndex = max((n, i) for i, n in enumerate(Len))
        return s[(centerIndex  - maxLen)//2: (centerIndex  + maxLen)//2]
```



```PYTHON

        def manacher(S):
            """
                回文问题三种解法：
                    1.中心扩散法
                    2.DP
                    3.manacher算法: O(n)
                本函数返回最大回文子字符串
            """
            # 1.解决长度奇偶性带来的对称轴位置问题
            #   其中首尾的作用是中心扩散的时候防止索引越界
            A = '^#' + '#'.join(S) + '#$'
            
            # 2.辅助数组LEN，存储的是以 i 为中心的最长回文的半径(**不包括i这个元素)
            #   LEN数组性质: LEN[i]就是该回文子串在原字符串S中的长度
            LEN = [0] * len(A)
            
            # 3.设置两个变量，right 和 center 。right 代表以 center 为中心的最长回文的右边界，
            #   也就是right = center + LEN[center] - 1。
            center = right = 0
            
            # 4.去掉首尾依次遍历A数组元素
            for i in range(1, len(A) - 1):
                    # 5.i <= right, j和i关于位置center对称, 以j为中心的回文串一定在以center为中心的回文串的内部,
                    #               LEN[i]=LEN[j]
                    #   i > right, 以i为中心的回文串在以center为中心的回文串的外部，
                    #   而大于right的部分我们还没有进行匹配，所以要从right+1位置开始一个一个进行匹配，直到发生失配，
                    #   从而更新P和对应的po以及Len[i]。
                if i < right:
                    LEN[i] = min(right - i, LEN[2 * center - i])
                else:
                    LEN[i] = 0
                # 6.奇数的中心扩散法 # 奇偶性问题由第一步已经解决了
                while A[i + LEN[i] + 1] == A[i - LEN[i] - 1]:
                    LEN[i] += 1
                # 7..如果i的右边界 > 原来的右边界，更新
                if i + LEN[i] > right:
                    center, right = i, i + LEN[i]
            
            # print('修改后的字符串', A)
            # print('辅助半径数组', LEN)
            maxLen, centerIndex = max((n, i) for i, n in enumerate(LEN))
            print((maxLen, centerIndex))
            # return maxLen  求最大回文子串长度
            # return s[(centerIndex  - maxLen)//2: (centerIndex  + maxLen)//2] 求最大回文子串
            # return sum([(1+x)//2 for x in LEN]) 求回文子串数量
            return S[(centerIndex  - maxLen)//2: (centerIndex  + maxLen)//2]

```



### 6.Z字形变幻

#### 1.题目描述

```
将一个给定字符串根据给定的行数，以从上往下、从左到右进行 Z 字形排列。

比如输入字符串为 "LEETCODEISHIRING" 行数为 3 时，排列如下：

L   C   I   R
E T O E S I I G
E   D   H   N
之后，你的输出需要从左往右逐行读取，产生出一个新的字符串，比如："LCIRETOESIIGEDHN"。

请你实现这个将字符串进行指定行数变换的函数：

string convert(string s, int numRows);
示例 1:

输入: s = "LEETCODEISHIRING", numRows = 3
输出: "LCIRETOESIIGEDHN"
示例 2:

输入: s = "LEETCODEISHIRING", numRows = 4
输出: "LDREOEIIECIHNTSG"
解释:

L     D     R
E   O E   I I
E C   I H   N
T     S     G

```



#### 2.解法
##### 2.1 方法一 TO(n)  SO(n)

```python
class Solution:
    def convert(self, s: str, numRows: int) -> str:
        res = ["" for _ in range(numRows)]
        i = 0
        Up = False
        for c in s:
            res[i] += c
            if Up:
                if i > 0:
                    i -= 1
                else:
                    Up = False
                    i += 1
            else:
                if i < numRows - 1:
                    i += 1
                else:
                    Up = True
                    i -= 1
        return "".join(res)
```



```java
class Solution {
    public String convert(String s, int numRows) {
        if(numRows == 1){
            return s;
        }
        ArrayList<ArrayList<Character>> rows = new ArrayList<>();
        for(int i = 0; i < numRows; i++){
            ArrayList<Character> list = new ArrayList<>();
            rows.add(list);
        }
        for(int i = 0, j = 0, sign = 1; i < s.length(); i++){
            rows.get(j).add(s.charAt(i));
            if(j == numRows - 1 || (i != 0 && j == 0)){
                sign = -sign;
            }
            j += sign;
        }
        StringBuilder builder = new StringBuilder();
        for(ArrayList<Character> list: rows){
            for(Character c: list){
                builder.append(c);
            }
        }
        return builder.toString();
    }
}


```

###12.整数转罗马数字

#### 题目描述

罗马数字包含以下七种字符： I， V， X， L，C，D 和 M。

字符          数值
I             1
V             5
X             10
L             50
C             100
D             500
M             1000
例如， 罗马数字 2 写做 II ，即为两个并列的 1。12 写做 XII ，即为 X + II 。 27 写做  XXVII, 即为 XX + V + II 。

通常情况下，罗马数字中小的数字在大的数字的右边。但也存在特例，例如 4 不写做 IIII，而是 IV。数字 1 在数字 5 的左边，所表示的数等于大数 5 减小数 1 得到的数值 4 。同样地，数字 9 表示为 IX。这个特殊的规则只适用于以下六种情况：

I 可以放在 V (5) 和 X (10) 的左边，来表示 4 和 9。
X 可以放在 L (50) 和 C (100) 的左边，来表示 40 和 90。 
C 可以放在 D (500) 和 M (1000) 的左边，来表示 400 和 900。
给定一个整数，将其转为罗马数字。输入确保在 1 到 3999 的范围内。

示例 1:

输入: 3
输出: "III"
示例 2:

输入: 4
输出: "IV"
示例 3:

输入: 9
输出: "IX"
示例 4:

输入: 58
输出: "LVIII"
解释: L = 50, V = 5, III = 3.
示例 5:

输入: 1994
输出: "MCMXCIV"
解释: M = 1000, CM = 900, XC = 90, IV = 4.


#### 解法

##### 解法一 Hash
```python
class Solution:
    def intToRoman(self, num: int) -> str:
        """
            数字范围 1 - 3999
            边界条件
            特殊条件
        """
        dic = {1: 'I', 5: 'V', 10: 'X', 50: 'L', 100: 'C', 500: 'D', 1000: 'M', 
         4: 'IV', 9: 'IX', 40: 'XL', 90: 'XC', 400: 'CD', 900: 'CM'}
        
        ans = ""
        bit = 0
        while num:
            a = num % 10 
            if a == 4 or a == 9:
                ans = dic[a * (10 ** bit)] + ans
            elif a >= 1 and a < 4:
                ans = dic[10 ** bit] * a + ans
            elif a >= 5 and a < 9:
                print(a)
                ans = dic[10 ** bit * 5] + dic[10 ** bit] * (a - 5) + ans
            num //= 10
            bit += 1
        return ans
```

##### 解法二 噗噗噗
```python
public static String intToRoman(int num) {
    String M[] = {"", "M", "MM", "MMM"};
    String C[] = {"", "C", "CC", "CCC", "CD", "D", "DC", "DCC", "DCCC", "CM"};
    String X[] = {"", "X", "XX", "XXX", "XL", "L", "LX", "LXX", "LXXX", "XC"};
    String I[] = {"", "I", "II", "III", "IV", "V", "VI", "VII", "VIII", "IX"};
    return M[num/1000] + C[(num%1000)/100] + X[(num%100)/10] + I[num%10];
}
```

##### 解法三 推荐

```python
class Solution:
    def intToRoman(self, num: int) -> str:
        values = [ 1000, 900, 500, 400, 100, 90, 50, 40, 10, 9, 5, 4, 1 ]
        numerals = [ "M", "CM", "D", "CD", "C", "XC", "L", "XL", "X", "IX", "V", "IV", "I" ]
        
        res = ""
        for i in range(len(values)):
            if num // values[i] > 0:
                res += (num // values[i]) * numerals[i]
                num %= values[i]
            
        return res
```



### 20.有效的括号

#### 题目描述

给定一个只包括 '('，')'，'{'，'}'，'['，']' 的字符串，判断字符串是否有效。

有效字符串需满足：

左括号必须用相同类型的右括号闭合。
左括号必须以正确的顺序闭合。
注意空字符串可被认为是有效字符串。

示例 1:

输入: "()"
输出: true
示例 2:

输入: "()[]{}"
输出: true
示例 3:

输入: "(]"
输出: false
示例 4:

输入: "([)]"
输出: false
示例 5:

输入: "{[]}"
输出: true

#### 解法

##### 解法一
```python
class Solution:
    def isValid(self, s: str) -> bool:
        stack = []
        mapping = {")": "(", "}": "{", "]": "["}
        for c in s:
            if c not in mapping:
                stack.append(c)
            else:
                top_element = stack.pop() if stack else '#'
                if mapping[c] != top_element:
                    return False
                
        return not stack
```

### 71.简化路径

#### 题目描述

以 Unix 风格给出一个文件的绝对路径，你需要简化它。或者换句话说，将其转换为规范路径。

在 Unix 风格的文件系统中，一个点（.）表示当前目录本身；此外，两个点 （..） 表示将目录切换到上一级（指向父目录）；两者都可以是复杂相对路径的组成部分。更多信息请参阅：Linux / Unix中的绝对路径 vs 相对路径

请注意，返回的规范路径必须始终以斜杠 / 开头，并且两个目录名之间必须只有一个斜杠 /。最后一个目录名（如果存在）不能以 / 结尾。此外，规范路径必须是表示绝对路径的最短字符串。

 

示例 1：

输入："/home/"
输出："/home"
解释：注意，最后一个目录名后面没有斜杠。
示例 2：

输入："/../"
输出："/"
解释：从根目录向上一级是不可行的，因为根是你可以到达的最高级。
示例 3：

输入："/home//foo/"
输出："/home/foo"
解释：在规范路径中，多个连续斜杠需要用一个斜杠替换。
示例 4：

输入："/a/./b/../../c/"
输出："/c"
示例 5：

输入："/a/../../b/../c//.//"
输出："/c"
示例 6：

输入："/a//b////c/d//././/.."
输出："/a/b/c"


#### 解法

##### 解法一 栈
```python
class Solution:
    def simplifyPath(self, path: str) -> str:
        stack = []
        words = path.split("/")

        for w in words:
            if w == '' or w == '.':
                continue
            if w == '..':
                if stack:
                    stack.pop()
            else:
                stack.append(w)
        res = "/" + "/".join(stack)
        return res
```

##### 解法二
```python

```



### 91.解码方法

#### 题目描述

一条包含字母 A-Z 的消息通过以下方式进行了编码：

'A' -> 1
'B' -> 2
...
'Z' -> 26
给定一个只包含数字的非空字符串，请计算解码方法的总数。

示例 1:

输入: "12"
输出: 2
解释: 它可以解码为 "AB"（1 2）或者 "L"（12）。
示例 2:

输入: "226"
输出: 3
解释: 它可以解码为 "BZ" (2 26), "VF" (22 6), 或者 "BBF" (2 2 6) 。


#### 解法

##### 解法一 DP

后一个字符与前缀若干个字符的组合可以通过公式 dp_i = dp_{i-1}+ dp_{i-2} 计算出来，dp[i]以当前i为结尾有多少种方法

```python
class Solution:
    def numDecodings(self, s: str) -> int:
        """
            DP
            边界条件:
                1.首字母为0
                2.中间某字母为0
        """
        if not s or s[0] == '0':
            return 0
        
        def check(num):
            return num >=10 and num <=26
        # dp[i]以当前i为结尾有多少种方法
        dp = [0] * (len(s) + 1)
        dp[0] = 1
        dp[1] = 1
        
        for i in range(2, len(s) + 1):
            dp[i] = (dp[i - 1] if s[i - 1] != '0' else 0) + (dp[i - 2] if check(int(s[i-2: i])) else 0) 
        return dp[len(s)]
```

##### 解法二
```python

```

### 125. 验证回文串

#### 125.1.题目描述

给定一个字符串，验证它是否是回文串，只考虑字母和数字字符，可以忽略字母的大小写。

**说明：**本题中，我们将空字符串定义为有效的回文串。

**示例 1:**

```
输入: "A man, a plan, a canal: Panama"
输出: true
```

**示例 2:**

```
输入: "race a car"
输出: false
```

#### 125.2.解法

##### 125.2.1 方法一

```python
class Solution:
    def isPalindrome(self, s: str) -> bool:
        # 1.整理
        ss = ""
        for c in s:
            if (c <= '9'and c >= '0') or (c <='z' and c >= 'a') or (c <='Z' and c >= 'A'):
                ss += c
        ss = ss.lower()
        # 2.验证
        if not ss:
            return True
        i, j = 0, len(ss)-1
        while i < j:
            if ss[i] != ss[j]:
                return False
            i += 1
            j -= 1
        return True
```

##### 125.2.2 方法二

```java
public class Solution {
    public boolean isPalindrome(String s) {
        String actual = s.replaceAll("[^A-Za-z0-9]", "").toLowerCase();
        String rev = new StringBuffer(actual).reverse().toString();
        return actual.equals(rev);
    }
}
```

### 394.字符串编码

#### 题目描述

给定一个经过编码的字符串，返回它解码后的字符串。

编码规则为: k[encoded_string]，表示其中方括号内部的 encoded_string 正好重复 k 次。注意 k 保证为正整数。

你可以认为输入字符串总是有效的；输入字符串中没有额外的空格，且输入的方括号总是符合格式要求的。

此外，你可以认为原始数据不包含数字，所有的数字只表示重复的次数 k ，例如不会出现像 3a 或 2[4] 的输入。

示例:

s = "3[a]2[bc]", 返回 "aaabcbc".
s = "3[a2[c]]", 返回 "accaccacc".
s = "2[abc]3[cd]ef", 返回 "abcabccdcdcdef".


#### 解法

##### 解法一 栈

```python
class Solution:
    def decodeString(self, s: str) -> str:
        stack = []
        res = ""
        multi = 0
        
        for c in s:
            if c == '[':
                stack.append([multi, res])
                res, multi = "", 0
            elif c == ']':
                cur_multi, last_res = stack.pop()
                res = last_res + cur_multi * res
            elif '0' <= c <= '9':
                multi = multi * 10 + int(c)            
            else:
                res += c
        return res 
```



```python
class Solution:
    def decodeString(self, s: str) -> str:
        """
            递归法：
            dfs：
            边界条件：
                字符串空
                数字不只是个位数
                括号可以嵌套
                不是必须以扩回结尾
        """
        def is_number(c):
            if type(c) == int:
                return True
            return c >= '0' and c <= '9'
        
        def is_char(c):
            if type(c) == int:
                return False
            return c not in ['[', ']'] 
        
        if not s:
            return s
        
        
        ans = ''
        stack = []
        for c in s:
            if is_number(c): 
                if stack and is_number(stack[-1]):
                    
                    stack[-1] = (stack[-1] * 10 + int(c))
                else:
                    stack.append(int(c))
            elif c == '[':
                stack.append(c)
            elif c == ']':
                if stack:
                    string = ''
                    if is_char(stack[-1]):
                        string = stack.pop()
                    stack.pop()
                    cnt = stack.pop()
                    string = string * cnt
                    if stack:
                        if is_char(stack[-1]):
                            stack[-1] = stack[-1] + string
                        else:
                            stack.append(string)
                    else:
                        ans += string
            else:
                if stack and is_char(stack[-1]):
                    stack[-1] = stack[-1] + c
                else:
                    stack.append(c)
        if stack:
            ans += stack[-1]
        return ans
                    
        
```

##### 解法二
```python

```

### 395.至少有K个重复字符的最长子串

#### 题目描述

找到给定字符串（由小写字符组成）中的最长子串 T ， 要求 T 中的每一字符出现次数都不少于 k 。输出 T 的长度。

示例 1:

输入:
s = "aaabb", k = 3

输出:
3

最长子串为 "aaa" ，其中 'a' 重复了 3 次。
示例 2:

输入:
s = "ababbc", k = 2

输出:
5

最长子串为 "ababb" ，其中 'a' 重复了 2 次， 'b' 重复了 3 次。


#### 解法

##### 解法一 找到边界 分而治之
```python
class Solution:
    def longestSubstring(self, s: str, k: int) -> int:
        """
            分而治之
            由于少于k个的字符一定不能出现在结果中 利用这个分割 递归
        """
        if not s:
            return 0
        for c in s:
            if s.count(c) < k:
                return max(self.longestSubstring(sub_s, k) for sub_s in s.split(c))
        return len(s)
        
```

##### 解法二
```python

```

### 409. 最长回文串

#### 409.1 题目描述

给定一个包含大写字母和小写字母的字符串，找到通过这些字母构造成的最长的回文串。

在构造过程中，请注意区分大小写。比如 `"Aa"` 不能当做一个回文字符串。

**注意:**
假设字符串的长度不会超过 1010。

**示例 1:**

```
输入:
"abccccdd"

输出:
7

解释:
我们可以构造的最长的回文串是"dccaccd", 它的长度是 7。
```



#### 409.2 解法

##### 409.2.1 Greedy [Accepted]

**Intuition**

A palindrome consists of letters with equal partners, plus possibly a unique center (without a partner). The letter `i` from the left has its partner `i` from the right. For example in `'abcba'`, `'aa'` and `'bb'` are partners, and `'c'` is a unique center.

Imagine we built our palindrome. It consists of as many partnered letters as possible, plus a unique center if possible. This motivates a greedy approach.

**Algorithm**

For each letter, say it occurs `v` times. We know we have `v // 2 * 2` letters that can be partnered for sure. For example, if we have `'aaaaa'`, then we could have `'aaaa'` partnered, which is `5 // 2 * 2 = 4` letters partnered.

At the end, if there was any `v % 2 == 1`, then that letter could have been a unique center. Otherwise, every letter was partnered. To perform this check, we will check for `v % 2 == 1` and `ans % 2 == 0`, the latter meaning we haven't yet added a unique center to the answer.

**Complexity Analysis**

- Time Complexity: O(N)*O*(*N*), where N*N* is the length of `s`. We need to count each letter.
- Space Complexity: O(1)*O*(1), the space for our count, as the alphabet size of `s` is fixed. We should also consider that in a bit complexity model, technically we need O(\log N)*O*(log*N*) bits to store the count values.

```python
    def longestPalindrome(self, s):
        """
        :type s: str
        :rtype: int
        """
        ans = 0
        for k, v in collections.Counter(s).most_common():
            ans += v // 2 * 2
            if ans % 2 == 0 and v % 2 == 1:
                ans += 1
        return ans
```

##### 409.2.2 利用set统计奇数次

```python
class Solution(object):
    def longestPalindrome(self, s):
        """
        :type s: str
        :rtype: int
        """
        hash = set()
        for c in s:
            if c not in hash:
                hash.add(c)
            else:
                hash.remove(c)
        # len(hash) is the number of the odd letters
        return len(s) - len(hash) + 1 if len(hash) > 0 else len(s)
```

##### 409.2.3 利用位运算统计奇数次

我统计有多少个字母出现奇数次。因为我们可以使用所有字母，除了每个奇数字母，我们必须留下一个，除了我们可以使用的一个。

```python
def longestPalindrome(self, s):
    odds = sum(v & 1 for v in collections.Counter(s).values())
    # bool(odds)作用是如果odds不为0 则bool(odds)为1
    return len(s) - odds + bool(odds)
```

##### 409.2.4 字符数组 类似上面3解法

```java
class Solution {
    public int longestPalindrome(String s) {
        if(s == null || "".equals(s)){
            return 0;
        }
        int[] charMap = new int[128];
        int maxLength = 0;
        int flag = 0;
        for(int i = 0; i < s.length(); i++){
            // 统计各字符数量
            charMap[(int)s.charAt(i)] += 1;
        }
        for(int i = 0; i < charMap.length; i++){
            
            if((charMap[i] & 1) == 0){
                // 如果字符数为even 直接加到结果中
                maxLength += charMap[i];
            }else{
                // 如果字符数为odd， 减1加到结果中，且中心点置为1
                maxLength += charMap[i] - 1;
                flag = 1;
            }
        }
        return maxLength + flag;
    }
}
```

### [438. 找到字符串中所有字母异位词](https://leetcode-cn.com/problems/find-all-anagrams-in-a-string/)

#### 题目描述

给定一个字符串 s 和一个非空字符串 p，找到 s 中所有是 p 的字母异位词的子串，返回这些子串的起始索引。

字符串只包含小写英文字母，并且字符串 s 和 p 的长度都不超过 20100。

说明：

字母异位词指字母相同，但排列不同的字符串。
不考虑答案输出的顺序。
示例 1:

输入:
s: "cbaebabacd" p: "abc"

输出:
[0, 6]

解释:
起始索引等于 0 的子串是 "cba", 它是 "abc" 的字母异位词。
起始索引等于 6 的子串是 "bac", 它是 "abc" 的字母异位词。
 示例 2:

输入:
s: "abab" p: "ab"

输出:
[0, 1, 2]

解释:
起始索引等于 0 的子串是 "ab", 它是 "ab" 的字母异位词。
起始索引等于 1 的子串是 "ba", 它是 "ab" 的字母异位词。
起始索引等于 2 的子串是 "ab", 它是 "ab" 的字母异位词。


#### 解法

##### 解法一 滑动窗口
```python
class Solution:
    def findAnagrams(self, s: str, p: str) -> List[int]:
        """
            滑动窗口
        """
        res = []
        window = {}     # 记录窗口中各个字符数量的字典
        needs = {}      # 记录目标字符串中各个字符数量的字典
            
        for c in p:
            needs[c] = needs.get(c, 0) + 1
            
        Len, limit = len(p), len(s)
        l = r = 0
        while r < limit:
            c = s[r]
            if c not in needs:
                window.clear()    
                l = r = r + 1
            else:
                window[c] = window.get(c, 0) + 1
                if r - l + 1 == Len:
                    if window == needs:
                        res.append(l)
                    window[s[l]] -= 1
                    l += 1
                r += 1
        return res
        
    
```

##### 解法二
```python

```

### 647.回文子串

#### 题目描述

给定一个字符串，你的任务是计算这个字符串中有多少个回文子串。

具有不同开始位置或结束位置的子串，即使是由相同的字符组成，也会被计为是不同的子串。

示例 1:

输入: "abc"
输出: 3
解释: 三个回文子串: "a", "b", "c".
示例 2:

输入: "aaa"
输出: 6
说明: 6个回文子串: "a", "a", "a", "aa", "aa", "aaa".
注意:

输入的字符串长度不会超过1000。


#### 解法

##### 解法一 中心扩散法 推荐

重点是一个中心还是两个中心

```python
class Solution:
    def countSubstrings(self, s: str) -> int:
        """
            不同的开始和结束O(n) 共 O(n ** 2) 中心扩散法
        """
        def isPalidrime(s, l, r):
            """
                一个中心l == r还是两个中心 l = r - 1
            """
            res = 0
            while l >= 0 and r < length and s[l] == s[r]:
                res += 1
                l -= 1
                r += 1
            return res
            
        length = len(s)
        ans = 0
        for i in range(length):
            ans += isPalidrime(s, i, i)
            ans += isPalidrime(s, i, i + 1)
        return ans
        
```

##### 解法二 Manacher
```python
class Solution:
    def countSubstrings(self, s: str) -> int:

        def manacher(S):
            """
                回文问题三种解法：
                    1.中心扩散法
                    2.DP
                    3.manacher算法: O(n)
                本函数返回最大回文子字符串
            """
            # 1.解决长度奇偶性带来的对称轴位置问题
            #   其中首尾的作用是中心扩散的时候防止索引越界
            A = '^#' + '#'.join(S) + '#$'
            
            # 2.辅助数组LEN，存储的是以 i 为中心的最长回文的半径(**不包括i这个元素)
            #   LEN数组性质: LEN[i]就是该回文子串在原字符串S中的长度
            LEN = [0] * len(A)
            
            # 3.设置两个变量，right 和 center 。right 代表以 center 为中心的最长回文的右边界，
            #   也就是right = center + LEN[center] - 1。
            center = right = 0
            
            # 4.去掉首尾依次遍历A数组元素
            for i in range(1, len(A) - 1):
                    # 5.i <= right, j和i关于位置center对称, 以j为中心的回文串一定在以center为中心的回文串的内部,
                    #               LEN[i]=LEN[j]
                    #   i > right, 以i为中心的回文串在以center为中心的回文串的外部，
                    #   而大于right的部分我们还没有进行匹配，所以要从right+1位置开始一个一个进行匹配，直到发生失配，
                    #   从而更新P和对应的po以及Len[i]。
                if i < right:
                    LEN[i] = min(right - i, LEN[2 * center - i])
                else:
                    LEN[i] = 0
                # 6.奇数的中心扩散法
                while A[i + LEN[i] + 1] == A[i - LEN[i] - 1]:
                    LEN[i] += 1
                # 7..如果i的右边界 > 原来的右边界，更新
                if i + LEN[i] > right:
                    center, right = i, i + LEN[i]
            
            # print('修改后的字符串', A)
            # print('辅助半径数组', LEN)
            maxLen, centerIndex = max((n, i) for i, n in enumerate(LEN))
            print((maxLen, centerIndex))
            # return maxLen  求最大回文子串长度
            # return s[(centerIndex  - maxLen)//2: (centerIndex  + maxLen)//2] 求最大回文子串
            # return sum([(1+x)//2 for x in LEN]) 求回文子串数量
            # LEN代表的是不包含中心点的半径,也是以它为中心点的长度, 奇数要加一除以二才能得到以它为中心点的回文子串数量, 偶数加一不影响结果
            return sum([(1+x)//2 for x in LEN])

        return manacher(s)
```

## 数组

### [4. 寻找两个有序数组的中位数](https://leetcode-cn.com/problems/median-of-two-sorted-arrays/) [hard]

#### 题目描述

给定两个大小为 m 和 n 的有序数组 nums1 和 nums2。

请你找出这两个有序数组的中位数，并且要求算法的时间复杂度为 O(log(m + n))。

你可以假设 nums1 和 nums2 不会同时为空。

示例 1:

nums1 = [1, 3]
nums2 = [2]

则中位数是 2.0
示例 2:

nums1 = [1, 2]
nums2 = [3, 4]

则中位数是 (2 + 3)/2 = 2.5


#### 解法

##### 解法一 递归 + 二分

https://mp.weixin.qq.com/s/FBlH7o-ssj_iMEPLcvsY2w

```python
class Solution(object):
    def findMedianSortedArrays(self, nums1, nums2):
        """
            A[k/2] = B[k/2],那么第 k 大的数就是 A[k/2]

            A[k/2] > B[k/2],那么第 k 大的数肯定在 A[0:k/2+1] 和 B[k/2:] 中，这样就将原来的所有数的总和减少到一半了，再在这个范围里面找第 k/2 大的数即可，这样也达到了二分查找的区别了。

            A[k/2] < B[k/2]，那么第 k 大的数肯定在 B[0:k/2+1]和 A[k/2:] 中，同理在这个范围找第 k/2 大的数就可以了。

            [注意] 由于该方法主要考虑前面元素 所以实现时可以不用删除A[0:k/2+1] B[0:k/2+1]剩下的元素
        """
        def getKth(nums1, s1, t1, nums2, s2, t2, k):
            len1 = t1 - s1 + 1
            len2 = t2 - s2 + 1
            if len1 == 0:
                return nums2[s2 + k - 1]
            if len2 == 0:
                return nums1[s1 + k - 1]
            if k == 1:
                print(s1, s2, t1, t2)
                return min(nums1[s1], nums2[s2])
            
            i = s1 + min(len1, k // 2) - 1
            j = s2 + min(len2, k // 2) - 1
            if nums1[i] > nums2[j]:
                return getKth(nums1, s1, t1, nums2, j + 1, t2, k - (j - s2 + 1))
            else:
                return getKth(nums1, i + 1, t1, nums2, s2, t2, k - (i - s1 + 1))


        n, m = len(nums1), len(nums2)
        # trick：将偶数和奇数的情况合并，如果是奇数，会求两次同样的 k 
        left_mid = (n + m + 1) // 2
        right_mid = (n + m + 2) // 2
        return (getKth(nums1, 0, n - 1, nums2, 0, m - 1, left_mid) + getKth(nums1, 0, n - 1, nums2, 0, m - 1, right_mid)) * 0.5

```

##### 解法二
```python

```

### 11.盛最多水的容器

#### 题目描述

给定 n 个非负整数 a1，a2，...，an，每个数代表坐标中的一个点 (i, ai) 。在坐标内画 n 条垂直线，垂直线 i 的两个端点分别为 (i, ai) 和 (i, 0)。找出其中的两条线，使得它们与 x 轴共同构成的容器可以容纳最多的水。

说明：你不能倾斜容器，且 n 的值至少为 2

图中垂直线代表输入数组 [1,8,6,2,5,4,8,3,7]。在此情况下，容器能够容纳水（表示为蓝色部分）的最大值为 49。

示例:

输入: [1,8,6,2,5,4,8,3,7]
输出: 49

#### 解法

##### 解法一 双指针法 O(n) 夹逼

这种方法背后的思路在于，两线段之间形成的区域总是会受到其中较短那条长度的限制。此外，两线段距离越远，得到的面积就越大。

我们在由线段长度构成的数组中使用两个指针，一个放在开始，一个置于末尾。 此外，我们会使用变量 maxareamaxarea 来持续存储到目前为止所获得的最大面积。 在每一步中，我们会找出指针所指向的两条线段形成的区域，更新 maxarea，并将指向较短线段的指针向较长线段那端移动一步。

这种方法如何工作？

最初我们考虑由最外围两条线段构成的区域。现在，为了使面积最大化，我们需要考虑更长的两条线段之间的区域。如果我们试图将指向较长线段的指针向内侧移动，矩形区域的面积将受限于较短的线段而不会获得任何增加。但是，在同样的条件下，移动指向较短线段的指针尽管造成了矩形宽度的减小，但却可能会有助于面积的增大。因为移动较短线段的指针会得到一条相对较长的线段，这可以克服由宽度减小而引起的面积减小。

```python
class Solution:
    def maxArea(self, height: List[int]) -> int:
        """
            双指针
            贪心 两端较短的往内移动
        """
        l, r = 0, len(height) - 1
        res = 0
        while l < r:
            res = max(res, (r - l) * min(height[r], height[l]))
            if height[l] <= height[r]:
                l += 1
            else:
                r -= 1
        return res
                
        
```



```java
class Solution {
    public int maxArea(int[] height) {
        int ans = 0, i = 0, j = height.length - 1;
        while(i < j){
            ans = Math.max((j - i) * Math.min(height[i], height[j]), ans);
            if (height[i] <= height[j]){
                i++;
            }else{
                j--;
            }
        }
        return ans;
    }
}

```

##### 解法二 bruce O(n**2)
```java
    public int maxArea(int[] height) {
        int ans = 0;
        for(int i = 0; i < height.length - 1; i++){
            for(int j = i + 1; j < height.length; j++){
                ans = Math.max((j - i ) * Math.min(height[i], height[j]), ans);
            }
        }
        return ans;
    }

    二分答案法
```

### 15.三数之和

#### 题目描述

给定一个包含 n 个整数的数组 nums，判断 nums 中是否存在三个元素 a，b，c ，使得 a + b + c = 0 ？找出所有满足条件且不重复的三元组。

注意：答案中不可以包含重复的三元组。

例如, 给定数组 nums = [-1, 0, 1, 2, -1, -4]，

满足要求的三元组集合为：
[
  [-1, 0, 1],
  [-1, -1, 2]
]

#### 解法

##### 解法一 O(n**2) hash法 较慢

遍历两个树 用hash寻找第三个数字

减枝 剪掉i数字相同的情况和剪掉j数字相同的情况

```java
class Solution {
    public List<List<Integer>> threeSum(int[] nums) {
        Arrays.sort(nums);

        HashMap<Integer, Integer> numsMap = new HashMap<Integer, Integer>();
        for(int i = 0; i < nums.length; i++){
            if(numsMap.containsKey(nums[i])){
                numsMap.put(nums[i], numsMap.get(nums[i]) + 1);
            }else{
                numsMap.put(nums[i], 0);
            }
        }
      
        for(int i = 0; i < nums.length - 2; i++){
            if(i != 0 && nums[i] == nums[i-1]){
                i = i + numsMap.get(nums[i]) - 1;
                continue;
            }
            for(int j = i + 1; j < nums.length - 1; j++){
                if(j - i > 1 && nums[j] == nums[j-1]){
                    if(nums[i] == nums[j]){
                        j = j + numsMap.get(nums[j]) - 2;
                    }else{
                        j = j + numsMap.get(nums[j]) - 1;
                    }
                    continue;
                }
                int num3 = - nums[i] - nums[j];
                if(num3 == nums[j+1]){
                    ArrayList<Integer> temp = new ArrayList<Integer>();
                    temp.add(nums[i]);
                    temp.add(num3);
                    temp.add(nums[j]);
                    ans.add(temp);
                }else if (num3 > nums[j+1] && numsMap.containsKey(num3)){
                    ArrayList<Integer> temp = new ArrayList<Integer>();
                    temp.add(nums[i]);
                    temp.add(num3);
                    temp.add(nums[j]);
                    ans.add(temp);
                }
            }
        }
        List<List<Integer>> ans1 = new ArrayList<>(ans);
        return ans1;
    }
}

```

##### 解法二 sort + 2sum + 剪枝

我们的想法是对输入数组进行排序，然后运行三元组中可能的第一个元素的所有索引。对于每个可能的第一个元素，我们对阵列的剩余部分进行标准的双向2Sum扫描。此外，我们希望跳过相同的元素，以避免在答案中重复，而不会像这样设置或smt

```python
    def threeSum(self, nums: List[int]) -> List[List[int]]:
        """
            双指针 夹逼 two-sum
            边界 无重复三元组
        """
        nums = sorted(nums)
        res = []
        for i in range(len(nums) - 2):
            if i > 0 and nums[i] == nums[i - 1]:
                continue
            l, r = i + 1, len(nums) - 1
            while l < r:
                if nums[l] + nums[r] + nums[i] == 0:
                    res.append([nums[i], nums[l], nums[r]])
                    while l < r and nums[l] == nums[l + 1]:
                        l += 1
                    while l < r and nums[r] == nums[r - 1]:
                        r -= 1
                    l += 1
                    r -= 1
                elif nums[l] + nums[r] + nums[i] < 0:
                    l += 1
                else:
                    r -= 1
        return res
```



```java
class Solution {
    public List<List<Integer>> threeSum(int[] nums) {
        Arrays.sort(nums);
        List<List<Integer>> res = new LinkedList<>();
        for(int i = 0; i < nums.length - 2; i++){
            // // skip same result
            if (i > 0 && nums[i] == nums[i - 1]) {              
                continue;
            }
            // bi-2sum
            int lo = i + 1, hi = nums.length - 1, sum = 0 - nums[i];
            while(lo < hi){
                if(nums[lo] + nums[hi] == sum){
                    res.add(Arrays.asList(nums[i], nums[lo], nums[hi]));
                    // 跳过重复元素
                    while (lo < hi && nums[lo] == nums[lo+1]) lo++;
                    while (lo < hi && nums[hi] == nums[hi-1]) hi--;
                    lo++; hi--;
                }else if(nums[lo] + nums[hi] < sum) {
                    lo++;
                }else{
                    hi--;
                }
            }
            
        }
        return res;
    }   
}
```

###18.四数之和

#### 题目描述

给定一个包含 n 个整数的数组 nums 和一个目标值 target，判断 nums 中是否存在四个元素 a，b，c 和 d ，使得 a + b + c + d 的值与 target 相等？找出所有满足条件且不重复的四元组。

注意：

答案中不可以包含重复的四元组。

示例：

给定数组 nums = [1, 0, -1, 0, -2, 2]，和 target = 0。

满足要求的四元组集合为：
[
  [-1,  0, 0, 1],
  [-2, -1, 1, 2],
  [-2,  0, 0, 2]
]


#### 解法

解法一 任意两数组合hash + 索引不一致 O(n**2)

```python
class Solution:
    def fourSum(self, nums, target):
        """
        :type nums: List[int]
        :type target: int
        :rtype: List[List[int]]
        """
        dic = collections.defaultdict(list)
        for i in range(len(nums)):
            for j in range(i+1,len(nums)):
                dic[nums[i]+nums[j]].append((i,j))

        
        result = set()
        for key in dic:
            if target - key in dic:
                list1 = dic[key]
                list2 = dic[target - key]
                # 索引不一致
                for (i,j) in list1:
                    for (k,l) in list2:
                        if i!=k and i!=l and j!=k and j!=l:
                            flist = [nums[i],nums[j],nums[k],nums[l]
                            flist.sort()
                            result.add(tuple(flist))
        return list(result)
```

##### 解法二 递归  O(n ** 2)

先排序

递归回溯

```python
    def fourSum(self, nums, target):
        def findNsum(l, r, target, N, result, results):
            if r-l+1 < N or N < 2 or target < nums[l]*N or target > nums[r]*N:  # early termination
                return
            if N == 2: # two pointers solve sorted 2-sum problem
                while l < r:
                    s = nums[l] + nums[r]
                    if s == target:
                        results.append(result + [nums[l], nums[r]])
                        l += 1
                        while l < r and nums[l] == nums[l-1]:
                            l += 1
                    elif s < target:
                        l += 1
                    else:
                        r -= 1
            else: # recursively reduce N
                for i in range(l, r+1):
                    if i == l or (i > l and nums[i-1] != nums[i]):
                        findNsum(i+1, r, target-nums[i], N-1, result+[nums[i]], results)

        nums.sort()
        results = []
        findNsum(0, len(nums)-1, target, 4, [], results)
        return results
```

### [23. 合并K个排序链表](https://leetcode-cn.com/problems/merge-k-sorted-lists/)

#### 题目描述

合并 k 个排序链表，返回合并后的排序链表。请分析和描述算法的复杂度。

示例:

输入:
[
  1->4->5,
  1->3->4,
  2->6
]
输出: 1->1->2->3->4->4->5->6


#### 解法

##### 解法一 优先级队列

问题1 非空数组要先heapify才能变成heap

问题2 要保证不止优先级第一个参数可比较 第二个参数也要可比较(或者保证第一个参数不重复)

```python
import heapq
class Solution:
    def mergeKLists(self, lists: List[ListNode]) -> ListNode:
        """
            优先级队列
            问题: 非空数组要先heapify才能变成heap
        """
        hp = [(n.val, idx) for idx, n in enumerate(lists) if n]
        heapq.heapify(hp)
        dummy = ListNode(-1)
        p = dummy
        while hp:
            v, idx = heapq.heappop(hp)
            p.next = ListNode(v)
            p = p.next
            if lists[idx].next:
                heapq.heappush(hp, (lists[idx].next.val, idx))
                lists[idx] = lists[idx].next
        return dummy.next
```

##### 解法二
```python

```

### 31.下一个排列

#### 题目描述

实现获取下一个排列的函数，算法需要将给定数字序列重新排列成字典序中下一个更大的排列。

如果不存在下一个更大的排列，则将数字重新排列成最小的排列（即升序排列）。

必须原地修改，只允许使用额外常数空间。

以下是一些例子，输入位于左侧列，其相应输出位于右侧列。
1,2,3 → 1,3,2
3,2,1 → 1,2,3
1,1,5 → 1,5,1

#### 解法

##### 解法一

该题目画图更容易

![ Next Permutation ](/Users/zhengxinzhi/typora_img/LeetCode题目总结/dd4e79b184b1922429d8cda6148a3f0b7579869e85626e04ba29ba88e8052729-file_1555696116786.png)

```
首先，我们观察到对于任何给定序列的降序，没有可能的下一个更大的排列。

例如，以下数组不可能有下一个排列：

[9, 5, 4, 3, 1]
我们需要从右边找到第一对两个连续的数字 a[i] 和 a[i-1]，它们满足 a[i]>a[i-1]。现在，没有对 a[i] 右侧的重新排列可以创建更大的排列，因为该子数组由数字按降序组成。因此，我们需要重新排列 a[i-1] 右边的数字，包括它自己。

现在，什么样子的重新排列将产生下一个更大的数字呢？我们想要创建比当前更大的排列。因此，我们需要将数字 a[i-1] 替换为位于其右侧区域的数字中**比它更大的最小数字**，例如 a[j]。


我们交换数字 a[i-1] 和 a[j]。我们现在在索引 i-1处有正确的数字。 但目前的排列仍然不是我们正在寻找的排列。我们需要通过仅使用 a[i-1]右边的数字来形成最小的排列。 因此，我们需要放置那些按升序排列的数字，以获得最小的排列。

但是，请记住，在从右侧扫描数字时，我们只是继续递减索引直到我们找到 a[i] 和 a[i-1] 这对数。其中，a[i] > a[i-1]。因此，a[i-1] 右边的所有数字都已按降序排序。此外，交换 a[i-1] 和 a[j]并未改变该顺序。因此，我们只需要反转 a[i-1]之后的数字，以获得下一个最小的字典排列。

复杂度分析

时间复杂度：O(n)，在最坏的情况下，只需要对整个数组进行两次扫描。

空间复杂度：O(1)，没有使用额外的空间，原地替换足以做到。
```

```java
class Solution {
    public void nextPermutation(int[] nums) {
        int i = nums.length - 1;
        for(;i > 0; i--){
            if(nums[i] > nums[i-1]){
                break
            }
        }
        if(i > 0 && nums[i] > nums[i-1]){
            int j = i + 1;
            while(j < nums.length && nums[j] > nums[i-1]){
                j++;
            }
            swap(nums, --j, i-1);
            reverse(nums, i, nums.length - 1);
            return;
        }
        reverse(nums, 0, nums.length - 1);
    }
    
    private void swap(int[] nums, int i, int j){
        int temp = nums[i];
        nums[i] = nums[j];
        nums[j] = temp;
    }
    
    private void reverse(int[] nums, int i, int j){
        int times = (j - i + 1) >> 1;
        for(int k = 0; k < times; k++){
            swap(nums, i + k, j - k);
        }
    }
}
```

###33.[搜索旋转排序数组](https://leetcode-cn.com/problems/search-in-rotated-sorted-array/)

#### 题目描述

假设按照升序排序的数组在预先未知的某个点上进行了旋转。

( 例如，数组 [0,1,2,4,5,6,7] 可能变为 [4,5,6,7,0,1,2] )。

搜索一个给定的目标值，如果数组中存在这个目标值，则返回它的索引，否则返回 -1 。

你可以假设数组中不存在重复的元素。

你的算法时间复杂度必须是 O(log n) 级别。

示例 1:

输入: nums = [4,5,6,7,0,1,2], target = 0
输出: 4
示例 2:

输入: nums = [4,5,6,7,0,1,2], target = 3
输出: -1
在真实的面试中遇到过这道题？


#### 解法

##### 解法一

假设`nums`看起来像这样：[12、13、14、15、16、17、18、19、0、1、2、3、4、5、6、7、8、9、10、11]

由于未完全排序，因此我们无法进行常规的二进制搜索。但是诀窍出在这里：

如果目标是14，那么我们`nums`将对此进行调整，其中“ inf”表示无穷大：
[12、13、14、15、15、17、18、19，inf，inf，inf，inf，inf，inf，inf ，inf，inf，inf，inf，inf

如果目标为7，则我们调整`nums`为：
[-inf，-inf，-inf，-inf，-inf，-inf，-inf，-inf，0、1、2、3、4、5， 6，7，8，9，10，11]

然后我们可以简单地进行普通的二进制搜索。当然，我们实际上并没有调整整个数组，而是仅动态调整仅查看的元素。通过将目标元素和实际元素都与nums [0]进行比较来进行调整。

```c++
int search(vector<int>& nums, int target) {
    int lo = 0, hi = nums.size();
    while (lo < hi) {
        int mid = (lo + hi) / 2;
        
        double num = (nums[mid] < nums[0]) == (target < nums[0])
                   ? nums[mid]
                   : target < nums[0] ? -INFINITY : INFINITY;
                   
        if (num < target)
            lo = mid + 1;
        else if (num > target)
            hi = mid;
        else
            return mid;
    }
    return -1;
}
```

##### 解法二 左闭右开二分查找
```python
class Solution(object):
    def search(self, nums, target):
        """
        :type nums: List[int]
        :type target: int
        :rtype: int
        """
        l, r = 0, len(nums) 
        while l < r:
            mid = l + (r - l) // 2
            if nums[mid] == target:
                return mid
            if nums[mid] > nums[l]:
                if nums[l] <= target and target < nums[mid]:
                    r = mid
                else:
                    l = mid + 1
            else:
                if nums[mid] < target and target <= nums[r - 1]:
                    l = mid + 1
                else:
                    r = mid
        return -1

```

##### 解法三 二次二分查找

第一次二分查找：
为了找到数组在哪一处可以分割两段升序数组，比如 [4,5,6,7,0,1,2]，我希望知道7这个元素的下标，这样我就能将整个数组分为有序的两个部分。

在这次查找的过程中，我利用了 nums[mid] 跟 nums[0] 的关系，来确认 nums[mid] 落在哪一段，以正确地调整 low 和 high 的大小。

第二次二分查找
为了找到target的下标，首先要知道target在两段升序数组中的哪一段（将 target 和 nums[0] 比大小）然后在所在的这一段进行常规的二分查找。
用了两次二分查找，所以时间复杂度是 O(2logn) = O(logn)O(2logn)=O(logn)

### 34. 在排序数组中查找元素的第一个和最后一个位置

#### 题目描述

给定一个按照升序排列的整数数组 nums，和一个目标值 target。找出给定目标值在数组中的开始位置和结束位置。

你的算法时间复杂度必须是 O(log n) 级别。

如果数组中不存在目标值，返回 [-1, -1]。

示例 1:

输入: nums = [5,7,7,8,8,10], target = 8
输出: [3,4]
示例 2:

输入: nums = [5,7,7,8,8,10], target = 6
输出: [-1,-1]

#### 解法

##### 解法一 二分查找

```python
class Solution:
    def searchRange(self, nums: List[int], target: int) -> List[int]:
        if not nums or len(nums) == 0:
            return [-1, -1]
        l, r = 0, len(nums) - 1
        while l < r:
            mid = l + ((r - l) >> 1)
            if nums[mid] < target:
                l = mid + 1
            else:
                r = mid
                
        if nums[l] != target:
            return [-1, -1]
        start = l
        
        l, r = 0, len(nums)
        while l < r:
            mid = l + ((r - l) >> 1)
            if nums[mid] <= target:
                l = mid + 1
            else:
                r = mid
        end = r - 1
        return [start, end]
        
```



```java
class Solution {
    public int[] searchRange(int[] nums, int target) {
        /**
         * 找到target的开始
         * 找到target的结束
         */
        int start = -1, end = -1;
        int[] result = new int[]{start, end};
        int left = 0, right = nums.length - 1;
        while(left <= right){
            int mid = (left + right) >> 1;
            if(nums[mid] == target && (mid == 0 || nums[mid] > nums[mid - 1])){
                start = mid;
                break;
            }else if (nums[mid] >= target){
                right = mid - 1;
            }else{
                left = mid + 1;
            }
        }
        if (start == -1){
            return result;
        }
        left = 0;
        right = nums.length - 1;
        while(left <= right){
            int mid = (left + right) >> 1;
            if(nums[mid] == target && (mid == nums.length-1 || nums[mid] < nums[mid+1])){
                end = mid;
                break;
            }else if (nums[mid] <= target){
                left = mid + 1;
            }else{
                right = mid - 1;
            }
        }
        result[0] = start;
        result[1] = end;
        return result;
    }
}
```

##### 解法二 
```java
class Solution {
    // returns leftmost (or rightmost) index at which `target` should be
    // inserted in sorted array `nums` via binary search.
    private int extremeInsertionIndex(int[] nums, int target, boolean left) {
        int lo = 0;
        int hi = nums.length - 1;

        while (lo <= hi) {
            int mid = (lo + hi) >> 1;
            if (nums[mid] > target || (left && target == nums[mid])) {
                hi = mid - 1;
            }
            else {
                lo = mid + 1;
            }
        }
        return lo;
    }

    public int[] searchRange(int[] nums, int target) {
        int[] targetRange = {-1, -1};

        int leftIdx = extremeInsertionIndex(nums, target, true);

        // assert that `leftIdx` is within the array bounds and that `target`
        // is actually in `nums`.
        if (leftIdx == nums.length || nums[leftIdx] != target) {
            return targetRange;
        }

        targetRange[0] = leftIdx;
        targetRange[1] = extremeInsertionIndex(nums, target, false)-1;

        return targetRange;
    }
}

```

### 48.旋转图像

#### 题目描述

给定一个 n × n 的二维矩阵表示一个图像。

将图像顺时针旋转 90 度。

说明：

你必须在原地旋转图像，这意味着你需要直接修改输入的二维矩阵。请不要使用另一个矩阵来旋转图像。

示例 1:

给定 matrix = 
[
  [1,2,3],
  [4,5,6],
  [7,8,9]
],

原地旋转输入矩阵，使其变为:
[
  [7,4,1],
  [8,5,2],
  [9,6,3]
]
示例 2:

给定 matrix =
[
  [ 5, 1, 9,11],
  [ 2, 4, 8,10],
  [13, 3, 6, 7],
  [15,14,12,16]
], 

原地旋转输入矩阵，使其变为:
[
  [15,13, 2, 5],
  [14, 3, 4, 1],
  [12, 6, 8, 9],
  [16, 7,10,11]
]

#### 解法

##### 解法一

1.计算待旋转层数

2.计算某一层如何旋转

![48_rectangles.png](/Users/zhengxinzhi/typora_img/LeetCode题目总结/7a684b207a95188ff6450e4724d6ee8bdf425fc483775a8e30082ed25060dac1-48_rectangles.png)

```python
class Solution:
    def rotate(self, matrix: List[List[int]]) -> None:
        """
        Do not return anything, modify matrix in-place instead.
        思路:
            旋转
            转置 + 翻转
        """
        n = len(matrix)
        cycles = n // 2
        if n == 0:
            return
        for k in range(cycles):
            for i in range(k, n - k - 1):
                temp = matrix[k][i]
                matrix[k][i] = matrix[n-1-i][k]
                matrix[n-1-i][k] = matrix[n-1-k][n-1-i]
                matrix[n-1-k][n-1-i] = matrix[i][n-1-k]
                matrix[i][n-1-k] = temp

```

##### 解法二 转置+翻转

- 时间复杂度：O(N^2).
- 空间复杂度：O(1) 由于旋转操作是 *就地* 完成的。 

最直接的想法是先转置矩阵，然后翻转每一行。这个简单的方法已经能达到最优的时间复杂度O(N^2)*O*(*N*2)。

```python
class Solution:
    def rotate(self, matrix: List[List[int]]) -> None:
        """
        Do not return anything, modify matrix in-place instead.
        思路:
            旋转
            转置 + 翻转
        """
        if not matrix or len(matrix) < 2:
            return
        
        n = len(matrix)
        for i in range(n):
            for j in range(i, n):
                matrix[i][j], matrix[j][i] = matrix[j][i], matrix[i][j]
        
        for i in range(n):
            matrix[i] = matrix[i][::-1]
```



```java
class Solution {
  public void rotate(int[][] matrix) {
    int n = matrix.length;

    // transpose matrix
    for (int i = 0; i < n; i++) {
      for (int j = i; j < n; j++) {
        int tmp = matrix[j][i];
        matrix[j][i] = matrix[i][j];
        matrix[i][j] = tmp;
      }
    }
    // reverse each row
    for (int i = 0; i < n; i++) {
      for (int j = 0; j < n / 2; j++) {
        int tmp = matrix[i][j];
        matrix[i][j] = matrix[i][n - j - 1];
        matrix[i][n - j - 1] = tmp;
      }
    }
  }
}
```

### 54.螺旋矩阵

#### 题目描述

给定一个包含 m x n 个元素的矩阵（m 行, n 列），请按照顺时针螺旋顺序，返回矩阵中的所有元素。

示例 1:

输入:
[
 [ 1, 2, 3 ],
 [ 4, 5, 6 ],
 [ 7, 8, 9 ]
]
输出: [1,2,3,6,9,8,7,4,5]
示例 2:

输入:
[
  [1, 2, 3, 4],
  [5, 6, 7, 8],
  [9,10,11,12]
]
输出: [1,2,3,4,8,12,11,10,9,5,6,7]

#### 解法

##### 解法一

```python
class Solution:
    def spiralOrder(self, matrix: List[List[int]]) -> List[int]:
        """
            规律：找到m和n中较小者 获得圈数
                 如果较小者为偶数k (k + 1) // 2
                    最内圈不是单层
                 如果较小者为奇数k (k + 1) // 2
                    最内圈是单层
        """
        def traverse(i):
            # 上面
            for j in range(i, n - i):
                ans.append(matrix[i][j])
            # 右面
            for j in range(i + 1, m - i):
                ans.append(matrix[j][n - i - 1])
            # 单层不执行后两条
            if i != ls - 1 or (k & 1) == 0:
                # 下面
                for j in range(n - i - 2, i - 1, -1):
                    ans.append(matrix[m - i - 1][j])
                # 左面
                for j in range(m - i - 2, i, -1):
                    ans.append(matrix[j][i])
            
        m = len(matrix)
        if not m:
            return matrix
        n = len(matrix[0])
        k = min(m, n)
        ls = (k + 1) >> 1
        ans = []
        for i in range(ls):
            
            traverse(i)
        return ans
```

##### 解法二 索引是否相遇判断是否是单层

```python
class Solution(object):
    def spiralOrder(self, matrix):
        def spiral_coords(r1, c1, r2, c2):
            for c in range(c1, c2 + 1):
                yield r1, c
            for r in range(r1 + 1, r2 + 1):
                yield r, c2
            # 如果r1 == r2 or c1 == c2 最中心单层遍历
            if r1 < r2 and c1 < c2:
                for c in range(c2 - 1, c1, -1):
                    yield r2, c
                for r in range(r2, r1, -1):
                    yield r, c1

        if not matrix: 
            return []
        ans = []
v
        while r1 <= r2 and c1 <= c2:
            for r, c in spiral_coords(r1, c1, r2, c2):
                ans.append(matrix[r][c])
            r1 += 1; r2 -= 1
            c1 += 1; c2 -= 1
        return an
```

### 55.跳跃游戏

#### 题目描述

给定一个非负整数数组，你最初位于数组的第一个位置。

数组中的每个元素代表你在该位置可以跳跃的最大长度。

判断你是否能够到达最后一个位置。

示例 1:

输入: [2,3,1,1,4]
输出: true
解释: 从位置 0 到 1 跳 1 步, 然后跳 3 步到达最后一个位置。
示例 2:

输入: [3,2,1,0,4]
输出: false
解释: 无论怎样，你总会到达索引为 3 的位置。但该位置的最大跳跃长度是 0 ， 所以你永远不可能到达最后一个位置。

#### 解法

从右向左的思路会更好·

##### 解法一 剪枝 速度较慢

最坏时间复杂度O(n**2)

```python
class Solution:
    def canJump(self, nums: List[int]) -> bool:
        dp = [False] * len(nums)
        dp[0] = True
        for i in range(1, len(nums)):
            for j in range(i - 1, -1, -1):
                if dp[j] and (i - j) <= nums[j]:
                    dp[i] = True
                    break
        return dp[len(nums)-1]
```

```python
class Solution:
    def canJump(self, nums: List[int]) -> bool:
        marks = [0]
        for i in range(1, len(nums)):
            length = len(marks)
            for x in range(length-1, -1, -1):
                j = marks[x]
                if (i - j) <= nums[j]:
                    marks.append(i)
                    break
        return marks != [] and marks[-1] + 1 == len(nums)
    
```

##### 解法二 动态规划 自右向左 

python 超时 O(n**2)

```python
class Solution:
    def canJump(self, nums: List[int]) -> bool:
        length = len(nums)
        dp = [False] * length
        # 自右向左
        dp[length - 1] = True
        for i in range(length - 2, -1, -1):
            next_farthest_step = min(length - 1, i + nums[i])
            for j in range(i + 1, next_farthest_step + 1):
                if dp[j]:
                    dp[i] = True
                    break
        return dp[0]
```

#####  解法三 贪心

当我们把代码改成自底向上的模式，我们会有一个重要的发现，从某个位置出发，我们只需要找到第一个标记为 GOOD 的坐标（由跳出循环的条件可得），也就是说找到最左边的那个坐标。如果我们用一个单独的变量来记录最左边的 GOOD 位置，我们就可以避免搜索整个数组，进而可以省略整个 memo 数组。

从右向左迭代，对于每个节点我们检查是否存在一步跳跃可以到达 GOOD 的位置（currPosition + nums[currPosition] >= leftmostGoodIndex）。如果可以到达，当前位置也标记为 GOOD ，同时，这个位置将成为新的最左边的 GOOD 位置，一直重复到数组的开头，如果第一个坐标标记为 GOOD 意味着可以从第一个位置跳到最后的位置。

模拟一下这个操作，对于输入数组 nums = [9, 4, 2, 1, 0, 2, 0]，我们用 G 表示 GOOD，用 B 表示 BAD 和 U 表示 UNKNOWN。我们需要考虑所有从 0 出发的情况并判断坐标 0 是否是好坐标。由于坐标 1 是 GOOD，我们可以从 0 跳到 1 并且 1 最终可以跳到坐标 6，所以尽管 nums[0] 可以直接跳到最后的位置，我们只需要一种方案就可以知道结果。

Index	0	1	2	3	4	5	6
nums	9	4	2	1	0	2	0
memo	U	G	B	B	B	G	G

```python
class Solution:
    def canJump(self, nums: List[int]) -> bool:
        length = len(nums)
        lastPos = length - 1
        for i in range(length - 2, -1, -1):
            if i + nums[i] >= lastPos:
                lastPos = i
                
        return lastPos == 0
      

```

##### 解法四 贪心

```c++
bool canJump(int A[], int n) {
    int i = 0;
    for (int reach = 0; i < n && i <= reach; ++i)
        reach = max(i + A[i], reach);
    return i == n;
}
```



###56.合并区间

#### 题目描述

给出一个区间的集合，请合并所有重叠的区间。

示例 1:

输入: [[1,3],[2,6],[8,10],[15,18]]
输出: [[1,6],[8,10],[15,18]]
解释: 区间 [1,3] 和 [2,6] 重叠, 将它们合并为 [1,6].
示例 2:

输入: [[1,4],[4,5]]
输出: [[1,5]]
解释: 区间 [1,4] 和 [4,5] 可被视为重叠区间。


#### 解法

##### 解法一

维护前一个待合并好的区间的左边界和右边界 left_max， right_max 

当当前区间的左边界大于带合并好的右边界，把合并好的边界放入结果集合，重新把当前区间左右边界更新成带合并好的区间边界

注意最后一个区间要放入结果集合中

```python
class Solution:
    def merge(self, intervals: List[List[int]]) -> List[List[int]]:
        if not intervals:
            return intervals
        intervals = sorted(intervals, key=lambda x: (x[0], x[1]))
        left_max = -1;
        right_max = -1;
        ans = []
        for left, right in intervals:
            if right_max == -1:
                left_max = left
                right_max = right
            elif left > right_max:
                ans.append([left_max, right_max])
                left_max = left
                right_max = right
            elif right > right_max:
                right_max = right

        if not ans or right_max != ans[-1][1]:
            ans.append([left_max, right_max])
        return ans
```

复杂度分析

时间复杂度：O(nlogn)

除去 sort 的开销，我们只需要一次线性扫描，所以主要的时间开销是排序的 O(nlgn)O(nlgn)

空间复杂度：O(1) (or O(n)

如果我们可以原地排序 intervals ，就不需要额外的存储空间；否则，我们就需要一个线性大小的空间去存储 intervals 的备份，来完成排序过程

##### 解法二

首先，我们将列表按上述方式排序。然后，我们将第一个区间插入 merged 数组中，然后按顺序考虑之后的每个区间：如果当前区间的左端点在前一个区间的右端点之后，那么他们不会重合，我们可以直接将这个区间插入 merged 中；否则，他们重合，我们用当前区间的右端点更新前一个区间的右端点 end 如果前者数值比后者大的话。

```python
class Solution:
    def merge(self, intervals: List[List[int]]) -> List[List[int]]:
        if not intervals:
            return intervals
        intervals = sorted(intervals, key=lambda x: (x[0], x[1]))
        
        ans = []
        for left, right in intervals:
            # 当结果集合空或者 新区间不与结果结合最后的区间重叠 则把该区间放入结果集合
            if not ans or ans[-1][1] < left:
                ans.append([left, right])
            # 否则更新结果集合最后一个去见的右边界
            else:
                ans[-1][1] = max(ans[-1][1], right)
        return ans

```

### 73.矩阵置零

#### 73.1.题目描述

给定一个 *m* x *n* 的矩阵，如果一个元素为 0，则将其所在行和列的所有元素都设为 0。请使用**原地**算法**。**

**示例 1:**

```
输入: 
[
  [1,1,1],
  [1,0,1],
  [1,1,1]
]
输出: 
[
  [1,0,1],
  [0,0,0],
  [1,0,1]
]
```

**示例 2:**

```
输入: 
[
  [0,1,2,0],
  [3,4,5,2],
  [1,3,1,5]
]
输出: 
[
  [0,0,0,0],
  [0,4,5,0],
  [0,3,1,0]
]
```

**进阶:**

- 一个直接的解决方案是使用  O(*m**n*) 的额外空间，但这并不是一个好的解决方案。
- 一个简单的改进方案是使用 O(*m* + *n*) 的额外空间，但这仍然不是最好的解决方案。
- 你能想出一个常数空间的解决方案吗？

#### 73.2.解法
##### 73.2.1 方法一 TO(n*m) SO(n+m) 

```java
class Solution {
    public void setZeroes(int[][] matrix) {
        if(matrix == null || matrix.length == 0){
            return;
        }
        // 建立行标记数组 列标记数组
        int[] rows = new int[matrix.length], cols = new int[matrix[0].length];
        for(int i = 0; i < matrix.length; i++){
            for(int j = 0; j < matrix[0].length; j++){
                if(matrix[i][j] == 0){
                    rows[i] += 1;
                    cols[j] += 1;
                }
            }
        }
        for(int i = 0; i < matrix.length; i++){
            if(rows[i] > 0){
                for(int j = 0; j < matrix[0].length; j++){
                    matrix[i][j] = 0;
                }
            }
        }
        
        for(int j = 0; j < matrix[0].length; j++){
            if(cols[j] > 0){
                for(int i = 0; i < matrix.length; i++){
                    matrix[i][j] = 0;
                }
            }
        }
    }
}
```

```java
class Solution {
  public void setZeroes(int[][] matrix) {
    int R = matrix.length;
    int C = matrix[0].length;
    Set<Integer> rows = new HashSet<Integer>();
    Set<Integer> cols = new HashSet<Integer>();

    // Essentially, we mark the rows and columns that are to be made zero
    for (int i = 0; i < R; i++) {
      for (int j = 0; j < C; j++) {
        if (matrix[i][j] == 0) {
          rows.add(i);
          cols.add(j);
        }
      }
    }

    // Iterate over the array once again and using the rows and cols sets, update the elements.
    for (int i = 0; i < R; i++) {
      for (int j = 0; j < C; j++) {
        if (rows.contains(i) || cols.contains(j)) {
          matrix[i][j] = 0;
        }
      }
    }
  }
}
```



##### 73.2.2 方法二 TO(n*m) SO(1)

空间复杂度 O(2) ，用两个布尔变量就可以解决。

利用数组的首行和首列来记录 0 值。从数组下标的 A[1][1]开始遍历，

两个布尔值记录首行首列是否需要置0

```java
class Solution {
    public void setZeroes(int[][] matrix) {
        if(matrix == null || matrix.length == 0 || matrix[0].length == 0){
            return;
        }
        // 记录首行首列是否置0
        boolean firstRow = false;
        for(int i = 0; i < matrix[0].length; i++){
            if(matrix[0][i] == 0){
                firstRow = true;
            }
        }
        boolean firstCol = false;
        for(int i = 0; i < matrix.length; i++){
            if(matrix[i][0] == 0){
                firstCol = true;
            }
        }
				// 从matrix[1][1]开始遍历， 利用数组首行首列来记录0值
        for(int i = 1; i < matrix.length; i++){
            for(int j = 1; j < matrix[0].length; j++){
                if(matrix[i][j] == 0){
                    matrix[i][0] = 0;
                    matrix[0][j] = 0;
                }
            }
        }
        for(int i = 1; i < matrix.length; i++){
            if(matrix[i][0] == 0){
                for(int j = 0; j < matrix[0].length; j++){
                    matrix[i][j] = 0;
                }
            }
        }
        // 更新首行首列
        for(int j = 1; j < matrix[0].length; j++){
            if(matrix[0][j] == 0){
                for(int i = 0; i < matrix.length; i++){
                    matrix[i][j] = 0;
                }
            }
        }
        if(firstRow){
            for(int i = 0; i < matrix[0].length; i++){
                matrix[0][i] = 0;
            }
        }
        if(firstCol){
            for(int i = 0; i < matrix.length; i++){
                matrix[i][0] = 0;
            }
        }
    }
}
```

```java
class Solution {
  public void setZeroes(int[][] matrix) {
    Boolean isCol = false;
    int R = matrix.length;
    int C = matrix[0].length;

    for (int i = 0; i < R; i++) {

      // Since first cell for both first row and first column is the same i.e. matrix[0][0]
      // We can use an additional variable for either the first row/column.
      // For this solution we are using an additional variable for the first column
      // and using matrix[0][0] for the first row.
      if (matrix[i][0] == 0) {
        isCol = true;
      }

      for (int j = 1; j < C; j++) {
        // If an element is zero, we set the first element of the corresponding row and column to 0
        if (matrix[i][j] == 0) {
          matrix[0][j] = 0;
          matrix[i][0] = 0;
        }
      }
    }

    // Iterate over the array once again and using the first row and first column, update the elements.
    for (int i = 1; i < R; i++) {
      for (int j = 1; j < C; j++) {
        if (matrix[i][0] == 0 || matrix[0][j] == 0) {
          matrix[i][j] = 0;
        }
        
      }
    }

    // See if the first row needs to be set to zero as well
    if (matrix[0][0] == 0) {
      for (int j = 0; j < C; j++) {
        matrix[0][j] = 0;
      }
    }

    // See if the first column needs to be set to zero as well
    if (isCol) {
      for (int i = 0; i < R; i++) {
        matrix[i][0] = 0;
      }
    }
  }
}
```

###74.搜索二维矩阵

#### 题目描述

编写一个高效的算法来判断 m x n 矩阵中，是否存在一个目标值。该矩阵具有如下特性：

每行中的整数从左到右按升序排列。
每行的第一个整数大于前一行的最后一个整数。
示例 1:

输入:
matrix = [
  [1,   3,  5,  7],
  [10, 11, 16, 20],
  [23, 30, 34, 50]
]
target = 3
输出: true
示例 2:

输入:
matrix = [
  [1,   3,  5,  7],
  [10, 11, 16, 20],
  [23, 30, 34, 50]
]
target = 13
输出: false


#### 解法

##### 解法一 对角 O(m + n)
```python
class Solution:
    def searchMatrix(self, matrix: List[List[int]], target: int) -> bool:
        if not matrix or len(matrix) == 0 or len(matrix[0]) == 0:
            return False
        m = len(matrix)
        n = len(matrix[0])
        
        row = m - 1
        col = 0
        while row >= 0 and col < n:
            if matrix[row][col] == target:
                return True
            elif matrix[row][col] > target:
                row -= 1
            else:
                col += 1
        return False
           
```

##### 解法二 O(logmn)
```python
class Solution:
    def searchMatrix(self, matrix: List[List[int]], target: int) -> bool:
        m = len(matrix)
        if m == 0:
            return False
        n = len(matrix[0])
        
        #二分查找
        left, right = 0, m * n - 1
        while left <= right:
                pivot_idx = (left + right) // 2
                pivot_element = matrix[pivot_idx // n][pivot_idx % n]
                if target == pivot_element:
                    return True
                else:
                    if target < pivot_element:
                        right = pivot_idx - 1
                    else:
                        left = pivot_idx + 1
        return False

```

### 75. 颜色分类

#### 75.1 题目描述

给定一个包含红色、白色和蓝色，一共 *n* 个元素的数组，**原地**对它们进行排序，使得相同颜色的元素相邻，并按照红色、白色、蓝色顺序排列。

此题中，我们使用整数 0、 1 和 2 分别表示红色、白色和蓝色。

**注意:**
不能使用代码库中的排序函数来解决这道题。

**示例:**

```
输入: [2,0,2,1,1,0]
输出: [0,0,1,1,2,2]
```

**进阶：**

- 一个直观的解决方案是使用计数排序的两趟扫描算法。
  首先，迭代计算出0、1 和 2 元素的个数，然后按照0、1、2的排序，重写当前数组。
- 你能想出一个仅使用常数空间的一趟扫描算法吗？

#### 75.2 解法

##### 75.2.1 类似于快排中的partition

**[NOTE]:** 和blue交换时white不要直接迭代到下一次, 要继续判断交换后的值是什么

**trick:** 把red white blue 改成 zero one two 可能更易写 更易读

```python
class Solution:
    def sortColors(self, nums: List[int]) -> None:
        """
        Do not return anything, modify nums in-place instead.
        """
        red, white, blue = -1, 0, len(nums)
        while white < blue:
            if nums[white] == 0:
                nums[white], nums[red+1] = nums[red+1], nums[white]
                red += 1
                white += 1
            elif nums[white] == 1:
                white += 1
            else:
                nums[white], nums[blue-1] = nums[blue-1], nums[white]
                blue -= 1
                            
```

##### 75.2.2 扫描两遍 交换

The idea is to sweep all 0s to the left and all 2s to the right, then all 1s are left in the middle.

It is hard to define what is a "one-pass" solution but this algorithm is bounded by O(2n), meaning that at most each element will be seen and operated twice (in the case of all 0s). You may be able to write an algorithm which goes through the list only once, but each step requires multiple operations, leading the total operations larger than O(2n).

```python
    class Solution {
    public:
        void sortColors(int A[], int n) {
            int second=n-1, zero=0;
            for (int i=0; i<=second; i++) {
                while (A[i]==2 && i<second) swap(A[i], A[second--]);
                while (A[i]==0 && i>zero) swap(A[i], A[zero++]);
            }
        }
    };
```

##### 75.2.3 计数排序

```python
    def sortColors(self, nums: List[int]) -> None:
        """
            计数排序
                1.找出待排序的数组中最大和最小的元素 本题直接得0,2
                2.统计数组中每个值为i的元素出现的次数，存入数组 C 的第i项
                3.对所有的计数累加（从 C 中的第一个元素开始，每一项和前一项相加）
                4.反向填充目标数组：将每个元素i放在新数组的第C[i]项，每放一个元素就将C[i]减去1

        """
        min_val, max_val = 0, 2
        c = [0] * (max_val-min_val+1)
        for i in range(0, len(nums)):
            c[nums[i]-min_val] += 1
        
        for j in range(1, len(c)):
            c[j] = c[j] + c[j-1]
            
        result = [0]*len(nums)
        for k in range(len(nums)-1, -1, -1):
            result[c[nums[k]-min_val]-1] = nums[k]
            c[nums[k]-min_val] -= 1
        return result
```

### 88. 合并两个有序数组

#### 88.1.题目描述

给定两个有序整数数组 *nums1* 和 *nums2*，将 *nums2* 合并到 *nums1* 中*，*使得 *num1* 成为一个有序数组。

**说明:**

- 初始化 *nums1* 和 *nums2* 的元素数量分别为 *m* 和 *n*。
- 你可以假设 *nums1* 有足够的空间（空间大小大于或等于 *m + n*）来保存 *nums2* 中的元素。

**示例:**

```
输入:
nums1 = [1,2,3,0,0,0], m = 3
nums2 = [2,5,6],       n = 3

输出: [1,2,2,3,5,6]
```

#### 88.2.解法

##### 88.2.1 方法一 把较大值放入最后

```python
class Solution:
    def merge(self, nums1: List[int], m: int, nums2: List[int], n: int) -> None:
        """
        Do not return anything, modify nums1 in-place instead.
        """
        l = m + n - 1
        while m and n:
            if nums1[m-1] > nums2[n-1]:
                nums1[l] = nums1[m-1]
                m -= 1
            else:
                nums1[l] = nums2[n-1]
                n -= 1
            l -= 1

        for i in range(n):
            nums1[i] = nums2[i]
            
    def merge(self, nums1, m, nums2, n):
          while m > 0 and n > 0:
              if nums1[m-1] >= nums2[n-1]:
                  nums1[m+n-1] = nums1[m-1]
                  m -= 1
              else:
                  nums1[m+n-1] = nums2[n-1]
                  n -= 1
          if n > 0:
              nums1[:n] = nums2[:n]
```

```java
class Solution {
  public:
      void merge(int A[], int m, int B[], int n) {
          int i=m-1;
          int j=n-1;
          int k = m+n-1;
          while(i >=0 && j>=0)
          {
            if(A[i] > B[j])
              A[k--] = A[i--];
            else
              A[k--] = B[j--];
          }
          while(j>=0)
            A[k--] = B[j--];
          }
  };
```

###130.被围绕的区域

#### 题目描述

给定一个二维的矩阵，包含 'X' 和 'O'（字母 O）。

找到所有被 'X' 围绕的区域，并将这些区域里所有的 'O' 用 'X' 填充。

示例:

X X X X
X O O X
X X O X
X O X X
运行你的函数后，矩阵变为：

X X X X
X X X X
X X X X
X O X X
解释:

被围绕的区间不会存在于边界上，换句话说，任何边界上的 'O' 都不会被填充为 'X'。 任何不在边界上，或不与边界上的 'O' 相连的 'O' 最终都会被填充为 'X'。如果两个元素在水平或垂直方向相邻，则称它们是“相连”的。


#### 解法

##### 解法一 DFS

 对边界上的 O 要特殊处理，那么剩下的 O 替换成 X 就可以了。问题转化为，如何寻找和边界联通的 O

```python
class Solution:
    def solve(self, board: List[List[str]]) -> None:
        """
            对边界上的 O 要特殊处理，那么剩下的 O 替换成 X 就可以了。问题转化为，如何寻找和边界联通的 O
        """
        def dfs(i, j):
            # 如果返回True 需要恢复已修改
            board[i][j] = '#'

            # 左 上 右 下
            directions = [(0, -1), (-1, 0), (0, 1), (1, 0)]
            for dx, dy in directions:
                if (i + dx) >= 0 and (i + dx) < m and (j + dy) >= 0 and (j + dy) < n and board[i + dx][j + dy] =='O':
                    dfs(i + dx, j + dy) 
            
        m = len(board)
        if not m:
            return 
        n = len(board[0])
        if not n:
            return 
        
        for j in range(n):
            if board[0][j] == 'O':
                dfs(0, j)
            if board[m-1][j] == 'O':
                dfs(m-1, j)
        for i in range(m):
            if board[i][0] == 'O':
                dfs(i, 0)
            if board[i][n-1] == 'O':
                dfs(i, n - 1)
        
        for i in range(m):
            for j in range(n):
                if board[i][j] == 'O':
                    board[i][j] = 'X'
                if board[i][j] == '#':
                    board[i][j] = 'O'
    
```

##### 解法二
```python

```

###134.加油站 [推荐]

#### 题目描述

在一条环路上有 N 个加油站，其中第 i 个加油站有汽油 gas[i] 升。

你有一辆油箱容量无限的的汽车，从第 i 个加油站开往第 i+1 个加油站需要消耗汽油 cost[i] 升。你从其中的一个加油站出发，开始时油箱为空。

如果你可以绕环路行驶一周，则返回出发时加油站的编号，否则返回 -1。

说明: 

如果题目有解，该答案即为唯一答案。
输入数组均为非空数组，且长度相同。
输入数组中的元素均为非负数。
示例 1:

输入: 
gas  = [1,2,3,4,5]
cost = [3,4,5,1,2]

输出: 3

解释:
从 3 号加油站(索引为 3 处)出发，可获得 4 升汽油。此时油箱有 = 0 + 4 = 4 升汽油
开往 4 号加油站，此时油箱有 4 - 1 + 5 = 8 升汽油
开往 0 号加油站，此时油箱有 8 - 2 + 1 = 7 升汽油
开往 1 号加油站，此时油箱有 7 - 3 + 2 = 6 升汽油
开往 2 号加油站，此时油箱有 6 - 4 + 3 = 5 升汽油
开往 3 号加油站，你需要消耗 5 升汽油，正好足够你返回到 3 号加油站。
因此，3 可为起始索引。
示例 2:

输入: 
gas  = [2,3,4]
cost = [3,4,3]

输出: -1

解释:
你不能从 0 号或 1 号加油站出发，因为没有足够的汽油可以让你行驶到下一个加油站。
我们从 2 号加油站出发，可以获得 4 升汽油。 此时油箱有 = 0 + 4 = 4 升汽油
开往 0 号加油站，此时油箱有 4 - 3 + 2 = 3 升汽油
开往 1 号加油站，此时油箱有 3 - 3 + 3 = 3 升汽油
你无法返回 2 号加油站，因为返程需要消耗 4 升汽油，但是你的油箱只有 3 升汽油。
因此，无论怎样，你都不可能绕环路行驶一周。


#### 解法

##### 解法一 贪心

                1.总油量 >= 总路程 一定可以走完全程
                2.第二个规则可以被一般化，我们引入变量 curr_tank ，记录当前油箱里剩余的总油量。
                  如果在某一个加油站 curr_tank比 0 小，意味着我们无法到达这个加油站。
                  **下一步我们把这个加油站当做新的起点，并将 curr_tank 重置为 0 ，因为重新出发，油箱中的油为 0 。
                  （从上一次重置的加油站到当前加油站的任意一个加油站出发，到达当前加油站之前， curr_tank 也一定会比 0 小）**
```python
class Solution:
    def canCompleteCircuit(self, gas: List[int], cost: List[int]) -> int:
        """
            贪心：
                1.总油量 >= 总路程 一定可以走完全程
                2.第二个规则可以被一般化，我们引入变量 curr_tank ，记录当前油箱里剩余的总油量。
                  如果在某一个加油站 curr_tank比 0 小，意味着我们无法到达这个加油站。
                  **下一步我们把这个加油站当做新的起点，并将 curr_tank 重置为 0 ，因为重新出发，油箱中的油为 0 。
                  （从上一次重置的加油站到当前加油站的任意一个加油站出发，到达当前加油站之前， curr_tank 也一定会比 0 小）**

            
        """
        total_gas = 0
        cur_gas = 0
        ans = 0
        for i in range(len(gas)):
            total_gas += gas[i] - cost[i]
            cur_gas += gas[i] - cost[i]
            if cur_gas < 0:
                ans = i + 1
                cur_gas = 0
                
        return ans if total_gas >= 0 else -1
```

##### 解法二
```python

```

### 150.逆波兰表达式求值

#### 题目描述

根据逆波兰表示法，求表达式的值。

有效的运算符包括 +, -, *, / 。每个运算对象可以是整数，也可以是另一个逆波兰表达式。

说明：

整数除法只保留整数部分。
给定逆波兰表达式总是有效的。换句话说，表达式总会得出有效数值且不存在除数为 0 的情况。
示例 1：

输入: ["2", "1", "+", "3", "*"]
输出: 9
解释: ((2 + 1) * 3) = 9
示例 2：

输入: ["4", "13", "5", "/", "+"]
输出: 6
解释: (4 + (13 / 5)) = 6
示例 3：

输入: ["10", "6", "9", "3", "+", "-11", "*", "/", "*", "17", "+", "5", "+"]
输出: 22
解释: 
  ((10 * (6 / ((9 + 3) * -11))) + 17) + 5
= ((10 * (6 / (12 * -11))) + 17) + 5
= ((10 * (6 / -132)) + 17) + 5
= ((10 * 0) + 17) + 5
= (0 + 17) + 5
= 17 + 5
= 22


#### 解法

##### 解法一

python的除法是陷阱，c++没问题

```python
class Solution:
    def evalRPN(self, tokens: List[str]) -> int:
        stack = []
        sign = {'+', '-', '*', '/'}
        ans = 0
        for c in tokens:
            if c in sign:
                num2 = stack.pop()
                num1 = stack.pop()
                if c == '+':
                    stack.append(num1 + num2)
                elif c == '-':
                    stack.append(num1 - num2)
                elif c == '*':
                    stack.append(num1 * num2)
                else:
                    # key:除法是陷阱 
                    # 另一种写法：int(d[-2]/d[-1])
                    if num1 * num2 >= 0:
                        stack.append(num1 // num2)
                    else:
                        stack.append(-(abs(num1) // abs(num2)))

                ans = stack[-1]
            else:
                stack.append(int(c))
                
        if stack:
            return stack[-1]
                
        return ans
```

##### 解法二
```python

```

### 152.乘积最大子序列 

#### 题目描述

给定一个整数数组 nums ，找出一个序列中乘积最大的连续子序列（该序列至少包含一个数）。

示例 1:

输入: [2,3,-2,4]
输出: 6
解释: 子数组 [2,3] 有最大乘积 6。
示例 2:

输入: [-2,0,-1]
输出: 0
解释: 结果不能为 2, 因为 [-2,-1] 不是子数组。


#### 解法

##### 解法一 正负指针 DP

 乘法和加法最大区别是乘法需要考虑正负号 替换大小索引

最大值 负最大值

标签：动态规划
遍历数组时计算当前最大值，不断更新
令imax为当前最大值，则当前最大值为 imax = max(imax * nums[i], nums[i])
由于存在负数，那么会导致最大的变最小的，最小的变最大的。因此还需要维护当前最小值imin，imin = min(imin * nums[i], nums[i])
当负数出现时则imax与imin进行交换再进行下一步计算
时间复杂度：O(n)O(n)

```python
class Solution:
    def maxProduct(self, nums: List[int]) -> int:
        """
            乘法和加法最大区别是乘法需要考虑正负号 替换大小索引
        """
        ans = float('-inf')
        minv = 1
        maxv = 1
        for num in nums:
            if num < 0:
                minv, maxv = maxv, minv
            maxv = max(maxv * num, num)
            minv = min(minv * num, num)
            ans = max(maxv, ans)
        return ans
```

##### 解法二
```python

```

###162.寻找峰值 

#### 题目描述

峰值元素是指其值大于左右相邻值的元素。

给定一个输入数组 nums，其中 nums[i] ≠ nums[i+1]，找到峰值元素并返回其索引。

数组可能包含多个峰值，在这种情况下，返回任何一个峰值所在位置即可。

你可以假设 nums[-1] = nums[n] = -∞。

示例 1:

输入: nums = [1,2,3,1]
输出: 2
解释: 3 是峰值元素，你的函数应该返回其索引 2。
示例 2:

输入: nums = [1,2,1,3,5,6,4]
输出: 1 或 5 
解释: 你的函数可以返回索引 1，其峰值元素为 2；
     或者返回索引 5， 其峰值元素为 6。
说明:

你的解法应该是 O(logN) 时间复杂度的。


#### 解法

##### 解法一 二分查找
```python
class Solution:
    def findPeakElement(self, nums: List[int]) -> int:
        left = 0
        right = len(nums) - 1
        while left < right:
            mid = left + ((right - left) >> 1)
            if nums[mid] < nums[mid + 1]:
                left = mid + 1
            elif nums[mid] > nums[mid + 1]:
                right = mid
        return left
```

##### 解法二
```python

```

### 167. 两数之和 II - 输入有序数组

#### 167.1 题目描述

给定一个已按照**升序排列** 的有序数组，找到两个数使得它们相加之和等于目标数。

函数应该返回这两个下标值 index1 和 index2，其中 index1 必须小于 index2*。*

**说明:**

- 返回的下标值（index1 和 index2）不是从零开始的。
- 你可以假设每个输入只对应唯一的答案，而且你不可以重复使用相同的元素。

**示例:**

```
输入: numbers = [2, 7, 11, 15], target = 9
输出: [1,2]
解释: 2 与 7 之和等于目标数 9 。因此 index1 = 1, index2 = 2 。
```

#### 167.2 解法

##### 167.2.1 方法一 差值的二分查找 O(nlogn)

```python
class Solution:
    def twoSum(self, numbers: List[int], target: int) -> List[int]:
        """
            Note: 
                index从1开始
                对应唯一答案
                不可重复使用相同元素
            思路: 差值的二分查找
        """
        
        highest = len(numbers) - 1
        for i, a in enumerate(numbers):
            b = target - a
            # 二分查找
            low = i + 1
            high = highest
            
            while low <= high:
                mid = (low + high) >> 1
                if numbers[mid] == b:
                    return i+1, mid+1
                elif numbers[mid] > b:
                    high = mid - 1
                else:
                    low = mid + 1
```

##### 167.2.2 方法二 字典替换上面的二分查找 O(n)  **'key in dict'操作是O(1)** [推荐]

```python
# dictionary           
def twoSum2(self, numbers, target):
    dic = {}
    for i, num in enumerate(numbers):
        if target-num in dic:
            return [dic[target-num]+1, i+1]
        dic[num] = i
```

##### 167.2.3 双指针 O(n) [推荐]

```python
def twoSum(self, numbers, target):
    l, r = 0, len(numbers)-1
    while l < r:
        s = numbers[l] + numbers[r]
        if s == target:
            return [l+1, r+1]
        elif s < target:
            l += 1
        else:
            r -= 1
```

### 169.求众数 [推荐]

#### 169.1 题目描述

给定一个大小为 *n* 的数组，找到其中的众数。众数是指在数组中出现次数**大于** `⌊ n/2 ⌋` 的元素。

你可以假设数组是非空的，并且给定的数组总是存在众数。

**示例 1:**

```
输入: [3,2,3]
输出: 3
```

**示例 2:**

```
输入: [2,2,1,1,1,2,2]
输出: 2
```

#### 169.2 解法

##### 169.2.1 方法一 Hash  timeO(n) spaceO(n)

```python
class Solution:
    def majorityElement(self, nums: List[int]) -> int:
        import collections
        return collections.Counter(nums).most_common(1)[0][0]
```

```java
import java.util.HashMap;
public class Solution {
    public int MoreThanHalfNum_Solution(int [] array) {
        if(array == null || array.length == 0){
            return 0;
        }
        if(array.length == 1){
            return array[0];
        }
        HashMap<Integer, Integer> map = new HashMap<>();
        for(int i = 0; i < array.length; i++){
            if(map.containsKey(array[i])){
                if(map.get(array[i]) + 1 > array.length / 2){
                    return array[i];
                }
                map.put(array[i], map.get(array[i]) + 1);
            }else{
                map.put(array[i], 1);
            }
        }
        return 0;
    }
}
```

##### 169.2.2 方法二 Bruce force timeO(n**2)  + spaceO(1)

你可以假设数组是非空的，并且给定的数组总是存在众数。

在剑指 offer中不成立，因为没有保证数组中总存在众数

```python
    def majorityElement(self, nums):
        majority_count = len(nums)//2
        for num in nums:
            count = sum(1 for elem in nums if elem == num)
            if count > majority_count:
                return num


```

##### 169.2.3 方法三 Sorting timeO(nlogn) spaceO(1 or n)

We sorted `nums` in place here - if that is not allowed, then we must spend linear additional space on a copy of `nums` and sort the copy instead.

```python
class Solution:
    def majorityElement(self, nums):
        nums.sort()
        return nums[len(nums)//2]

```

##### 169.2.4 方法四 random (预期时间复杂度线性)

![image-20190323154354074](/Users/zhengxinzhi/typora_img/LeetCode%E9%A2%98%E7%9B%AE%E6%80%BB%E7%BB%93/image-20190323154354074.png)

```python
import random

class Solution:
    def majorityElement(self, nums):
        majority_count = len(nums)//2
        while True:
            candidate = random.choice(nums)
            if sum(1 for elem in nums if elem == candidate) > majority_count:
                return candidate
```

##### 169.2.5 方法五 Boyer-Moore Voting Algorithm(摩尔投票法)

**摩尔投票算法是基于这个事实：每次从序列里选择两个不相同的数字删除掉（或称为“抵消”），最后剩下一个数字或几个相同的数字，就是出现次数大于总数一半的那个**。

你可以假设数组是非空的，并且给定的数组总是存在众数。

在剑指 offer中不成立，因为没有保证数组中总存在众数

![image-20190323155144541](/Users/zhengxinzhi/typora_img/LeetCode%E9%A2%98%E7%9B%AE%E6%80%BB%E7%BB%93/image-20190323155144541.png)

```python
    def majorityElement(self, nums):
        count = 0
        candidate = None

        for num in nums:
            if count == 0:
                candidate = num
            count += (1 if num == candidate else -1)

        return candidate
```

##### 169.2.5 方法6 位运算[todo]

```java
public int majorityElement(int[] nums) {
        int[] bit = new int[32];
        for (int i = 0; i < nums.length; i++) {
            for (int j = 0; j < 32; j++) {
                bit[j] += (nums[i] >> j) & 1;
            }
        }
        int majority = 0;
        for (int j = 0; j < 32; j++) {
            bit[j] = bit[j] > (nums.length / 2)? 1 : 0;
            majority += bit[j] << j;
        }
        return majority;
    }
```

### 209. 长度最小的子数组[双指针推荐] [二分法todo]

#### 1.题目描述

给定一个含有 **n** 个正整数的数组和一个正整数 **s ，**找出该数组中满足其和 **≥ s** 的长度最小的连续子数组**。**如果不存在符合条件的连续子数组，返回 0。

**示例:** 

```
输入: s = 7, nums = [2,3,1,2,4,3]
输出: 2
解释: 子数组 [4,3] 是该条件下的长度最小的连续子数组。
```

**进阶:**

如果你已经完成了*O*(*n*) 时间复杂度的解法, 请尝试 *O*(*n* log *n*) 时间复杂度的解法。

#### 2.解法

##### 2.1 方法一 双指针

![image-20191012154509633](/Users/zhengxinzhi/typora_img/LeetCode题目总结/image-20191012154509633.png)

```python
    def minSubArrayLen(self, s: int, nums: List[int]) -> int:
        sumv = 0
        minLen = len(nums) + 1
        l, r = 0, 0
        while r < len(nums):
            sumv += nums[r]
            while sumv >= s:
                minLen = min(minLen, r - l + 1)
                sumv -= nums[l]
                l += 1
            r += 1
        return minLen if minLen != len(nums) + 1 else 0
```



##### 2.2 方法二 二分查找 逐元素累加 

```java
class Solution {
    public int minSubArrayLen(int s, int[] nums) {
        int[] sums = new int[nums.length + 1];
        for (int i = 1; i < sums.length; i++){
          	sums[i] = sums[i - 1] + nums[i - 1];
        } 
        int minLen = Integer.MAX_VALUE;
        // i是开始结点 
        for (int i = 0; i < sums.length; i++) {
            int end = binarySearch(i + 1, sums.length - 1, sums[i] + s, sums);
            if (end == sums.length) break;
            if (end - i < minLen) minLen = end - i;
        }
        return minLen == Integer.MAX_VALUE ? 0 : minLen;
    }
    
    private int binarySearch(int lo, int hi, int key, int[] sums) {
        while (lo <= hi) {
           int mid = (lo + hi) / 2;
           if (sums[mid] >= key){
               hi = mid - 1;
           } else {
               lo = mid + 1;
           }
        }
        return lo;
    }
}
```

### 215. 数组中的第K个最大元素

#### 215.1 题目描述

在未排序的数组中找到第 **k** 个最大的元素。请注意，你需要找的是数组排序后的第 k 个最大的元素，而不是第 k 个不同的元素。

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

**说明:**

你可以假设 k 总是有效的，且 1 ≤ k ≤ 数组的长度。O(n)

#### 215.2 解法

##### 215.2.1 方法一 partition 重点看第二个

```python
import random
class Solution:
    def findKthLargest(self, nums: List[int], k: int) -> int:
        """
            找到第k大, 即找到第小的元素
            方法1:
                先排序 再挑第(n + 1 - k)个 O(nlogn)
                
            方法2:
                1.随机化
                2.构造partition: 找到指定元素的位置, 并把比它的小的放左面, 其它的放右面
                3.把2中获取到的位置(+1)和(n+1-k)进行比较(分而治之)
                
                
        """
        def partition(nums, start, end, item):
            """
                找到指定元素的位置, 
                并把比它的小的放左面, 其它的放右面
            """
            i = start - 1            
            for j in range(start, end):
                if nums[j] < item:
                    nums[i+1], nums[j] = nums[j], nums[i+1]
                    i += 1
                else:
                    continue
            nums[i+1], nums[end] = nums[end], nums[i+1]
            return i+1
            
            
        def random_findKthLargest(nums, start, end, k):  
            n = len(nums)
            # 1.随机化
            i = random.randint(start, end)
            nums[i], nums[end] = nums[end], nums[i]

            # 2.partition
            x = partition(nums, start, end, nums[end])
            # 3.分而治之
            if  x + 1 == n + 1 - k:
                return nums[x]
            elif x + 1 > n + 1 - k:
                return random_findKthLargest(nums, start, x - 1, k)
            else:
                return random_findKthLargest(nums, x + 1, end, k)

        return random_findKthLargest(nums, 0, len(nums)-1, k)
```

```python
import random
class Solution:
    def findKthLargest(self, nums: List[int], k: int) -> int:
        def partition(l, r):
            i = l - 1
            for j in range(l, r):
                if nums[j] < nums[r]:
                    i += 1
                    nums[i], nums[j] = nums[j], nums[i]
            nums[i + 1], nums[r] = nums[r], nums[i + 1]
            return i + 1
            
            
        def random_findKthSmallest(l, r, k):
            index = random.randint(l, r)
            nums[index], nums[r] = nums[r], nums[index]
            q = partition(l, r)
            if q == k:
                return nums[q]
            elif q < k:
                return random_findKthSmallest(q + 1, r, k)
            else:
                return random_findKthSmallest(l, q - 1, k)
        
        return random_findKthSmallest(0, len(nums) - 1, len(nums) - k)
```

##### 215.2.2 最小堆 Time: O(k) + O(n * logk) | Space: O(K)

```python
import heapq
class Solution(object):
    def findKthLargest(self, nums, k):
        min_heap = [-float('inf')] * k
        heapq.heapify(min_heap)
        for num in nums:
            if num > min_heap[0]:
                heapq.heappop(min_heap)
                heapq.heappush(min_heap, num)
        return min_heap[0]
```

##### 215.2.3 最大堆 Time: O(n + klog(n)) | Space: O(n)

首先解释下为什么要`nums = [-num for num in nums]`. 因为Python的Standard Library里面调用heapify的时候，永远是一个min_heap，然后因为没有Max Heap的implementation，你要做的就是通过Min Heap来模拟Max Heap的运算，**最简单的就是将所有的数变成`-num`**，这是不是一个好的Practice？不一定，很多人也自己implement了Max Heap的数据结构。具体推荐大家看看这个帖子：[StackOverFlow](https://stackoverflow.com/questions/2501457/what-do-i-use-for-a-max-heap-implementation-in-python)

```python
import heapq
class Solution(object):
    def findKthLargest(self, nums, k):
        nums = [-num for num in nums]
        heapq.heapify(nums)
        res = float('inf')
        for _ in range(k):
            res = heapq.heappop(nums)
        return -res
```

##### 215.2.4 Max Heap vs. Min Heap

哪个算法更加好？

Max: `Time: O(n + klog(n)) | Space: O(n)`
Min: `Time: O(k) + O((n-k) * logk) | Space: O(K)`

如果考虑k无限接近n
Max: `O(n + nlog(n)) ~= O(nlogn)`
Min: `O(n + logk) ~= O(n)`

如果考虑k = 0.5n
Max: `O(n + nlogn)`
Min: `O(n + nlogn)`

如果考虑n 无限大
Max: `O(constant * n)` 为什么是constant * n，[参考](http://www.cs.umd.edu/~meesh/351/mount/lectures/lect14-heapsort-analysis-part.pdf)
Min: `O(log(k) * n)`





### 238. 除自身以外数组的乘积

#### 238.1 题目描述

给定长度为 *n* 的整数数组 `nums`，其中 *n* > 1，返回输出数组 `output` ，其中 `output[i]` 等于 `nums` 中除 `nums[i]` 之外其余各元素的乘积。

**示例:**

```
输入: [1,2,3,4]
输出: [24,12,8,6]
```

**说明:** 请**不要使用除法，**且在 O(*n*) 时间复杂度内完成此题。

**进阶：**
你可以在常数空间复杂度内完成这个题目吗？（ 出于对空间复杂度分析的目的，输出数组**不被视为**额外空间。）

#### 238.2 解法

##### 238.2.1 方法一 逐元素累乘

```python
import math
class Solution:
    def productExceptSelf(self, nums: List[int]) -> List[int]:
        forward = [1]
        for num in nums:
            forward.append(forward[-1]*num)
        forward = forward[:-1]
        
        back = [1]
        for num in nums[::-1]:
            back.append(back[-1]*num)
        back = back[:-1][::-1]
        return [forward[i] * back[i] for i, num in enumerate(nums)]
```

```java
public class Solution {
public int[] productExceptSelf(int[] nums) {
    int n = nums.length;
    int[] res = new int[n];
    res[0] = 1;
    for (int i = 1; i < n; i++) {
        res[i] = res[i - 1] * nums[i - 1];
    }
    int right = 1;
    for (int i = n - 1; i >= 0; i--) {
        res[i] *= right;
        right *= nums[i];
    }
    return res;
}
```

### 240.搜索二维矩阵II

#### 题目描述

编写一个高效的算法来搜索 m x n 矩阵 matrix 中的一个目标值 target。该矩阵具有以下特性：

每行的元素从左到右升序排列。
每列的元素从上到下升序排列。
示例:

现有矩阵 matrix 如下：

[
  [1,   4,  7, 11, 15],
  [2,   5,  8, 12, 19],
  [3,   6,  9, 16, 22],
  [10, 13, 14, 17, 24],
  [18, 21, 23, 26, 30]
]
给定 target = 5，返回 true。

给定 target = 20，返回 false。

#### 解法

##### 解法一 矩阵二分查找

```
    二分查找： 从左下角或右上角开始作为中值
             如果小于target 向右
             如果大于target 向下
```

```python
class Solution:
    def searchMatrix(self, matrix, target):
        """
        :type matrix: List[List[int]]
        :type target: int
        :rtype: bool
        二分查找： 从左下角或右上角开始作为中值
                 如果小于target 向右
                 如果大于target 向下
        """
        if len(matrix) == 0 or len(matrix[0]) == 0:
            return False
        
        m = len(matrix)
        n = len(matrix[0])
        i = m - 1
        j = 0
        mid = matrix[i][j]
        while i >= 0 and j < n:
            if target == matrix[i][j]:
                return True
            elif target < matrix[i][j]:
                i -= 1
            elif target > matrix[i][j]:
                j += 1
        
        return False
```

### 242. 有效的字母异位词

#### 242.1 题目描述

给定两个字符串 *s* 和 *t* ，编写一个函数来判断 *t* 是否是 *s* 的一个字母异位词。

**示例 1:**

```
输入: s = "anagram", t = "nagaram"
输出: true
```

**示例 2:**

```
输入: s = "rat", t = "car"
输出: false

```

**说明:**
你可以假设字符串只包含小写字母。

**进阶:**
如果输入字符串包含 unicode 字符怎么办？你能否调整你的解法来应对这种情况？

#### 242.2 解法

##### 242.2.1 方法一 Hash

```python
class Solution(object):
    def isAnagram(self, s, t):
        """
        :type s: str
        :type t: str
        :rtype: bool
        """
        
        dic = dict()
        for i in s:
            dic.setdefault(i, 0)
            dic[i] += 1
            
        for j in t:
            if j not in dic or dic[j] == 0:
                return False
            else:
                dic[j] -= 1
        
        for key, val in dic.items():
            if val != 0:
                return False
            
        return True

```

**Complexity analysis**

- Time complexity : O(n)O(n). Time complexity is O(n)O(n) because accessing the counter table is a constant time operation.
- Space complexity : O(1)O(1). Although we do use extra space, the space complexity is O(1)O(1) because the table's size stays constant no matter how large nn is.

##### 242.2.2 方法二 数组 

不能解决进阶问题

```python
def isAnagram2(self, s, t):
    # ord() : 它以一个字符串（Unicode 字符）作为参数，返回对应的 ASCII 数值
    dic1, dic2 = [0]*26, [0]*26
    for item in s:
        dic1[ord(item)-ord('a')] += 1
    for item in t:
        dic2[ord(item)-ord('a')] += 1
    return dic1 == dic2

```

242.2.3 方法三 排序 比较

```python
def isAnagram3(self, s, t):
    return sorted(s) == sorted(t)

```



### 260. 只出现一次的数字 III [推荐]

#### 260.1 题目描述

给定一个整数数组 `nums`，其中恰好有两个元素只出现一次，其余所有元素均出现两次。 找出只出现一次的那两个元素。

**示例 :**

```
输入: [1,2,1,3,2,5]
输出: [3,5]
```

**注意：**

1. 结果输出的顺序并不重要，对于上面的例子， `[5, 3]` 也是正确答案。
2. 你的算法应该具有线性时间复杂度。你能否仅使用常数空间复杂度来实现？

#### 260.2 解法

##### 260.2.1 方法一 hash

```python
class Solution:
    def singleNumber(self, nums: List[int]) -> List[int]:
        """
            方法一: dict
        """
        dic = dict()
        for num in nums:
            if num in dic:
                del dic[num]
            else:
                dic.setdefault(num, 1)
        return list(dic.keys())
```

##### 260.2.2 方法二 位运算(二遍, 第一遍得到了两个单值异或, 第二遍和上一遍结果)

```java
public class Solution {
    public int[] singleNumber(int[] nums) {
        int diff = 0;
        for (int num : nums) {
            diff ^= num; 
        }
        // pick one bit as flag 
        // (~ (diff - 1)) == -diff
        // x代表的是只出现一遍的值的A B的异或 A^B
        // x & (-x) 取得是x二进制表示中最小位上的1其他位为0代表的数值
        // 则bitFlag为A与B有区别的最小位代表的值 异或：不一样的位置为1
        int bitFlag = (diff & (~ (diff - 1)));
        int[] res = new int[2];
        for (int num : nums) {
            if ((num  & bitFlag) == 0) {
                res[0] ^= num;
            } else {
                res[1] ^= num;
            }
        }
        return res;
    }
}
```

###278.第一个错误的版本

#### 题目描述

你是产品经理，目前正在带领一个团队开发新的产品。不幸的是，你的产品的最新版本没有通过质量检测。由于每个版本都是基于之前的版本开发的，所以错误的版本之后的所有版本都是错的。

假设你有 n 个版本 [1, 2, ..., n]，你想找出导致之后所有版本出错的第一个错误的版本。

你可以通过调用 bool isBadVersion(version) 接口来判断版本号 version 是否在单元测试中出错。实现一个函数来查找第一个错误的版本。你应该尽量减少对调用 API 的次数。

示例:

给定 n = 5，并且 version = 4 是第一个错误的版本。

调用 isBadVersion(3) -> false
调用 isBadVersion(5) -> true
调用 isBadVersion(4) -> true

所以，4 是第一个错误的版本。 


#### 解法

##### 解法一 左开右闭二分查找
```python
# The isBadVersion API is already defined for you.
# @param version, an integer
# @return a bool
# def isBadVersion(version):

class Solution(object):
    def firstBadVersion(self, n):
        """
        :type n: int
        :rtype: int
        """
        l, r = 1, n
        while l < r:
            mid = l + (r - l) // 2
            if not isBadVersion(mid):
                l = mid + 1
            else:
                r = mid
        return l
```

##### 解法二
```python

```

###283.移动零

#### 题目描述

给定一个数组 nums，编写一个函数将所有 0 移动到数组的末尾，同时保持非零元素的相对顺序。

示例:

输入: [0,1,0,3,12]
输出: [1,3,12,0,0]
说明:

必须在原数组上操作，不能拷贝额外的数组。
尽量减少操作次数。


#### 解法

##### 解法一 插入排序
```python
class Solution:
    def moveZeroes(self, nums: List[int]) -> None:
        """
        排序
        插入排序
        """
        if not nums or len(nums) == 1:
            return
        
        j = -1
        for i in range(len(nums)):
            if nums[i] != 0:
                j += 1
                nums[i], nums[j] = nums[j], nums[i]
        
```

##### 解法二
```python

```

###287.寻找重复数

#### 题目描述

给定一个包含 n + 1 个整数的数组 nums，其数字都在 1 到 n 之间（包括 1 和 n），可知至少存在一个重复的整数。假设只有一个重复的整数，找出这个重复的数。

示例 1:

输入: [1,3,4,2,2]
输出: 2
示例 2:

输入: [3,1,3,4,2]
输出: 3
说明：

不能更改原数组（假设数组是只读的）。
只能使用额外的 O(1) 的空间。
时间复杂度小于 O(n2) 。
数组中只有一个重复的数字，但它可能不止重复出现一次。


#### 解法

##### 解法一 排序和Hash（不满足条件）
```python
    def findDuplicate(self, nums):
        nums.sort()
        for i in range(1, len(nums)):
            if nums[i] == nums[i-1]:
                return nums[i]

class Solution:
    def findDuplicate(self, nums):
        seen = set()
        for num in nums:
            if num in seen:
                return num
            seen.add(num)
```

##### 解法二 弗洛伊德的乌龟和兔子（循环检测）

本质上是个快慢指针

乌龟走一步 兔子走两步 当相遇时知道兔子比乌龟快一倍，即乌龟再继续走，兔子从头走

使用数组中的值作为索引下标进行遍历，遍历的结果肯定是一个环（有一个重复元素） 检测重复元素问题转换成检测环的入口 为了找到环的入口，可以进行如下步骤：

设置两个快慢指针， fast每次走两步，slow每次走一步，最终走了slow走了n步与fast相遇，fast走了2*n，fast可能比slow多饶了环的i圈，得到环的周长为n/i
slow指针继续走, 且另设第三个指针每次走一步，两个指针必定在入口处相遇
假设环的入口和起点的距离时m
当第三个指针走了m步到环的入口时
slow刚好走了n + m步，换句话说时饶了环i圈（环的周长为n/i）加m步（起点到入口的距离）
得到相遇的是环的入口，入口元素即为重复元素

```python
class Solution:
    def findDuplicate(self, nums):
        # Find the intersection point of the two runners.
        slow = nums[0]
        fast = nums[0]
        while True:
            slow = nums[slow]
            fast = nums[nums[fast]]
            # 在数组中相遇了，循环的入口元素即是重复元素
            if slow == fast:
                break
        
        # 找到循环的入口元素
        ptr1 = nums[0]
        ptr2 = slow
        while ptr1 != ptr2:
            ptr1 = nums[ptr1]
            ptr2 = nums[ptr2]
        
        return ptr1

```

### 289.生命游戏

#### 题目描述

根据百度百科，生命游戏，简称为生命，是英国数学家约翰·何顿·康威在1970年发明的细胞自动机。

给定一个包含 m × n 个格子的面板，每一个格子都可以看成是一个细胞。每个细胞具有一个初始状态 live（1）即为活细胞， 或 dead（0）即为死细胞。每个细胞与其八个相邻位置（水平，垂直，对角线）的细胞都遵循以下四条生存定律：

如果活细胞周围八个位置的活细胞数少于两个，则该位置活细胞死亡；
如果活细胞周围八个位置有两个或三个活细胞，则该位置活细胞仍然存活；
如果活细胞周围八个位置有超过三个活细胞，则该位置活细胞死亡；
如果死细胞周围正好有三个活细胞，则该位置死细胞复活；
根据当前状态，写一个函数来计算面板上细胞的下一个（一次更新后的）状态。下一个状态是通过将上述规则同时应用于当前状态下的每个细胞所形成的，其中细胞的出生和死亡是同时发生的。

示例:

输入: 
[
  [0,1,0],
  [0,0,1],
  [1,1,1],
  [0,0,0]
]
输出: 
[
  [0,0,0],
  [1,0,1],
  [0,1,1],
  [0,1,0]
]
进阶:

你可以使用原地算法解决本题吗？请注意，面板上所有格子需要同时被更新：你不能先更新某些格子，然后使用它们的更新后的值再更新其他格子。
本题中，我们使用二维数组来表示面板。原则上，面板是无限的，但当活细胞侵占了面板边界时会造成问题。你将如何解决这些问题？


#### 解法

##### 解法一

矩阵的元素是同时改变的，也就是说不能用更新后的元素进行计算，但是原地算法又不能使用额外的空间存储更新后的元素，所以想到了用原矩阵同时保存原始数据和新数据。
为了同时保存两个数据，那么原来的0和1两个变量就不能满足要求了，由于一共有四种可能，活-活，活-死，死-活，死-死。所以使用四个标识符来记录信息。

```python
class Solution:
    def gameOfLife(self, board: List[List[int]]) -> None:
        """
        Do not return anything, modify board in-place instead.
        """
        m = len(board)
        if not m:
            return
        n = len(board[0])
        if not n:
            return 
        for i in range(m):
            for j in range(n):
                cnt = 0
                # 左 左上 上 右上 右 右下 下 左下
                directions = [(0, -1), (-1, -1), (-1, 0), (-1, 1), (0, 1), (1, 1), (1, 0), (1, -1)]
                for dx, dy in directions:
                    if (i + dx) >= 0 and (i + dx) < m and (j + dy) >= 0 and (j + dy) < n and board[i + dx][j + dy] in [1, 11, -9]:
                        cnt += 1
                print((i, j, cnt))
                if cnt == 3 or (board[i][j] == 1 and cnt == 2):
                    board[i][j] += 10
                else:
                    board[i][j] -= 10

        for i in range(m):
            for j in range(n):
                if board[i][j] > 0:
                    board[i][j] = 1
                else:
                    board[i][j] = 0
                
        
```

##### 解法二 上面代码优化

只修改 | 活 -> 死: -1  | 死 -> 活:2 | 其他不改变

```python

```

###303.[区域和检索 - 数组不可变](https://leetcode-cn.com/problems/range-sum-query-immutable/)

#### 题目描述

给定一个整数数组  nums，求出数组从索引 i 到 j  (i ≤ j) 范围内元素的总和，包含 i,  j 两点。

示例：

给定 nums = [-2, 0, 3, -5, 2, -1]，求和函数为 sumRange()

sumRange(0, 2) -> 1
sumRange(2, 5) -> -1
sumRange(0, 5) -> -3
说明:

你可以假设数组不可变。
会多次调用 sumRange 方法。


#### 解法

##### 解法一 前缀和
```python
class NumArray:

    def __init__(self, nums: List[int]):
        self.cumsum = [0]
        for num in nums:
            self.cumsum.append(self.cumsum[-1] + num)

    def sumRange(self, i: int, j: int) -> int:
        return self.cumsum[j + 1] - self.cumsum[i]


# Your NumArray object will be instantiated and called as such:
# obj = NumArray(nums)
# param_1 = obj.sumRange(i,j)
```

##### 解法二
```python

```

### [304. 二维区域和检索 - 矩阵不可变](https://leetcode-cn.com/problems/range-sum-query-2d-immutable/)

#### 题目描述

给定一个二维矩阵，计算其子矩形范围内元素的总和，该子矩阵的左上角为 (row1, col1) ，右下角为 (row2, col2)。

![Range Sum Query 2D](/Users/zhengxinzhi/typora_img/LeetCode题目总结/304.png)


上图子矩阵左上角 (row1, col1) = (2, 1) ，右下角(row2, col2) = (4, 3)，该子矩形内元素的总和为 8。

示例:

给定 matrix = [
  [3, 0, 1, 4, 2],
  [5, 6, 3, 2, 1],
  [1, 2, 0, 1, 5],
  [4, 1, 0, 1, 7],
  [1, 0, 3, 0, 5]
]

sumRegion(2, 1, 4, 3) -> 8
sumRegion(1, 1, 2, 2) -> 11
sumRegion(1, 2, 2, 4) -> 12
说明:

你可以假设矩阵不可变。
会多次调用 sumRegion 方法。
你可以假设 row1 ≤ row2 且 col1 ≤ col2。


#### 解法

##### 解法一 前缀和
```python
class NumMatrix:
    """
        类似于前缀和
        我们存储以某一结点为右下边界的所有前缀
    """
    def __init__(self, matrix: List[List[int]]):
        if not matrix or len(matrix) == 0 or len(matrix[0]) == 0:
            return           
        m = len(matrix)
        n = len(matrix[0])
        dp = [[0] * (n + 1) for _ in range(m + 1)]

        for i in range(1, m + 1):
            for j in range(1, n + 1):
                dp[i][j] = matrix[i - 1][j - 1] + dp[i][j - 1] + dp[i - 1][j] - dp[i - 1][j - 1]

        self.cumsum = dp1

    def sumRegion(self, row1: int, col1: int, row2: int, col2: int) -> int:
        return self.cumsum[row2 + 1][col2 + 1] - self.cumsum[row2 + 1][col1] - self.cumsum[row1][col2 + 1] + self.cumsum[row1][col1]


# Your NumMatrix object will be instantiated and called as such:
# obj = NumMatrix(matrix)
# param_1 = obj.sumRegion(row1,col1,row2,col2)
```

##### 解法二
```python

```

###324.摆动排序 [推荐] 

#### 题目描述

给定一个无序的数组 nums，将它重新排列成 nums[0] < nums[1] > nums[2] < nums[3]... 的顺序。

示例 1:

输入: nums = [1, 5, 1, 1, 6, 4]
输出: 一个可能的答案是 [1, 4, 1, 5, 1, 6]
示例 2:

输入: nums = [1, 3, 2, 2, 3, 1]
输出: 一个可能的答案是 [2, 3, 1, 3, 1, 2]
说明:
你可以假设所有输入都会得到有效的结果。

进阶:
你能用 O(n) 时间复杂度和 / 或原地 O(1) 额外空间来实现吗？


#### 解法

##### 解法一 排序 O(nlogn)

对数组进行排序，然后将前半段倒序填入奇数下标，将后半段倒序填入偶数下标，其原理是如果题目有解，则对于排好序的数组，间隔超过n/2的两个元素必不相等。时间复杂度O(nlogn)，空间复杂度O(n)。

```python
def wiggleSort(self, nums):
    nums.sort()
    half = len(nums[::2])
    nums[::2], nums[1::2] = nums[:half][::-1], nums[half:][::-1]
```

##### 解法二 虚拟索引 + 荷兰国旗问题（三色球问题）

先用findKthLargestElement的思路将数组进行partition，让median的左边全都大于median，右边全都小于median。举例来说，[1,4,5,6,1,1] => [5,6,4,1,1,1]

利用virtual indexing的性质，条件为`mapIndex = (1 + 2 * index) % (n | 1)`，n 代表数组个数

**n | 1作用 最低位置1， 让奇数不变 偶数加1**

**这么映射的原因是让median左面的在odd index, 让median右面的在Even index**

可以将index这样转换 - 
index `0 1 2 3 4 5`
mapp `1 3 5 0 2 4`

解题思路：
用quickSelect方法找到median
利用sort color的方法（**荷兰国旗问题**），对mapped index所对应的值进行交换。

```c++
void wiggleSort(vector<int>& nums) {
    int n = nums.size();
    
    // Find a median.
    auto midptr = nums.begin() + n / 2;
    nth_element(nums.begin(), midptr, nums.end());
    int mid = *midptr;
    
    // Index-rewiring.
    #define A(i) nums[(1+2*(i)) % (n|1)]

    // 3-way-partition-to-wiggly in O(n) time with O(1) space.
    // 目的让虚拟索引符合 big ... big median small .. small 的形式，原始数组就变成了摇摆序列
    int i = 0, j = 0, k = n - 1;
    while (j <= k) {
        if (A(j) > mid)
            swap(A(i++), A(j++));
        else if (A(j) < mid)
            swap(A(j), A(k--));
        else
            j++;
    }
}
```

```java
     public void wiggleSort(int[] nums) {
        int median = findKthLargestElement(nums, nums.length / 2);
        int red = 0;
        int blue = nums.length - 1;
        int n = nums.length;
        int i =0;
        while(i <= blue) {
            if (nums[mapIndex(i, n)] > median) {
                swap(nums, mapIndex(red, n), mapIndex(i, n));
                red++;
                i++;
            } else if (nums[mapIndex(i, n)] < median) {
                swap(nums, mapIndex(blue, n), mapIndex(i, n));
                blue--;
            } else {
                i++;
            }
        }
        return; 
    }
    private void swap(int[] nums, int i, int j) {
        int temp = nums[i];
        nums[i] = nums[j];
        nums[j] = temp;
    }
    private int mapIndex(int index, int n) {
        return (1 + 2 * index) % (n | 1);
    }
```

### 334.递增的三元子序列

#### 题目描述

给定一个未排序的数组，判断这个数组中是否存在长度为 3 的递增子序列。

数学表达式如下:

如果存在这样的 i, j, k,  且满足 0 ≤ i < j < k ≤ n-1，
使得 arr[i] < arr[j] < arr[k] ，返回 true ; 否则返回 false 。
说明: 要求算法的时间复杂度为 O(n)，空间复杂度为 O(1) 。

示例 1:

输入: [1,2,3,4,5]
输出: true
示例 2:

输入: [5,4,3,2,1]
输出: false


#### 解法

##### 解法一 贪心

先说下这道题的思路： 首先找到一个相对小的值，然后找到比这个小一点的值大的值(中间值)，然后看能够在最后找到比中间值大的值。

我来说下为什么这种思路能保证覆盖所有的情况。

首先，如果只有一个最小值，然后找不到中间值，那么这个数组必然不包含递增的三个数（因为连递增的两个数都找不到）。

然后假设我们找到了两个递增的值，那么如果下一个值小于最小值，我们就应该将最小值的指针定位到这个值上。我们尽可能的使用最小值，防止后面出现了更小的一对递增值，而即使不出现，也不妨碍我们找到解（因为最终是看能否找到大于中间值的值）。 如果下一个值大于最小值，且小于中间值，则我们使用该值作为中间值(因为如果最小的中间值都得不到解，那么就是false，这样也保证了覆盖所有的情况)。

最后，如果找到了大于中间值的值，则为true.

```java
class Solution {
    public boolean increasingTriplet(int[] nums) {
        int one = Integer.MAX_VALUE;
        int two = Integer.MAX_VALUE;
        
        for (int num : nums) {
            if (num <= one) {
                one = num;
            } else if (num <= two) {
                two = num;
            } else {
                return true;
            }
        return false;
    }
}
```

##### 解法二
```python

```

### 341.扁平化嵌套列表迭代器

#### 题目描述

给定一个嵌套的整型列表。设计一个迭代器，使其能够遍历这个整型列表中的所有整数。

列表中的项或者为一个整数，或者是另一个列表。

示例 1:

输入: [[1,1],2,[1,1]]
输出: [1,1,2,1,1]
解释: 通过重复调用 next 直到 hasNext 返回false，next 返回的元素的顺序应该是: [1,1,2,1,1]。
示例 2:

输入: [1,[4,[6]]]
输出: [1,4,6]
解释: 通过重复调用 next 直到 hasNext 返回false，next 返回的元素的顺序应该是: [1,4,6]。


#### 解法

##### 解法一 栈
```python
# """
# This is the interface that allows for creating nested lists.
# You should not implement it, or speculate about its implementation
# """
#class NestedInteger(object):
#    def isInteger(self):
#        """
#        @return True if this NestedInteger holds a single integer, rather than a nested list.
#        :rtype bool
#        """
#
#    def getInteger(self):
#        """
#        @return the single integer that this NestedInteger holds, if it holds a single integer
#        Return None if this NestedInteger holds a nested list
#        :rtype int
#        """
#
#    def getList(self):
#        """
#        @return the nested list that this NestedInteger holds, if it holds a nested list
#        Return None if this NestedInteger holds a single integer
#        :rtype List[NestedInteger]
#        """

class NestedIterator(object):

    def __init__(self, nestedList):
        """
            借用stack
        """
        """
        Initialize your data structure here.
        :type nestedList: List[NestedInteger]
        """
        self.stack = nestedList[::-1] if nestedList else []
                
        

    def next(self):
        """
        :rtype: int
        """
        return self.stack.pop()

    def hasNext(self):
        """
        :rtype: bool
        """
        if self.stack:
            top = self.stack.pop()
            while not top.isInteger():
                self.stack += top.getList()[::-1]
                if self.stack:
                    top = self.stack.pop()
                else:
                    return False
            self.stack.append(top)
            return True
        else:
            return False
# Your NestedIterator object will be instantiated and called as such:
# i, v = NestedIterator(nestedList), []
# while i.hasNext(): v.append(i.next())
```

##### 解法二
```python

```



### 347. 前K个高频元素 [推荐]

#### 347.1 题目描述

给定一个非空的整数数组，返回其中出现频率前 **k** 高的元素。

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

**说明：**

- 你可以假设给定的 *k* 总是合理的，且 1 ≤ k ≤ 数组中不相同的元素的个数。
- 你的算法的时间复杂度**必须**优于 O(*n* log *n*) , *n* 是数组的大小。

##### 347.2 解法

该题思路容易但编码难。很容易想出要统计每个元素出现的次数，然后取出其中前 k 高的。但采用何种数据结构就比较讲究了。

第一种思路是利用优先队列，即最大堆。这里注意存入堆中的元素是 pair 类型的，且以出现的次数为 key,值为 val.因为 pair 的排序是先比较 first，再比较 second。

第二种思路是利用桶排序，10个元素每个元素出现的次数是0 ~ 10，所以桶的个数为nums.size() + 1。注意可能有相同次数的元素出现，因此桶中存储的是一个数组而非单一元素。

##### 347.2.1 方法一 hash+堆排序

```python
import heapq
class Solution:
    def topKFrequent(self, nums: List[int], k: int) -> List[int]:
        """
            1.先进行Hash: key:num val:count of num
            2.法一 堆排序:
                我们可以用堆排序来按照出现次数进行排序。
                使用一个最大堆来按照映射次数从大到小排列
                
            法二 桶排序:
                在建立好数字和其出现次数的映射后，我们按照其出现次数将数字放到对应的位置中去，
                这样我们从桶的后面向前面遍历，最先得到的就是出现次数最多的数字，
                我们找到k个后返回即可，参见代码如下：
            [Note] headp只提供最小堆的实现, 想用最大堆一个办法是加负号
        """
        # 1.Hash nums count
        cnt_dict = dict()
        for num in nums:
            cnt_dict.setdefault(num, 0)
            cnt_dict[num] += 1
         
        # 堆排序
        heap = [tuple([val, key]) for key, val in cnt_dict.items()]
        return [item[1] for item in heapq.nlargest(k, heap)]
```

![image-20190313201418135](/Users/zhengxinzhi/typora_img/LeetCode%E9%A2%98%E7%9B%AE%E6%80%BB%E7%BB%93/image-20190313201418135.png)

##### 347.2.2 方法二 hash+桶排序

```python
class Solution:
    def topKFrequent(self, nums: List[int], k: int) -> List[int]:
        """
            1.先进行Hash: key:num val:count of num
            2.法一 堆排序:
                我们可以用堆排序来按照出现次数进行排序。
                使用一个最大堆来按照映射次数从大到小排列
                
            法二 桶排序:
                在建立好数字和其出现次数的映射后，我们按照其出现次数将数字放到对应的位置中去，
                这样我们从桶的后面向前面遍历，最先得到的就是出现次数最多的数字，
                我们找到k个后返回即可，参见代码如下：
            [Note] headp只提供最小堆的实现, 想用最大堆一个办法是加负号
        """
        # 1.Hash nums count
        cnt_dict = dict()
        for num in nums:
            cnt_dict.setdefault(num, 0)
            cnt_dict[num] += 1

        # 2.桶排序
        min_cnt, max_cnt = min(cnt_dict.values()), max(cnt_dict.values())

        # 建立桶
        c = [[] for i in range(max_cnt - min_cnt + 1)]

        for num, cnt in cnt_dict.items():
            c[cnt - min_cnt].append(num)

        result = []
        for item in c[::-1]:
            if k == 0:
                return result
            if item == []:
                continue
            else:
                result.extend(item)
                k = k - len(item)
        if k == 0:
            return result
        
```



```java
public List<Integer> topKFrequent(int[] nums, int k) {

	List<Integer>[] bucket = new List[nums.length + 1];
	Map<Integer, Integer> frequencyMap = new HashMap<Integer, Integer>();

	for (int n : nums) {
		frequencyMap.put(n, frequencyMap.getOrDefault(n, 0) + 1);
	}

	for (int key : frequencyMap.keySet()) {
		int frequency = frequencyMap.get(key);
		if (bucket[frequency] == null) {
			bucket[frequency] = new ArrayList<>();
		}
		bucket[frequency].add(key);
	}

	List<Integer> res = new ArrayList<>();

	for (int pos = bucket.length - 1; pos >= 0 && res.size() < k; pos--) {
		if (bucket[pos] != null) {
			res.addAll(bucket[pos]);
		}
	}
	return res;
}
```

##### 347.2.3 collections.Counter  计数api

```python
import collections       
class Solution:
    def topKFrequent(self, nums: List[int], k: int) -> List[int]:
        """
            https://docs.python.org/3.6/library/collections.html#collections.Counter
            A Counter is a dict subclass for counting hashable objects. 
            It is an unordered collection where elements are stored as dictionary 
            keys and their counts are stored as dictionary values. 
            Counts are allowed to be any integer value including zero or negative counts. 
            The Counter class is similar to bags or multisets in other languages.
        """
        return [key for key, val in collections.Counter(nums).most_common(k)]
```

###375.猜数字大小II[[推荐]]

#### 题目描述

我们正在玩一个猜数游戏，游戏规则如下：

我从 1 到 n 之间选择一个数字，你来猜我选了哪个数字。

每次你猜错了，我都会告诉你，我选的数字比你的大了或者小了。

然而，当你猜了数字 x 并且猜错了的时候，你需要支付金额为 x 的现金。直到你猜到我选的数字，你才算赢得了这个游戏。

示例:

n = 10, 我选择了8.

第一轮: 你猜我选择的数字是5，我会告诉你，我的数字更大一些，然后你需要支付5块。
第二轮: 你猜是7，我告诉你，我的数字更大一些，你支付7块。
第三轮: 你猜是9，我告诉你，我的数字更小一些，你支付9块。

游戏结束。8 就是我选的数字。

你最终要支付 5 + 7 + 9 = 21 块钱。
给定 n ≥ 1，计算你至少需要拥有多少现金才能确保你能赢得这个游戏。


#### 解法

##### 解法一 dp 递归+记忆化

state: dp\[s][e]: 在s到e之间选择一个数的最少胜利现金

state transfer: 在s到e中遍历选择一个数字, 计算最少现金 从所有遍历结果选择最小的

```python
class Solution {
    public int getMoneyAmount(int n) {
        int[][] table = new int[n+1][n+1];
        return DP(table, 1, n);
    }

    int DP(int[][] t, int s, int e){
        if (s >= e) return 0;
        if (t[s][e] != 0) return t[s][e];
        int res = Integer.MAX_VALUE;
        for (int x = s; x <= e; x++){
            // key: state transfer
            int tmp = x + Math.max(DP(t, s, x - 1), DP(t, x + 1, e));
            res = Math.min(res, tmp);
        }
        t[s][e] = res;
        return res;
    }
}
```

##### 解法二 dp trick

state: dp\[s][e]: 在s到e之间选择一个数的最少胜利现金

state transfer: 在s到e中遍历选择一个数字, 计算最少现金 从所有遍历结果选择最小的

trick: j从小到大遍历, i从大到小遍历 可以找到gap由小到大的dp

```python
class Solution {
    public int getMoneyAmount(int n) {
        int[][] dp = new int[n+1][n+1];
        for (int j = 2; j < n + 1; j++){
            for (int i = j - 1; i > 0; i--){
                int global = Integer.MAX_VALUE;
                for (int k = i + 1; k < j; k++){
                    int local = k + Math.max(dp[i][k-1], dp[k+1][j]);
                    global = Math.min(local, global);
                }
                // init: dp[i][i+1] = i
                dp[i][j] = i+1==j?i:global;
            }
        }
        return dp[1][n];
    }
}

```

### 378. 有序矩阵中第K小的元素 [推荐]

#### 378.1 题目描述

给定一个 *n x n* 矩阵，其中每行和每列元素均按升序排序，找到矩阵中第k小的元素。
请注意，它是排序后的第k小元素，而不是第k个元素。

**示例:**

```
matrix = [
   [ 1,  5,  9],
   [10, 11, 13],
   [12, 13, 15]
],
k = 8,

返回 13。
```

**说明:** 
你可以假设 k 的值永远是有效的, 1 ≤ k ≤ n2 。

#### 378.2 解法

378.2.1 方法一  变成1维列表  partition O(z) 取巧

```python
import random
class Solution(object):
    def kthSmallest(self, matrix, k):
        """
        :type matrix: List[List[int]]
        :type k: int
        :rtype: int
        方法1: 变成1维列表 partition O(z) 其中 z=n**2
        """
        def random_partition(nums, start, end):
            x = random.randint(start, end)
            nums[x], nums[end] = nums[end], nums[x]
            item = nums[end]
            i = start - 1
            for j in range(start, end):
                if nums[j] < item:
                    i += 1
                    nums[i], nums[j] = nums[j], nums[i]
            nums[i+1], nums[end] = nums[end], nums[i+1]
            return i + 1
            
        def findKthSmallest(nums, start, end, k):
            # 2.调用随机化partition
            t = random_partition(nums, start, end)
            if t == k:
                return nums[k]
            elif t > k:
                return findKthSmallest(nums, start, t-1, k)
            else:
                return findKthSmallest(nums, t+1, end, k)
            
        
        # 1.变成1维列表
        nums = []
        for i in matrix:
            nums.extend(i)
            
        # 第k个元素在第k-1位
        return findKthSmallest(nums, 0, len(nums)-1, k-1)
```

##### 378.2.2 min_heap 利用了题目中矩阵的规律 O(klogk)

Since the matrix is sorted, we do not need to put all the elements in heap at one time. We can simply pop and put for k times. By observation, if we look at the matrix diagonally, we can tell that if we do not pop matrix[i][j], we do not need to put on matrix[i][j + 1] and matrix[i + 1][j] since they are bigger.

e.g., given the matrix below:
1 2 4
3 5 7
6 8 9
We put 1 first, then pop 1 and put 2 and 3, then pop 2 and put 4 and 5, then pop 3 and put 6…(斜着看)

靠优先级队列来排序

```python
class Solution(object):
    def kthSmallest(self, matrix, k):
        """
        :type matrix: List[List[int]]
        :type k: int
        :rtype: int
        """
        result, heap = None, []
        # (element, row, index)
        heapq.heappush(heap, (matrix[0][0], 0, 0))
        while k > 0:
            # key: pop kth 得到结果
            result, i, j = heapq.heappop(heap)
            # key: i != 0 时 元素已经放入过了
            if i == 0 and j + 1 < len(matrix):
                heapq.heappush(heap, (matrix[i][j + 1], i, j + 1))
            if i + 1 < len(matrix):
                heapq.heappush(heap, (matrix[i + 1][j], i + 1, j))
            k -= 1
        return result
```

##### 378.2.3 二分法

思路非常简单：
1.找出二维矩阵中最小的数left，最大的数right，那么第k小的数必定在left~right之间
2.mid=(left+right) / 2；在二维矩阵中寻找小于等于mid的元素个数count
3.若这个count小于k，表明第k小的数在右半部分且不包含mid，即left=mid+1, right=right，又保证了第k小的数在left~right之间
4.若这个count大于k，表明第k小的数在左半部分且可能包含mid，即left=left, right=mid，又保证了第k小的数在left~right之间
5.因为每次循环中都保证了第k小的数在left~right之间，当left==right时，第k小的数即被找出，等于right

注意：这里的left mid right是数值，不是索引位置。



```java
 public int kthSmallest(int[][] matrix, int k) {
        int row = matrix.length;
        int col = matrix[0].length;
        int left = matrix[0][0];
        int right = matrix[row - 1][col - 1];
        while (left < right) {
            // 每次循环都保证第K小的数在start~end之间，当start==end，第k小的数就是start
            int mid = (left + right) / 2;
            // 找二维矩阵中<=mid的元素总个数
            int count = findNotBiggerThanMid(matrix, mid, row, col);
            if (count < k) {
                // 第k小的数在右半部分，且不包含mid
                left = mid + 1;
            } else {
                // 第k小的数在左半部分，可能包含mid
                right = mid;
            }
        }
        return right;
    }

    private int findNotBiggerThanMid(int[][] matrix, int mid, int row, int col) {
        // 以列为单位找，找到每一列最后一个<=mid的数即知道每一列有多少个数<=mid
        int i = row - 1;
        int j = 0;
        int count = 0;
        while (i >= 0 && j < col) {
            if (matrix[i][j] <= mid) {
                // 第j列有i+1个元素<=mid
                count += i + 1;
                j++;
            } else {
                // 第j列目前的数大于mid，需要继续在当前列往上找
                i--;
            }
        }
        return count;
    }

```

###380. 常数时间插入、删除和获取随机元素 [推荐]

#### 题目描述

设计一个支持在平均 时间复杂度 O(1) 下，执行以下操作的数据结构。

insert(val)：当元素 val 不存在时，向集合中插入该项。
remove(val)：元素 val 存在时，从集合中移除该项。
getRandom：随机返回现有集合中的一项。每个元素应该有相同的概率被返回。
示例 :

// 初始化一个空的集合。
RandomizedSet randomSet = new RandomizedSet();

// 向集合中插入 1 。返回 true 表示 1 被成功地插入。
randomSet.insert(1);

// 返回 false ，表示集合中不存在 2 。
randomSet.remove(2);

// 向集合中插入 2 。返回 true 。集合现在包含 [1,2] 。
randomSet.insert(2);

// getRandom 应随机返回 1 或 2 。
randomSet.getRandom();

// 从集合中移除 1 ，返回 true 。集合现在包含 [2] 。
randomSet.remove(1);

// 2 已在集合中，所以返回 false 。
randomSet.insert(2);

// 由于 2 是集合中唯一的数字，getRandom 总是返回 2 。
randomSet.getRandom();


#### 解法

##### 解法一 set法 getRandom不是O(1)
```python
class RandomizedSet:
    """
        双向Hash
    """

    def __init__(self):
        """
        Initialize your data structure here.
        """
        self.hash = set()


    def insert(self, val: int) -> bool:
        """
        Inserts a value to the set. Returns true if the set did not already contain the specified element.
        """
        if val in self.hash:
            return False
        else:
            self.hash.add(val)
            return True
            
        

    def remove(self, val: int) -> bool:
        """
        Removes a value from the set. Returns true if the set contained the specified element.
        """
        if val in self.hash:
            self.hash.remove(val)
            return True
        else:
            return False

    def getRandom(self) -> int:
        """
        Get a random element from the set.
        """
        rand_i  = random.randint(len(self.hash))
        return list(self.hash)[rand_i]
        


# Your RandomizedSet object will be instantiated and called as such:
# obj = RandomizedSet()
# param_1 = obj.insert(val)
# param_2 = obj.remove(val)
# param_3 = obj.getRandom()
```

##### 解法二 索引hash

存储各元素的索引字典，如果移除把最后一个元素交换位置

```python
class RandomizedSet:
    """
        双向Hash
    """

    def __init__(self):
        """
        Initialize your data structure here.
        """
        # pos 存储在nums上的索引
        self.nums, self.pos = [], {}


    def insert(self, val: int) -> bool:
        """
        Inserts a value to the set. Returns true if the set did not already contain the specified element.
        """
        if val not in self.pos:
            self.nums.append(val)
            self.pos[val] = len(self.nums) - 1
            return True
        return False
            
        

    def remove(self, val: int) -> bool:
        """
        Removes a value from the set. Returns true if the set contained the specified element.
        """
        if val in self.pos:
            idx, last = self.pos[val], self.nums[-1]
            self.nums[idx], self.pos[last] = last, idx
            self.nums.pop(); self.pos.pop(val, 0)
            return True
        return False
    

    def getRandom(self) -> int:
        """
        Get a random element from the set.
        """
        return self.nums[random.randint(0, len(self.nums) - 1)]
        


# Your RandomizedSet object will be instantiated and called as such:
# obj = RandomizedSet()
# param_1 = obj.insert(val)
# param_2 = obj.remove(val)
# param_3 = obj.getRandom()
```

### 384.打乱数组

#### 题目描述

打乱一个没有重复元素的数组。

示例:

// 以数字集合 1, 2 和 3 初始化数组。
int[] nums = {1,2,3};
Solution solution = new Solution(nums);

// 打乱数组 [1,2,3] 并返回结果。任何 [1,2,3]的排列返回的概率应该相同。
solution.shuffle();

// 重设数组到它的初始状态[1,2,3]。
solution.reset();

// 随机返回数组[1,2,3]打乱后的结果。
solution.shuffle();


#### 解法

##### 解法一 Fisher–Yates shuffle 洗牌算法

等概率数组随机排列

写下从 1 到 N 的数字
取一个从 1 到剩下的数字（包括这个数字）的随机数 k
从低位开始，得到第 k 个数字（这个数字还没有被取出），把它写在独立的一个列表的最后一位
重复第 2 步，直到所有的数字都被取出
第 3 步写出的这个序列，现在就是原始数字的随机排列

**现代版本的描述与原始略有不同，因为如果按照原始方法，愚蠢的计算机会花很多无用的时间去计算上述第 3 步的剩余数字。这里的方法是在每次迭代时交换这个被取出的数字到原始列表的最后。这样就将时间复杂度从 O(n^2) 减小到了 O(n)。

-- To shuffle an array a of n elements (indices 0..n-1):
for i from n−1 downto 1 do
     j ← random integer such that 0 ≤ j ≤ i
     exchange a[j] and a[i]

```python
import random
class Solution:

    def __init__(self, nums: List[int]):
        self.nums = nums
        self.backup = list(nums)

    def reset(self) -> List[int]:
        """
        Resets the array to its original configuration and return it.
        """
        self.nums = list(self.backup)
        return self.nums

    def shuffle(self) -> List[int]:
        """
        Returns a random shuffling of the array.
        """
        
    def shuffle(self):
        for i in range(len(self.nums)):
            swap_idx = random.randrange(i, len(self.nums))
            self.nums[i], self.nums[swap_idx] = self.nums[swap_idx], self.nums[i]
        
        return self.nums

# Your Solution object will be instantiated and called as such:
# obj = Solution(nums)
# param_1 = obj.reset()
# param_2 = obj.shuffle()
```

##### 解法二
```python

```

###406.根据身高重建队列 [推荐]

#### 题目描述

假设有打乱顺序的一群人站成一个队列。 每个人由一个整数对(h, k)表示，其中h是这个人的身高，k是排在这个人前面且身高大于或等于h的人数。 编写一个算法来重建这个队列。

注意：
总人数少于1100人。

示例

输入:
[[7,0], [4,4], [7,1], [5,0], [6,1], [5,2]]

输出:
[[5,0], [7,0], [5,2], [6,1], [4,4], [7,1]]


#### 解法

##### 解法一 先排序 再补位置

h升序 k升序

```python
class Solution(object):
    def reconstructQueue(self, people):
        """
        :type people: List[List[int]]
        :rtype: List[List[int]]
        """
        nums = sorted(people, key=lambda x: (x[0], x[1]))
 
        res = [None for _ in range(len(nums))] 
        for h, k in nums:
            j = 0
            for i in range(len(res)):
                if k == j and res[i] == None:
                    res[i] = [h, k]
                    break
                if res[i] == None or res[i][0] >= h:
                    j += 1

        return res
```

##### 解法二 先排序 再插入

h降序， k升序

假设候选队列为 A，已经站好队的队列为 B.

从 A 里挑身高最高的人 x 出来，插入到 B. 因为 B 中每个人的身高都比 x 要高，因此 x 插入的位置，就是看 x 前面应该有多少人就行了。比如 x 前面有 5 个人，那 x 就插入到队列 B 的第 5 个位置。

```python
class Solution(object):
    def reconstructQueue(self, people):
        """
        :type people: List[List[int]]
        :rtype: List[List[int]]
        """
        nums = sorted(people, key=lambda x: (-x[0], x[1]))
        res = []
        for h, k in nums:
            res.insert(k, (h, k))
        return res
```

### 416.分割等和子集[推荐]

#### 题目描述

给定一个只包含正整数的非空数组。是否可以将这个数组分割成两个子集，使得两个子集的元素和相等。

注意:

每个数组中的元素不会超过 100
数组的大小不会超过 200
示例 1:

输入: [1, 5, 11, 5]

输出: true

解释: 数组可以分割成 [1, 5, 5] 和 [11].


示例 2:

输入: [1, 2, 3, 5]

输出: false

解释: 数组不能分割成两个元素和相等的子集.

#### 解法

            只包含正整数
            分成元素和想等 那么每个集合都是 sum(nums) // 2 （在sum(nums)为偶数 ，奇数没答案
            问题变成哪几个元素和等于 sum(nums) // 2 
##### 解法一 带结果回溯法

回溯 + 逆排序

```python
class Solution:
    def canPartition(self, nums: List[int]) -> bool:
        total = sum(nums)
        if total & 1:
            return False
        n = len(nums)
        nums = sorted(nums, reverse=True)

        def generate(i, target):
            if target == 0:
                return True
            if i < n and target < nums[i]:
                return False
            for j in range(i, n):
                if generate(j + 1, target - nums[i]):
                    return True
            return False
            
        return generate(0, total // 2)
```

##### 解法二 DP 01背包问题

这是一道以 0-1 背包问题为背景的算法练习题，我们把这个题目翻译一下：

给定一个只包含正整数的非空数组。是否可以从这个数组中挑选出一些正整数，每个数只能用一次，使得这些数的和等于整个数组元素的和的一半。

0-1 背包问题也是最基础的背包问题，它的特点是：待挑选的物品有且仅有一个，可以选择也可以不选择。下面我们定义状态，不妨就用问题的问法定义状态试试看。

dp[i][j] ：表示从数组的 [0, i] 这个子区间内挑选一些正整数，每个数只能用一次，使得这些数的和等于 j。

根据我们学习的 0-1 背包问题的状态转移推导过程，新来一个数，例如是 nums[i]，根据这个数可能选择也可能不被选择：

如果不选择 nums[i]，在 [0, i - 1] 这个子区间内已经有一部分元素，使得它们的和为 j ，那么 dp[i][j] = true；
如果选择 nums[i]，在 [0, i - 1] 这个子区间内就得找到一部分元素，使得它们的和为 j - nums[i] ，我既然这样写出来了，你就应该知道，这里讨论的前提条件是 nums[i] <= j。
以上二者成立一条都行。于是得到状态转移方程是：

dp[i][j] = dp[i - 1][j] or dp[i - 1][j - nums[i]], (nums[i] <= j)
于是按照 0-1 背包问题的模板，我们不难写出以下代码。

```python
class Solution:
    def canPartition(self, nums: List[int]) -> bool:
        """
            方法三：DP-01背包问题 
            dp[i][j] ：表示从数组的 [0, i] 这个子区间内挑选一些正整数，每个数只能用一次，使得这些数的和等于 j。
            
        """
        total = sum(nums)
        if total & 1:
            return False
        n = len(nums)
        target = total // 2
        # dp[i][j] ：表示从数组的 [0, i] 这个子区间内挑选一些正整数，每个数只能用一次，使得这些数的和等于 j。
        dp = [[False for _ in range(target + 1)] for _ in range(n)]
        
        # 先写第 1 行：看看第 1 个数是不是能够刚好填满容量为 target
        for j in range(target + 1):
            dp[0][j] = True if nums[0] == j else False
            
        # i 表示物品索引
        for i in range(1, n):
            # 表示容量
            for j in range(target + 1):
                dp[i][j] = dp[i-1][j] or (j >= nums[i] and dp[i-1][j - nums[i]])
        return dp[-1][-1]

```

复杂度分析:
时间复杂度:O(NC):这里N是数组元素的个数,C是数组元素的和的一半。
空间复杂度:O(NC)

##### 解法三 DP 类似01背包 二维变一维

在编写的过程中，我们还发现：

1、填写第 1 个物品是否满足状态的时候，因为 nums[0] 是不变的，而 j 在不断增加，只要满足 nums[0] == j 成立，后面的 j 就不必做判断了，因为一定有 nums[0] < j 成立；

2、从后向前写的过程中，一旦 j >= nums[i] 不满足，可以马上退出当前循环，因为后面 j 肯定越来越小，没有必要继续做判断，直接进入外层循环的下一层。

以下代码根据上面的“参考代码 1”修改，修改和需要注意的地方，我都加了注释。

```python
class Solution:
    def canPartition(self, nums: List[int]) -> bool:
        total = sum(nums)
        if total & 1:
            return False
        n = len(nums)
        target = total // 2
        # dp[i][j] ：表示从数组的 [0, i] 这个子区间内挑选一些正整数，每个数只能用一次，使得这些数的和等于 j。
        dp = [False for _ in range(target + 1)]
        
        # 先写第 1 行：看看第 1 个数是不是能够刚好填满容量为 target
        for j in range(target + 1):
            if nums[0] == j:
                dp[j] = True
                # 如果等于，后面就不用做判断了，因为 j 会越来越大，肯定不等于 nums[0]
                break
            
        # 注意：因为后面的DP参考了前面一层的DP，我们从后向前计算
        for i in range(1, n):
            # 表示容量
            for j in range(target, -1, -1):
                if j >= nums[i]:
                    dp[j] = dp[j] or dp[j -nums[i]]
                else:
                    break
            
        return dp[-1]

```

复杂度分析
时间复杂度:ONO):这里N是数组元素的个数,C是数组元素的和的一半
空间复杂度:O(C):减少了物品那个维度,无论来多少个数,用一行表示状态就够了

##### 解法四  DP优化二：注意到本题的特殊性，提前结束循环

```python
class Solution:
    def canPartition(self, nums: List[int]) -> bool:
        """
            只包含正整数
            分成元素和想等 那么每个集合都是 sum(nums) // 2 （在sum(nums)为偶数 ，奇数没答案
            问题变成哪几个元素和等于 sum(nums) // 2 
            方法一： DFS + 记忆化 + 逆排序数组 超时 X
            方法二：回溯 + 逆排序 超过100%
            方法三：DP-01背包问题 
            dp[i][j] ：表示从数组的 [0, i] 这个子区间内挑选一些正整数，每个数只能用一次，使得这些数的和等于 j。
            
        """
        total = sum(nums)
        if total & 1:
            return False
        n = len(nums)
        target = total // 2
        # dp[i][j] ：表示从数组的 [0, i] 这个子区间内挑选一些正整数，每个数只能用一次，使得这些数的和等于 j。
        dp = [False for _ in range(target + 1)]
        
        # 先写第 1 行：看看第 1 个数是不是能够刚好填满容量为 target
        for j in range(target + 1):
            if nums[0] == j:
                dp[j] = True
                # 如果等于，后面就不用做判断了，因为 j 会越来越大，肯定不等于 nums[0]
                break
            
        # 注意：因为后面的DP参考了前面一层的DP，我们从后向前计算
        for i in range(1, n):
            # 先看最后一个数是不是返回 True，如果是后面就没有必要计算了，方法可以直接返回 True
            # **即在上一轮 [0, i]已经满足了找到target的要求**
            if dp[-1] or dp[target - nums[i]]:
                return True

            # 表示容量
            for j in range(target, -1, -1):
                if j >= nums[i]:
                    dp[j] = dp[j] or dp[j -nums[i]]
                else:
                    break
            
        return dp[-1]

```

### [448. 找到所有数组中消失的数字](https://leetcode-cn.com/problems/find-all-numbers-disappeared-in-an-array/)

#### 题目描述

给定一个范围在  1 ≤ a[i] ≤ n ( n = 数组大小 ) 的 整型数组，数组中的元素一些出现了两次，另一些只出现一次。

找到所有在 [1, n] 范围之间没有出现在数组中的数字。

您能在不使用额外空间且时间复杂度为O(n)的情况下完成这个任务吗? 你可以假定返回的数组不算在额外空间内。

示例:

输入:
[4,3,2,7,8,2,3,1]

输出:
[5,6]


#### 解法

##### 解法一
```python
class Solution:
    def findDisappearedNumbers(self, nums: List[int]) -> List[int]:
        """
            法1.排序 nlogn
            法2.把数组(值 - 1)变成索引, 把对应索引的数变成负数
        """
        res = []
        for i in range(len(nums)):
            if nums[abs(nums[i]) - 1] < 0:
                continue
            nums[abs(nums[i]) - 1] = -nums[abs(nums[i]) - 1] 
        
        for i in range(len(nums)):
            if nums[i] > 0:
                res.append(i + 1)
        return res
                
```

##### 解法二
```python

```

### 453. 最小移动次数使数组元素相等[tag:math]

#### 453.1 题目描述

给定一个长度为 *n* 的**非空**整数数组，找到让数组所有元素相等的最小移动次数。每次移动可以使 *n* - 1 个元素增加 1。

**示例:**

```
输入:
[1,2,3]

输出:
3

解释:
只需要3次移动（注意每次移动会增加两个元素的值）：

[1,2,3]  =>  [2,3,3]  =>  [3,4,3]  =>  [4,4,4]
```

#### 453.2 解法

##### 453.2.1 方法一

```python
class Solution:
    def minMoves(self, nums: List[int]) -> int:
        """
            每次选择最小的n-1个数向上移动 <--> 每次减小最大值
            问题变化成 非最小值与最小值的差值之和
        """
        return sum(nums) - min(nums) * len(nums)
```

### 454.四数相加 II

#### 题目描述

给定四个包含整数的数组列表 A , B , C , D ,计算有多少个元组 (i, j, k, l) ，使得 A[i] + B[j] + C[k] + D[l] = 0。

为了使问题简单化，所有的 A, B, C, D 具有相同的长度 N，且 0 ≤ N ≤ 500 。所有整数的范围在 -228 到 228 - 1 之间，最终结果不会超过 231 - 1 。

例如:

输入:
A = [ 1, 2]
B = [-2,-1]
C = [-1, 2]
D = [ 0, 2]

输出:
2

解释:
两个元组如下:
1. (0, 0, 0, 1) -> A[0] + B[0] + C[0] + D[1] = 1 + (-2) + (-1) + 2 = 0
2. (1, 1, 0, 0) -> A[1] + B[1] + C[0] + D[0] = 2 + (-1) + (-1) + 0 = 0


#### 解法

##### 解法一
```python
def fourSumCount(self, A, B, C, D):
    AB = collections.Counter(a+b for a in A for b in B)
    return sum(AB[-c-d] for c in C for d in D)
```

##### 解法二
```python

```

### 455 分发饼干

#### 455.1 题目描述 

假设你是一位很棒的家长，想要给你的孩子们一些小饼干。但是，每个孩子最多只能给一块饼干。对每个孩子 i ，都有一个胃口值 gi ，这是能让孩子们满足胃口的饼干的最小尺寸；并且每块饼干 j ，都有一个尺寸 sj 。如果 sj >= gi ，我们可以将这个饼干 j 分配给孩子 i ，这个孩子会得到满足。你的目标是尽可能满足越多数量的孩子，并输出这个最大数值。

**注意：**

你可以假设胃口值为正。 一个小朋友最多只能拥有一块饼干。

**示例 1:**

```python
输入: [1,2,3], [1,1]

输出: 1

解释: 
你有三个孩子和两块小饼干，3个孩子的胃口值分别是：1,2,3。
虽然你有两块小饼干，由于他们的尺寸都是1，你只能让胃口值是1的孩子满足。
所以你应该输出1。
输入: [1,2], [1,2,3]

输出: 2

解释: 
你有两个孩子和三块小饼干，2个孩子的胃口值分别是1,2。
你拥有的饼干数量和尺寸都足以让所有孩子满足。
所以你应该输出2.
class Solution:
    def findContentChildren(self, g: List[int], s: List[int]) -> int:
        """
            贪心算法
            给孩子数组和饼干数组逆序排列
            尽可能把最大的饼干分给胃口大的孩子
            
            要求一:自己写排序算法
        """
        g.sort(reverse=True)
        s.sort(reverse=True)
        
        cnt = 0
        index = 0
        for sj in s:
            for j in range(index, len(g)):
                if sj >= g[j]:
                    cnt += 1
                    index = j+1
                    break
        return cnt
    # Thanks! short&beautiful!
    def findContentChildren(self, g, s):
        """
        :type g: List[int]
        :type s: List[int]
        :rtype: int
        """
        g.sort()
        s.sort()
        
        childi = 0
        cookiei = 0       
        while cookiei < len(s) and childi < len(g):
            if s[cookiei] >= g[childi]:
                childi += 1
            cookiei += 1
        return childi
```

正序, 逆序比较皆可以

#### 455.2 解法

**示例 2:**

###461.汉明距离

#### 题目描述

两个整数之间的汉明距离指的是这两个数字对应二进制位不同的位置的数目。

给出两个整数 x 和 y，计算它们之间的汉明距离。

注意：
0 ≤ x, y < 231.

示例:

输入: x = 1, y = 4

输出: 2

解释:
1   (0 0 0 1)
4   (0 1 0 0)
       ↑   ↑

上面的箭头指出了对应二进制位不同的位置。


#### 解法

##### 解法一 位运算
x &= x-1
将二进制数据中的最后位的1转换为0.【可用于计算二进制中1的个数】

```python
class Solution:
    def hammingDistance(self, x: int, y: int) -> int:
        """
            ^ 按位异或后求二进制中1的数量
        """
        c = x ^ y
        cnt = 0
        while c:
            c = c & (c - 1)
            cnt += 1
        return cnt
  
```

##### 解法二
```python

```

### 462. 最少移动次数使数组元素相等 II[tag:math]

#### 462.1 题目描述

给定一个非空整数数组，找到使所有数组元素相等所需的最小移动数，其中每次移动可将选定的一个元素加1或减1。 您可以假设数组的长度最多为10000。

**例如:**

```
输入:
[1,2,3]

输出:
2

说明：
只有两个动作是必要的（记得每一步仅可使其中一个元素加1或减1）： 

[1,2,3]  =>  [2,2,3]  =>  [2,2,2]
```

#### 462.2 解法

这道题的关键是找到中位数

可以用快速排序中partition思想找到medium

##### 462.2.1 方法一 

```python
class Solution:
    def minMoves2(self, nums: List[int]) -> int:
        """
            找中位数 计算每个位和中位数绝对差值之和
            partition O(n)
        """
        
        nums = sorted(nums)
        medium_val = nums[len(nums)>>1]
        return sum([abs(num - medium_val) for num in nums])
```

##### 462.2.2 方法二

```python
def minMoves2(self, nums):
    nums.sort()
    # ~x == -x - 1
    return sum(nums[~i] - nums[i] for i in range(len(nums) / 2))
```

### 494.目标和

#### 题目描述

给定一个非负整数数组，a1, a2, ..., an, 和一个目标数，S。现在你有两个符号 + 和 -。对于数组中的任意一个整数，你都可以从 + 或 -中选择一个符号添加在前面。

返回可以使最终数组和为目标数 S 的所有添加符号的方法数。

示例 1:

输入: nums: [1, 1, 1, 1, 1], S: 3
输出: 5
解释: 

-1+1+1+1+1 = 3
+1-1+1+1+1 = 3
+1+1-1+1+1 = 3
+1+1+1-1+1 = 3
+1+1+1+1-1 = 3

一共有5种方法让最终目标和为3。
注意:

数组的长度不会超过20，并且数组中的值全为正数。
初始的数组的和不会超过1000。
保证返回的最终结果为32位整数。


#### 解法

##### 解法一 DFS + 记忆法
```python
class Solution:
    def findTargetSumWays(self, nums: List[int], S: int) -> int:
        """
            边界条件：-非负整数
            直接回溯超时
            递归 + 记忆化
            DP：
            
            子函数中使用函数的变量
        """
        memo = dict()
        
        def dfs(i, S):
            if i == len(nums):
                return 1 if S == 0 else 0
            if (i, S) in memo:
                return memo[(i, S)]
            ans1 = dfs(i+1, S - nums[i])
            ans2 = dfs(i+1, S + nums[i])
            memo[(i, S)] = ans1 + ans2
            return memo[(i, S)]
        
        if not nums:
            return 0
        return dfs(0, S)
       
```

##### 解法二 DP 01背包问题 todo
```python

```

### 560.和为K的子数组[推荐]

#### 题目描述

给定一个整数数组和一个整数 k，你需要找到该数组中和为 k 的连续的子数组的个数。

示例 1 :

输入:nums = [1,1,1], k = 2
输出: 2 , [1,1] 与 [1,1] 为两种不同的情况。
说明 :

数组的长度为 [1, 20,000]。
数组中元素的范围是 [-1000, 1000] ，且整数 k 的范围是 [-1e7, 1e7]。


#### 解法

##### 解法一 逐元素累加 或 bruce 
```python
class Solution:
    def subarraySum(self, nums: List[int], k: int) -> int:
        """
            Bruce O(n**2)  java ✅
            逐元素累加 超时 java ✅
            边界条件： 
                整数有负数
        """
        
        if not nums:
            return 0
        forward = [0]
        for num in nums:
            forward.append(forward[-1] + num)


        ans = 0
        le = len(nums)
        for i in range(le):
            for j in range(i + 1, le + 1):
                if (forward[j] - forward[i]) == k:
                     ans = ans + 1
        return ans
```

##### 解法二 HashMap 有难度 【常思考】

从解决方案1中，我们知道解决此问题的关键是SUM [i，j]。因此，如果我们知道SUM [0，i  -  1]和SUM [0，j]，那么我们可以很容易地得到SUM [i，j]。为了实现这一点，我们只需要遍历数组，计算当前总和并将所有看到的PreSum的数量保存到HashMap。时间复杂度O（n），空间复杂度O（n）。

---

这种方法背后的想法如下：

1.如果两个索引间累积和相同，即sum [i] = sum [j] 这些指数之间的元素之和为零。（作用是可以累计次数）

2.如果累积总和达到两个指数，比如i和j是k的差值，即如果sum [i] -sum [j] = k，则总和位于指数i和j之间的元素是k。

基于这些想法，我们使用hashmap，用于存储可能的所有索引的累积总和以及相同总和发生的次数。

我们遍历数组nums并继续查找累积总和。每当我们遇到一个新的和时，我们在hashmap中创建一个与该总和相对应的新条目。如果再次出现相同的和（与上一个相同的累加和的索引中间的累加和为0），我们增加与hashmap中的和相对应的计数。

此外，对于遇到的每个和，我们还确定总sum-k已经出现的次数，因为它将确定具有和k的子阵列发生到当前索引的次数。我们将count增加相同的数量。

遍历完整数组后，count将提供所需的结果。

```python
class Solution(object):
    def subarraySum(self, nums, k):
        """
        :type nums: List[int]
        :type k: int
        :rtype: int
        """
        sumv = 0
        # 前缀和: 出现次数
        dic = {0: 1}
        res = 0
        for i in range(len(nums)):
            sumv += nums[i]
            if sumv - k in dic:
                res += dic[sumv - k]
            dic.setdefault(sumv, 0)
            dic[sumv] += 1
        return res
```

### [581. 最短无序连续子数组](https://leetcode-cn.com/problems/shortest-unsorted-continuous-subarray/)

#### 题目描述

给定一个整数数组，你需要寻找一个连续的子数组，如果对这个子数组进行升序排序，那么整个数组都会变为升序排序。

你找到的子数组应是最短的，请输出它的长度。

示例 1:

输入: [2, 6, 4, 8, 10, 9, 15]
输出: 5
解释: 你只需要对 [6, 4, 8, 10, 9] 进行升序排序，那么整个表都会变为升序排序。
说明 :

输入的数组长度范围在 [1, 10,000]。
输入的数组可能包含重复元素 ，所以升序的意思是<=。


#### 解法

##### 解法一 排序 比较 O(nlogn)
```python
    def findUnsortedSubarray(self, nums: List[int]) -> int:
        """
            方法一: 排序 找到不相等的第一个点 和最后一个点 O(NlogN)
            方法二: 存入山谷的区间 找到边界 O(N)

        """
        nums_sorted = sorted(nums)
        start_i = -1
        end_i = -1
        for i in range(len(nums)):
            if nums_sorted[i] != nums[i]:
                if start_i == -1:
                    start_i = i
                else:
                    end_i = i
        return end_i - start_i + 1 if start_i != -1 else 0
```

##### 解法二 使用栈 O(n)

方法 4：使用栈
算法

这个方法背后的想法仍然是选择排序。我们需要找到无序子数组中最小元素和最大元素分别对应的正确位置，来求得我们想要的无序子数组的边界。

为了达到这一目的，此方法中，我们使用 栈栈 。我们从头遍历 numsnums 数组，如果遇到的数字大小一直是升序的，我们就不断把对应的下标压入栈中，这么做的目的是因为这些元素在目前都是处于正确的位置上。一旦我们遇到前面的数比后面的数大，也就是 nums[j]比栈顶元素小，我们可以知道 nums[j]一定不在正确的位置上。

为了找到 nums[j]的正确位置，我们不断将栈顶元素弹出，直到栈顶元素比 nums[j] 小，我们假设栈顶元素对应的下标为 k ，那么我们知道 nums[j]nums[j] 的正确位置下标应该是 k + 1 。

我们重复这一过程并遍历完整个数组，这样我们可以找到最小的 k， 它也是无序子数组的左边界。

类似的，我们逆序遍历一遍 nums 数组来找到无序子数组的右边界。这一次我们将降序的元素压入栈中，如果遇到一个升序的元素，我们像上面所述的方法一样不断将栈顶元素弹出，直到找到一个更大的元素，以此找到无序子数组的右边界。

我们可以看下图作为参考。我们观察到上升还是下降决定了相对顺序，我们还可以观察到指针 bb 在下标 0 后面标记着无序子数组的左边界，指针 aa 在下标 7 前面标记着无序子数组的右边界。

![image.png](/Users/zhengxinzhi/typora_img/LeetCode题目总结/5c6b77b2f1cf11fbd4607ed0b407d25e1fb76eaef1486fd3cd3292ced9829e6e-image.png)

```python
class Solution:
    def findUnsortedSubarray(self, nums: List[int]) -> int:
        stack = []
        min_i = len(nums)
        for i in range(len(nums)):
            while stack and nums[stack[-1]] > nums[i]:
                j = stack.pop()
                min_i = min(min_i, j)
            stack.append(i)
            
        stack = []
        max_i = -1
        for i in range(len(nums) - 1, -1, -1):
            while stack and nums[stack[-1]] < nums[i]:
                j = stack.pop()
                max_i = max(max_i, j)
            stack.append(i)

        return max_i - min_i + 1 if max_i != -1 else 0
```

#### 解法三 不用栈

这个算法背后的思想是无序子数组中最小元素的正确位置可以决定左边界，最大元素的正确位置可以决定右边界。

因此，首先我们需要找到原数组在哪个位置开始不是升序的。我们从头开始遍历数组，一旦遇到降序的元素，我们记录最小元素为 minmin 。

类似的，我们逆序扫描数组 numsnums，当数组出现升序的时候，我们记录最大元素为 maxmax。

然后，我们再次遍历 numsnums 数组并通过与其他元素进行比较，来找到 minmin 和 maxmax 在原数组中的正确位置。我们只需要从头开始找到第一个大于 minmin 的元素，从尾开始找到第一个小于 maxmax 的元素，它们之间就是最短无序子数组。

我们可以再次使用下图作为说明：

![image.png](/Users/zhengxinzhi/typora_img/LeetCode题目总结/5c6b77b2f1cf11fbd4607ed0b407d25e1fb76eaef1486fd3cd3292ced9829e6e-image.png)

```python
class Solution:
    def findUnsortedSubarray(self, nums: List[int]) -> int:
        """
            方法一: 排序 找到不相等的第一个点 和最后一个点 O(NlogN)
            方法二: 存入山谷的区间 栈

        """
        min_v = float('inf')
        max_v = float('-inf')
        flag = False
        
        # 1.找到降序后元素最小值
        for i in range(1, len(nums)):
            if nums[i] < nums[i-1] or flag:
                flag = True
                min_v = min(nums[i], min_v)
                
        # 2.逆序遍历找到升序后元素最大值
        flag = False
        for i in range(len(nums) - 2, -1, -1):
            if nums[i] > nums[i + 1] or flag:
                flag = True
                max_v = max(nums[i], max_v)
        
        left = -1
        right = -1
        # 3.我们只需要从头开始找到第一个大于 min 的元素，从尾开始找到第一个小于 max 的元素，它们之间就是最短无序子数组。
        for i in range(len(nums)):
            if nums[i] > min_v:
                left = i
                break
        for i in range(len(nums) - 1, -1, -1):
            if nums[i] < max_v:
                right = i
                break
        return right - left + 1 if left != -1 else 0
     
```



### 621.任务调度器

#### 题目描述

给定一个用字符数组表示的 CPU 需要执行的任务列表。其中包含使用大写的 A - Z 字母表示的26 种不同种类的任务。任务可以以任意顺序执行，并且每个任务都可以在 1 个单位时间内执行完。CPU 在任何一个单位时间内都可以执行一个任务，或者在待命状态。

然而，两个相同种类的任务之间必须有长度为 n 的冷却时间，因此至少有连续 n 个单位时间内 CPU 在执行不同的任务，或者在待命状态。

你需要计算完成所有任务所需要的最短时间。

示例 1：

输入: tasks = ["A","A","A","B","B","B"], n = 2
输出: 8
执行顺序: A -> B -> (待命) -> A -> B -> (待命) -> A -> B.
注：

任务的总个数为 [1, 10000]。
n 的取值范围为 [0, 100]。


#### 解法

##### 解法一

完成所有任务的最短时间取决于**出现次数最多的任务数量**

因为相同任务必须要有时间片为 `n` 的间隔，所以我们先把出现次数最多的任务 A 安排上

计算出有多少空槽，用其它任务填满

有个注意，如果有n个和次数最多的人物一样多的任务，在最后一个A后面要+n

```python
class Solution(object):
    def leastInterval(self, tasks, n):
        # 1.计算各字母频数并排序
        charmap = [0] * 26
        for c in tasks:
            charmap[ord(c) - ord('A')] += 1
        charmap = sorted(charmap, reverse=True)

        # 2.求空槽数量
        space_nums = (charmap[0] - 1) * n

        # 3.填充空槽
        for num in charmap[1:]:
            space_nums -= min(num, charmap[0] - 1)
        
        # 4.结果 = 空槽数+ task数
        return space_nums + len(tasks) if space_nums > 0 else len(tasks)
```

##### 解法二
```python

```

### 704.二分查找【经典】

#### 题目描述

给定一个 n 个元素有序的（升序）整型数组 nums 和一个目标值 target  ，写一个函数搜索 nums 中的 target，如果目标值存在返回下标，否则返回 -1。


示例 1:

输入: nums = [-1,0,3,5,9,12], target = 9
输出: 4
解释: 9 出现在 nums 中并且下标为 4
示例 2:

输入: nums = [-1,0,3,5,9,12], target = 2
输出: -1
解释: 2 不存在 nums 中因此返回 -1


提示：

你可以假设 nums 中的所有元素是不重复的。
n 将在 [1, 10f000]之间。
nums 的每个元素都将在 [-9999, 9999]之间。


#### 解法

##### 解法一 二分查找模版代码 左闭右开
```python
class Solution(object):
    def search(self, nums, target):
        """
        :type nums: List[int]
        :type target: int
        :rtype: int
        """
        l, r = 0, len(nums)
        while l < r:
            mid = l + ((r - l) >> 1)
            if nums[mid] < target:
                l = mid + 1
            else:
                r = mid
        if l == len(nums) or nums[l] != target:
            return -1
        return l
```

##### 解法二
```python

```

### 739.每日温度

#### 题目描述

根据每日 气温 列表，请重新生成一个列表，对应位置的输入是你需要再等待多久温度才会升高超过该日的天数。如果之后都不会升高，请在该位置用 0 来代替。

例如，给定一个列表 temperatures = [73, 74, 75, 71, 69, 72, 76, 73]，你的输出应该是 [1, 1, 4, 2, 1, 1, 0, 0]。

提示：气温 列表长度的范围是 [1, 30000]。每个气温的值的均为华氏度，都是在 [30, 100] 范围内的整数。


#### 解法

##### 解法一 画图 栈
```python
class Solution:
    def dailyTemperatures(self, T: List[int]) -> List[int]:
        """
            bruce O(n**2)
            画图 升序部分均为1 降序部分索引放入stack中
            
        """
        ans = [0] * len(T)
        stack = []
        for i in range(len(T) - 1):
            if T[i + 1] - T[i] > 0:
                while stack:
                    if T[stack[-1]] < T[i + 1]:
                        index = stack.pop()
                        ans[index] = i + 1 - index
                    else:
                        break
                
                ans[i] = 1
            else:
                stack.append(i)
        return ans
```

##### 解法二
```python

```

###1124.顺次数

#### 题目描述

我们定义「顺次数」为：每一位上的数字都比前一位上的数字大 1 的整数。

请你返回由 [low, high] 范围内所有顺次数组成的 有序 列表（从小到大排序）。 

示例 1：

输出：low = 100, high = 300
输出：[123,234]
示例 2：

输出：low = 1000, high = 13000
输出：[1234,2345,3456,4567,5678,6789,12345]


提示：

10 <= low <= high <= 10^9


#### 解法

##### 解法一
```python
class Solution:
    def sequentialDigits(self, low: int, high: int) -> List[int]:
        if low > high:
            return []
        lLen, rLen = len(str(low)), len(str(high))
        res = []
        for Len in range(lLen, rLen + 1):
            for i in range(1, 11 - Len):
                val = i
                for j in range(i + 1, i + Len):
                    val = val * 10 + j
                if val >= low and val <= high:
                    res.append(val)
        return res
        
```

##### 解法二 先保存在选取
```python
  class Solution2 {
     private var sequentialDigits = [12,23,34,45,56,67,78,89,123,234,345,456,567,678,789,1234,2345,3456,4567,5678,6789,12345,23456,34567,45678,56789,123456,234567,345678,456789,1234567,2345678,3456789,12345678,23456789,123456789]
     func sequentialDigits(_ low: Int, _ high: Int) -> [Int] {
         return sequentialDigits.filter { $0 >= low && $0 <= high}
     }
  }

```

###1275.[找出井字棋的获胜者](https://leetcode-cn.com/problems/find-winner-on-a-tic-tac-toe-game/)

#### 题目描述

A 和 B 在一个 3 x 3 的网格上玩井字棋。

井字棋游戏的规则如下：

玩家轮流将棋子放在空方格 (" ") 上。
第一个玩家 A 总是用 "X" 作为棋子，而第二个玩家 B 总是用 "O" 作为棋子。
"X" 和 "O" 只能放在空方格中，而不能放在已经被占用的方格上。
只要有 3 个相同的（非空）棋子排成一条直线（行、列、对角线）时，游戏结束。
如果所有方块都放满棋子（不为空），游戏也会结束。
游戏结束后，棋子无法再进行任何移动。
给你一个数组 moves，其中每个元素是大小为 2 的另一个数组（元素分别对应网格的行和列），它按照 A 和 B 的行动顺序（先 A 后 B）记录了两人各自的棋子位置。

如果游戏存在获胜者（A 或 B），就返回该游戏的获胜者；如果游戏以平局结束，则返回 "Draw"；如果仍会有行动（游戏未结束），则返回 "Pending"。

你可以假设 moves 都 有效（遵循井字棋规则），网格最初是空的，A 将先行动。

 

示例 1：

输入：moves = [[0,0],[2,0],[1,1],[2,1],[2,2]]
输出："A"
解释："A" 获胜，他总是先走。
"X  "    "X  "    "X  "    "X  "    "X  "
"   " -> "   " -> " X " -> " X " -> " X "
"   "    "O  "    "O  "    "OO "    "OOX"
示例 2：

输入：moves = [[0,0],[1,1],[0,1],[0,2],[1,0],[2,0]]
输出："B"
解释："B" 获胜。
"X  "    "X  "    "XX "    "XXO"    "XXO"    "XXO"
"   " -> " O " -> " O " -> " O " -> "XO " -> "XO " 
"   "    "   "    "   "    "   "    "   "    "O  "
示例 3：

输入：moves = [[0,0],[1,1],[2,0],[1,0],[1,2],[2,1],[0,1],[0,2],[2,2]]
输出："Draw"
输出：由于没有办法再行动，游戏以平局结束。
"XXO"
"OOX"
"XOX"
示例 4：

输入：moves = [[0,0],[1,1]]
输出："Pending"
解释：游戏还没有结束。
"X  "
" O "
"   "


提示：

1 <= moves.length <= 9
moves[i].length == 2
0 <= moves[i][j] <= 2
moves 里没有重复的元素。
moves 遵循井字棋的规则。


#### 解法

##### 解法一 暴力
```python
class Solution:
    def tictactoe(self, moves: List[List[int]]) -> str:
        """
        
        """
        def dfs(board):
            for i in range(3):
                if sum(board[i]) == 3:
                    return True
                if sum([board[j][i] for j in range(3)]) == 3:
                    return True
            
            if sum([board[i][i] for i in range(3)]) == 3:
                return True
            if sum([board[i][2-i] for i in range(3)]) == 3:
                return True
            return False        
  
        # 1.初始化方格
        As = [[0] * 3 for _ in range(3)]
        for x, y in moves[:: 2]:
            As[x][y] = 1
        if dfs(As):
            return 'A'
                
        Bs = [[0] * 3 for _ in range(3)]   
        for x, y in moves[1::2]:
            Bs[x][y] = 1
        if dfs(Bs):
            return 'B'
                
        if len(moves) == 9:
            return "Draw"
        else:
            return "Pending"

```

##### 解法二
```python

```

###1277. 统计全为 1 的正方形子矩阵

#### 题目描述

给你一个 `m * n` 的矩阵，矩阵中的元素不是 `0` 就是 `1`，请你统计并返回其中完全由 `1` 组成的 **正方形** 子矩阵的个数。

 

**示例 1：**

```
输入：matrix =
[
  [0,1,1,1],
  [1,1,1,1],
  [0,1,1,1]
]
输出：15
解释： 
边长为 1 的正方形有 10 个。
边长为 2 的正方形有 4 个。
边长为 3 的正方形有 1 个。
正方形的总数 = 10 + 4 + 1 = 15.
```

**示例 2：**

```
输入：matrix = 
[
  [1,0,1],
  [1,1,0],
  [1,1,0]
]
输出：7
解释：
边长为 1 的正方形有 6 个。 
边长为 2 的正方形有 1 个。
正方形的总数 = 6 + 1 = 7.
```

 

**提示：**

- `1 <= arr.length <= 300`
- `1 <= arr[0].length <= 300`
- `0 <= arr[i][j] <= 1`


#### 解法

##### 解法一 dp
```python
class Solution:
    def countSquares(self, matrix: List[List[int]]) -> int:
        m = len(matrix)
        if m == 0:
            return 0
        n = len(matrix[0])
        if n == 0:
            return 0
        dp = [[0] * (n + 1) for _ in range(m + 1)]
        res = 0
        for i in range(1, m + 1):
            for j in range(1, n + 1):
                if matrix[i-1][j-1] == 1:
                    dp[i][j] = min(dp[i-1][j], dp[i][j-1], dp[i-1][j-1]) + 1
                    res += dp[i][j]

        return res
```

##### 解法二
```python

```

###1282.用户分组

#### 题目描述

有 n 位用户参加活动，他们的 ID 从 0 到 n - 1，每位用户都 恰好 属于某一用户组。给你一个长度为 n 的数组 groupSizes，其中包含每位用户所处的用户组的大小，请你返回用户分组情况（存在的用户组以及每个组中用户的 ID）。

你可以任何顺序返回解决方案，ID 的顺序也不受限制。此外，题目给出的数据保证至少存在一种解决方案。

 

示例 1：

输入：groupSizes = [3,3,3,3,3,1,3]
输出：[[5],[0,1,2],[3,4,6]]
解释： 
其他可能的解决方案有 [[2,1,6],[5],[0,4,3]] 和 [[5],[0,6,2],[4,3,1]]。
示例 2：

输入：groupSizes = [2,1,3,3,3,2]
输出：[[1],[0,5],[2,3,4]]


提示：

groupSizes.length == n
1 <= n <= 500
1 <= groupSizes[i] <= n




#### 解法

##### 解法一
```python
class Solution:
    def groupThePeople(self, groupSizes: List[int]) -> List[List[int]]:
        res = []
        dic = collections.defaultdict(list)
        for i, g in enumerate(groupSizes):
            dic[g].append(i)
        for key, values in dic.items():
            vLen = len(values)
            if key == vLen:
                res.append(values)
            else:
                res.extend([values[i: i+key] for i in range(0, vLen, key)])
        return res
```

##### 解法二
```python
class Solution:
    def groupThePeople(self, groupSizes: List[int]) -> List[List[int]]:
        groupdict={}
        ans=[]
        for i,gs in enumerate(groupSizes):
            if gs==1:
                ans.append([i])
            else:
                if gs in groupdict:
                    groupdict[gs].append(i)
                    if len(groupdict[gs])==gs:
                        ans.append(groupdict[gs])
                        del groupdict[gs]   
                else:
                    groupdict[gs]=[i]
        return ans
```

###1283.[使结果不超过阈值的最小除数](https://leetcode-cn.com/problems/find-the-smallest-divisor-given-a-threshold/)

#### 题目描述

给你一个整数数组 nums 和一个正整数 threshold  ，你需要选择一个正整数作为除数，然后将数组里每个数都除以它，并对除法结果求和。

请你找出能够使上述结果小于等于阈值 threshold 的除数中 最小 的那个。

每个数除以除数后都向上取整，比方说 7/3 = 3 ， 10/2 = 5 。

题目保证一定有解。

 

示例 1：

输入：nums = [1,2,5,9], threshold = 6
输出：5
解释：如果除数为 1 ，我们可以得到和为 17 （1+2+5+9）。
如果除数为 4 ，我们可以得到和为 7 (1+1+2+3) 。如果除数为 5 ，和为 5 (1+1+1+2)。
示例 2：

输入：nums = [2,3,5,7,11], threshold = 11
输出：3
示例 3：

输入：nums = [19], threshold = 5
输出：4


提示：

1 <= nums.length <= 5 * 10^4
1 <= nums[i] <= 10^6
nums.length <= threshold <= 10^6


#### 解法

##### 解法一 二分答案法
```python
class Solution:
    def smallestDivisor(self, nums: List[int], threshold: int) -> int:
        def check(divider):
            res = 0
            for num in nums:
                res += math.ceil(num / divider)
            return res <= threshold

        l, r = 1, max(nums)

        while l < r:
            mid = l + (r - l) // 2
            res = check(mid)
            print(mid, res)
            if res:
                r = mid
            else:
                l = mid + 1

        return l
```

##### 解法二
```python

```

###1285.[元素和小于等于阈值的正方形的最大边长](https://leetcode-cn.com/problems/maximum-side-length-of-a-square-with-sum-less-than-or-equal-to-threshold/)

#### 题目描述

给你一个大小为 m x n 的矩阵 mat 和一个整数阈值 threshold。

请你返回元素总和小于或等于阈值的正方形区域的最大边长；如果没有这样的正方形区域，则返回 0 。


示例 1：



输入：mat = [[1,1,3,2,4,3,2],[1,1,3,2,4,3,2],[1,1,3,2,4,3,2]], threshold = 4
输出：2
解释：总和小于 4 的正方形的最大边长为 2，如图所示。
示例 2：

输入：mat = [[2,2,2,2,2],[2,2,2,2,2],[2,2,2,2,2],[2,2,2,2,2],[2,2,2,2,2]], threshold = 1
输出：0
示例 3：

输入：mat = [[1,1,1,1],[1,0,0,0],[1,0,0,0],[1,0,0,0]], threshold = 6
输出：3
示例 4：

输入：mat = [[18,70],[61,1],[25,85],[14,40],[11,96],[97,96],[63,45]], threshold = 40184
输出：2


提示：

1 <= m, n <= 300
m == mat.length
n == mat[i].length
0 <= mat[i][j] <= 10000
0 <= threshold <= 10^5


#### 解法

##### 解法一 行前缀和 + 遍历

为了统计正方形矩阵的面积，可以假定当前位置ij为正方形的右下角点，这个正方形可能的最大边长为maxlen = Math.min(i, j) + 1。

沿着同一列j向上遍历，遍历行数即为当前正方形的边长len，利用前缀和可以求得当前正方形的每一行的面积rowarea。

```java
class Solution {
    public int maxSideLength(int[][] mat, int threshold) {
        int result = 0;
        for(int i = 0; i < mat.length; i++){
            for (int j = 0; j < mat[0].length; j++){
                if (j != 0){
                    mat[i][j] += mat[i][j - 1];
                }
                int len = 0;
                int maxLen = Math.min(i, j) + 1;
                while (len < maxLen){
                    int area = 0;
                    for (int k = 0; k < len + 1; k++) {
                        int prefix = j-len-1 < 0 ? 0 : mat[i-k][j-len-1];
                        area += mat[i-k][j] - prefix;
                    }
                    if (area > threshold){
                        break;
                    }
                    len++;
                }
                result = len > result ? len : result;
            }
        }
        return result;
    }
}
```

##### 解法二
```python

```

## Hash



### 39.有效的数独

#### 题目描述

判断一个 9x9 的数独是否有效。只需要根据以下规则，验证已经填入的数字是否有效即可。

数字 1-9 在每一行只能出现一次。
数字 1-9 在每一列只能出现一次。
数字 1-9 在每一个以粗实线分隔的 3x3 宫内只能出现一次。


上图是一个部分填充的有效的数独。

数独部分空格内已填入了数字，空白格用 '.' 表示。

示例 1:

输入:
[
  ["5","3",".",".","7",".",".",".","."],
  ["6",".",".","1","9","5",".",".","."],
  [".","9","8",".",".",".",".","6","."],
  ["8",".",".",".","6",".",".",".","3"],
  ["4",".",".","8",".","3",".",".","1"],
  ["7",".",".",".","2",".",".",".","6"],
  [".","6",".",".",".",".","2","8","."],
  [".",".",".","4","1","9",".",".","5"],
  [".",".",".",".","8",".",".","7","9"]
]
输出: true
示例 2:

输入:
[
  ["8","3",".",".","7",".",".",".","."],
  ["6",".",".","1","9","5",".",".","."],
  [".","9","8",".",".",".",".","6","."],
  ["8",".",".",".","6",".",".",".","3"],
  ["4",".",".","8",".","3",".",".","1"],
  ["7",".",".",".","2",".",".",".","6"],
  [".","6",".",".",".",".","2","8","."],
  [".",".",".","4","1","9",".",".","5"],
  [".",".",".",".","8",".",".","7","9"]
]
输出: false
解释: 除了第一行的第一个数字从 5 改为 8 以外，空格内其他数字均与 示例1 相同。
     但由于位于左上角的 3x3 宫内有两个 8 存在, 因此这个数独是无效的。
说明:

一个有效的数独（部分已被填充）不一定是可解的。
只需要根据以上规则，验证已经填入的数字是否有效即可。
给定数独序列只包含数字 1-9 和字符 '.' 。
给定数独永远是 9x9 形式的。


#### 解法

##### 解法一

一个简单的解决方案是遍历该 9 x 9 数独 三 次，以确保：

行中没有重复的数字。
列中没有重复的数字。
3 x 3 子数独内没有重复的数字。

```python
class Solution:
    def isValidSudoku(self, board: List[List[str]]) -> bool:
        # 1.校验所有行
        for i in range(9):
            aset = set()
            for num in board[i]:
                if num == '.':
                    continue
                if num in aset:
                    return False
                else:
                    aset.add(num)
                

        # 2.校验所有列
        for j in range(9):
            aset = set()
            for i in range(9):
                if board[i][j] == '.':
                    continue
                if board[i][j] in aset:
                    return False
                else:
                    aset.add(board[i][j])
        
        # 校验9宫格
        for i_add in range(3):
            for j_add in range(3):
                aset = set()
                for i in range(3):
                    for j in range(3):
                        num = board[3 * i_add + i][3 * j_add + j]
                        if num == '.':
                            continue
                        if num in aset:
                            print(aset)
                            return False
                        else:
                            aset.add(num)

        return True

```

##### 解法二

实际上，所有这一切都可以在一次迭代中完成。

```python
class Solution:
    def isValidSudoku(self, board):
        """
        :type board: List[List[str]]
        :rtype: bool
        """
        # init data
        rows = [{} for i in range(9)]
        columns = [{} for i in range(9)]
        boxes = [{} for i in range(9)]

        # validate a board
        for i in range(9):
            for j in range(9):
                num = board[i][j]
                if num != '.':
                    num = int(num)
                    box_index = (i // 3 ) * 3 + j // 3
                    
                    # keep the current cell value
                    rows[i][num] = rows[i].get(num, 0) + 1
                    columns[j][num] = columns[j].get(num, 0) + 1
                    boxes[box_index][num] = boxes[box_index].get(num, 0) + 1
                    
                    # check if this value has been already seen before
                    if rows[i][num] > 1 or columns[j][num] > 1 or boxes[box_index][num] > 1:
                        return False         
        return True
```

### 49.字母异位词分组

#### 1.题目描述

给定一个字符串数组，将字母异位词组合在一起。字母异位词指字母相同，但排列不同的字符串。

**示例:**

```
输入: ["eat", "tea", "tan", "ate", "nat", "bat"],
输出:
[
  ["ate","eat","tea"],
  ["nat","tan"],
  ["bat"]
]
```

**说明：**

- 所有输入均为小写字母。
- 不考虑答案输出的顺序。

#### 2.解法

##### 2.1 方法一 Hash sort

思路：利用HashMap 键为排序后相等的异位词 值为List 里面装的是所有排序后相等的异或词集合

Time Complexity: O(NK log K）

Space Complexity: O(NK)

```python
class Solution(object):
    def groupAnagrams(self, strs):
        """
        :type strs: List[str]
        :rtype: List[List[str]]
        """
        dic = collections.defaultdict(list)
        for s in strs:
            ss = "".join(sorted(list(s)))
            dic[ss].append(s)
        return list(dic.values())
                
```



##### 2.2 方法二 Categorize by Count

统计bagOfWord的词频数作为key

Time Complexity: O(NK)

Space Complexity: O(NK)

![Anagrams](/Users/zhengxinzhi/typora_img/LeetCode%E9%A2%98%E7%9B%AE%E6%80%BB%E7%BB%93/49_groupanagrams2.png)

```python
class Solution(object):
    def groupAnagrams(self, strs):
        """
        :type strs: List[str]
        :rtype: List[List[str]]
        """
        dic = collections.defaultdict(list)
        for s in strs:
            cnts = [0] * 26
            for c in s:
                cnts[ord(c) - ord('a')] += 1
            dic[tuple(cnts)].append(s)
        return list(dic.values())


```



### 290.单词模式

#### 290.1.题目描述

给定一种 `pattern(模式)` 和一个字符串 `str` ，判断 `str` 是否遵循相同的模式。

这里的**遵循**指完全匹配，例如， `pattern` 里的每个字母和字符串 `str` 中的每个非空单词之间存在着双向连接的对应模式。

**示例1:**

```
输入: pattern = "abba", str = "dog cat cat dog"
输出: true
```

**示例 2:**

```
输入:pattern = "abba", str = "dog cat cat fish"
输出: false
```

**示例 3:**

```
输入: pattern = "aaaa", str = "dog cat cat dog"
输出: false
```

**示例 4:**

```
输入: pattern = "abba", str = "dog dog dog dog"
输出: false
```

**说明:**
你可以假设 `pattern` 只包含小写字母， `str` 包含了由单个空格分隔的小写字母。    

#### 290.2.解法
##### 290.2.1 方法一

```java
import java.util.HashMap;
class Solution {
    public boolean wordPattern(String pattern, String str) {
        String[] strArray = str.split(" ");
        if(strArray.length != pattern.length()){
            return false;
        }
        HashMap<String, Character> map = new HashMap<>();
        HashMap<Character, String> cmap = new HashMap<>();
        for(int i = 0; i < pattern.length(); i++){
            if(map.containsKey(strArray[i])){
                if(pattern.charAt(i) != map.get(strArray[i])){
                    return false;
                }
            }else{
                map.put(strArray[i], pattern.charAt(i));
            }
            if(cmap.containsKey(pattern.charAt(i))){
                if(!strArray[i].equals(cmap.get(pattern.charAt(i)))){
                    return false;
                }
            }else{
                cmap.put(pattern.charAt(i), strArray[i]);
            }
            
        }
        
        return true;
    }
}
```

```java
public boolean wordPattern(String pattern, String str) {
    String[] words = str.split(" ");
    if (words.length != pattern.length())
        return false;
    Map index = new HashMap();
    for (Integer i=0; i<words.length; ++i)
        // the previous value associated with `key`, or `null` if there was no mapping for `key`. (A `null` return can also indicate that the map previously associated `null` with `key`.)
		if (!Objects.equals(index.put(pattern.charAt(i), i),
                            index.put(words[i], i)))
            return false;
    return true;
}
```

```java
public class Solution {
    public boolean wordPattern(String pattern, String str) {
        String[] arr= str.split(" ");
        HashMap<Character, String> map = new HashMap<Character, String>();
        if(arr.length!= pattern.length())
            return false;
        for(int i=0; i<arr.length; i++){
            char c = pattern.charAt(i);
            if(map.containsKey(c)){
                if(!map.get(c).equals(arr[i]))
                    return false;
            }else{
                if(map.containsValue(arr[i]))
                    return false;
                map.put(c, arr[i]);
            }    
        }
        return true;
    }
}
```



##### 290.2.2 方法二

![image-20190507153942703](/Users/zhengxinzhi/typora_img/LeetCode题目总结/image-20190507153942703.png)

![image-20190507154356567](/Users/zhengxinzhi/typora_img/LeetCode题目总结/image-20190507154356567.png)



### 187.重复的DNA序列

#### 187.1.题目描述

所有 DNA 由一系列缩写为 A，C，G 和 T 的核苷酸组成，例如：“ACGAATTCCG”。在研究 DNA 时，识别 DNA 中的重复序列有时会对研究非常有帮助。

编写一个函数来查找 DNA 分子中所有出现超多一次的10个字母长的序列（子串）。

**示例:**

```
输入: s = "AAAAACCCCCAAAAACCCCCCAAAAAGGGTTT"

输出: ["AAAAACCCCC", "CCCCCAAAAA"]
```

#### 187.2.解法
##### 187.2.1 方法一

```python
class Solution(object):
    def findRepeatedDnaSequences(self, s):
        """s
        :type s: str
        :rtype: List[str]
        """
        sset = set()
        res = set()
        for i in range(len(s) - 9):
            if s[i: i + 10] in sset:
                res.add(s[i: i + 10])
            else:
                sset.add(s[i: i + 10])
        
        return list(res)
```



##### 187.2.2 方法二 位运算

```java
class Solution {
    public List<String> findRepeatedDnaSequences(String s) {
        Set<Integer> words = new HashSet<>();
        Set<Integer> doubleWords = new HashSet<>();
        List<String> ans = new ArrayList<>();
        char[] map = new char[26];
        map['A' - 'A'] = 0;
        map['C' - 'A'] = 1;
        map['G' - 'A'] = 2;
        map['T' - 'A'] = 3;
        
        for(int i = 0; i < s.length() - 9; i++){
            int v = 0;
            for(int j = i; j < i + 10; j++){
                // --- 每个字符占两位 --
                v <<= 2;
                v |= map[s.charAt(j) - 'A'];
                // --- v即是最后我们要的字符 --
            }
            if(!words.add(v) && doubleWords.add(v)){
                ans.add(s.substring(i, i+10));
            }
        }
        return ans;
    }
}
```



## 回溯

### 17. 电话号码的字母组合

#### 17.1.题目描述

给定一个仅包含数字 `2-9` 的字符串，返回所有它能表示的字母组合。

给出数字到字母的映射如下（与电话按键相同）。注意 1 不对应任何字母。

![img](/Users/zhengxinzhi/typora_img/LeetCode题目总结/200px-Telephone-keypad2.svg.png)

**示例:**

```
输入："23"
输出：["ad", "ae", "af", "bd", "be", "bf", "cd", "ce", "cf"].
```

**说明:**
尽管上面的答案是按字典序排列的，但是你可以任意选择答案输出的顺序。

#### 17.2.解法

递归法

##### 17.2.1 方法一 回溯

```python
class Solution:
    def letterCombinations(self, digits: str) -> List[str]:
        "7:15"
        def generate(i, item, result, nums):
            """
                (x - a)//3 + 2
            """
            if i == len(nums):
                result.append(''.join(item))
                return
            for v in dic[nums[i]]:
                item.append(v)
                generate(i+1, item, result, nums)
                item.pop()
               
        dic =  {'2': ['a', 'b', 'c'],
                 '3': ['d', 'e', 'f'],
                 '4': ['g', 'h', 'i'],
                 '5': ['j', 'k', 'l'],
                 '6': ['m', 'n', 'o'],
                 '7': ['p', 'q', 'r', 's'],
                 '8': ['t', 'u', 'v'],
                 '9': ['w', 'x', 'y', 'z']}
            
        nums = list(digits)
        item = []
        result = []
        if nums:
        		generate(0, item, result, nums)
        return result
```

##### 17.2.2 方法二 回溯

```python
class Solution:
    def letterCombinations(self, digits):
        """
        :type digits: str
        :rtype: List[str]
        """
        phone = {'2': ['a', 'b', 'c'],
                 '3': ['d', 'e', 'f'],
                 '4': ['g', 'h', 'i'],
                 '5': ['j', 'k', 'l'],
                 '6': ['m', 'n', 'o'],
                 '7': ['p', 'q', 'r', 's'],
                 '8': ['t', 'u', 'v'],
                 '9': ['w', 'x', 'y', 'z']}
                
        def backtrack(combination, next_digits):
            # if there is no more digits to check
            if len(next_digits) == 0:
                # the combination is done
                output.append(combination)
            # if there are still digits to check
            else:
                # iterate over all letters which map 
                # the next available digit
                for letter in phone[next_digits[0]]:
                    # append the current letter to the combination
                    # and proceed to the next digits
                    backtrack(combination + letter, next_digits[1:])
                    
        output = []
        if digits:
            backtrack("", digits)
        return output
```

**Complexity Analysis**

- Time complexity : O(3N×4M)where `N` is the number of digits in the input that maps to 3 letters (*e.g.* `2, 3, 4, 5, 6, 8`) and `M` is the number of digits in the input that maps to 4 letters (*e.g.* `7, 9`), and `N+M` is the total number digits in the input.
- Space complexity : O(3N×4M) since one has to keep 3N×4M3*N*×4*M* solutions.

### 78 子集

#### 78.1 描述

给定一组不含重复元素的整数数组 nums，返回该数组所有可能的子集（幂集）。

说明：解集不能包含重复的子集。

示例:

输入: nums = [1,2,3]
输出:
[
  [3],
  [1],
  [2],
  [1,2,3],
  [1,3],
  [2,3],
  [1,2],
  []
]

#### 78.2 思路

首先 ，这个题是NP问题，没有多项式时间内的算法，只能用搜索解决的问题

选择用DFS-backtracking 的递归方式解决


##### 78.2.1 递归法

```python
import copy
class Solution:
    def subsets(self, nums: List[int]) -> List[List[int]]:        
        result = []
        if len(nums) == 0:            
            return [[]]
        # 待判断是否放入集合的元素
        sub_result = self.subsets(nums[1:])
        result.extend(copy.deepcopy(sub_result))     
        for item in sub_result:
            item.append(nums[0])
            result.append(item)
        return result

```

##### 78.2.2 回溯(暴力搜索法O(2^n))(特殊方法 不清楚的很难自己想出)

![img](https://upload-images.jianshu.io/upload_images/13612449-0aa5ad2614b65ba6.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/654/format/webp)

回溯法:
利用回溯法生成子集, 对于每个元素, 都有试探放入或不放入集合中的两个选择:

- 放入该元素, 递归的进行后续元素的选择, 完成放入该元素的后续所有元素的试探
- 不放入该元素, 递归的进行后续元素的选择, 完成不放入该元素的所有元素的试探

**例如 nums=[1, 2, 3, 4, 5], 
若放入1,  subset =[1], 继续递归处理[2, 3, 4, 5]; 若继续放入2, item=[1, 2]
若不放入1, subset = [], 继续递归处理[2, 3, 4, 5]**

 	本解法采用回溯算法实现，回溯算法的基本形式是“**递归+循环**”，正因为循环中嵌套着递归，递归中包含循环，这才使得回溯比一般的递归和单纯的循环更难理解，其实我们熟悉了它的基本形式，就会觉得这样的算法难度也不是很大。原数组中的每个元素有两种状态：存在和不存在。
**① 外层循环逐一往中间集合 temp 中加入元素 nums[i]，使这个元素处于存在状态
② 开始递归，递归中携带加入新元素的 temp，并且下一次循环的起始是 i 元素的下一个，因而递归中更新 i 值为 i + 1
③ 将这个从中间集合 temp 中移除，使该元素处于不存在状态**

![image-20190226224406196](/Users/zhengxinzhi/typora_img/LeetCode题目总结/image-20190226224406196.png)

```python
** 清晰 ** 推荐 **

        def generate(i, nums, item, result):
            if i == len(nums):
                return
            item.append(nums[i])
            result.append(copy.deepcopy(item))
            generate(i+1, nums, item, result)
            item.pop()
            generate(i+1, nums, item, result)
        
        item = []
        result = []
        result.append(item)
        generate(0, nums, item, result)
        return result
```



```python
class Solution:
    def subsets(self, nums):
        # 是上面解题形式的变种 不好理解 可以记作一种套路 迭代+递归
        def generate(i, nums, item, result):
            result.append(list(item))
            for j in range(i, len(nums)):
                item.append(nums[j])
                generate(j+1, nums, item, result)
                item.pop()

        result = []
        generate(0, nums, [], result)
        return result
```

##### 78.2.3 位运算法(特殊方法 不清楚的很难自己想出)

> & | ^ ~ << >>

本题使用到 & and <<

![image-20190226232758000](/Users/zhengxinzhi/typora_img/LeetCode题目总结/image-20190226232758000.png)

![image-20190226233159527](/Users/zhengxinzhi/typora_img/LeetCode题目总结/image-20190226233159527.png)

```c++
public:
    std::vector<std::vector<int>> subsets(std::vector<int>& nums){
        // 设置全部集合的最大值 + 1 即(2 ^ nums.size()+1
		int all_set = 1 << nums.size()
        // 整数i代表从0至2^n-1这所有的集合
        // (1<<j)即为构造nums数组的第j个元素
        // 若i & (1<<j) 为真则nums[j]放入item
        for(int i = 0; i < all_set; i++){
            std::vector<int> item;
            // j是 ABC是否出现 出现则将该元素push入集合
            for int j = 0; j < nums.size(); j++){
                // 若i 和 100 010 001 位与(&)运算后为1 push入item
                if(i & (1 << j)){
                    item.push_back(nums[j])
                }
            }
            // 根据上一个for loop得知这一个集合是什么 放入result中
            result.push_back(item)
        }
    }
```



#### 78.3 问题 数组浅拷贝深拷贝问题

> result.extend(copy.deepcopy(sub_result))

1、copy.copy 浅拷贝 只拷贝父对象，不会拷贝对象的内部的子对象。

2、copy.deepcopy 深拷贝 拷贝对象及其子对象

更推荐的写法

result.extend(list(sub_result))

### 90 子集II

#### 90.1 题目描述

给定一个可能包含重复元素的整数数组 **\*nums***，返回该数组所有可能的子集（幂集）。

**说明：**解集不能包含重复的子集。

**示例:**

```
输入: [1,2,2]
输出:
[
  [2],
  [1],
  [1,2,2],
  [2,2],
  [1,2],
  []
]

```

#### 90.2 解法

##### 90.2.1 方法一 回溯

```python

class Solution:
    def subsetsWithDup(self, nums: List[int]) -> List[List[int]]:
        """
            和子集I区别在于有重复
            重复分两种 一种是集合相同但位置不同 可通过排序解决
                      一种是集合相同位置也相同 可通过结果用set保存
            set 直接存list 返回 type error: unhashable type: list
            
        """
        def generate(i, nums, item, result):
            result.append(list(item))
            for j in range(i, len(nums)):
                item.append(nums[j])
                generate(j+1, nums, item, result)
                item.pop()
        # 做排序 防止第一种重复
        nums = sorted(nums)
        result = []
        item = []
        generate(0, nums, item, result)
        ## 如何对列表中的列表去重 或者程序中如何做判断防止相同的列表进入
        result = [list(j) for j in set([tuple(item) for item in result])]
        return result
        
```

##### 90.2.2 方法二 递归

```python
class Solution:
    def subsetsWithDup(self, nums: List[int]) -> List[List[int]]:
        if not nums:
            return [[]]
        nums = sorted(nums)
        num = nums[0]
        subsets = self.subsetsWithDup(nums[1:])
        subsets = subsets + [i+[num] for i in subsets]
        subsets = [list(j) for j in set([tuple(item) for item in subsets])]
        return subsets
```

#### 90.4 问题 unhash able type list

##### 90.4.1 unhash able type list

set中存有的是hash化的对象, 然而list等值可以改变的对象难以hash, 所以不能直接放入

##### 90.4.2 对列表中的列表(二级列表)去重

result = [list(j) for j in set([tuple(item) for item in result])]

### 39.组合总和

#### 29.1.题目描述

给定一个**无重复元素**的数组 `candidates` 和一个目标数 `target` ，找出 `candidates` 中所有可以使数字和为 `target` 的组合。

`candidates` 中的数字可以无限制重复被选取。

**说明：**

- 所有数字（包括 `target`）都是正整数。
- 解集不能包含重复的组合。 

**示例 1:**

```
输入: candidates = [2,3,6,7], target = 7,
所求解集为:
[
  [7],
  [2,2,3]
]
```

**示例 2:**

```
输入: candidates = [2,3,5], target = 8,
所求解集为:
[
  [2,2,2,2],
  [2,3,3],
  [3,5]
]
```

#### 29.2.解法

##### 29.2.1 方法一 回溯 **

```python
class Solution:
    def combinationSum(self, candidates, target):

        def generate(i, item, result, candidates, target):
            if target < 0:
                return
            elif target == 0:
                result.append(list(item))
            else:
                for j in range(i, len(candidates)):
                    item.append(candidates[j])
                    # not i + 1 because we can reuse same elements
                    generate(j, item, result, candidates, target - candidates[j])
                    item.pop()

        item = []
        result = []
        candidates = sorted(candidates)
        generate(0, item, result, candidates, target)
        return result

sol = Solution()
print(sol.combinationSum([2,3,5], 8))
```

##### 

### 40 组合总和II

#### 40.1 题目描述

给定一个数组 `candidates` 和一个目标数 `target` ，找出 `candidates` 中所有可以使数字和为 `target` 的组合。

`candidates` 中的每个数字在每个组合中只能使用一次。

**说明：**

- 所有数字（包括目标数）都是正整数。
- 解集不能包含重复的组合。 

**示例 1:**

```
输入: candidates = [10,1,2,7,6,1,5], target = 8,
所求解集为:
[
  [1, 7],
  [1, 2, 5],
  [2, 6],
  [1, 1, 6]
]
```

**示例 2:**

```
输入: candidates = [2,5,2,1,2], target = 5,
所求解集为:
[
  [1,2,2],
  [5]
]
```

#### 40.2 解法

```python
import copy
class Solution:
    def combinationSum2(self, candidates: List[int], target: int) -> List[List[int]]:
        """
            边界条件: candidates中可能含有重复值
            前置题目: 78 子集 90 子集II
            本题创新点: 如何剪枝
            思考: 当子集累计值大于 >= target时 剪枝(因为所有数字都是正整数)
            防止不能包含重复的组合:
                两种重复:
                1.集合中元素位置不同 值相同(先排序)
                2.集合中元素相同(二重列表去重 或防止重复集合进入结果集)
            最后计算剩余集合中元素和等于target的子集
            Note: deepcopy
        """
        def back_track(i, item, candidates, set_sum, result):
            """
            set_sum: 待传入的子集的和
            	与subsets题目相比 增加个剪枝 如何剪枝 增加set_sum变量来判断
            """
            # 跳出条件
            if i >= len(candidates) or set_sum > target:
                return
            # *** 剪枝 ***
            set_sum = candidates[i] + set_sum
            item.append(candidates[i])
            
            if set_sum == target: 
                result.append(copy.deepcopy(item))
            back_track(i+1, item, candidates, set_sum, result)
            # ** back_track **  这个地方容易回溯时容易忘记减掉不加入的元素值
            set_sum = set_sum - candidates[i]
            item.pop()
            back_track(i+1, item, candidates, set_sum, result)
            
        result = []
        # 排序防止第一种重复
        candidates.sort()
        # 子集
        item = []
        back_track(0, item, candidates, 0, result)
        
        final_result = []
        # 对result去重且保留等于target的子集
        final_result = list(set([tuple(f) for f in result]))
        final_result = [list(f) for f in final_result]
        return final_result
        
        
        
```

```python
def combinationSum2(self, candidates, target):
    # Sorting is really helpful, se we can avoid over counting easily
    candidates.sort()                      
    result = []
    self.combine_sum_2(candidates, 0, [], result, target)
    return result
    
def combine_sum_2(self, nums, start, path, result, target):
    # Base case: if the sum of the path satisfies the target, we will consider 
    # it as a solution, and stop there
    if not target:
        result.append(path)
        return
    
    for i in xrange(start, len(nums)):
        # Very important here! We don't use `i > 0` because we always want 
        # to count the first element in this recursive step even if it is the same 
        # as one before. To avoid overcounting, we just ignore the duplicates
        # after the first element.
        if i > start and nums[i] == nums[i - 1]:
            continue

        # If the current element is bigger than the assigned target, there is 
        # no need to keep searching, since all the numbers are positive
        if nums[i] > target:
            break

        # We change the start to `i + 1` because one element only could
        # be used once
        self.combine_sum_2(nums, i + 1, path + [nums[i]], 
                           result, target - nums[i])
```

##### 42.2.2 回溯 （推荐）

```python
class Solution:
    def combinationSum2(self, candidates: List[int], target: int) -> List[List[int]]:
        def generate(start, item, result, nums, target):
            if target < 0:
                return 
            if target == 0:
                result.append(list(item))
                return
            
            for j in range(start, len(nums)):
                if j > start and nums[j] == nums[j-1]:
                    continue
                item.append(nums[j])
                generate(j+1, item, result, nums, target - nums[j])
                item.pop()
            
            
        nums = sorted(candidates)
        item = []
        result = []
        generate(0, item, result, nums, target)
        return result
```

### 216. 组合总和 III

#### 216.1.题目描述

找出所有相加之和为 ***n*** 的 **k** 个数的组合**。**组合中只允许含有 1 - 9 的正整数，并且每种组合中不存在重复的数字。

**说明：**

- 所有数字都是正整数。
- 解集不能包含重复的组合。 

**示例 1:**

```
输入: k = 3, n = 7
输出: [[1,2,4]]
```

**示例 2:**

```
输入: k = 3, n = 9
输出: [[1,2,6], [1,3,5], [2,3,4]]
```

#### 216.2.解法
##### 216.2.1 方法一

```python
class Solution:
    def combinationSum3(self, k: int, n: int) -> List[List[int]]:
        
        def generate(start, item, result, nums, n):
            if n < 0 or len(item) > k:
                return
            if n == 0 and len(item) == k:
              result.append(list(item))
              return 
            for j in range(start, len(nums)):
                item.append(nums[j])
                generate(j+1, item, result, nums, n-nums[j])
                item.pop()
            
        nums = range(1, min(10, n))
        item = []
        result = []
        generate(0, item, result, nums, n)
        return result
```

##### 216.2.2 方法二

### 377. 组合总和 Ⅳ DP[推荐]

#### 377.1.题目描述

给定一个由正整数组成且不存在重复数字的数组，找出和为给定目标正整数的组合的个数。

示例:

nums = [1, 2, 3]
target = 4

所有可能的组合为：
(1, 1, 1, 1)
(1, 1, 2)
(1, 2, 1)
(1, 3)
(2, 1, 1)
(2, 2)
(3, 1)

请注意，顺序不同的序列被视作不同的组合。

因此输出为 7。
进阶：
如果给定的数组中含有负数会怎么样？
问题会产生什么变化？
我们需要在题目中添加什么限制来允许负数的出现？

#### 377.2.解法
##### 377.2.1 方法一 递归 超出时间限制

```java
class Solution {
    public int combinationSum4(int[] nums, int target) {
        if (target == 0){
            return 1;
        }
        int res = 0;
        for(int i = 0; i < nums.length; i++){
            if(target >= nums[i]){
                res += combinationSum4(nums, target- nums[i]);
            }
        }
        return res;
    }
}
```

##### 377.2.2 方法二 DP

```java
class Solution:
    def combinationSum4(self, nums: List[int], target: int) -> int:            
        dp = [0] * (target+1)
        dp[0] = 1
        for i in range(1, target+1):
            for num in nums:
                if i >= num:
                    dp[i] += dp[i-num]
        return dp[target]
```

### 46.全排列

#### 46.1.题目描述

给定一个**没有重复**数字的序列，返回其所有可能的全排列。

进阶：有重复数字

**示例:**

```
输入: [1,2,3]
输出:
[
  [1,2,3],
  [1,3,2],
  [2,1,3],
  [2,3,1],
  [3,1,2],
  [3,2,1]
]
```

#### 46.2.解法
##### 46.2.1 方法一

```python
class Solution:
    def permute(self, nums: List[int]) -> List[List[int]]:
        def generate(item, result, nums):
            if len(item) == len(nums):
                result.append(list(item))
            else:
                for j in range(len(nums)):
                    if nums[j] not in item:
                        item.append(nums[j])
                        generate(item, result, nums)
                        item.pop()
        item = []
        result = []
        generate(item, result, nums)
        return result
```



##### 46.2.2 方法二

### 47. 全排列 II

#### 47.1 题目描述

给定一个可包含重复数字的序列，返回所有不重复的全排列。

**示例:**

```
输入: [1,1,2]
输出:
[
  [1,1,2],
  [1,2,1],
  [2,1,1]
]
```

#### 47.2 解法

##### 47.2.1 回溯(基于数组位置)

**关键**: 使用一个额外的字典来记录是否某个位置的值已经被使用了

result使用set

```python
class Solution:
    def permuteUnique(self, nums: List[int]) -> List[List[int]]:
        """返回不重复的全排列(可包含重复数字)
        	
        """
        def backtrack(nums, i, items, result, dic):
            """
                nums(list): 包含重复数字的序列
                i, 序列包含几个数字
            """
            if len(items) == len(nums):
                # 是否重复
                result.add(tuple(items))
                return
            
            for i in range(len(nums)):
                if dic[i]:
                    continue
                items.append(nums[i])
                dic[i] = True
                backtrack(nums, i+1, items, result, dic)
                dic[i] = False
                items.pop()
                          
        result = set()
        items = []
        dic = dict((i, False) for i in range(len(nums)))
        backtrack(nums, 0, items, result, dic)
        result = [list(x) for x in result]
        return result
   
```

result使用list (nums先排序, dfs做校验 if(i>0 &&nums[i-1]==nums[i] && !used[i-1]) continue;)

```java
public class Solution {
    public List<List<Integer>> permuteUnique(int[] nums) {
        List<List<Integer>> res = new ArrayList<List<Integer>>();
        if(nums==null || nums.length==0) return res;
        boolean[] used = new boolean[nums.length];
        List<Integer> list = new ArrayList<Integer>();
        // result使用list
        Arrays.sort(nums);
        dfs(nums, used, list, res);
        return res;
    }

    public void dfs(int[] nums, boolean[] used, List<Integer> list, List<List<Integer>> res){
        if(list.size()==nums.length){
            res.add(new ArrayList<Integer>(list));
            return;
        }
        for(int i=0;i<nums.length;i++){
            if(used[i]) continue;
            // result使用list导致要校验重复情况 (剪枝)
            if(i>0 &&nums[i-1]==nums[i] && !used[i-1]) continue;
            used[i]=true;
            list.add(nums[i]);
            dfs(nums,used,list,res);
            used[i]=false;
            list.remove(list.size()-1);
        }
    }
}

```

##### 47.2.2 回溯(基于各字母的数量)

![image-20190309115418571](/Users/zhengxinzhi/typora_img/LeetCode%E9%A2%98%E7%9B%AE%E6%80%BB%E7%BB%93/image-20190309115418571.png)

![image-20190309120347808](/Users/zhengxinzhi/typora_img/LeetCode%E9%A2%98%E7%9B%AE%E6%80%BB%E7%BB%93/image-20190309120347808.png)

![image-20190309120949430](/Users/zhengxinzhi/typora_img/LeetCode%E9%A2%98%E7%9B%AE%E6%80%BB%E7%BB%93/image-20190309120949430.png)

##### 47.2.3 (todo: )

https://leetcode.com/problems/permutations-ii/discuss/18602/9-line-python-solution-with-1-line-to-handle-duplication-beat-99-of-others-%3A-)

Great solution! Here is a short (casual) **proof** about why `break` can avoid the duplication.
Argument: Assume `ans` is a list of unique permutations with each item length `k`, then `new_ans` is a list of unique permutation with length `k+1`.

1. When `k=0`, it holds.
2. Then we prove it will also holds in each iteration using proof by contradiction.

- Suppose duplicate happens when inserting `n` into the `i`th location, the result is `[l2[:i], n, l2[i:]]`, and it duplicates with the item `[l1[:j], n, l1[j:]]`
- Suppose `i < j`, then we have `l1[i] ==n`, however, we will break when `l1[i]==n`, and thus `n` will not be inserted after `l1[:j] ->` contradiction,
- Suppose `i > j`, then we have `l2[j] == n`, however we will break when `l2[j] == n`, and thus `n` will not be inserted after `l2[:i]` `->` contradiction.
- Suppose `i==j`, then we have `l1==l2`, which contradicts the assumption that `ans` is a list of **unique** permutations.
  Thus the argument hold.

```python
def permuteUnique(self, nums):
    ans = [[]]
    for n in nums:
        new_ans = []
        for l in ans:
            for i in xrange(len(l)+1):
                new_ans.append(l[:i]+[n]+l[i:])
                if i<len(l) and l[i]==n: break              #handles duplication
        ans = new_ans
    return ans
```

### 22.括号生成

#### 22.1 题目描述

给出 *n* 代表生成括号的对数，请你写出一个函数，使其能够生成所有可能的并且**有效的**括号组合。

例如，给出 *n* = 3，生成结果为：

```
[
  "((()))",
  "(()())",
  "(())()",
  "()(())",
  "()()()"
]
```

#### 22.2 解法

```python
import copy
class Solution:
    def generateParenthesis(self, n: int) -> List[str]:
        """
            1.搜索所有的情况(不考虑括号对错) 
            2.剪枝:
                2.1 左括号数 > n or 右括号数 > n
                2.2 左括号数 > 右括号数
        """
#         def search(n, item, result):
#             """
#                  搜索所有的情况(不考虑括号对错) 
#                 item(str): 括号串

#             """
#             if len(item) == 2 * n:
#                 result.append(item)
#                 return
            
#             search(n-1, item+'(', result)
#             search(n-1, item+')', result)
        
        def search_condition(item, left, right, result):
            """
                item(str): 括号串
                left(int): 左括号剩余数量
                right(int): 右括号剩余数量
                result(list)
            """
            if left == 0 and right == 0:
                result.append(copy.deepcopy(item))

            if left > 0:
                search_condition(item+'(', left-1, right, result)
            if right > 0 and left < right:
                search_condition(item+')', left, right-1, result)
            
        
        result = []
        search_condition("", n, n, result)
        return result
        
```

##### 其他解法

```python
# 法一
def generateParenthesis(self, n):
    def generate(p, left, right, parens=[]):
        """
        	p is the parenthesis-string built so far, 
        	left and right tell the number of left and right parentheses still to add, 
        	parens collects the parentheses.
        	
        	解法和上面解法一致
        """
        if left:         generate(p + '(', left-1, right)
        if right > left: generate(p + ')', left, right-1)
        if not right:    parens += p,
        return parens
    return generate('', n, n)

# 法二 Here I wrote an actual Python generator. I allow myself to put the yield q at the end of the line because it's not that bad and because in "real life" I use Python 3 where I just say yield from generate(...).
def generateParenthesis(self, n):
    def generate(p, left, right):
        if right >= left >= 0:
            if not right:
                yield p
            for q in generate(p + '(', left-1, right): yield q
            for q in generate(p + ')', left, right-1): yield q
    return list(generate('', n, n))

# 法三 Improved version of this. Parameter open tells the number of "already opened" parentheses, and I continue the recursion as long as I still have to open parentheses (n > 0) and I haven't made a mistake yet (open >= 0).
def generateParenthesis(self, n, open=0):
    if n > 0 <= open:
        return ['(' + p for p in self.generateParenthesis(n-1, open+1)] + \
               [')' + p for p in self.generateParenthesis(n, open-1)]
    return [')' * open] * (not n)
 
class Solution(object):
    def generateParenthesis(self, n):
        """
        :type n: int
        :rtype: List[str]
        """
        def generate(l, r, item, res):
            if r < l or l < 0 or r < 0:
                return
            elif l == 0 and r == 0:
                res.append(item)
            else:
                if l > 0:
                    generate(l - 1, r, item + "(", res)
                if r > 0:
                    generate(l, r - 1, item + ")", res)
            
        item = ""
        res = []
        generate(n, n, item, res)
        return res

```

### 51.N皇后

#### 51.1 题目描述

n* 皇后问题研究的是如何将 *n* 个皇后放置在 *n*×*n* 的棋盘上，并且使皇后彼此之间不能相互攻击。

![image-20190302180821649](/Users/zhengxinzhi/typora_img/LeetCode题目总结/image-20190302180821649.png)

上图为 8 皇后问题的一种解法。

给定一个整数 *n*，返回所有不同的 *n* 皇后问题的解决方案。

每一种解法包含一个明确的 *n* 皇后问题的棋子放置方案，该方案中 `'Q'` 和 `'.'` 分别代表了皇后和空位。

**示例:**

```
输入: 4
输出: [
 [".Q..",  // 解法 1
  "...Q",
  "Q...",
  "..Q."],

 ["..Q.",  // 解法 2
  "Q...",
  "...Q",
  ".Q.."]
]
解释: 4 皇后问题存在两个不同的解法。
```

#### 51.2 解法

##### 51.2.1 方法一

```python
import copy
class Solution:
    
    def solveNQueens(self, n: int) -> List[List[str]]:
        """
            1.初始化一个 n * n 的棋盘矩阵
            2.放置皇后的策略:
                从第一行开始任选一个位置当做皇后并更新棋盘第一个皇后的攻击范围
            
            边界条件:
        """
        
        def put_down_queen(x, y, n, board):
            """
            	O(n**2)
                作用: 放下一个皇后, 更新棋盘矩阵的值
                步骤: 构建方向数组用来更新棋盘
            """
            dx = [-1, -1, 0, 1, 1, 1, 0, -1]
            dy = [0, 1, 1, 1, 0, -1, -1, -1]
            # 放置皇后
            board[x][y] = 1
            # 置皇后的攻击范围均为1
            for i in range(8):
                for j in range(n):
                    if (x + dx[i] * j) >= 0 and (x + dx[i] * j) < n and (y + dy[i] * j) >= 0 and (y + dy[i] * j) < n:
                        board[x+dx[i]*j][y+dy[i]*j] = 1
            return board
               
            
        def dfsHelper(i, n, board, queen_board, result):
            # i == n 存入board
            if i == n:
                queen_str = [''.join(lst) for lst in queen_board]
                result.append(copy.deepcopy(queen_str))
                return
            # 这一列值全为1 没有皇后可放置位置
            if sum(board[i]) == n:
                return
            
            for j in range(n):
                if board[i][j] == 0:
                    tmp = copy.deepcopy(board)
                    queen_board[i][j] = 'Q'
                    board = put_down_queen(i, j, n, board)
                    dfsHelper(i+1, n, board, queen_board, result)
                    board = copy.deepcopy(tmp)
                    queen_board[i][j] = '.'
                    
            
        # 1.初始化n * n的棋盘
        board = [[0 for j in range(n)] for i in range(n)]
        queen_board = [['.' for j in range(n)] for i in range(n)]
        result = []
        dfsHelper(0, n, board, queen_board, result)
        
        return result
       
```

##### 51.2.2 方法二

[后面的方法思路都类似于方法二, 时间复杂度更低]

In this problem, we can go row by row, and in each position, we need to check if the `column`, the `45° diagonal` and the `135° diagonal` had a queen before.

**Solution A:** Directly check the validity of each position, *12ms*:

```c++
class Solution {
public:
    std::vector<std::vector<std::string> > solveNQueens(int n) {
        std::vector<std::vector<std::string> > res;
        std::vector<std::string> nQueens(n, std::string(n, '.'));
        solveNQueens(res, nQueens, 0, n);
        return res;
    }
private:
    void solveNQueens(std::vector<std::vector<std::string> > &res, std::vector<std::string> &nQueens, int row, int &n) {
        if (row == n) {
            res.push_back(nQueens);
            return;
        }
        for (int col = 0; col != n; ++col)
            if (isValid(nQueens, row, col, n)) {
                nQueens[row][col] = 'Q';
                solveNQueens(res, nQueens, row + 1, n);
                nQueens[row][col] = '.';
            }
    }
    bool isValid(std::vector<std::string> &nQueens, int row, int col, int &n) {
        // 时间复杂度 O(n)
        //check if the column had a queen before.
        for (int i = 0; i != row; ++i)
            if (nQueens[i][col] == 'Q')
                return false;
        //check if the 45° diagonal had a queen before.
        for (int i = row - 1, j = col - 1; i >= 0 && j >= 0; --i, --j)
            if (nQueens[i][j] == 'Q')
                return false;
        //check if the 135° diagonal had a queen before.
        for (int i = row - 1, j = col + 1; i >= 0 && j < n; --i, ++j)
            if (nQueens[i][j] == 'Q')
                return false;
        return true;
    }
};
```



##### 52.2.3 方法三(推荐掌握 很规范)

![image-20190303125851748](/Users/zhengxinzhi/typora_img/LeetCode题目总结/image-20190303125851748.png)

https://www.youtube.com/watch?v=wGbuCyNpxIg

![image-20190303130609856](/Users/zhengxinzhi/typora_img/LeetCode题目总结/image-20190303130609856.png)



![image-20190303130854776](/Users/zhengxinzhi/typora_img/LeetCode题目总结/image-20190303130854776.png)

##### 52.2.4 方法四

Use the `DFS` helper function to find solutions recursively. A solution will be found when the length of `queens` is equal to `n` ( `queens` is a list of the indices of the queens).


In this problem, whenever a location `(x, y`) is occupied, any other locations `(p, q )` where `p + q == x + y` or `p - q == x - y` would be invalid. We can use this information to keep track of the indicators (`xy_dif` and `xy_sum` ) of the invalid positions and then call DFS recursively with valid positions only. 



At the end, we convert the result (a list of lists; each sublist is the indices of the queens) into the desire format.

```python
def solveNQueens(self, n):
    def DFS(queens, xy_dif, xy_sum):
        """
        	queens(list): 每一种棋盘 每一个元素是queen在每一行的index
        				 `queens` is a list of the indices of the queens
        	xy_dif(list): x - y
        	xy_sum(list): x + y
        	result 记录的是
        """
        # p行数 q列数
        p = len(queens)
        if p==n:
            result.append(queens)
            return None
        for q in range(n):
            # 限制条件 `p + q == x + y` or `p - q == x - y` is invalid
            if q not in queens and p-q not in xy_dif and p+q not in xy_sum: 
                DFS(queens+[q], xy_dif+[p-q], xy_sum+[p+q])  
    result = []
    DFS([],[],[])
    return [ ["."*i + "Q" + "."*(n-i-1) for i in sol] for sol in result]
```

### 131.分割字符串 【重点】

#### 题目描述

给定一个字符串 s，将 s 分割成一些子串，使每个子串都是回文串。

返回 s 所有可能的分割方案。

示例:

输入: "aab"
输出:
[
  ["aa","b"],
  ["a","a","b"]
]


#### 解法

##### 解法一 回溯

如果输入是“aab”，检查[0,0]“a”是否是回文。然后检查[0,1]“aa”，然后检查[0,2]“aab”。
在检查[0,0]时，字符串的其余部分为“ab”，使用ab作为输入来进行递归调用。
如果输入是“aab”，检查[0,0]“a”是否是回文。然后检查[0,1]“aa”，然后检查[0,2]“aab”。在检查[0,0]时，字符串的其余部分为“ab”，使用ab作为输入来进行递归调用

```python
class Solution:
    def partition(self, s: str) -> List[List[str]]:
        def isPalindrome(l, r):
            if l == r:
                return True
            while l < r:
                if s[l] != s[r]:
                    return False
                l += 1
                r -= 1
            return True

        def generate(i, item, s, result):
            if i == len(s):
                result.append(item)
            else:
                for j in range(i, len(s)):
                    # key: 如果已有的是回文子串 剩下的继续回溯
                    if isPalindrome(i, j):
                        generate(j + 1, item + [s[i: j + 1]], s, result)
                

        item = []
        result = []
        generate(0, item, s, result)
        return result

```

##### 解法二
```python

```

###89.格雷编码

#### 题目描述

格雷编码是一个二进制数字系统，在该系统中，两个连续的数值仅有一个位数的差异。

给定一个代表编码总位数的非负整数 n，打印其格雷编码序列。格雷编码序列必须以 0 开头。

示例 1:

输入: 2
输出: [0,1,3,2]
解释:
00 - 0
01 - 1
11 - 3
10 - 2

对于给定的 n，其格雷编码序列并不唯一。
例如，[0,2,3,1] 也是一个有效的格雷编码序列。

00 - 0
10 - 2
11 - 3
01 - 1
示例 2:

输入: 0
输出: [0]
解释: 我们定义格雷编码序列必须以 0 开头。
     给定编码总位数为 n 的格雷编码序列，其长度为 2n。当 n = 0 时，长度为 20 = 1。
     因此，当 n = 0 时，其格雷编码序列为 [0]。


#### 解法

##### 解法一 回溯 O(2**n)
```python
class Solution:
    def grayCode(self, n: int) -> List[int]:
        
        def generate(item, visited):
            res.append(int(item, 2))
            for j in range(0, n):
                if item[:j] + str(1 - int(item[j])) + item[j + 1:] not in visited:
                    item = item[:j] + str(1 - int(item[j])) + item[j + 1:]
                    visited.add(item)
                    generate(item, visited)
                    
                
                
        if n == 0:
            return [0]
        item = "0" * n
        res = []
        visited = {item}
        generate(item, visited)
        return res
```

##### 解法二 镜面反射法(动态规划) O(2**n)

![image-20191021095951684](/Users/zhengxinzhi/typora_img/LeetCode题目总结/image-20191021095951684.png)

```python
class Solution:
    def grayCode(self, n: int) -> List[int]:
        res, head = [0], 1
        for i in range(n):
            for j in range(len(res) - 1, -1, -1):
                res.append(head + res[j])
            head <<= 1
        return res
```

## DFS

### 79.单词搜索

#### 题目描述

给定一个二维网格和一个单词，找出该单词是否存在于网格中。

单词必须按照字母顺序，通过相邻的单元格内的字母构成，其中“相邻”单元格是那些水平相邻或垂直相邻的单元格。同一个单元格内的字母不允许被重复使用。

示例:

board =
[
  ['A','B','C','E'],
  ['S','F','C','S'],
  ['A','D','E','E']
]

给定 word = "ABCCED", 返回 true.
给定 word = "SEE", 返回 true.
给定 word = "ABCB", 返回 false.

#### 解法

##### 解法一 DFS 

```python
class Solution(object):
    def exist(self, board, word):
        """
        :type board: List[List[str]]
        :type word: str
        :rtype: bool
        """
        def dfs(i, j, board, word):
            if not word:
                return True
            
            directions = [(0, -1), (-1, 0), (0, 1), (1, 0)]
            for dx, dy in directions:
                if i >= 0 and i < m and j >= 0 and j < n and board[i][j] == word[0]:
                    temp = board[i][j]
                    board[i][j] = '#'
                    if dfs(i + dx, j + dy, board, word[1:]):
                        return True
                    board[i][j] = temp
            return False
            
            
        if not board or len(board) == 0:
            return False
        m = len(board)
        n = len(board[0])
        
        for i in range(m):
            for j in range(n):
                if dfs(i, j, board, word):
                    return True
                
        return False
        
```

##### 解法二 推荐

```python
class Solution(object):
    def exist(self,board,word):
        if not board:
            return False
        for i in range(len(board)):
            for j in range(len(board[0])):
                if self.dfs(board,i,j,word):
                    return True
        return False

    def dfs(self,board,i,j,word):
        if len(word)==0:
            return True
        # 终止条件
        if i < 0 or i >= len(board) or j < 0 or j >= len(board[0]) or word[0] != board[i][j]:
            return False
        tmp = board[i][j]
        # 标记
        board[i][j] = '#'
        res = self.dfs(board, i + 1, j, word[1:]) or self.dfs(board, i - 1, j, word[1:]) \
              or self.dfs(board, i, j + 1, word[1:]) or self.dfs(board, i, j - 1, word[1:])
        # 恢复标记
        board[i][j] = tmp
        return res
```

##### 解法三

```python
class Solution(object):
    
    # 定义上下左右四个行走方向
    directs = [(0, 1), (0, -1), (1, 0), (-1, 0)]
    
    def exist(self, board, word):
        """
        :type board: List[List[str]]
        :type word: str
        :rtype: bool
        """
        m = len(board)
        if m == 0:
            return False
        n = len(board[0])
        mark = [[0 for _ in range(n)] for _ in range(m)]
                
        for i in range(len(board)):
            for j in range(len(board[0])):
                if board[i][j] == word[0]:
                    # 将该元素标记为已使用
                    mark[i][j] = 1
                    if self.backtrack(i, j, mark, board, word[1:]) == True:
                        return True
                    else:
                        # 回溯
                        mark[i][j] = 0
        return False
        
        
    def backtrack(self, i, j, mark, board, word):
        if len(word) == 0:
            return True
        
        for direct in self.directs:
            cur_i = i + direct[0]
            cur_j = j + direct[1]
            
            if cur_i >= 0 and cur_i < len(board) and cur_j >= 0 and cur_j < len(board[0]) and board[cur_i][cur_j] == word[0]:
                # 如果是已经使用过的元素，忽略
                if mark[cur_i][cur_j] == 1:
                    continue
                # 将该元素标记为已使用
                mark[cur_i][cur_j] = 1
                if self.backtrack(cur_i, cur_j, mark, board, word[1:]) == True:
                    return True
                else:
                    # 回溯
                    mark[cur_i][cur_j] = 0
        return False
```

##### 解法四 回溯

```python
class Solution:
    def exist(self, board: List[List[str]], word: str) -> bool:
        row = len(board)
        col = len(board[0])

        def helper(i, j, k, visited):
            #print(i,j, k,visited)
            if k == len(word):
                return True
            for x, y in [(-1, 0), (1, 0), (0, 1), (0, -1)]:
                tmp_i = x + i
                tmp_j = y + j
                if 0 <= tmp_i < row and 0 <= tmp_j < col and (tmp_i, tmp_j) not in visited \
                and board[tmp_i][tmp_j] == word[k]:
                    visited.add((tmp_i, tmp_j))
                    if helper(tmp_i, tmp_j, k+1, visited):
                        return True
                    visited.remove((tmp_i, tmp_j)) # 回溯
            return False
        
        for i in range(row):
            for j in range(col):
                if board[i][j] == word[0] and helper(i, j, 1,{(i, j)}) :
                        return True
        return False
```

### 100.相同的树

#### 100.1.题目描述

给定两个二叉树，编写一个函数来检验它们是否相同。

如果两个树在结构上相同，并且节点具有相同的值，则认为它们是相同的。

**示例 1:**

```
输入:       1         1
          / \       / \
         2   3     2   3

        [1,2,3],   [1,2,3]

输出: true
```

**示例 2:**

```
输入:      1          1
          /           \
         2             2

        [1,2],     [1,null,2]

输出: false
```

**示例 3:**

```
输入:       1         1
          / \       / \
         2   1     1   2

        [1,2,1],   [1,1,2]

输出: false
```

#### 100.2.解法

##### 100.2.1 方法一 递归

```python
class Solution:
    def isSameTree(self, p: TreeNode, q: TreeNode) -> bool:
        """
            方法一: 递归
        """
        if not p and not q:
            return True
        if not p or not q:
            return False
        if p.val != q.val:
            return False
        return self.isSameTree(p.left, q.left) and self.isSameTree(p.right, q.right)
```

##### 100.2.2 方法二 BFS

```python
from collections import deque
class Solution:
    def isSameTree(self, p, q):
        """
        :type p: TreeNode
        :type q: TreeNode
        :rtype: bool
        """    
        def check(p, q):
            if not p and not q:
                return True
            if not q or not p:
                return False
            if p.val != q.val:
                return False
            return True
        
        deq = deque([(p, q),])
        while deq:
            p, q = deq.popleft()
            if not check(p, q):
                return False
            if p:
                deq.append((p.left, q.left))
                deq.append((p.right, q.right))
                    
        return True
      
```

##### 100.2.3 方法三 DFS

```python

class Solution:
    def isSameTree(self, p, q):
        """
        :type p: TreeNode
        :type q: TreeNode
        :rtype: bool
        """    
        def check(p, q):
            if not p and not q:
                return True
            if not q or not p:
                return False
            if p.val != q.val:
                return False
            return True
        
        stack = [(p, q)]
        while stack:
            p, q = stack.pop()
            if not check(p, q):
                return False
            if p:
                stack.append((p.left, q.left))
                stack.append((p.right, q.right))
        return True
    
```

### 101. 对称二叉树

#### 101. 1.题目描述

给定一个二叉树，检查它是否是镜像对称的。

例如，二叉树 `[1,2,2,3,4,4,3]` 是对称的。

```
    1
   / \·12
  2   2
 / \ / \
3  4 4  3
```

但是下面这个 `[1,2,2,null,3,null,3]` 则不是镜像对称的:

```
    1
   / \
  2   2
   \   \
   3    3
```

#### 101. 2.解法

##### 101. 2.1 方法一 递归法

```python
class Solution:
    def isSymmetric(self, root: TreeNode) -> bool:
        """
            递归
        """
        def symSame(p, q):
            if not p and not q:
                return True
            if not p or not q:
                return False
            if p.val != q.val:
                return False
            return symSame(p.left, q.right) and symSame(p.right, q.left)
        
        if not root:
            return True
        return symSame(root.left, root.right)
```

##### 101. 2.2 方法二 BFS

```python
from collections import deque
class Solution:
    def isSymmetric(self, root: TreeNode) -> bool:
        def check(p, q):
            if not p and not q:
                return True
            if not p or not q:
                return False
            if p.val != q.val:
                return False
            return True
        
        if not root:
            return True
        deq = deque([(root.left, root.right)])
        while deq:
            p, q = deq.popleft()
            if not check(p, q):
                return False
            if p:
                deq.append((p.left, q.right))
                deq.append((p.right, q.left))
        return True     
```

##### 101.2.3 方法1三 DFS

```python
class Solution:
    def isSymmetric(self, root: TreeNode) -> bool:
        def check(p, q):
            if not p and not q:
                return True
            if not p or not q:
                return False
            if p.val != q.val:
                return False
            return True
        
        if not root:
            return True
        stack = [(root.left, root.right)]
        while stack:
            p, q = stack.pop()
            if not check(p, q):
                return False
            if p:
                stack.append((p.left, q.right))
                stack.append((p.right, q.left))
        return True                
```

### 104. 二叉树的最大深度

#### 104.1.题目描述

给定一个二叉树，找出其最大深度。

二叉树的深度为根节点到最远叶子节点的最长路径上的节点数。

**说明:** 叶子节点是指没有子节点的节点。

**示例：**
给定二叉树 `[3,9,20,null,null,15,7]`，

```
    3
   / \
  9  20
    /  \
   15   7
```

返回它的最大深度 3 

#### 104.2.解法
##### 104.2.1 方法一 递归

```python
class Solution:
    def maxDepth(self, root: TreeNode) -> int:
        if not root:
            return 0
        return max(1+self.maxDepth(root.left), 1+self.maxDepth(root.right))
```

##### 104.2.2 方法二 BFS

```python
from collections import deque
class Solution:
    def maxDepth(self, root: TreeNode) -> int:
        if not root:
            return 0
        max_v = 0
        deq = deque([(root, 1)])
        while deq:
            p, cur_v = deq.popleft()
            if p:
                max_v = max(max_v, cur_v)
                deq.append((p.left, cur_v+1))
                deq.append((p.right, cur_v+1))
        return max_v
```

##### 104.2.3 方法三 DFS

```python
class Solution:
    def maxDepth(self, root: TreeNode) -> int:
        if not root:
            return 0
        max_v = 0
        stack = [(root, 1)]
        while stack:
            p, cur_v = stack.pop()
            if p:
                max_v = max(max_v, cur_v)
                stack.append((p.left, cur_v+1))
                stack.append((p.right, cur_v+1))
        return max_v
```

### 108. 将有序数组转换为二叉搜索树

#### 108.1.题目描述

将一个按照升序排列的有序数组，转换为一棵高度平衡二叉搜索树。

本题中，一个高度平衡二叉树是指一个二叉树*每个节点* 的左右两个子树的高度差的绝对值不超过 1。

**示例:**

```
给定有序数组: [-10,-3,0,5,9],

一个可能的答案是：[0,-3,9,-10,null,5]，它可以表示下面这个高度平衡二叉搜索树：

      0
     / \
   -3   9
   /   /
 -10  5
```

#### 108.2.解法
##### 108.2.1 方法一 递归

```python
class Solution:
    def sortedArrayToBST(self, nums: List[int]) -> TreeNode:
        if not nums:
            return None
        mid = len(nums) >> 1
        root = TreeNode(nums[mid])
        root.left = self.sortedArrayToBST(nums[:mid])
        root.right = self.sortedArrayToBST(nums[mid+1:])
        return root
```

### 110.平衡二叉树

#### 110.1.题目描述

给定一个二叉树，判断它是否是高度平衡的二叉树。

本题中，一棵高度平衡二叉树定义为：

> 一个二叉树*每个节点* 的左右两个子树的高度差的绝对值不超过1。

**示例 1:**

给定二叉树 `[3,9,20,null,null,15,7]`

```
    3
   / \
  9  20
    /  \
   15   7
```

返回 `true` 。

**示例 2:**

给定二叉树 `[1,2,2,3,3,null,null,4,4]`

```
       1
      / \
     2   2
    / \
   3   3
  / \
 4   4
```

返回 `false` 。

#### 110.2.解法
##### 110.2.1 方法一 递归

```python
class Solution:
    def isBalanced(self, root: TreeNode) -> bool:
        """
            递归
        """
        def helper(root):      
            if not root:
                return 0
            l_len = helper(root.left)
            if l_len == -1:
                return -1
            r_len = helper(root.right)
            if r_len == -1:
                return -1           
            if abs(l_len - r_len) <= 1:
                return max(l_len, r_len) + 1
            else:
                return -1
        return helper(root) >= 0
```

### 111. 二叉树的最小深度

#### 111.1.题目描述

给定一个二叉树，找出其最小深度。

最小深度是从根节点到最近叶子节点的最短路径上的节点数量。

**说明:** 叶子节点是指没有子节点的节点。

**示例:**

给定二叉树 `[3,9,20,null,null,15,7]`,

```
    3
   / \
  9  20
    /  \
   15   7
```

返回它的最小深度  2.

#### 111.2.解法
##### 111.2.1 方法一 递归

```python
class Solution:
    def minDepth(self, root: TreeNode) -> int:
        if not root:
            return 0
        l_len = self.minDepth(root.left)
        r_len = self.minDepth(root.right)
        if l_len == 0 or r_len == 0:
            return max(l_len, r_len) + 1
        return min(l_len, r_len) + 1
```

### 112. 路径总和

#### 112.1.题目描述

给定一个二叉树和一个目标和，判断该树中是否存在根节点到叶子节点的路径，这条路径上所有节点值相加等于目标和。

**说明:** 叶子节点是指没有子节点的节点。

**示例:** 
给定如下二叉树，以及目标和 `sum = 22`，

```
              5
             / \
            4   8
           /   / \
          11  13  4
         /  \      \
        7    2      1
```

返回 `true`, 因为存在目标和为 22 的根节点到叶子节点的路径 `5->4->11->2`。

#### 112.2.解法
##### 112.2.1 方法一 递归

```python
class Solution:
    def hasPathSum(self, root: TreeNode, sum: int) -> bool:
        if not root:
            return False
        sum -= root.val
        if not root.left and not root.right:
            return sum == 0
        return self.hasPathSum(root.left, sum) or self.hasPathSum(root.right, sum)
```

##### 112.2.2 方法二 dfs

```python
class Solution:
    def hasPathSum(self, root: TreeNode, sum: int) -> bool:
        if not root:
            return False
        
        stack = [(root, sum - root.val)]
        while stack:
            node, sum = stack.pop()
            if not node.left and not node.right and sum == 0:
                return True
            if node.left:
                stack.append((node.left, sum - node.left.val))
            if node.right:
                stack.append((node.right, sum - node.right.val))
        return False
```

###113.[路径总和 II](https://leetcode-cn.com/problems/path-sum-ii/)

#### 题目描述

给定一个二叉树和一个目标和，找到所有从根节点到叶子节点路径总和等于给定目标和的路径。

说明: 叶子节点是指没有子节点的节点。

示例:
给定如下二叉树，以及目标和 sum = 22，

              5
             / \
            4   8
           /   / \
          11  13  4
         /  \    / \
        7    2  5   1
返回:

[
   [5,4,11,2],
   [5,8,4,5]
]
在真实的面试


#### 解法

##### 解法一 dfs

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None

class Solution:
    def pathSum(self, root: TreeNode, sum: int) -> List[List[int]]:
        if not root:
            return []
        parents = {root: None}
        stack = [(root, sum - root.val)]
        ends = []
        while stack:
            root, sum = stack.pop()
            if not root.left and not root.right and sum == 0:
                ends.append(root)
            if root.left:
                parents[root.left] = root
                stack.append((root.left, sum - root.left.val))
            if root.right:
                parents[root.right] = root
                stack.append((root.right, sum - root.right.val))
        res = []
        for end in ends:
            lst = []
            while end:
                lst.append(end.val)
                end = parents[end]
            res.append(lst[::-1])

        return res
```

##### 解法二 递归
```python
class Solution:
    def pathSum(self, root: TreeNode, sum: int) -> List[List[int]]:
        if not root:
            return []
        res = []
        def dfs(root, sum, tmp):
            if not root:
                return 
            if not root.left and not root.right and sum - root.val == 0 :
                tmp += [root.val]
                res.append(tmp)
            dfs(root.left, sum - root.val, tmp + [root.val])
            dfs(root.right, sum - root.val, tmp + [root.val])   
        dfs(root, sum, [])  
        return res
```

### 200.岛屿数量

#### 题目描述

给定一个由 '1'（陆地）和 '0'（水）组成的的二维网格，计算岛屿的数量。一个岛被水包围，并且它是通过水平方向或垂直方向上相邻的陆地连接而成的。你可以假设网格的四个边均被水包围。

示例 1:

输入:
11110
11010
11000
00000

输出: 1
示例 2:

输入:
11000
11000
00100
00011

输出: 3


#### 解法

##### 解法一 DFS 

思路一：遇到一个'1' 把和这个'1'相连的所有'1'变为0 结果累加1 

方法：DFS or BFS

思路二: 碰到一个新的'1'(即左面和上面均没有'1')累加1

```python
class Solution:
    def numIslands(self, grid: List[List[str]]) -> int:
        """
            思路一：遇到一个'1' 把和这个'1'相连的所有'1'变为0 结果累加1
            思路二: 碰到一个新的'1'(即左面和上面均没有'1')累加1
        """
        def dfs(grid, i, j):
            # 记录已访问
            grid[i][j] = 0
            # 左 上 右 下
            directions = [(0, -1), (-1, 0), (0, 1), (1, 0)]
            for dx, dy in directions:
                if (i + dx) >= 0 and (i + dx) < length and (j + dy) >= 0 and (j + dy) < width and grid[i + dx][j + dy] == '1':
                    dfs(grid, i + dx, j + dy)
            
        
        if not grid:
            return 0
        length = len(grid)
        width = len(grid[0])
        res = 0
        for i in range(length):
            for j in range(width):
                if grid[i][j] == '1':
                    res += 1
                    dfs(grid, i, j)
        return res
```

##### 解法二 BFS
```python
from typing import List
from collections import deque


class Solution:
    #        x-1,y
    # x,y-1    x,y      x,y+1
    #        x+1,y
    # 方向数组，它表示了相对于当前位置的 4 个方向的横、纵坐标的偏移量，这是一个常见的技巧
    directions = [(-1, 0), (0, -1), (1, 0), (0, 1)]

    def numIslands(self, grid: List[List[str]]) -> int:
        m = len(grid)
        # 特判
        if m == 0:
            return 0
        n = len(grid[0])
        marked = [[False for _ in range(n)] for _ in range(m)]
        count = 0
        # 从第 1 行、第 1 格开始，对每一格尝试进行一次 DFS 操作
        for i in range(m):
            for j in range(n):
                # 只要是陆地，且没有被访问过的，就可以使用 BFS 发现与之相连的陆地，并进行标记
                if not marked[i][j] and grid[i][j] == '1':
                    # count 可以理解为连通分量，你可以在广度优先遍历完成以后，再计数，
                    # 即这行代码放在【位置 1】也是可以的
                    count += 1
                    queue = deque()
                    queue.append((i, j))
                    # 注意：这里要标记上已经访问过
                    marked[i][j] = True
                    while queue:
                        cur_x, cur_y = queue.popleft()
                        # 得到 4 个方向的坐标
                        for direction in self.directions:
                            new_i = cur_x + direction[0]
                            new_j = cur_y + direction[1]
                            # 如果不越界、没有被访问过、并且还要是陆地，我就继续放入队列，放入队列的同时，要记得标记已经访问过
                            if 0 <= new_i < m and 0 <= new_j < n and not marked[new_i][new_j] and grid[new_i][new_j] == '1':
                                queue.append((new_i, new_j))
                                #【特别注意】在放入队列以后，要马上标记成已经访问过，语义也是十分清楚的：反正只要进入了队列，你迟早都会遍历到它
                                # 而不是在出队列的时候再标记
                                #【特别注意】如果是出队列的时候再标记，会造成很多重复的结点进入队列，造成重复的操作，这句话如果你没有写对地方，代码会严重超时的
                                marked[new_i][new_j] = True
                    #【位置 1】
        return count
```

### 236.二叉树的最近公共祖先

#### 题目描述

给定一个二叉树, 找到该树中两个指定节点的最近公共祖先。

百度百科中最近公共祖先的定义为：“对于有根树 T 的两个结点 p、q，最近公共祖先表示为一个结点 x，满足 x 是 p、q 的祖先且 x 的深度尽可能大（一个节点也可以是它自己的祖先）。”

例如，给定如下二叉树:  root = [3,5,1,6,2,0,8,null,null,7,4

示例 1:

输入: root = [3,5,1,6,2,0,8,null,null,7,4], p = 5, q = 1
输出: 3
解释: 节点 5 和节点 1 的最近公共祖先是节点 3。
示例 2:

输入: root = [3,5,1,6,2,0,8,null,null,7,4], p = 5, q = 4
输出: 5
解释: 节点 5 和节点 4 的最近公共祖先是节点 5。因为根据定义最近公共祖先节点可以为节点本身。


说明:

所有节点的值都是唯一的。
p、q 为不同节点且均存在于给定的二叉树中。


#### 解法

##### 解法一 递归

当你遇到节点 p 或 q 时，返回一些布尔标记。该标志有助于确定是否在任何路径中找到了所需的节点。最不常见的祖先将是两个子树递归都返回真标志的节点。它也可以是一个节点，它本身是p或q中的一个，对于这个节点,子树递归返回一个真标志。

```python
class Solution:
    def lowestCommonAncestor(self, root: 'TreeNode', p: 'TreeNode', q: 'TreeNode') -> 'TreeNode':
        """
            思路1: 递归
            思路1: 某结点是否是另一个结点的祖先结点 n * n
            思路2: DFS
            思路3: 按照标号除2
        """
        # 1.终止条件
        if not root:
            return root
        if root == p or root == q:
            return root
        
        # 2.递归
        left = self.lowestCommonAncestor(root.left, p, q)
        right = self.lowestCommonAncestor(root.right, p, q)
        # 3.递归结果处理
        if left and right:
            return root
        elif left:
            return left
        elif right:
            return right
        return None
```

复杂度分析

时间复杂度：O\big(N\big)O(N)，NN 是二叉树中的节点数，最坏情况下，我们需要访问二叉树的所有节点。
空间复杂度：O(N)O(N)，这是因为递归堆栈使用的最大空间位 NN,斜二叉树的高度可以是 NN。

##### 解法二 字典存储双亲结点 + DFS

```python
class Solution:
    def lowestCommonAncestor(self, root, p, q):
        # 1.dfs遍历 存储每个父亲结点
        stack = [root]
        parent= {root:None}
        while p not in parent or q not in parent:
            node = stack.pop()
            if node.left:
                parent[node.left] = node
                stack.append(node.left)
            if node.right:
                parent[node.right] = node
                stack.append(node.right)
                
        # 2.先找到p的所有祖先结点放入set
        ancestors = set()
        while p:
            ancestors.add(p)
            p = parent[p]
        
        # 3.判断q的祖先结点是否是p的祖先结点
        while q not in ancestors:
            q = parent[q]
        return 
 
		def lowestCommonAncestor(self, root, p, q):
        if not root:
            return None
        parents = {root: None}
        stack = [root]
        while stack:
            node = stack.pop()
            if node.left:
                parents[node.left] = node
                stack.append(node.left)
            if node.right:
                parents[node.right] = node
                stack.append(node.right)

        left_set = set()
        while p:
            left_set.add(p)
            p = parents[p]
        while q:
            if q in left_set:
                return q
            else:
                q = parents[q]
        return None
```

##### 解法三  BFS + 树索引性质

类似解法二 

BFS遍历存储所有结点的索引 从1开始

类用父亲结点是孩子结点的1/2来判断

```python
class Solution:
    def lowestCommonAncestor(self, root: 'TreeNode', p: 'TreeNode', q: 'TreeNode') -> 'TreeNode':
				tree = dict()
        # 1. BFS遍历存储所有结点的索引 从1开始
        que = queue.Queue()
        que.put((root, 1))
        p_index = -1
        q_index = -1
        while not que.empty():
            node, index = que.get()
            if p == node:
                p_index = index
            elif q == node:
                q_index = index
            tree[index] = node
            if node.left:
                que.put((node.left, 2 * index))
            if node.right:
                que.put((node.right, 2 * index + 1))
        
        p_set = set()
        # 2.计算所有p的祖先结点（包括自己）
        while p_index:
            p_set.add(p_index)
            p_index = p_index >> 1
        # 3. 判断p q的公共祖先结点
        while q_index:
            if q_index in p_set:
                return tree[q_index]
            q_index= q_index >> 1
```



## 链表

### [2. 两数相加](https://leetcode-cn.com/problems/add-two-numbers/)

#### 题目描述

给出两个 非空 的链表用来表示两个非负的整数。其中，它们各自的位数是按照 逆序 的方式存储的，并且它们的每个节点只能存储 一位 数字。

如果，我们将这两个数相加起来，则会返回一个新的链表来表示它们的和。

您可以假设除了数字 0 之外，这两个数都不会以 0 开头。

示例：

输入：(2 -> 4 -> 3) + (5 -> 6 -> 4)
输出：7 -> 0 -> 8
原因：342 + 465 = 807


#### 解法

##### 解法一
```python
class Solution:
    def addTwoNumbers(self, l1: ListNode, l2: ListNode) -> ListNode:
        jinwei = 0
        head = None
        tail = None
        while l1 or l2:
            l1_val = l1.val if l1 else 0
            l2_val = l2.val if l2 else 0

            val = (l1_val + l2_val + jinwei) % 10
            jinwei = (l1_val + l2_val + jinwei) // 10
            
            if head:
                tail.next = ListNode(val)
                tail = tail.next
            else:
                head = ListNode(val)
                tail = head
            
            l1 = l1.next if l1 else l1
            l2 = l2.next if l2 else l2
            
        if jinwei:
            tail.next = ListNode(jinwei)
        return head
                
```

##### 解法二
```python

```

###19. 删除链表的倒数第N个节点

#### 题目描述

给定一个链表，删除链表的倒数第 *n* 个节点，并且返回链表的头结点。

**示例：**

```
给定一个链表: 1->2->3->4->5, 和 n = 2.

当删除了倒数第二个节点后，链表变为 1->2->3->5.
```

**说明：**

给定的 *n* 保证是有效的。

**进阶：**

你能尝试使用一趟扫描实现吗？

#### 解法

##### 解法一
```python
class Solution:
    def removeNthFromEnd(self, head: ListNode, n: int) -> ListNode:

        f, cur, p = None, head, head
        for i in range(n):
            p = p.next
        
        if not p:
            return head.next
        while p:
            f = cur
            cur = cur.next
            p = p.next
            
        if cur and cur.next:
            f.next = cur.next
        elif cur:
            f.next = None
        return head
       			
```

##### 解法二 推荐
```python
class Solution:
    def removeNthFromEnd(self, head: ListNode, n: int) -> ListNode:
        """
            边界条件: 删除末尾
        """
        dummy = ListNode(0)
        dummy.next = head
        first, second = dummy, dummy
        while n + 1:
            first = first.next
            n -= 1
            
        while first:
            first = first.next
            second = second.next
        second.next = second.next.next
        return dummy.next
```

###21.[合并两个有序链表](https://leetcode-cn.com/problems/merge-two-sorted-lists/)

#### 题目描述

将两个有序链表合并为一个新的有序链表并返回。新链表是通过拼接给定的两个链表的所有节点组成的。 

示例：

输入：1->2->4, 1->3->4
输出：1->1->2->3->4->4


#### 解法

##### 解法一
```python
class Solution:
    def mergeTwoLists(self, l1, l2):
        dummy = ListNode(-1)
        prev = dummy
        while l1 and l2:
            if l1.val <= l2.val:
                prev.next = l1
                l1 = l1.next
            else:
                prev.next = l2
                l2 = l2.next
            prev = prev.next
        prev.next = l1 if l1 is not None else l2
        return dummy.next

```

##### 解法二 递归
```python
class Solution:
    def mergeTwoLists(self, l1, l2):
        if l1 is None:
            return l2
        elif l2 is None:
            return l1
        elif l1.val < l2.val:
            l1.next = self.mergeTwoLists(l1.next, l2)
            return l1
        else:
            l2.next = self.mergeTwoLists(l1, l2.next)
            return l2

```

###24.两两交换链表中的结点

#### 题目描述

给定一个链表，两两交换其中相邻的节点，并返回交换后的链表。

你不能只是单纯的改变节点内部的值，而是需要实际的进行节点交换。

 

示例:

给定 1->2->3->4, 你应该返回 2->1->4->3.


#### 解法

##### 解法一
```python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, x):
#         self.val = x
#         self.next = None

class Solution:
    def swapPairs(self, head: ListNode) -> ListNode:
        if not head:
            return head
        dummy = ListNode(-1)
        dummy.next = head
        p = dummy
        while p.next and p.next.next:
            second = p.next.next
            p.next.next = second.next
            second.next = p.next
            p.next = second
            p = p.next.next
        return dummy.next
```

##### 解法二
```python

```

###61.旋转链表

#### 题目描述

给定一个链表，旋转链表，将链表每个节点向右移动 k 个位置，其中 k 是非负数。

示例 1:

输入: 1->2->3->4->5->NULL, k = 2
输出: 4->5->1->2->3->NULL
解释:
向右旋转 1 步: 5->1->2->3->4->NULL
向右旋转 2 步: 4->5->1->2->3->NULL
示例 2:

输入: 0->1->2->NULL, k = 4
输出: 2->0->1->NULL
解释:
向右旋转 1 步: 2->0->1->NULL
向右旋转 2 步: 1->2->0->NULL
向右旋转 3 步: 0->1->2->NULL
向右旋转 4 步: 2->0->1->NULL
在真实的面试中遇到过这道题？


#### 解法

##### 解法一
```python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, x):
#         self.val = x
#         self.next = None

class Solution:
    def rotateRight(self, head: ListNode, k: int) -> ListNode:
        """
            法一 逆转链表 且把前k个再次逆转, 再把剩下的逆转
            边界条件: k大于链表长度
        """
        def getLength(head):
            cnt = 0
            while head:
                cnt += 1
                head = head.next
            return cnt
        if not head:
            return None
        # 计算长度
        Len = getLength(head)
        k = k % Len
        
        first, second = head, head
        while k:
            first = first.next
            k -= 1
        if not first:
            return head
        while first.next:
            first = first.next
            second = second.next
        first.next = head
        
        res = second.next
        second.next = None
        return res
       
```

##### 解法二 

- 先将链表闭合成环
- 找到相应的位置断开这个环，确定新的链表头和链表尾

```python
    def rotateRight(self, head: 'ListNode', k: 'int') -> 'ListNode':
        # base cases
        if not head:
            return None
        if not head.next:
            return head
        
        # close the linked list into the ring
        old_tail = head
        n = 1
        while old_tail.next:
            old_tail = old_tail.next
            n += 1
        old_tail.next = head
        
        # find new tail : (n - k % n - 1)th node
        # and new head : (n - k % n)th node
        new_tail = head
        for i in range(n - k % n - 1):
            new_tail = new_tail.next
        new_head = new_tail.next
        
        # break the ring
        new_tail.next = None
        
        return new_head

```

###86.分隔链表

#### 题目描述

给定一个链表和一个特定值 x，对链表进行分隔，使得所有小于 x 的节点都在大于或等于 x 的节点之前。

你应当保留两个分区中每个节点的初始相对位置。

示例:

输入: head = 1->4->3->2->5->2, x = 3
输出: 1->2->2->4->3->5


#### 解法

##### 解法一
```python
class Solution(object):
    def partition(self, head, x):
        """
        :type head: ListNode
        :type x: int
        :rtype: ListNode
        """
        f_dummy, s_dummy = ListNode(-1), ListNode(-1)
        first, second = f_dummy, s_dummy
        
        while head:
            if head.val < x:
                first.next = head
                first = first.next
            else:
                second.next = head
                second = second.next
            head = head.next
        second.next = None
        first.next = s_dummy.next
        
        return f_dummy.next
```

##### 解法二
```python

```

### 92. 反转链表 II [推荐]

#### 92.1 题目描述

反转从位置 *m* 到 *n* 的链表。请使用一趟扫描完成反转。

**说明:**
1 ≤ *m* ≤ *n* ≤ 链表长度。

**示例:**

```
输入: 1->2->3->4->5->NULL, m = 2, n = 4
输出: 1->4->3->2->5->NULL
```

#### 92.2 解法

92.2.1 方法一 维护个前结点

```python
# Definition for singly-linked list.
# class ListNode(object):
#     def __init__(self, x):
#         self.val = x
#         self.next = None

class Solution(object):
    def reverseBetween(self, head, m, n):
        """
        :type head: ListNode
        :type m: int
        :type n: int
        :rtype: ListNode
        """
        if not head:
            return head
        
        cur, prev = head, None
        while m > 1:
            prev = cur
            cur = cur.next
            m, n = m-1, n-1
            
        tail, con = cur, prev
        while n:
            third = cur.next
            cur.next = prev
            prev = cur
            cur = third
            n -= 1
        
        if con:
            con.next = prev
        else:
            head = prev
        tail.next = cur
        return head
```

###[138. 复制带随机指针的链表](https://leetcode-cn.com/problems/copy-list-with-random-pointer/)

#### 题目描述

给定一个链表，每个节点包含一个额外增加的随机指针，该指针可以指向链表中的任何节点或空节点。

要求返回这个链表的深拷贝。 



示例：

![img](/Users/zhengxinzhi/typora_img/LeetCode题目总结/1470150906153-2yxeznm.png)

输入：
{"$id":"1","next":{"$id":"2","next":null,"random":{"$ref":"2"},"val":2},"random":{"$ref":"2"},"val":1}

解释：
节点 1 的值是 1，它的下一个指针和随机指针都指向节点 2 。
节点 2 的值是 2，它的下一个指针指向 null，随机指针指向它自己。


提示：

你必须返回给定头的拷贝作为对克隆列表的引用。




#### 解法

##### 解法一
```python
"""
# Definition for a Node.
class Node:
    def __init__(self, val, next, random):
        self.val = val
        self.next = next
        self.random = random
"""
class Solution:
    def copyRandomList(self, head: 'Node') -> 'Node':
        if not head:
            return None
            
        node_index_dic = dict()
        index_node_dic = dict()
        cur = head
        i = 0
        dummy = ListNode(-1)
        new_cur = dummy
        while cur:
            node_index_dic[cur] = i
            new_cur.next = ListNode(cur.val)
            cur = cur.next
            new_cur = new_cur.next
            index_node_dic[i] = new_cur
            i += 1
        
        cur = head
        new_cur = dummy.next
        while cur:
            new_cur.random = index_node_dic[node_index_dic[cur.random]] if cur.random else None
            new_cur = new_cur.next
            cur = cur.next
        
        return dummy.next
            
```

##### 解法二
```python

```

### 141. 环形链表

#### 题目描述

给定一个链表，判断链表中是否有环。

为了表示给定链表中的环，我们使用整数 pos 来表示链表尾连接到链表中的位置（索引从 0 开始）。 如果 pos 是 -1，则在该链表中没有环。

 

示例 1：

输入：head = [3,2,0,-4], pos = 1
输出：true
解释：链表中有一个环，其尾部连接到第二个节点。


示例 2：

输入：head = [1,2], pos = 0
输出：true
解释：链表中有一个环，其尾部连接到第一个节点。


示例 3：

输入：head = [1], pos = -1
输出：false
解释：链表中没有环。




进阶：

你能用 O(1)（即，常量）内存解决此问题吗？


#### 解法

##### 解法一 快慢指针
```python
    def hasCycle(self, head: ListNode) -> bool:
        if not head:
            return False
        
        slow = fast = head
        while fast and fast.next:
            slow = slow.next
            fast = fast.next.next
            if slow == fast:
                return True
        return False
```

##### 解法二
```python

```

### [142. 环形链表 II](https://leetcode-cn.com/problems/linked-list-cycle-ii/)

#### 题目描述

给定一个链表，返回链表开始入环的第一个节点。 如果链表无环，则返回 null。

为了表示给定链表中的环，我们使用整数 pos 来表示链表尾连接到链表中的位置（索引从 0 开始）。 如果 pos 是 -1，则在该链表中没有环。

说明：不允许修改给定的链表。

 

示例 1：

输入：head = [3,2,0,-4], pos = 1
输出：tail connects to node index 1
解释：链表中有一个环，其尾部连接到第二个节点。


示例 2：

输入：head = [1,2], pos = 0
输出：tail connects to node index 0
解释：链表中有一个环，其尾部连接到第一个节点。


示例 3：

输入：head = [1], pos = -1
输出：no cycle
解释：链表中没有环


#### 解法

##### 解法一 快慢指针
```python
class Solution:
    def detectCycle(self, head: ListNode) -> ListNode:
        if not head:
            return None
        slow = fast = head
        while fast and fast.next:
            slow = slow.next
            fast = fast.next.next
            if slow == fast:
                break
        if not fast or not fast.next:
            return None
        fast = head
        while fast != slow:
            slow = slow.next
            fast = fast.next
        return fast
```

##### 解法二
```python

```

### 148.排序链表

#### 题目描述

在 O(n log n) 时间复杂度和**常数级空间复杂度**（没法用递归）下，对链表进行排序。

示例 1:

输入: 4->2->1->3
输出: 1->2->3->4
示例 2:

输入: -1->5->3->4->0
输出: -1->0->3->4->5


#### 解法

##### 解法一 归并 利用了递归 不是常数级空间复杂度

```python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, x):
#         self.val = x
#         self.next = None

class Solution:
    def sortList(self, head: ListNode) -> ListNode:
        """
            归并
        """
        if not head or not head.next:
            return head
        
        # 1.计算链表长度
        length = 0
        p = head
        while p:
            p = p.next
            length += 1
        
        # 2.把原始链表平均拆分成两部分
        head1 = head
        head2 = head
        prev = None
        for _ in range(length//2):
            prev = head2
            head2 = head2.next
        prev.next = None
        
        # 3.把两部分链表递归排序
        head1 = self.sortList(head1)
        head2 = self.sortList(head2)
        
        dummy = ListNode(-1)
        p = dummy
        # 4.合并两个链表
        while head1 and head2:
            if head1.val < head2.val:
                p.next = head1
                head1 = head1.next
            else:
                p.next = head2
                head2 = head2.next
            p = p.next
        
        if not head1:
            p.next = head2
        if not head2:
            p.next = head1
        return dummy.next
```

##### 解法二 bottom-to-up归并 常数级空间

```python

```



### 160.相交链表

#### 160.1 题目描述

编写一个程序，找到两个单链表相交的起始节点。

如下面的两个链表**：**

[![img](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2018/12/14/160_statement.png)](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2018/12/14/160_statement.png)

在节点 c1 开始相交。

 

**示例 1：**

[![img](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2018/12/14/160_example_1.png)](https://assets.leetcode.com/uploads/2018/12/13/160_example_1.png)

```
输入：intersectVal = 8, listA = [4,1,8,4,5], listB = [5,0,1,8,4,5], skipA = 2, skipB = 3
输出：Reference of the node with value = 8
输入解释：相交节点的值为 8 （注意，如果两个列表相交则不能为 0）。从各自的表头开始算起，链表 A 为 [4,1,8,4,5]，链表 B 为 [5,0,1,8,4,5]。在 A 中，相交节点前有 2 个节点；在 B 中，相交节点前有 3 个节点。
```

 

**示例 2：**

[![img](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2018/12/14/160_example_2.png)](https://assets.leetcode.com/uploads/2018/12/13/160_example_2.png)

```
输入：intersectVal = 2, listA = [0,9,1,2,4], listB = [3,2,4], skipA = 3, skipB = 1
输出：Reference of the node with value = 2
输入解释：相交节点的值为 2 （注意，如果两个列表相交则不能为 0）。从各自的表头开始算起，链表 A 为 [0,9,1,2,4]，链表 B 为 [3,2,4]。在 A 中，相交节点前有 3 个节点；在 B 中，相交节点前有 1 个节点。
```

 

**示例 3：**

[![img](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2018/12/14/160_example_3.png)](https://assets.leetcode.com/uploads/2018/12/13/160_example_3.png)

```
输入：intersectVal = 0, listA = [2,6,4], listB = [1,5], skipA = 3, skipB = 2
输出：null
输入解释：从各自的表头开始算起，链表 A 为 [2,6,4]，链表 B 为 [1,5]。由于这两个链表不相交，所以 intersectVal 必须为 0，而 skipA 和 skipB 可以是任意值。
解释：这两个链表不相交，因此返回 null。
```

 

**注意：**

- 如果两个链表没有交点，返回 `null`.
- 在返回结果后，两个链表仍须保持原有的结构。
- 可假定整个链表结构中没有循环。
- 程序尽量满足 O(*n*) 时间复杂度，且仅用 O(*1*) 内存。

#### 160.2 解法

##### Approach 1: Brute Force

For each node ai in list A, traverse the entire list B and check if any node in list B coincides with ai.

**Complexity Analysis**

- Time complexity : O(mn)O
- Space complexity : O(1)

------

##### Approach 2: Hash Table

Traverse list A and store the address / reference to each node in a hash set. Then check every node bi in list B: if biappears in the hash set, then bi is the intersection node.

**Complexity Analysis**

- Time complexity : O(m+n)
- Space complexity : O(m) or O(n)

------

##### Approach 3: Two Pointers [todo]

- Maintain two pointers pA and pB initialized at the head of A and B, respectively. Then let them both traverse through the lists, one node at a time.
- When pA reaches the end of a list, then redirect it to the head of B (yes, B, that's right.); similarly when pB reaches the end of a list, redirect it the head of A.
- If at any point pA meets pB, then pA/pB is the intersection node.
- **To see why the above trick would work, consider the following two lists: A = {1,3,5,7,9,11} and B = {2,4,9,11}, which are intersected at node '9'. Since B.length (=4) < A.length (=6), pB would reach the end of the merged list first, because pB traverses exactly 2 nodes less than pA does. By redirecting pB to head A, and pA to head B, we now ask pB to travel exactly 2 more nodes than pA would. So in the second iteration, they are guaranteed to reach the intersection node at the same time.**
- If two lists have intersection, then their last nodes must be the same one. So when pA/pB reaches the end of a list, record the last element of A/B respectively. If the two last elements are not the same one, then the two lists have no intersections.

**Complexity Analysis**

- Time complexity : O(m+n)

- Space complexity : O(1)

  我发现大多数解决方案在这里预处理链接列表以获得len的差异。
  实际上我们并不关心差异的“价值”，我们只想确保两个指针同时到达交叉点节点。

  我们可以使用两次迭代来做到这一点。在第一次迭代中，我们将在到达尾节点之后将一个链表的指针重置到另一个链表的头部。在第二次迭代中，我们将移动两个指针，直到它们指向同一个节点。我们在第一次迭代中的操作将帮助我们抵消差异。因此，如果两个链表相交，则第二次迭代中的会合点必须是交点。如果两个链表完全没有交集，那么第二次迭代中的会议指针必须是两个列的尾节点，即null

```python
# Definition for singly-linked list.
# class ListNode(object):
#     def __init__(self, x):
#         self.val = x
#         self.next = None

class Solution(object):
    def getIntersectionNode(self, headA, headB):
        """
            1.计算每个链表长度, 计算链表长度差值, 长链表先走差值步 tO(m+n) sO(1)
            2.遍历一个链表, 放入map中, 另一个结点遍历判断是否在map中 tO(m+n) sO(m)
            3.在第一次迭代中，我们将在到达尾节点之后将一个链表的指针重置到另一个链表的头部。在第二次迭代中，我们将移动两个指针，直到它们指向同一个节点。 tO(m+n) sO(1)
        """
        if not headA or not headB:
            return
        pa = headA
        pb = headB
        while pa != pb:
            pa = pa.next if pa else headB
            pb = pb.next if pb else headA
        return pa
```



##### 方法四 [推荐]

计算两个链表长度 把长链表去掉长度差

```python
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
        思路1: set
        思路2: 计算两个链表长度 把长链表去掉长度差
        """
        if not headA or not headB:
            return None
        def cal_len(head):
            linked_len = 0
            while head:
                head = head.next
                linked_len = linked_len + 1
            return linked_len
        
        def move_n_forward(head, n):
            for i in range(n):
                head = head.next
            return head
        
        if not headA or not headB:
            return None
        lenA = cal_len(headA)
        lenB = cal_len(headB)
        if lenA < lenB:
            headB = move_n_forward(headB, lenB - lenA)
        else:
            headA = move_n_forward(headA, lenA - lenB)
        while headA and headB:
            if headA is headB:
                return headA
            headA = headA.next
            headB = headB.next
        return None
   
```



### 206.反转链表

#### 206.1 题目描述

反转一个单链表。

**示例:**

```
输入: 1->2->3->4->5->NULL
输出: 5->4->3->2->1->NULL
```

**进阶:**
你可以迭代或递归地反转链表。你能否用两种方法解决这道题？

#### 206.2 解法

##### 206.2.1 方法一  迭代

```python
class Solution(object):
    def reverseList(self, head):
        """
        :type head: ListNode
        :rtype: ListNode
        """
        new_head = None
        while head:
            tmp = head
            head = head.next
            tmp.next = new_head
            new_head = tmp
        return new_head
```

##### 206.2.2 方法二 递归

```java
public ListNode reverseList(ListNode head) {
    if (head == null || head.next == null) return head;
    ListNode p = reverseList(head.next);
    head.next.next = head;
    head.next = null;
    return p;
}
```

###234.回文链表

#### 题目描述

请判断一个链表是否为回文链表。

示例 1:

输入: 1->2
输出: false
示例 2:

输入: 1->2->2->1
输出: true
进阶：
你能否用 O(n) 时间复杂度和 O(1) 空间复杂度解决此题？


#### 解法

##### 解法一
```python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, x):
#         self.val = x
#         self.next = None

class Solution:
    def isPalindrome(self, head: ListNode) -> bool:
        """
            前半部分链表逆序
            之后两个指针分别指向开始和中间
            或者快慢指针找中点
        """
        # 1.计算链表长度
        p = head
        Len = 0
        while p:
            Len += 1
            p = p.next
        cnt = (Len + 1) // 2
        
        # 2.前半部分逆序
        p = head
        new_head = None
        while p and cnt:
            temp = p.next
            p.next = new_head
            new_head = p
            p = temp
            cnt -= 1

        # 3.对比两端链表
        if Len & 1:
            new_head = new_head.next
        while new_head and p and new_head.val == p.val:
            new_head = new_head.next
            p = p.next

        if not new_head and not p:
            return True
        return False
        
```

##### 解法二
```python

```

### 328.奇偶链表 [推荐]

#### 题目描述

给定一个单链表，把所有的奇数节点和偶数节点分别排在一起。请注意，这里的奇数节点和偶数节点指的是节点编号的奇偶性，而不是节点的值的奇偶性。

请尝试使用原地算法完成。你的算法的空间复杂度应为 O(1)，时间复杂度应为 O(nodes)，nodes 为节点总数。

示例 1:

输入: 1->2->3->4->5->NULL
输出: 1->3->5->2->4->NULL
示例 2:

输入: 2->1->3->5->6->4->7->NULL 
输出: 2->3->6->7->1->5->4->NULL
说明:

应当保持奇数节点和偶数节点的相对顺序。
链表的第一个节点视为奇数节点，第二个节点视为偶数节点，以此类推。


#### 解法

##### 解法一 双指针解法
```python
class Solution:
    def oddEvenList(self, head: ListNode) -> ListNode:
        if not head or not head.next:
            return head
        
        odd = head
        even_head = head.next
        even = even_head
        while even and even.next:
            odd.next = even.next
            odd = odd.next
            even.next = odd.next
            even = even.next
        odd.next = even_head
        return head
```

##### 解法二
```python

```

## 栈

### 155.最小栈

#### 题目描述

设计一个支持 push，pop，top 操作，并能在常数时间内检索到最小元素的栈。

push(x) -- 将元素 x 推入栈中。
pop() -- 删除栈顶的元素。
top() -- 获取栈顶元素。
getMin() -- 检索栈中的最小元素。
示例:

MinStack minStack = new MinStack();
minStack.push(-2);
minStack.push(0);
minStack.push(-3);
minStack.getMin();   --> 返回 -3.
minStack.pop();
minStack.top();      --> 返回 0.
minStack.getMin();   --> 返回 -2.

#### 解法

##### 解法一 辅助最小栈

```python
class MinStack:
    def __init__(self):
        """
        initialize your data structure here.
        """
        self.stack = []
        self.min_stack = []

    def push(self, x: int) -> None:
        self.stack.append(x)
        if not self.min_stack or x <= self.min_stack[-1]:
            self.min_stack.append(x)
        else:
            self.min_stack.append(self.min_stack[-1])

    def pop(self) -> None:
        self.stack.pop()
        self.min_stack.pop()

    def top(self) -> int:
        return self.stack[-1]

    def getMin(self) -> int:
        return self.min_stack[-1]

```

##### 解法二

```

```

###227.基本计算器II

#### 题目描述

实现一个基本的计算器来计算一个简单的字符串表达式的值。

字符串表达式仅包含非负整数，+， - ，*，/ 四种运算符和空格  。 整数除法仅保留整数部分。

示例 1:

输入: "3+2*2"
输出: 7
示例 2:

输入: " 3/2 "
输出: 1
示例 3:

输入: " 3+5 / 2 "
输出: 5
说明：

你可以假设所给定的表达式都是有效的。
请不要使用内置的库函数 eval。


#### 解法

##### 解法一
```python
class Solution:
    def calculate(self, s: str) -> int:
        """
            1.为什么会有空格
            2.stack
            3.乘除优先级高
            可以先按照符号 split
        """
        
        def is_number(c):
            return c >= '0' and c <= '9'
        
        stack = []
        sign_stack = []
        signs = {'+', '-', '*', '/'}
        ans = 0
        
        for i, c in enumerate(s):
            if c == ' ':
                continue
            elif c in signs:
                sign_stack.append(c)  
            else:

                if i > 0 and is_number(s[i - 1]):
                    stack[-1] = stack[-1] * 10 + int(c)
                else:
                    stack.append(int(c))
                if i + 1 < len(s) and is_number(s[i + 1]):
                    continue
                
                if sign_stack and sign_stack[-1] in {'*', '/'}:
                    num2 = stack.pop()
                    num1 = stack.pop()
                    op = sign_stack.pop()
                    if op == '*':
                        stack.append(num1 * num2)
                    else:
                        stack.append(num1 // num2)

        ans = stack[0]
        for i in range(len(sign_stack)):
            if sign_stack[i] == '+':
                ans += stack[i+1]
            elif sign_stack[i] == '-':
                ans -= stack[i+1]
        return ans

```

##### 解法二

先按照加减split, 剩下就剩乘法除法的先按次序算好了。再回到循环累减再累加就好了

```python
import re
class Solution:
    def calculate(self, s: str) -> int:
        """
            先加减split 再乘除
        """
        ss = re.split("[+-]", s)
        add_plus = re.sub("[^+-]", "", s)
        
        res = 0
        for i in range(len(ss)):
            sss = re.split("[*/]", ss[i])
            mul_div = re.sub("[^*/]", "", ss[i])
            c = 0
            for j in range(len(sss)):
                if j == 0:
                    c = int(sss[j])
                else:
                    if mul_div[j - 1] == '*':
                        c *= int(sss[j])
                    else:
                        c //= int(sss[j])
            if i == 0:
                res = c
            else:
                if add_plus[i - 1] == '+':
                    res += c
                else:
                    res -= c
        return res
```

### 232. 用栈实现队列

#### 232.1 题目描述

使用栈实现队列的下列操作：

- push(x) -- 将一个元素放入队列的尾部。
- pop() -- 从队列首部移除元素。
- peek() -- 返回队列首部的元素。
- empty() -- 返回队列是否为空。

**示例:**

```
MyQueue queue = new MyQueue();

queue.push(1);
queue.push(2);  
queue.peek();  // 返回 1
queue.pop();   // 返回 1
queue.empty(); // 返回 false
```

**说明:**

- 你只能使用标准的栈操作 -- 也就是只有 `push to top`, `peek/pop from top`, `size`, 和 `is empty` 操作是合法的。
- 你所使用的语言也许不支持栈。你可以使用 list 或者 deque（双端队列）来模拟一个栈，只要是标准的栈操作即可。
- 假设所有操作都是有效的 （例如，一个空的队列不会调用 pop 或者 peek 操作）。

#### 232.2 解法

```python
class MyQueue(object):

    def __init__(self):
        """
        Initialize your data structure here.
        """
        self.stack = []
        

    def push(self, x):
        """
        Push element x to the back of queue.
        :type x: int
        :rtype: None
        """
        tmp = self.stack[::-1]
        tmp.append(x)
        self.stack = tmp[::-1]

    def pop(self):
        """
        Removes the element from in front of queue and returns that element.
        :rtype: int
        假设所有操作都是有效的 （例如，一个空的队列不会调用 pop 或者 peek 操作）
        """
        return self.stack.pop()
        

    def peek(self):
        """
        Get the front element.
        :rtype: int
        假设所有操作都是有效的 （例如，一个空的队列不会调用 pop 或者 peek 操作）
        """
        return self.stack[-1]

    def empty(self):
        """
        Returns whether the queue is empty.
        :rtype: bool
        """
        return len(self.stack) == 0
        
# Your MyQueue object will be instantiated and called as such:
# obj = MyQueue()
# obj.push(x)
# param_2 = obj.pop()
# param_3 = obj.peek()
# param_4 = obj.empty()
```

```python
class MyQueue:

    def __init__(self):
        """
        Initialize your data structure here.
        """
        self.stack = []
        self.asstack = []
        

    def push(self, x: int) -> None:
        """
        Push element x to the back of queue.
        """
        while self.stack:
            self.asstack.append(self.stack.pop())
        self.stack.append(x)
        while self.asstack:
            self.stack.append(self.asstack.pop())
        

    def pop(self) -> int:
        """
        Removes the element from in front of queue and returns that element.
        """
        if self.stack:
            return self.stack.pop()
        

    def peek(self) -> int:
        """
        Get the front element.
        """
        if self.stack:
            return self.stack[-1]
        

    def empty(self) -> bool:
        """
        Returns whether the queue is empty.
        """
        return self.stack == []
        


# Your MyQueue object will be instantiated and called as such:
# obj = MyQueue()
# obj.push(x)
# param_2 = obj.pop()
# param_3 = obj.peek()
# param_4 = obj.empty()
```



## 二叉树

### 96.不同的二叉搜索树

#### 96.1.题目描述

给定一个整数 *n*，求以 1 ... *n* 为节点组成的二叉搜索树有多少种？

**示例:**

```
输入: 3
输出: 5
解释:
给定 n = 3, 一共有 5 种不同结构的二叉搜索树:

   1         3     3      2      1
    \       /     /      / \      \
     3     2     1      1   3      2
    /     /       \                 \
   2     1         2                 3
```

#### 96.2.解法
##### 96.2.1 方法一 DP

结题思路：假设n个节点存在二叉排序树的个数是G(n)，1为根节点，2为根节点，...，n为根节点，当1为根节点时，其左子树节点个数为0，右子树节点个数为n-1，同理当2为根节点时，其左子树节点个数为1，右子树节点为n-2，所以可得G(n) = G(0)*G(n-1)+G(1)*(n-2)+...+G(n-1)*G(0)

```python
class Solution:
    def numTrees(self, n: int) -> int:
        
        dp = [0] * (n+1)
        dp[0], dp[1] = 1, 1
        for i in range(2, n+1):
            for j in range(1, i+1):
                dp[i] += dp[j-1]*dp[i-j]
        return dp[n]
```



###98.验证二叉搜索树

#### 题目描述

给定一个二叉树，判断其是否是一个有效的二叉搜索树。

假设一个二叉搜索树具有如下特征：

节点的左子树只包含小于当前节点的数。
节点的右子树只包含大于当前节点的数。
所有左子树和右子树自身必须也是二叉搜索树。
示例 1:

输入:
    2
   / \
  1   3
输出: true
示例 2:

输入:
    5
   / \
  1   4
     / \
    3   6
输出: false
解释: 输入为: [5,1,4,null,null,3,6]。
     根节点的值为 5 ，但是其右子节点值为 4 。

#### 解法

##### 解法一 最小最大值辅助函数
```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None


class Solution:
    def isValidBST(self, root: TreeNode) -> bool:
        # 标记校验不通过的情况
        minValue = float('-inf')
        
        # 检验是否是二叉搜索树 若是返回最小最大值 若不是返回(minValue, minValue)
        def helper(root): 
            if not root:
                return None, None
            
            lmin, lmax = helper(root.left)
            if lmin == minValue:
                return minValue, minValue
            if lmin != None and lmax >= root.val:
                return minValue, minValue

            rmin, rmax = helper(root.right)
            if rmin == minValue:
                return minValue, minValue
            if rmin != None and rmin <= root.val:
                return minValue, minValue

            return lmin if lmin else root.val, rmax if rmax else root.val

            
        if not root:
            return True
        return helper(root) != (minValue, minValue)
```

##### 解法二 中序遍历

中序遍历 + 二叉搜索树 == 有序

```python
class Solution:
    def isValidBST(self, root: TreeNode) -> bool:
        def inorder(root):
            if root:
                inorder(root.left)
                arr.append(root.val)
                inorder(root.right)
                
        arr = []
        inorder(root)
        for i in range(1, len(arr)):
            if arr[i] <= arr[i-1]:
                return False
        return True
```

```python
  # 非递归版  
  def isValidBST(self, root: TreeNode) -> bool:
        arr = []
        stack = []
        while stack or root:
            if root:
                stack.append(root)
                root = root.left
            else:
                root = stack.pop()
                arr.append(root.val)
                root = root.right
                
        for i in range(1, len(arr)):
            if arr[i] <= arr[i-1]:
                return False
        return True
```

##### 解法三 区间验证

```python
class Solution:
    def isValidBST(self, root: TreeNode) -> bool:
        # 验证这个二叉平衡树(子树)的范围
        def isValidBST(root, minVal, maxVal):
            if not root:
                return True
            if root.val >= maxVal or root.val <= minVal:
                return False
            return isValidBST(root.left, minVal, root.val) and isValidBST(root.right, root.val, maxVal)
        
        return isValidBST(root, float('-inf'), float('inf'))
```

###102.二叉树的层次遍历

#### 题目描述

给定一个二叉树，返回其按层次遍历的节点值。 （即逐层地，从左到右访问所有节点）。

例如:
给定二叉树: [3,9,20,null,null,15,7],

    3
   / \
  9  20
    /  \
   15   7
返回其层次遍历结果：

[
  [3],
  [9,20],
  [15,7]
]


#### 解法

##### 解法一

层次遍历的改进 增加了树深度

```python
class Solution:
    def levelOrder(self, root: TreeNode) -> List[List[int]]:
        if not root:
            return []
        que = []
        que.append((root, 0))
        ans = []
        while que:
            node, depth = que[0]
            que = que[1:]
            if depth == len(ans):
                ans.append([])
            ans[depth].append(node.val)
            if node.left:
                que.append((node.left, depth+1))
            if node.right:
                que.append((node.right, depth+1))
        return ans
```

##### 解法二
```

```

### 103.二叉树的锯齿形层次遍历

#### 题目描述

给定一个二叉树，返回其节点值的锯齿形层次遍历。（即先从左往右，再从右往左进行下一层遍历，以此类推，层与层之间交替进行）。

例如：
给定二叉树 [3,9,20,null,null,15,7],

    3
   / \
  9  20
    /  \
   15   7
返回锯齿形层次遍历如下：

[
  [3],
  [20,9],
  [15,7]
]


#### 解法

##### 解法一
```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None
import queue
class Solution:
    def zigzagLevelOrder(self, root: TreeNode) -> List[List[int]]:
        """
            先层次遍历 在逆转奇数索引的数组
        """
        def reverse(lst):
            return lst[::-1]
        
        if not root:
            return []
        ans = []
        que = queue.Queue()
        que.put((root, 0))
        while not que.empty():
            root, ls = que.get()
            if ls == len(ans):
                ans.append([])
            ans[ls].append(root.val)
            if root.left:
                que.put((root.left, ls + 1))
            if root.right:
                que.put((root.right, ls + 1))
        # 逆转奇数索引数组
        for i in range(1, len(ans), 2):
            ans[i] = reverse(ans[i])
        return ans

```

##### 解法二
```java
class Solution {
    public List<List<Integer>> zigzagLevelOrder(TreeNode root) {
        List<List<Integer>> result = new ArrayList<>();
        if (root == null)
            return result;
        Queue<TreeNode> queue = new LinkedList<>();
        queue.add(root);
        // 记录是否反转
        boolean isReverse = false;
        while (!queue.isEmpty()) {
            LinkedList<Integer> oneLevel = new LinkedList<>();
            // 每次都取出一层的所有数据
            int count = queue.size();
            for (int i = 0; i < count; i++) {
                TreeNode node = queue.poll();
                if (!isReverse)
                    oneLevel.add(node.val);
                else
                    oneLevel.addFirst(node.val);
                if (node.left != null)
                    queue.add(node.left);
                if (node.right != null)
                    queue.add(node.right);
            }
            isReverse = !isReverse;
            result.add(oneLevel);
        }
        return result;
    }
}

```

###105.从前序与中序遍历序列构造二叉树

#### 题目描述

根据一棵树的前序遍历与中序遍历构造二叉树。

注意:
你可以假设树中没有重复的元素。

例如，给出

前序遍历 preorder = [3,9,20,15,7]
中序遍历 inorder = [9,3,15,20,7]
返回如下的二叉树： 

   3

   / \
  9  20
    /  \
   15   7

#### 解法

##### 解法一 递归
```python
class Solution(object):
    def buildTree(self, preorder, inorder):
        """
        :type preorder: List[int]
        :type inorder: List[int]
        :rtype: TreeNode
        """
        if len(inorder) == 0:
            return None
        # 前序遍历第一个值为根节点
        root = TreeNode(preorder[0])
        # 因为没有重复元素，所以可以直接根据值来查找根节点在中序遍历中的位置
        mid = inorder.index(preorder[0])
        # 构建左子树
        root.left = self.buildTree(preorder[1:mid+1], inorder[:mid])
        # 构建右子树
        root.right = self.buildTree(preorder[mid+1:], inorder[mid+1:])
        
        return root
```

##### 解法二
```

```

### [106. 从中序与后序遍历序列构造二叉树](https://leetcode-cn.com/problems/construct-binary-tree-from-inorder-and-postorder-traversal/)

#### 题目描述

根据一棵树的中序遍历与后序遍历构造二叉树。

注意:
你可以假设树中没有重复的元素。

例如，给出

中序遍历 inorder = [9,3,15,20,7]
后序遍历 postorder = [9,15,7,20,3]
返回如下的二叉树：

    3
   / \
  9  20
    /  \
   15   7


#### 解法

##### 解法一
```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None

class Solution:
    def buildTree(self, inorder: List[int], postorder: List[int]) -> TreeNode:
        if not inorder or not postorder:
            return None
        root = TreeNode(postorder[-1])
        mid = inorder.index(postorder[-1])
        root.left = self.buildTree(inorder[:mid], postorder[: mid])
        root.right = self.buildTree(inorder[mid + 1:], postorder[mid: -1])
        return root
```

##### 解法二
```python

```

### 110.平衡二叉树

#### 110.1 题目描述

给定一个二叉树，判断它是否是高度平衡的二叉树。

本题中，一棵高度平衡二叉树定义为：

> 一个二叉树*每个节点* 的左右两个子树的高度差的绝对值不超过1。

**示例 1:**

给定二叉树 `[3,9,20,null,null,15,7]`

```
    3
   / \
  9  20
    /  \
   15   7
```

返回 `true` 。

**示例 2:**

给定二叉树 `[1,2,2,3,3,null,null,4,4]`

```
       1
      / \
     2   2
    / \
   3   3
  / \
 4   4
```

返回 `false` 。

#### 110.2 解法

This problem is generally believed to have two solutions: the top down approach and the bottom up way.

##### 110.2.1 方法一 递归 自底向上

The second method is based on DFS. Instead of calling depth() explicitly for each child node, we return the height of the current node in DFS recursion. When the sub tree of the current node (inclusive) is balanced, the function dfsHeight() returns a non-negative value as the height. Otherwise -1 is returned. According to the leftHeight and rightHeight of the two children, the parent node could check if the sub tree
is balanced, and decides its return value.

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None

class Solution:
    def isBalanced(self, root: TreeNode) -> bool:
        def treeHelper(sub_node):
            """
                如果该子树是balanced_tree 返回树的高度
                否则返回-1                
            """
            if not sub_node:
                return 0
            left_height = treeHelper(sub_node.left)
            if left_height == -1:
                return -1
            right_height = treeHelper(sub_node.right)
            if right_height == -1:
                return -1
            return max(left_height, right_height)+1 if abs(left_height-right_height) <= 1 else -1
        return if treeHelper(root) != -1 
```

##### 110.2.2 递归 自顶向下

The first method checks whether the tree is balanced strictly according to the definition of balanced binary tree: the difference between the heights of the two sub trees are not bigger than 1, and both the left sub tree and right sub tree are also balanced. With the helper function depth(), we could easily write the code;

For the current node root, calling depth() for its left and right children actually has to access all of its children, thus the complexity is O(N). We do this for each node in the tree, so the overall complexity of isBalanced will be O(N^2)❌. This is a top down approach.

```c++
class solution {
public:
    int depth (TreeNode *root) {
        if (root == NULL) return 0;
        return max (depth(root -> left), depth (root -> right)) + 1;
    }

    bool isBalanced (TreeNode *root) {
        if (root == NULL) return true;
        
        int left=depth(root->left);
        int right=depth(root->right);
        
        return abs(left - right) <= 1 && isBalanced(root->left) && isBalanced(root->right);
    }
};
```

![image-20190326124540247](/Users/zhengxinzhi/typora_img/LeetCode题目总结/image-20190326124540247.png)

### 114. 二叉树展开为链表

#### 题目描述

给定一个二叉树，原地将它展开为链表。

例如，给定二叉树

​    1

   / \
  2   5
 / \   \
3   4   6
将其展开为：

1
 \
  2
   \
    3
     \
      4
       \
        5
         \
          6


#### 解法

##### 解法一 递归法
```python
class Solution:
    def flatten(self, root: TreeNode) -> None:
				if not root:
            return 
        right = root.right
        left = root.left
        
        self.flatten(root.left)
        self.flatten(right)
        root.right = left
        
        p = root
        while p.right:
            p = p.right
            
        p.right = right
        root.left = None
```

##### 解法二 前序遍历
###116. 填充每个节点的下一个右侧节点指针

#### 题目描述

给定一个完美二叉树，其所有叶子节点都在同一层，每个父节点都有两个子节点。二叉树定义如下：

struct Node {
  int val;
  Node *left;
  Node *right;
  Node *next;
}
填充它的每个 next 指针，让这个指针指向其下一个右侧节点。如果找不到下一个右侧节点，则将 next 指针设置为 NULL。

初始状态下，所有 next 指针都被设置为 NULL。

![img](/Users/zhengxinzhi/typora_img/LeetCode题目总结/116_sample.png)



输入：{"$id":"1","left":{"$id":"2","left":{"$id":"3","left":null,"next":null,"right":null,"val":4},"next":null,"right":{"$id":"4","left":null,"next":null,"right":null,"val":5},"val":2},"next":null,"right":{"$id":"5","left":{"$id":"6","left":null,"next":null,"right":null,"val":6},"next":null,"right":{"$id":"7","left":null,"next":null,"right":null,"val":7},"val":3},"val":1}

输出：{"$id":"1","left":{"$id":"2","left":{"$id":"3","left":null,"next":{"$id":"4","left":null,"next":{"$id":"5","left":null,"next":{"$id":"6","left":null,"next":null,"right":null,"val":7},"right":null,"val":6},"right":null,"val":5},"right":null,"val":4},"next":{"$id":"7","left":{"$ref":"5"},"next":null,"right":{"$ref":"6"},"val":3},"right":{"$ref":"4"},"val":2},"next":null,"right":{"$ref":"7"},"val":1}

解释：给定二叉树如图 A 所示，你的函数应该填充它的每个 next 指针，以指向其下一个右侧节点，如图 B 所示。


提示：

你只能使用常量级额外空间。
使用递归解题也符合要求，本题中递归程序占用的栈空间不算做额外的空间复杂度。


#### 解法

##### 解法一 BFS 未满足常量级辅助空间
```python
"""
# Definition for a Node.
class Node:
    def __init__(self, val, left, right, next):
        self.val = val
        self.left = left
        self.right = right
        self.next = next
"""
import queue
class Solution:
    def connect(self, root: 'Node') -> 'Node':
        """
            BFS遍历 存储上一个遍历的结点
        """
        if not root:
            return root
        que = queue.Queue()
        que.put((root, 0))
        last = None
        last_l = -1
        while not que.empty():
            node, ls = que.get()
            if ls != last_l:
                last_l = ls
            else:
                last.next = node
            last = node
                
            if node.left:
                que.put((node.left, ls + 1))
            if node.right:
                que.put((node.right, ls + 1))
        return root
        
```

##### 解法二 分治法
```c
"""
# Definition for a Node.
class Node:
    def __init__(self, val, left, right, next):
        self.val = val
        self.left = left
        self.right = right
        self.next = next
"""
class Solution:
    def connect(self, root: 'Node') -> 'Node':
        if not root:
            return None
            
        leftT = self.connect(root.left)
        rightT = self.connect(root.right)
        while leftT and rightT:
            leftT.next = rightT
            leftT = leftT.right if leftT.right else leftT.left
            rightT = rightT.left if rightT.left else rightT.right
        return root
```

##### 解法三 非递归法

```python
"""
# Definition for a Node.
class Node:
    def __init__(self, val, left, right, next):
        self.val = val
        self.left = left
        self.right = right
        self.next = next
"""
import queue
class Solution:
    def connect(self, root: 'Node') -> 'Node':
        # 注意题目完美二叉树 所有层都是满的
        layer_start_node = root
        while layer_start_node:
            p = layer_start_node
            # 处理一层的结点 从这一层第一个结点处理到最后一个结点
            while p:
                if p.left:
                    p.left.next = p.right
                if p.right and p.next:
                    p.right.next = p.next.left
                p = p.next
            # 跳到下一层
            layer_start_node = layer_start_node.left
        
        return root
```

### [117. 填充每个节点的下一个右侧节点指针 II](https://leetcode-cn.com/problems/populating-next-right-pointers-in-each-node-ii/)

#### 题目描述

给定一个二叉树

struct Node {
  int val;
  Node *left;
  Node *right;
  Node *next;
}
填充它的每个 next 指针，让这个指针指向其下一个右侧节点。如果找不到下一个右侧节点，则将 next 指针设置为 NULL。

初始状态下，所有 next 指针都被设置为 NULL。

 

示例：

![img](/Users/zhengxinzhi/typora_img/LeetCode题目总结/117_sample.png)

输入：{"$id":"1","left":{"$id":"2","left":{"$id":"3","left":null,"next":null,"right":null,"val":4},"next":null,"right":{"$id":"4","left":null,"next":null,"right":null,"val":5},"val":2},"next":null,"right":{"$id":"5","left":null,"next":null,"right":{"$id":"6","left":null,"next":null,"right":null,"val":7},"val":3},"val":1}

输出：{"$id":"1","left":{"$id":"2","left":{"$id":"3","left":null,"next":{"$id":"4","left":null,"next":{"$id":"5","left":null,"next":null,"right":null,"val":7},"right":null,"val":5},"right":null,"val":4},"next":{"$id":"6","left":null,"next":null,"right":{"$ref":"5"},"val":3},"right":{"$ref":"4"},"val":2},"next":null,"right":{"$ref":"6"},"val":1}

解释：给定二叉树如图 A 所示，你的函数应该填充它的每个 next 指针，以指向其下一个右侧节点，如图 B 所示。


提示：

你只能使用常量级额外空间。
使用递归解题也符合要求，本题中递归程序占用的栈空间不算做额外的空间复杂度。


#### 解法

##### 解法一 层次链表法
```python
"""
# Definition for a Node.
class Node:
    def __init__(self, val, left, right, next):
        self.val = val
        self.left = left
        self.right = right
        self.next = next
"""
class Solution:
    def connect(self, root: 'Node') -> 'Node':
        """
            层序遍历 但违反空间限制
            层序遍历变种 每层维护两个变量(相当于每层维护一个链表)
        """

        head = root
        while head:
            layer_head = Node(-1)
            layer_tail = layer_head
            node = head
            while node:
                if node.left:
                    layer_tail.next = node.left
                    layer_tail = layer_tail.next
                if node.right:
                    layer_tail.next = node.right
                    layer_tail = layer_tail.next
                node = node.next
            head = layer_head.next
        return root
        
```

##### 解法二
```python

```

###129.[ 求根到叶子节点数字之和](https://leetcode-cn.com/problems/sum-root-to-leaf-numbers/)

#### 题目描述

给定一个二叉树，它的每个结点都存放一个 0-9 的数字，每条从根到叶子节点的路径都代表一个数字。

例如，从根到叶子节点路径 1->2->3 代表数字 123。

计算从根到叶子节点生成的所有数字之和。

说明: 叶子节点是指没有子节点的节点。

示例 1:

输入: [1,2,3]
    1
   / \
  2   3
输出: 25
解释:
从根到叶子节点路径 1->2 代表数字 12.
从根到叶子节点路径 1->3 代表数字 13.
因此，数字总和 = 12 + 13 = 25.
示例 2:

输入: [4,9,0,5,1]
    4
   / \
  9   0
 / \
5   1
输出: 1026
解释:
从根到叶子节点路径 4->9->5 代表数字 495.
从根到叶子节点路径 4->9->1 代表数字 491.
从根到叶子节点路径 4->0 代表数字 40.
因此，数字总和 = 495 + 491 + 40 = 1026.


#### 解法

##### 解法一 dfs
```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None

class Solution:
    def sumNumbers(self, root: TreeNode) -> int:
        if not root:
            return 0
        stack = [(root, root.val)]
        res = []
        while stack:
            node, val = stack.pop()
            if not node.left and not node.right:
                res.append(val)
            if node.left:
                stack.append((node.left, val * 10 + node.left.val))
            if node.right:
                stack.append((node.right, val * 10 + node.right.val))
                
        return sum(res)
```

##### 解法二
```python

```

### 144.二叉树的前序遍历

#### 144.1 题目描述

给定一个二叉树，返回它的 *前序* 遍历。

 **示例:**

```
输入: [1,null,2,3]  
   1
    \
     2
    /
   3 

输出: [1,2,3]
```

**进阶:** 递归算法很简单，你可以通过迭代算法完成吗？

#### 144.2 解法

前序遍历：根节点->左子树->右子树

中序遍历：左子树->根节点->右子树

后序遍历：左子树->右子树->根节点

##### 144.2.1 递归

```python
    def preorderTraversal(self, root: TreeNode) -> List[int]:  
        def treeHelper(sub_root, res):
            if not sub_root:
                return 
            res.append(sub_root.val)
            treeHelper(sub_root.left, res)
            treeHelper(sub_root.right, res)
            
        res = []
        treeHelper(root, res)
        return res
```

##### 144.2.2 非递归

```python
    def preorderTraversal(self, root: TreeNode) -> List[int]: 
        if not root:
            return []
        stack = [root]
        res = []
        while stack:
            node = stack.pop()
            res.append(node.val)
            # 先右后左
            if node.right:
                stack.append(node.right)
            if node.left:
                stack.append(node.left)
        return res
```

##### 144.2.3 非递归方法二

先遍历左子树 在遍历右子树

```python
    def preorderTraversal(self, root: TreeNode) -> List[int]: 
        stack = []
        res = []
        while stack or root:
            if root:
                res.append(root.val)
                stack.append(root)
                root = root.left
            else:
                root = stack.pop()
                root = root.right
        return res
```

### [173. 二叉搜索树迭代器](https://leetcode-cn.com/problems/binary-search-tree-iterator/)

#### 题目描述

实现一个二叉搜索树迭代器。你将使用二叉搜索树的根节点初始化迭代器。

调用 next() 将返回二叉搜索树中的下一个最小的数。

 

示例：



BSTIterator iterator = new BSTIterator(root);
iterator.next();    // 返回 3
iterator.next();    // 返回 7
iterator.hasNext(); // 返回 true
iterator.next();    // 返回 9
iterator.hasNext(); // 返回 true
iterator.next();    // 返回 15
iterator.hasNext(); // 返回 true
iterator.next();    // 返回 20
iterator.hasNext(); // 返回 false


提示：

next() 和 hasNext() 操作的时间复杂度是 O(1)，并使用 O(h) 内存，其中 h 是树的高度。
你可以假设 next() 调用总是有效的，也就是说，当调用 next() 时，BST 中至少存在一个下一个最小的数。


#### 解法

##### 解法一
```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None

class BSTIterator:
    # 见到二叉搜索树想中序遍历
    def __init__(self, root: TreeNode):
        self.stack = []
        while root:
            self.stack.append(root)
            root = root.left

    def next(self) -> int:
        """
        @return the next smallest number
        """

        root = self.stack.pop()
        res = root.val
        root = root.right
        while root:
            self.stack.append(root)
            root = root.left
        return res
        

    def hasNext(self) -> bool:
        """
        @return whether we have a next smallest number
        """
        return len(self.stack) != 0
        


# Your BSTIterator object will be instantiated and called as such:
# obj = BSTIterator(root)
# param_1 = obj.next()
# param_2 = obj.hasNext()

```

但是很多小伙伴会对next()中的循环操作的复杂度感到疑惑，认为既然加入了循环在里面，那时间复杂度肯定是大于O(1)不满足题目要求的。

仔细分析一下，该循环只有在节点有右子树的时候才需要进行，也就是不是每一次操作都需要循环的，循环的次数加上初始化的循环总共会有O(n)次操作，均摊到每一次next()的话平均时间复杂度则是O(n)/n=O(1)，因此可以确定该实现方式满足O(1)的要求。

这种分析方式称为摊还分析，详细的学习可以看看**《算法导论》- 第17章 摊还分析**

##### 解法二

```python

```

### [199. 二叉树的右视图](https://leetcode-cn.com/problems/binary-tree-right-side-view/)

#### 题目描述

给定一棵二叉树，想象自己站在它的右侧，按照从顶部到底部的顺序，返回从右侧所能看到的节点值。

示例:

输入: [1,2,3,null,5,null,4]
输出: [1, 3, 4]
解释:

   1            <---
 /   \
2     3         <---
 \     \
  5     4       <---


#### 解法

##### 解法一 层次遍历
```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None
import queue
class Solution:
    def rightSideView(self, root: TreeNode) -> List[int]:
        if not root:
            return []
        que = queue.Queue()
        que.put((root, 0))
        last = -1
        res = []
        while not que.empty():
            root, d = que.get()
 
            if d != last:
                res.append(root.val)
                last = d
            if root.right:
                que.put((root.right, d + 1))
            if root.left:
                que.put((root.left, d + 1))

        return res
        
```

##### 解法二 dfs
```python
class Solution(object):
    def rightSideView(self, root):
        rightmost_value_at_depth = dict() # depth -> node.val
        max_depth = -1

        stack = [(root, 0)]
        while stack:
            node, depth = stack.pop()

            if node is not None:
                # maintain knowledge of the number of levels in the tree.
                max_depth = max(max_depth, depth)

                # only insert into dict if depth is not already present.
                rightmost_value_at_depth.setdefault(depth, node.val)

                stack.append((node.left, depth+1))
                stack.append((node.right, depth+1))

        return [rightmost_value_at_depth[depth] for depth in range(max_depth+1)]

```

### 208. 实现 Trie (前缀树)

#### 208.1 题目描述

实现一个 Trie (前缀树)，包含 `insert`, `search`, 和 `startsWith` 这三个操作。

**示例:**

```
Trie trie = new Trie();

trie.insert("apple");
trie.search("apple");   // 返回 true
trie.search("app");     // 返回 false
trie.startsWith("app"); // 返回 true
trie.insert("app");   
trie.search("app");     // 返回 true
```

**说明:**

- 你可以假设所有的输入都是由小写字母 `a-z` 构成的。
- 保证所有输入均为非空字符串。

#### 208.2 解法

##### 208.1 方法一 字典建树

```python
class Trie:

    def __init__(self):
        """
        Initialize your data structure here.
        """
        self.root = dict()
        

    def insert(self, word: str) -> None:
        """
        Inserts a word into the trie.
        """
        node = self.root
        for c in word:
            if c in node:
                node = node[c]
            else:
                node[c] = dict()
                node = node[c]
        node['is_word'] = True
        
              
    def search(self, word: str) -> bool:
        """
        Returns if the word is in the trie.
        """
        node = self.root
        for c in word:
            if c in node:
                node = node[c]
            else:
                return False
        return 'is_word' in node

    
    def startsWith(self, prefix: str) -> bool:
        """
        Returns if there is any word in the trie that starts with the given prefix.
        """
        node = self.root
        for c in prefix:
            if c not in node:
                return False
            else:
                node = node[c]
        return True


# Your Trie object will be instantiated and called as such:
# obj = Trie()
# obj.insert(word)
# param_2 = obj.search(word)
# param_3 = obj.startsWith(prefix)
```

### 230. 二叉搜索树中第K小的元素

#### 230.1 题目描述   

给定一个二叉搜索树，编写一个函数 `kthSmallest` 来查找其中第 **k** 个最小的元素。

**说明：**
你可以假设 k 总是有效的，1 ≤ k ≤ 二叉搜索树元素个数。

**示例 1:**

```
输入: root = [3,1,4,null,2], k = 1
   3
  / \
 1   4
  \
   2
输出: 1
```

**示例 2:**

```
输入: root = [5,3,6,2,4,null,null,1], k = 3
       5
      / \
     3   6
    / \
   2   4
  /
 1
输出: 3
```

**进阶：**
如果二叉搜索树经常被修改（插入/删除操作）并且你需要频繁地查找第 k 小的值，你将如何优化 `kthSmallest` 函数？

#### 230.2 解法

##### 230.2.1 方法一 中序遍历 递归

```python
class Solution:
    def kthSmallest(self, root: TreeNode, k: int) -> int:
        """
            中序遍历, 遍历到第k个元素时停止
        """
        def treeHelper(sub_root, res):
            if not sub_root:
                return 
            treeHelper(sub_root.left, res)
            res.append(sub_root.val)
            treeHelper(sub_root.right, res)
        
        res = []
        treeHelper(root, res)
        return res[k-1]
```

##### 230.2.1 方法二 方法一改进版

```python
    def kthSmallest(self, root: TreeNode, k: int) -> int:
        """
            中序遍历, 遍历到第k个元素时停止
        """
        def treeHelper(sub_root, res):
            if not sub_root:
                return 
            treeHelper(sub_root.left, res)
            if len(res) == k:
                return
            res.append(sub_root.val)
            treeHelper(sub_root.right, res)
        
        res = []
        treeHelper(root, res)
        return res[k-1]
```

##### 230.2.3 方法三 中序遍历 非递归

```python
    def kthSmallest(self, root: TreeNode, k:int) -> int:
        stack = []
        while stack or root:
            if root:
                stack.append(root)
                root = root.left
            else:
                root = stack.pop()
                k = k-1
                if not k:
                    return root.val
                root = root.right
```

##### 230.2.4 方法四 二分搜索

```python
    def kthSmallest(self, root: TreeNode, k:int) -> int:
        """
            二分搜索
        """
        def cnt_nodes(sub_root):
            if not sub_root:
                return 0
            return cnt_nodes(sub_root.left) + cnt_nodes(sub_root.right) + 1
        
        left_num = cnt_nodes(root.left)
        if k == left_num + 1:
            return root.val
        elif k > left_num + 1:
            return self.kthSmallest(root.right, k - 1 - left_num)
        else:
            return self.kthSmallest(root.left, k)
```

##### 230.2.5 方法五 yield

```python
class Solution:
    # @param {TreeNode} root
    # @param {integer} k
    # @return {integer}
    def kthSmallest(self, root, k):
        for val in self.inorder(root):
            if k == 1:
                return val
            else:
                k -= 1
        
    def inorder(self, root):
        if root is not None:
            for val in self.inorder(root.left):
                yield val
            yield root.val
            for val in self.inorder(root.right):
                yield val
```

###437.路径总和III

#### 题目描述

给定一个二叉树，它的每个结点都存放着一个整数值。

找出路径和等于给定数值的路径总数。

路径不需要从根节点开始，也不需要在叶子节点结束，但是路径方向必须是向下的（只能从父节点到子节点）。

二叉树不超过1000个节点，且节点数值范围是 [-1000000,1000000] 的整数。

示例：

root = [10,5,-3,3,2,null,11,3,-2,null,1], sum = 8

      10
     /  \
    5   -3
   / \    \
  3   2   11
 / \   \
3  -2   1

返回 3。和等于 8 的路径有:

1.  5 -> 3
2.  5 -> 2 -> 1
3.  -3 -> 11


#### 解法

##### 解法一 双递归法
```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None

class Solution:
    def pathSum(self, root: TreeNode, sum: int) -> int:
        """
            边界条件: 负数 0 
            前缀和
            递归
        """
        def helper(root, sum):
            """
                包括根节点的指定路径数量
            """
            if not root:
                return 0

            return helper(root.left, sum - root.val) + helper(root.right, sum - root.val) + (1 if sum == root.val else 0)
        
        if not root:
            return 0
        
        return self.pathSum(root.left, sum) + self.pathSum(root.right, sum) + helper(root, sum)
```

##### 解法二
```python

```

### 513. 找树左下角的值

#### 513.1 题目描述

给定一个二叉树，在树的最后一行找到最左边的值。

**示例 1:**

```
输入:

    2
   / \
  1   3

输出:
1
```

 

**示例 2:**

```
输入:

        1
       / \
      2   3
     /   / \
    4   5   6
       /
      7

输出:
7
```

 

**注意:** 您可以假设树（即给定的根节点）不为 **NULL**。

#### 513.2 解法

##### 513.2.1 方法一 递归自底向上

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None

class Solution:
    def findBottomLeftValue(self, root: TreeNode) -> int:
        def treeHelper(sub_root):
            """
                返回子树的高度以及左下角的值
            """
            if not sub_root:
                return 0, None
            left_height, left_val = treeHelper(sub_root.left)
            right_height, right_val = treeHelper(sub_root.right)
            if not left_val and not right_val:
                return 1, sub_root.val
            return max(left_height, right_height)+1, right_val if right_height>left_height else left_val
        
        return treeHelper(root)[1]
```

##### 513.2.2 BFS

```python
def findLeftMostNode(self, root):
    queue = [root]
    for node in queue:
        # queue += filter(None, (node.right, node.left))
        if node.right:
          queue += node.right
        if node.left:
        	queue += node.left
    return node.val
```

##### 513.2.3 DFS

```c++
class Solution {
public:
    int findBottomLeftValue(TreeNode* root) {
        int bottomLeft = 0;
        int height = 0;
        dfs(root, 1, height, bottomLeft);
        return bottomLeft;
    }

private:
    void dfs(TreeNode* node, int depth, int& height, int& res) {
        if (!node) {
            return;
        }
        if (depth > height) {
            res = node->val;    // update res only when redefine the height
            height = depth;
        }
        dfs(node->left, depth + 1, height, res);
        dfs(node->right, depth + 1, height, res);
    }
};
```

##### 513.2.4 DFS + stack

```python
#DFS + stack   

    def findBottomLeftValue(self, root):
        if not root:
            return
        max_depth = 0
        stack = [(root, 1)]
         
        while stack:
            curr, level = stack.pop()
            if curr:
                if level > max_depth:
                    max_depth = level
                    ans = curr.val
                stack.append((curr.right, level + 1))
                stack.append((curr.left, level + 1))
        return ans
```

###538.[把二叉搜索树转换为累加树](https://leetcode-cn.com/problems/convert-bst-to-greater-tree/)

#### 题目描述

给定一个二叉搜索树（Binary Search Tree），把它转换成为累加树（Greater Tree)，使得每个节点的值是原来的节点值加上所有大于它的节点值之和。

例如：

输入: 二叉搜索树:
              5
            /   \
           2     13

输出: 转换为累加树:
             18
            /   \
          20     13


#### 解法

##### 解法一 逆中序遍历

        """
            边界条件 :相等的点 不要累加
            二叉搜索树 左 < 中 < 右
            中序遍历 左 -> 中 -> 右
            逆中序思路 右 -> 中 -> 左
            递归版
```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None

class Solution:
    def convertBST(self, root: TreeNode) -> TreeNode:
        """
            边界条件 :相等的点 不要累加
            二叉搜索树 左 < 中 < 右
            中序遍历 左 -> 中 -> 右
            逆中序思路 右 -> 中 -> 左
            递归版
        """
        p = root
        stack = []
        cum = 0
        last_v = 0
        while stack or p:
            if p:
                stack.append(p)
                p = p.right
            else:
                p = stack.pop()
                if p.val == last_v:
                    p.val = cum
                else:
                    p.val += cum
                    cum = p.val
                p = p.left
        return root
    
```

##### 解法二
```python

```

###543.[二叉树的直径](https://leetcode-cn.com/problems/diameter-of-binary-tree/)

#### 题目描述

给定一棵二叉树，你需要计算它的直径长度。一棵二叉树的直径长度是任意两个结点路径长度中的最大值。这条路径可能穿过根结点。

示例 :
给定二叉树

          1
         / \
        2   3
       / \     
      4   5    
返回 3, 它的长度是路径 [4,2,1,3] 或者 [5,2,1,3]。

注意：两结点之间的路径长度是以它们之间边的数目表示。


#### 解法

##### 解法一 双重递归

计算左子树的最大直径 计算右子树的最大直径 (计算左子树的深度 - 1 + 右子树的深度 - 1 + 2) 三者最大值为结果 

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None

class Solution:
    def diameterOfBinaryTree(self, root: TreeNode) -> int:
        """
            递归
        """
        def getDepth(root):
            if not root:
                return 0
            return max(getDepth(root.left), getDepth(root.right)) + 1
            
            
        if not root:
            return 0
        leftmax = self.diameterOfBinaryTree(root.left)
        rightmax = self.diameterOfBinaryTree(root.right)
        return max(getDepth(root.left) + getDepth(root.right), leftmax, rightmax)
        
```

##### 解法二 递归

计算深度的过程中把所有子树的最大直径作比较 找到最大的

```python
class Solution:
    def diameterOfBinaryTree(self, root: TreeNode) -> int:
        self.ans = 1
        def depth(node):
            if not node:
                return 0
            L = depth(node.left)
            R = depth(node.right)
            self.ans = max(self.ans, L + R + 1)
            return max(L, R) + 1
        depth(root)
        return self.ans - 1
```

###617.合并二叉树

#### 题目描述

给定两个二叉树，想象当你将它们中的一个覆盖到另一个上时，两个二叉树的一些节点便会重叠。

你需要将他们合并为一个新的二叉树。合并的规则是如果两个节点重叠，那么将他们的值相加作为节点合并后的新值，否则不为 NULL 的节点将直接作为新二叉树的节点。

示例 1:

输入: 
	Tree 1                     Tree 2                  
          1                         2                             
         / \                       / \                            
        3   2                     1   3                        
       /                           \   \                      
      5                             4   7                  
输出: 
合并后的树:
	     3
	    / \
	   4   5
	  / \   \ 
	 5   4   7
注意: 合并必须从两个树的根节点开始。


#### 解法

##### 解法一 递归

- Time complexity : O(m)*O*(*m*). A total of m*m* nodes need to be traversed. Here, m*m* represents the minimum number of nodes from the two given trees.
- Space complexity : O(m)*O*(*m*). The depth of the recursion tree can go upto m*m* in the case of a skewed tree. In average case, depth will be O(logm)*O*(*l**o**g**m*).

```python
class Solution:
    def mergeTrees(self, t1: TreeNode, t2: TreeNode) -> TreeNode:
        if not t1:
            return t2
        if not t2:
            return t1
        head = TreeNode(t1.val + t2.val)
        head.left = self.mergeTrees(t1.left, t2.left)
        head.right = self.mergeTrees(t1.right, t2.right)
        return head
```

##### 解法二 dfs

- Time complexity : O(n)*O*(*n*). We traverse over a total of n*n* nodes. Here, n*n* refers to the smaller of the number of nodes in the two trees.
- Space complexity : O(n)*O*(*n*). The depth of stack can grow upto n*n* in case of a skewed tree.

```python
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode(int x) { val = x; }
 * }
 */
public class Solution {
    public TreeNode mergeTrees(TreeNode t1, TreeNode t2) {
        if (t1 == null)
            return t2;
        Stack < TreeNode[] > stack = new Stack < > ();
        stack.push(new TreeNode[] {t1, t2});
        while (!stack.isEmpty()) {
            TreeNode[] t = stack.pop();
            if (t[0] == null || t[1] == null) {
                continue;
            }
            t[0].val += t[1].val;
            if (t[0].left == null) {
                t[0].left = t[1].left;
            } else {
                stack.push(new TreeNode[] {t[0].left, t[1].left});
            }
            if (t[0].right == null) {
                t[0].right = t[1].right;
            } else {
                stack.push(new TreeNode[] {t[0].right, t[1].right});
            }
        }
        return t1;
    }
}

```

## 图

### 图的常用解法

BFS DFS 拓扑排序 并查集 黑白染色法 最短路径 一笔画问题(欧拉回路 欧拉路径)

拓扑排序可以解决有向无环图 找图中的环



### 127.单词接龙

#### 题目描述

给定两个单词（beginWord 和 endWord）和一个字典，找到从 beginWord 到 endWord 的最短转换序列的长度。转换需遵循如下规则：

每次转换只能改变一个字母。
转换过程中的中间单词必须是字典中的单词。
说明:

如果不存在这样的转换序列，返回 0。
所有单词具有相同的长度。
所有单词只由小写字母组成。 
字典中不存在重复的单词。
你可以假设 beginWord 和 endWord 是非空的，且二者不相同。
示例 1:

输入:
beginWord = "hit",
endWord = "cog",
wordList = ["hot","dot","dog","lot","log","cog"]

输出: 5

解释: 一个最短转换序列是 "hit" -> "hot" -> "dot" -> "dog" -> "cog",
     返回它的长度 5。
示例 2:

输入:
beginWord = "hit"
endWord = "cog"
wordList = ["hot","dot","dog","lot","log"]

输出: 0

解释: endWord "cog" 不在字典中，所以无法进行转换。

#### 解法

##### 解法一 BFS

```
        1.把只差一个字母的单词相连，可形成一个关于word的图，用BFS遍历
        2.对wordList做预处理，生成一个key为某一元素被*替代的通用形式，value是符合通用形式的具体的单词
```

```python
import queue
class Solution:
    def ladderLength(self, beginWord: str, endWord: str, wordList: List[str]) -> int:
        """
            1.把只差一个字母的单词相连，可形成一个关于word的图，用BFS遍历
            2.对wordList做预处理，生成一个key为某一元素被*替代的通用形式，value是符合通用形式的具体的单词
        """
        # wordList预处理
        word_dic = dict()
        word_len = len(beginWord)
        for word in wordList:
            for i in range(word_len):
                key = word[:i] + '*' + word[i + 1:]
                if key in word_dic:
                    word_dic[key].append(word)
                else:
                    word_dic[key] = [word]
            
        # BFS
        visited = {beginWord}
        que = queue.Queue()
        que.put((beginWord, 1))
        while not que.empty():
            node, dist = que.get()
            # 通过key找到相邻的结点遍历
            for i in range(word_len):
                key = node[:i] + '*' + node[i + 1:]
                if key not in word_dic:
                    continue
                for tail in word_dic[key]:
                    if tail == endWord:
                        return dist + 1
                    if tail not in visited:
                        que.put((tail, dist + 1))
                        visited.add(tail)
                                  
        return 0
```

时间复杂度：O(M×N)，其中 M 是单词的长度 N 是单词表中单词的总数。找到所有的变换需要对每个单词做 MM次操作。同时，最坏情况下广度优先搜索也要访问所有的 N 个单词。
空间复杂度：O(M×N)，要在 all_combo_dict 字典中记录每个单词的 M 个通用状态。访问数组的大小是 NN。广搜队列最坏情况下需要存储 NN 个单词。

##### 解法二 双端BFS

根据给定字典构造的图可能会很大，而广度优先搜索的搜索空间大小依赖于每层节点的分支数量。假如每个节点的分支数量相同，搜索空间会随着层数的增长指数级的增加。考虑一个简单的二叉树，每一层都是满二叉树的扩展，节点的数量会以 2 为底数呈指数增长。

如果使用两个同时进行的广搜可以有效地减少搜索空间。一边从 beginWord 开始，另一边从 endWord 开始。我们每次从两边各扩展一个节点，当发现某一时刻两边都访问了某一顶点时就停止搜索。这就是双向广度优先搜索，它可以可观地减少搜索空间大小，从而降低时间和空间复杂度。

![Word_Ladder_3.png](/Users/zhengxinzhi/typora_img/LeetCode%E9%A2%98%E7%9B%AE%E6%80%BB%E7%BB%93/be92086801e264f49bb1c01593dbfee5b08e52c600b62576c5fa0c1ef2d54eb8-Word_Ladder_3.png)

算法

1. 算法与之前描述的标准广搜方法相类似。
2. 唯一的不同是我们从两个节点同时开始搜索，同时搜索的结束条件也有所变化。
3. 我们现在有两个访问数组，分别记录从对应的起点是否已经访问了该节点。
4. 如果我们发现一个节点被两个搜索同时访问，就结束搜索过程。因为我们找到了双向搜索的交点。过程如同从中间相遇而不是沿着搜索路径一直走。
5. 双向搜索的结束条件是找到一个单词被两边搜索都访问过了。
6. 最短变换序列的长度就是中间节点在两边的层次之和。因此，我们可以在访问数组中记录节点的层次。



```python
from collections import defaultdict
class Solution(object):
    def __init__(self):
        self.length = 0
        # Dictionary to hold combination of words that can be formed,
        # from any given word. By changing one letter at a time.
        self.all_combo_dict = defaultdict(list)

    def visitWordNode(self, queue, visited, others_visited):
        current_word, level = queue.pop(0)
        for i in range(self.length):
            # Intermediate words for current word
            intermediate_word = current_word[:i] + "*" + current_word[i+1:]

            # Next states are all the words which share the same intermediate state.
            for word in self.all_combo_dict[intermediate_word]:
                # If the intermediate state/word has already been visited from the
                # other parallel traversal this means we have found the answer.
                if word in others_visited:
                    return level + others_visited[word]
                if word not in visited:
                    # Save the level as the value of the dictionary, to save number of hops.
                    visited[word] = level + 1
                    queue.append((word, level + 1))
        return None

    def ladderLength(self, beginWord, endWord, wordList):
        """
        :type beginWord: str
        :type endWord: str
        :type wordList: List[str]
        :rtype: int
        """

        if endWord not in wordList or not endWord or not beginWord or not wordList:
            return 0

        # Since all words are of same length.
        self.length = len(beginWord)

        for word in wordList:
            for i in range(self.length):
                # Key is the generic word
                # Value is a list of words which have the same intermediate generic word.
                self.all_combo_dict[word[:i] + "*" + word[i+1:]].append(word)


        # Queues for birdirectional BFS
        queue_begin = [(beginWord, 1)] # BFS starting from beginWord
        queue_end = [(endWord, 1)] # BFS starting from endWord

        # Visited to make sure we don't repeat processing same word
        visited_begin = {beginWord: 1}
        visited_end = {endWord: 1}
        ans = None

        # We do a birdirectional search starting one pointer from begin
        # word and one pointer from end word. Hopping one by one.
        while queue_begin and queue_end:

            # One hop from begin word
            ans = self.visitWordNode(queue_begin, visited_begin, visited_end)
            if ans:
                return ans
            # One hop from end word
            ans = self.visitWordNode(queue_end, visited_end, visited_begin)
            if ans:
                return ans

        return 0

```

时间复杂度：O(M \times N)O(M×N)，其中 MM 是单词的长度 NN 是单词表中单词的总数。与单向搜索相同的是，找到所有的变换需要 M * NM∗N 次操作。但是搜索时间会被缩小一半，因为两个搜索会在中间某处相遇。
空间复杂度：O(M \times N)O(M×N)，要在 all_combo_dict 字典中记录每个单词的 MM 个通用状态，这与单向搜索相同。但是因为会在中间相遇，所以双向搜索的搜索空间变小。

###133. 克隆图

#### 题目描述

给定无向连通图中一个节点的引用，返回该图的深拷贝（克隆）。图中的每个节点都包含它的值 val（Int） 和其邻居的列表（list[Node]）。

示例：

![img](/Users/zhengxinzhi/typora_img/LeetCode题目总结/113_sample.png)

输入：
{"$id":"1","neighbors":[{"$id":"2","neighbors":[{"$ref":"1"},{"$id":"3","neighbors":[{"$ref":"2"},{"$id":"4","neighbors":[{"$ref":"3"},{"$ref":"1"}],"val":4}],"val":3}],"val":2},{"$ref":"4"}],"val":1}

解释：
节点 1 的值是 1，它有两个邻居：节点 2 和 4 。
节点 2 的值是 2，它有两个邻居：节点 1 和 3 。
节点 3 的值是 3，它有两个邻居：节点 2 和 4 。
节点 4 的值是 4，它有两个邻居：节点 1 和 3 。


#### 解法

##### 解法一 DFS
```python
"""
# Definition for a Node.
class Node:
    def __init__(self, val, neighbors):
        self.val = val
        self.neighbors = neighbors
"""
class Solution:
    def cloneGraph(self, node: 'Node') -> 'Node':
        copy = Node(node.val, []) 
        stack = [node]
        cp_stack = [copy]
        visited = {node.val: copy}
        
        while stack:
            v = stack.pop()
            cp_v = cp_stack.pop()
            for adj_v in v.neighbors:
                if adj_v.val not in visited:
                    stack.append(adj_v)
                    cp_adj_v = Node(adj_v.val, [])
                    visited[adj_v.val] = cp_adj_v
                    cp_v.neighbors.append(cp_adj_v)
                    cp_stack.append(cp_adj_v)
                else:
                    cp_v.neighbors.append(visited[adj_v.val])
        return copy
```

##### 解法二
```java
/*
// Definition for a Node.
class Node {
    public int val;
    public List<Node> neighbors;

    public Node() {}

    public Node(int _val,List<Node> _neighbors) {
        val = _val;
        neighbors = _neighbors;
    }
};
*/

```

###207.课程表

#### 题目描述

现在你总共有 n 门课需要选，记为 0 到 n-1。

在选修某些课程之前需要一些先修课程。 例如，想要学习课程 0 ，你需要先完成课程 1 ，我们用一个匹配来表示他们: [0,1]

给定课程总量以及它们的先决条件，判断是否可能完成所有课程的学习？

示例 1:

输入: 2, [[1,0]] 
输出: true
解释: 总共有 2 门课程。学习课程 1 之前，你需要完成课程 0。所以这是可能的。
示例 2:

输入: 2, [[1,0],[0,1]]
输出: false
解释: 总共有 2 门课程。学习课程 1 之前，你需要先完成课程 0；并且学习课程 0 之前，你还应先完成课程 1。这是不可能的。
说明:

输入的先决条件是由边缘列表表示的图形，而不是邻接矩阵。详情请参见图的表示法。
你可以假定输入的先决条件中没有重复的边。
提示:

这个问题相当于查找一个循环是否存在于有向图中。如果存在循环，则不存在拓扑排序，因此不可能选取所有课程进行学习。
通过 DFS 进行拓扑排序 - 一个关于Coursera的精彩视频教程（21分钟），介绍拓扑排序的基本概念。
拓扑排序也可以通过 BFS 完成。


#### 解法

##### 解法一 拓扑排序
```python
import queue
class Graph:
    def __init__(self, N):
        self.V = N
        # 邻接表
        self.adj_list = [[] for _ in range(N)]
        # 维持入度为0的结点队列
        self.que = queue.Queue()
        # 入度
        self.indegree = [0] * N
        
        
    def add_path(self, s, t):
        self.adj_list[s].append(t)
        self.indegree[t] += 1
    
    
    def topological_sort(self):
        # 1.把所有入度为0的点入队
        for i, d in enumerate(self.indegree):
            if d == 0:
                self.que.put(i)
        
        # 2.拓扑排序
        count = 0
        while not self.que.empty():
            v = self.que.get()
            count += 1
            for adj_v in self.adj_list[v]:
                self.indegree[adj_v] -= 1
                if self.indegree[adj_v] == 0:
                    self.que.put(adj_v)
        # 3. 判断环
        if count < self.V:
            return False
        else:
            return True
        
class Solution:
    
    def canFinish(self, a: int, prerequisites: List[List[int]]) -> bool:
        """
            拓扑排序
            寻找没有入度的点 DFS
            判断遍历所有点之后是否会存在循环
        """
        g = Graph(numCourses)
        for t, s in prerequisites:
            g.add_path(s, t)
    
        return g.topological_sort()
    
 
```

```python
import queue
class Solution:
    def canFinish(self, numCourses: int, prerequisites: List[List[int]]) -> bool:
        # 拓扑排序
        # todo change
c
        
        # 1.构建图及保存入度
        graph = [[] for _ in range(N)]
        indegree = [0] * N
        for t, s in nums:
            indegree[t] += 1
            graph[s].append(t)
            
        # 2.拓扑排序开始之前，先把所有入度为 0 的结点加入到一个队列中
        zero_que = queue.Queue()
        for i, d in enumerate(indegree):
            if d == 0:
                zero_que.put(i)
                
        # 3.拓扑排序 BFS逐步消除入度为0的结点
        cnt = 0
        while not zero_que.empty():
            node = zero_que.get()
            cnt += 1
            for adj_v in graph[node]:
                indegree[adj_v] -= 1
                if indegree[adj_v] == 0:
                    zero_que.put(adj_v)
        
        return cnt == N
```

##### 解法二  推荐

```python
import queue
class Solution:
    def canFinish(self, numCourses: int, prerequisites: List[List[int]]) -> bool:
        # 拓扑排序
        # todo change
        N = numCourses
        nums = prerequisites
        
        # 1.构建图及保存入度
        graph = [[] for _ in range(N)]
        indegree = [0] * N
        for t, s in nums:
            indegree[t] += 1
            graph[s].append(t)
            
        # 2.拓扑排序开始之前，先把所有入度为 0 的结点加入到一个队列中
        zero_que = queue.Queue()
        for i, d in enumerate(indegree):
            if d == 0:
                zero_que.put(i)
                
        # 3.拓扑排序 BFS逐步消除入度为0的结点
        cnt = 0
        while not zero_que.empty():
            node = zero_que.get()
            cnt += 1
            for adj_v in graph[node]:
                indegree[adj_v] -= 1
                if indegree[adj_v] == 0:
                    zero_que.put(adj_v)
        
        return cnt == N
```

###210.课程表II

#### 题目描述

现在你总共有 n 门课需要选，记为 0 到 n-1。

在选修某些课程之前需要一些先修课程。 例如，想要学习课程 0 ，你需要先完成课程 1 ，我们用一个匹配来表示他们: [0,1]

给定课程总量以及它们的先决条件，返回你为了学完所有课程所安排的学习顺序。

可能会有多个正确的顺序，你只要返回一种就可以了。如果不可能完成所有课程，返回一个空数组。

示例 1:

输入: 2, [[1,0]] 
输出: [0,1]
解释: 总共有 2 门课程。要学习课程 1，你需要先完成课程 0。因此，正确的课程顺序为 [0,1] 。
示例 2:

输入: 4, [[1,0],[2,0],[3,1],[3,2]]
输出: [0,1,2,3] or [0,2,1,3]
解释: 总共有 4 门课程。要学习课程 3，你应该先完成课程 1 和课程 2。并且课程 1 和课程 2 都应该排在课程 0 之后。
     因此，一个正确的课程顺序是 [0,1,2,3] 。另一个正确的排序是 [0,2,1,3] 。
说明:

输入的先决条件是由边缘列表表示的图形，而不是邻接矩阵。详情请参见图的表示法。
你可以假定输入的先决条件中没有重复的边。
提示:

这个问题相当于查找一个循环是否存在于有向图中。如果存在循环，则不存在拓扑排序，因此不可能选取所有课程进行学习。
通过 DFS 进行拓扑排序 - 一个关于Coursera的精彩视频教程（21分钟），介绍拓扑排序的基本概念。
拓扑排序也可以通过 BFS 完成


#### 解法 拓扑排序

##### 解法一 O(n)
```python
import queue
class Graph:
    def __init__(self, N):
        self.V = N
        self.adj_list = [[] for _ in range(N)]
        self.que = queue.Queue()
        self.indegree = [0] * N
        
        
    def add_path(self, s, t):
        self.adj_list[s].append(t)
        self.indegree[t] += 1
    
    
    def topological_sort(self):
        # 1.把所有入度为0的点入队
        for i, d in enumerate(self.indegree):
            if d == 0:
                self.que.put(i)
        
        # 2.拓扑排序
        orders = []
        count = 0
        while not self.que.empty():
            v = self.que.get()
            count += 1
            orders.append(v)
            for adj_v in self.adj_list[v]:
                self.indegree[adj_v] -= 1
                if self.indegree[adj_v] == 0:
                    self.que.put(adj_v)
        # 3. 判断环
        if count < self.V:
            return []
        else:
            return orders
        
class Solution:
    def findOrder(self, numCourses: int, prerequisites: List[List[int]]) -> List[int]:
        g = Graph(numCourses)
        for t, s in prerequisites:
            g.add_path(s, t)
            
        return g.topological_sort()
```

##### 解法二 推荐
```python
import queue
class Solution:
    def findOrder(self, numCourses: int, prerequisites: List[List[int]]) -> List[int]:
        N = numCourses
        nums = prerequisites
        # 1.初始化图 & 记录入度
        graph = [[] for _ in range(N)]
        indegree = [0] * N
        for t, s in nums:
            graph[s].append(t)
            indegree[t] += 1

            
        # 2.找到入度为0的结点放入零入度队列中
        que = queue.Queue()
        for i, d in enumerate(indegree):
            if d == 0:
                que.put(i)
    
        # 3. BFS topo排序
        res = []
        cnt = 0
        while not que.empty():
            node = que.get()
            cnt += 1
            res.append(node)
            for adj_v in graph[node]:
                indegree[adj_v] -= 1
                if indegree[adj_v] == 0:
                    que.put(adj_v)
                    
        return res if cnt == N else []
```

###332.重新安排行程

#### 题目描述

给定一个机票的字符串二维数组 [from, to]，子数组中的两个成员分别表示飞机出发和降落的机场地点，对该行程进行重新规划排序。所有这些机票都属于一个从JFK（肯尼迪国际机场）出发的先生，所以该行程必须从 JFK 出发。

说明:

如果存在多种有效的行程，你可以按字符自然排序返回最小的行程组合。例如，行程 ["JFK", "LGA"] 与 ["JFK", "LGB"] 相比就更小，排序更靠前
所有的机场都用三个大写字母表示（机场代码）。
假定所有机票至少存在一种合理的行程。
示例 1:

输入: [["MUC", "LHR"], ["JFK", "MUC"], ["SFO", "SJC"], ["LHR", "SFO"]]
输出: ["JFK", "MUC", "LHR", "SFO", "SJC"]
示例 2:

输入: [["JFK","SFO"],["JFK","ATL"],["SFO","ATL"],["ATL","JFK"],["ATL","SFO"]]
输出: ["JFK","ATL","JFK","SFO","ATL","SFO"]
解释: 另一种有效的行程是 ["JFK","SFO","ATL","JFK","ATL","SFO"]。但是它自然排序更大更靠后。


#### 解法

##### 解法一 DFS
```python
class Solution:
    def findItinerary(self, tickets: List[List[str]]) -> List[str]:
        """
            边界条件：可能有环
                dfs过程中可能有思路 走不通回溯
        """
        def DFS(s, path, paths):
            if len(path) == N:
                path.append(s)
                return path
            
            for v in g[s]:
                if (s, v) in paths and paths[(s, v)] != 0:
                    paths[(s, v)] -= 1
                    ans = DFS(v, path + [s], paths)
                    if ans:
                        return ans
                    paths[(s, v)] += 1
                    
                
        # 1.构建图
        g = dict()
        paths = collections.defaultdict(int)
        for s, t in tickets:
            g.setdefault(s, [])
            g.setdefault(t, [])
            g[s].append(t)
            paths[(s, t)] += 1
            
        
        for k, v in g.items():
            g[k] = sorted(v)
            
        N = len(tickets) 
        
        return DFS("JFK", [], paths)
```

##### 解法二
```java
import java.util.*;
class Solution {
    public List findItinerary(List> tickets) {
        // 因为逆序插入，所以用链表
        List ans = new LinkedList<>();
        if (tickets == null || tickets.size() == 0)
            return ans;
        Map<> graph = new HashMap<>();
        for (List pair : tickets) {
            // 因为涉及删除操作，我们用链表
            PriorityQueue nbr = graph.computeIfAbsent(pair.get(0), k -> new PriorityQueue<>());
            nbr.add(pair.get(1));
        }
        visit(graph, "JFK", ans);
        return ans;
    }
    // DFS方式遍历图，当走到不能走为止，再将节点加入到答案
    private void visit(Map> graph, String src, List ans) {
        PriorityQueue nbr = graph.get(src);
        while (nbr != null && nbr.size() > 0) {
            String dest = nbr.poll();
            visit(graph, dest, ans);
        }
        ans.add(0, src); // 逆序插入
    }
}

```

### 785.判断二分图

#### 785.1.题目描述

给定一个无向图`graph`，当这个图为二分图时返回`true`。

如果我们能将一个图的节点集合分割成两个独立的子集A和B，并使图中的每一条边的两个节点一个来自A集合，一个来自B集合，我们就将这个图称为二分图。

`graph`将会以邻接表方式给出，`graph[i]`表示图中与节点`i`相连的所有节点。每个节点都是一个在`0`到`graph.length-1`之间的整数。这图中没有自环和平行边： `graph[i]` 中不存在`i`，并且`graph[i]`中没有重复的值。

```
示例 1:
输入: [[1,3], [0,2], [1,3], [0,2]]
输出: true
解释: 
无向图如下:
0----1
|    |
|    |
3----2
我们可以将节点分成两组: {0, 2} 和 {1, 3}。
示例 2:
输入: [[1,2,3], [0,2], [0,1,3], [0,2]]
输出: false
解释: 
无向图如下:
0----1
| \  |
|  \ |
3----2
我们不能将节点分割成两个独立的子集。
```

**注意:**

- `graph` 的长度范围为 `[1, 100]`。
- `graph[i]` 中的元素的范围为 `[0, graph.length - 1]`。
- `graph[i]` 不会包含 `i` 或者有重复的值。
- 图是无向的: 如果`j` 在 `graph[i]`里边, 那么 `i` 也会在 `graph[j]`里边。

#### 785.2.解法

##### 785.2.1 方法一 黑白染色法

`Our goal`正在尝试使用两种颜色为图形着色，并查看是否有任何相邻的节点具有相同的颜色。
为每个节点初始化一个color []数组。这是`colors[]`数组的三种状态：
`0: Haven't been colored yet.`
`1: Blue.`
`-1: Red.`
对于每个节点，

1. 如果尚未着色，请使用一种颜色对其进行着色。然后使用另一种颜色为其所有相邻节点（DFS）着色。
2. 如果已着色，请检查当前颜色是否与将用于着色的颜色相同

```python
class Solution:
    def isBipartite(self, graph: List[List[int]]) -> bool:
        color = {}
        
        def dfs(i):
            for adj in graph[i]:
                if adj in color:
                    if color[adj] == color[i]:
                        return False
                else:
                    color[adj] = -color[i]
                    if not dfs(adj):
                        return False
            return True
                    
        # his graph might be a disconnected graph. So check each unvisited node.
        for i in range(len(graph)):
            if i not in color:
                color[i] = 1
                if not dfs(i):
                    return False
        return True
```



```java
class Solution {
    public boolean isBipartite(int[][] graph) {
        int n = graph.length;
        int[] colors = new int[n];			
				
        for (int i = 0; i < n; i++) {              //This graph might be a disconnected graph. So check each unvisited node.
            if (colors[i] == 0 && !validColor(graph, colors, 1, i)) {
                return false;
            }
        }
        return true;
    }
    
    public boolean validColor(int[][] graph, int[] colors, int color, int node) {
        if (colors[node] != 0) {
            return colors[node] == color;
        }       
        colors[node] = color;       
        for (int next : graph[node]) {
            if (!validColor(graph, colors, -color, next)) {
                return false;
            }
        }
        return true;
    }
}
```



##### 785.2.2 方法二

### 274.H指数

#### 274.1.题目描述

给定一位研究者论文被引用次数的数组（被引用次数是非负整数）。编写一个方法，计算出研究者的 *h* 指数。

[h 指数的定义](https://baike.baidu.com/item/h-index/3991452?fr=aladdin): “h 代表“高引用次数”（high citations），一名科研人员的 h 指数是指他的轮种有 h 篇论文分别被引用了**至少** h 次，其余的 *N - h* 篇论文每篇被引用次数**小于** *h* 次

A scientist has index *h* if *h* of his/her *N* papers have **at least** *h* citations each, and the other *N − h* papers have **no more than** *h* citations each

**示例:**

```
输入: citations = [3,0,6,1,5]
输出: 3 
解释: 给定数组表示研究者总共有 5 篇论文，每篇论文相应的被引用了 3, 0, 6, 1, 5 次。
     由于研究者有 3 篇论文每篇至少被引用了 3 次，其余两篇论文每篇被引用不多于 3 次，所以她的 h 指数是 3。
```

**说明:** 如果 *h* 有多种可能的值，*h* 指数是其中最大的那个。

#### 274.2.解法
##### 274.2.1 方法一

```python
class Solution:
    def hIndex(self, citations: List[int]) -> int:
        for h in range(len(citations), -1, -1):
            cnt = 0
            for num in citations:
                if num < h:
                    cnt += 1
            if cnt <= len(citations) - h:
                return h
        return 0
```



##### 274.2.2 方法二

```python
class Solution:
    def hIndex(self, citations: List[int]) -> int:
        citations.sort()
        for i, num in enumerate(citations):
            if len(citations) - i <= num:
                return len(citations) - i
        return 0
```

```java
class NumArray {

    int[] tree;
    int n;
    public NumArray(int[] nums) {
        if (nums.length > 0) {
            n = nums.length;
            tree = new int[n * 2];
            buildTree(nums);
        }
    }
    private void buildTree(int[] nums) {
        for (int i = n, j = 0;  i < 2 * n; i++,  j++)
            tree[i] = nums[j];
        for (int i = n - 1; i > 0; --i)
            tree[i] = tree[i * 2] + tree[i * 2 + 1];
    }
    
    void update(int pos, int val) {
        pos += n;
        tree[pos] = val;
        while (pos > 0) {
            int left = pos;
            int right = pos;
            if (pos % 2 == 0) {
                right = pos + 1;
            } else {
                left = pos - 1;
            }
            // parent is updated after child is updated
            tree[pos / 2] = tree[left] + tree[right];
            pos /= 2;
        }
    }

    public int sumRange(int l, int r) {
        // get leaf with value 'l'
        l += n;
        // get leaf with value 'r'
        r += n;
        int sum = 0;
        while (l <= r) {
            if ((l % 2) == 1) {
            sum += tree[l];
            l++;
            }
            if ((r % 2) == 0) {
            sum += tree[r];
            r--;
            }
            l /= 2;
            r /= 2;
        }
        return sum;
    }

}

/**
 * Your NumArray object will be instantiated and called as such:
 * NumArray obj = new NumArray(nums);
 * obj.update(i,val);
 * int param_2 = obj.sumRange(i,j);
 */
```



###399.除法求值 

#### 题目描述

给出方程式 A / B = k, 其中 A 和 B 均为代表字符串的变量， k 是一个浮点型数字。根据已知方程式求解问题，并返回计算结果。如果结果不存在，则返回 -1.0。

示例 :
给定 a / b = 2.0, b / c = 3.0
问题: a / c = ?, b / a = ?, a / e = ?, a / a = ?, x / x = ? 
返回 [6.0, 0.5, -1.0, 1.0, -1.0 ]

输入为: vector<pair<string, string>> equations, vector<double>& values, vector<pair<string, string>> queries(方程式，方程式结果，问题方程式)， 其中 equations.size() == values.size()，即方程式的长度与方程式结果长度相等（程式与结果一一对应），并且结果值均为正数。以上为方程式的描述。 返回vector<double>类型。

基于上述例子，输入如下：

equations(方程式) = [ ["a", "b"], ["b", "c"] ],
values(方程式结果) = [2.0, 3.0],
queries(问题方程式) = [ ["a", "c"], ["b", "a"], ["a", "e"], ["a", "a"], ["x", "x"] ]. 
输入总是有效的。你可以假设除法运算中不会出现除数为0的情况，且不存在任何矛盾的结果。


#### 解法

##### 解法一
```python
import queue
class Solution(object):
    def calcEquation(self, equations, values, queries):
        g = collections.defaultdict(dict)
        for [s, t], v in zip(equations, values):
            g[s][t] = v
            g[t][s] = 1 / v
            g[s][s] = 1.0
            g[t][t] = 1.0
        
        def find_path(s, t):
            if s not in g or t not in g:
                return -1.
 
            que = queue.Queue()
            que.put((s, 1.0))
            visited = set()
            while not que.empty():
                v, cur_product = que.get()
                if v == t:
                    return cur_product
                visited.add(v)
                for adj_v, val in g[v].items():
                    if adj_v not in visited:
                        # 这里是乘法
                        que.put((adj_v, cur_product * val))
            
            return -1.0
        

        return [find_path(s, t) for s, t in queries]
```

##### 解法二 预先建立所有的连接

陷阱 顺序不一样有可能导致不能建立所有的连接

```python
class Solution:
    def calcEquation(self, equations, values, queries):
        path_w = collections.defaultdict(dict)
        for (num1, num2), val in zip(equations, values):
            path_w[num1][num1] = 1.0
            path_w[num2][num2] = 1.0
            path_w[num1][num2] = val
            path_w[num2][num1] = 1 / val
        # for k, i, j in itertools.permutations(quot, 3):
        for k in path_w:
            for i in path_w[k]:
                for j in path_w[k]:
                    path_w[i][j] = path_w[i][k] * path_w[k][j]

        return [path_w[num1].get(num2, -1.0) for num1, num2 in queries]
```

### 684.冗余连接

#### 题目描述

在本问题中, 树指的是一个连通且无环的无向图。

输入一个图，该图由一个有着N个节点 (节点值不重复1, 2, ..., N) 的树及一条附加的边构成。附加的边的两个顶点包含在1到N中间，这条附加的边不属于树中已存在的边。

结果图是一个以边组成的二维数组。每一个边的元素是一对[u, v] ，满足 u < v，表示连接顶点u 和v的无向图的边。

返回一条可以删去的边，使得结果图是一个有着N个节点的树。如果有多个答案，则返回二维数组中最后出现的边。答案边 [u, v] 应满足相同的格式 u < v。

示例 1：

输入: [[1,2], [1,3], [2,3]]
输出: [2,3]
解释: 给定的无向图为:
  1
 / \
2 - 3
示例 2：

输入: [[1,2], [2,3], [3,4], [1,4], [1,5]]
输出: [1,4]
解释: 给定的无向图为:
5 - 1 - 2
    |   |
    4 - 3
注意:

输入的二维数组大小在 3 到 1000。
二维数组中的整数在1到N之间，其中N是输入数组的大小。


#### 解法

##### 解法一 拓扑排序

```python
import queue
class Solution:
    def findRedundantConnection(self, edges: List[List[int]]) -> List[int]:
        """
            拓扑排序(无向图)
        """
        # 1.初始化图及度
        graph = [[] for _ in range(len(edges))]
        degree = [0] * len(edges)
        for s, t in edges:
            graph[s - 1].append(t - 1) 
            graph[t - 1].append(s - 1)
            degree[s - 1] += 1
            degree[t - 1] += 1

        # 2.找到度为1的点
        que = queue.Queue()
        for i, d in enumerate(degree):
            if d == 1:
                print(i)
                que.put(i)
        
        # 3.拓扑排序
        visited = set()
        while not que.empty():
            node = que.get()
            visited.add(node)
            for adj in graph[node]:
                degree[node] -= 1
                degree[adj] -= 1
                if degree[adj] == 1:
                    que.put(adj)
        not_visited = set(range(len(edges))) - visited

        for s, t in edges[::-1]:
            if s - 1 in not_visited and t - 1 in not_visited:
                return [s, t]
    
```



```python
# 拓扑排序 无向图 每一论去掉度为1的点
import queue
class Graph:
    def __init__(self, N):
        """ 无向图的类拓扑排序
            V: 顶点数
            node_list: 邻接表
            que: 度为1的顶点的集合
            degree: 记录每个顶点的度
        """
        self.V = N
        self.adj_list = [[] for _ in range(N)]
        self.que = queue.Queue()
        self.degree = [0] * N
        self.visited = set()
        self.not_visited = set()

    def addEdge(self, s, t):
        self.adj_list[s].append(t)
        self.adj_list[t].append(s)
        self.degree[s] += 1
        self.degree[t] += 1

    # O(n + e)
    def topological_sort(self):
        # 1.将所有入度为1的点入队
        for i in range(self.V):
            if self.degree[i] == 1:
                self.que.put(i)

        # 2.计数，记录当前已经输出的顶点数
        count = 0
        self.visited = set()
        while not self.que.empty():
            v = self.que.get()
            self.visited.add(v)
            count += 1
            # 2.1 将所有v指向的顶点度减一，同时把度为1的点入队列
            for adj_v in self.adj_list[v]:
                self.degree[adj_v] -= 1
                if self.degree[adj_v] == 1:
                    self.que.put(adj_v)

        # 3.未浏览过的结点是环中结点
        self.not_visited = set(range(self.V)) - self.visited

        # 4.如果没有全部输出顶点，代表图中有环路
        if count < self.V:
            return False
        else:
            return True

class Solution:
    def findRedundantConnection(self, edges: List[List[int]]) -> List[int]:
        """
            题目可转变为找环，删掉环的任意一个边
            
            法一：并查集
            
            法二：拓扑排序
        """
        # 1.建邻接表
        g = Graph(len(edges))
        for s, t in edges:
            g.addEdge(s - 1, t - 1)
        
        g.topological_sort()
        # print(g.not_visited)
        for s, t in edges[::-1]:
            if s - 1 in g.not_visited and t - 1 in g.not_visited:
                return [s, t]


```

##### 解法二 并查集

```python
class DFU:
    def __init__(self, N):
        self.subsets = list(range(N))
        # 集合中元素数量或深度
        self.rank = [1] * N
    
    def find(self, x):
        if self.subsets[x] != x:
            self.subsets[x] = self.find(self.subsets[x])
        return self.subsets[x]
        
    def union(self, x, y):
        xp = self.find(x)
        yp = self.find(y)
        if xp != yp:
            if self.rank[x] <= self.rank[y]:
                self.subsets[xp] = yp
            elif self.rank[x] > self.rank[y]:
                self.subsets[yp] = xp
            self.rank[x] = self.rank[y] = self.rank[x] + self.rank[y]

    
class Solution:
    def findRedundantConnection(self, edges: List[List[int]]) -> List[int]:
        """
            拓扑排序(无向图)
            并查集
        """
        N = len(edges)
        dfu = DFU(N)
        
        for x, y in edges:
            if dfu.find(x - 1) == dfu.find(y - 1):
                return [x, y]
            else:
                dfu.union(x - 1, y - 1)
```

###743.网络延迟时间 [推荐 最短路径]

#### 题目描述

有 N 个网络节点，标记为 1 到 N。

给定一个列表 times，表示信号经过有向边的传递时间。 times[i] = (u, v, w)，其中 u 是源节点，v 是目标节点， w 是一个信号从源节点传递到目标节点的时间。

现在，我们向当前的节点 K 发送了一个信号。需要多久才能使所有节点都收到信号？如果不能使所有节点收到信号，返回 -1。

注意:

N 的范围在 [1, 100] 之间。
K 的范围在 [1, N] 之间。
times 的长度在 [1, 6000] 之间。
所有的边 times[i] = (u, v, w) 都有 1 <= u, v <= N 且 0 <= w <= 100。

#### 解法

最短路径问题

##### 解法一 每次找最短的距离进行遍历 优先级队列 O(E log N)

```python
class Solution:
    def networkDelayTime(self, times: List[List[int]], N: int, K: int) -> int:
        # 1.构建图
        graph = collections.defaultdict(dict)
        for u, v, w in times:
            graph[u][v] = w
            
        heap = [(0, K)]
        visited = set()
        res = -1
        # 2.最短路径问题 遍历 每次找距源点最短的距离遍历
        while heap:
            w, v = heapq.heappop(heap)
            if v not in visited:
                visited.add(v)
                res = w
                for adj in graph[v]:
                    heapq.heappush(heap, (w + graph[v][adj], adj))
        return res if len(visited) == N else -1
```

##### 解法二 DFS brute 较慢
```python
class Solution(object):
    def networkDelayTime(self, times, N, K):
        g = collections.defaultdict(list)
        for u, v, w in times:
            g[u].append((w, v))
        
        dist = {v: float('inf') for v in range(1, N + 1)}
        
        def dfs(v, elapsed):
            if elapsed >= dist[v]:
                return
            dist[v] = elapsed
            # key sorted
            for w, adj_v in sorted(g[v]):
                dfs(adj_v, elapsed + w)
                
        dfs(K, 0)
        ans = max(dist.values())
        return ans if ans < float('inf') else -1
  

```

##### 解法三 Bellman-Ford算法 O(NE)

```python
class Solution(object):
    def networkDelayTime(self, times, N, K):
        
        def bellman_ford(edges, s):
            for _ in range(N - 1):
                for u, v, w in edges:
                    dist[v] = min(dist[v], dist[u] + w)
            

        dist = {v: float('inf') for v in range(1, N + 1)}
        dist[K] = 0
        bellman_ford(times, K)
        max_d = max(dist.values()) 
        return max_d if max_d != float('inf') else -1
  


```



###997.找到小镇的法官

#### 题目描述


#### 解法

##### 解法一 入度出度

找到入读为N-1出度为0的点

```python
class Solution:
    def findJudge(self, N: int, trust: List[List[int]]) -> int:
        indegree = [0] * N
        outdegree = [0] * N
        for s, t in trust:
            indegree[t - 1] += 1
            outdegree[s - 1] += 1
            
        for i in range(N):
            if outdegree[i] == 0 and indegree[i] == N - 1 :
                return i + 1
        return -1
```

##### 解法二 优化

Consider `trust` as a graph, all pairs are directed edge.
The point with `in-degree - out-degree = N - 1` become the judge

**Explanation**:
Count the degree, and check at the end

**Time Complexity**:
Time `O(T + N)`, space `O(N)`

```python
class Solution:
    def findJudge(self, N, trust):
        count = [0] * (N + 1)
        for i, j in trust:
            count[i] -= 1
            count[j] += 1
        for i in range(1, N + 1):
            if count[i] == N - 1:
                return i
        return -1
```

### 1042. 不邻接植花 [推荐]

#### 题目描述

有 N 个花园，按从 1 到 N 标记。在每个花园中，你打算种下四种花之一。

paths[i] = [x, y] 描述了花园 x 到花园 y 的双向路径。

另外，没有花园有 3 条以上的路径可以进入或者离开。

你需要为每个花园选择一种花，使得通过路径相连的任何两个花园中的花的种类互不相同。

以数组形式返回选择的方案作为答案 answer，其中 answer[i] 为在第 (i+1) 个花园中种植的花的种类。花的种类用  1, 2, 3, 4 表示。保证存在答案。

 

示例 1：

输入：N = 3, paths = [[1,2],[2,3],[3,1]]
输出：[1,2,3]
示例 2：

输入：N = 4, paths = [[1,2],[3,4]]
输出：[1,2,1,2]
示例 3：

输入：N = 4, paths = [[1,2],[2,3],[3,4],[4,1],[1,3],[2,4]]
输出：[1,2,3,4]


提示：

1 <= N <= 10000
0 <= paths.size <= 20000
不存在花园有 4 条或者更多路径可以进入或离开。
保证存在答案。


#### 解法

##### 解法一 邻接表 + 遍历
```python
class Solution:
    def gardenNoAdj(self, N: int, paths: List[List[int]]) -> List[int]:
        
        graph = [[] for _ in range(N)]
        for x, y in paths:
            graph[x - 1].append(y - 1)
            graph[y - 1].append(x - 1)
            
        res = [0] * N
        for i in range(N):
            left = {1, 2, 3, 4}
            for j in graph[i]:
                left = left - {res[j]}
            res[i] = left.pop()
        return res
        
```

##### 解法二
```python

```

## 动规

### [01背包问题](https://www.acwing.com/problem/content/2/)

#### 题目描述

有 $N$ 件物品和一个容量是 $V$ 的背包。每件物品只能使用一次。

第 $i$ 件物品的体积是 $v_i$，价值是 $w_i$。

求解将哪些物品装入背包，可使这些物品的总体积不超过背包容量，且总价值最大。
输出最大价值。

#### 输入格式

第一行两个整数，$N，V$，用空格隔开，分别表示物品数量和背包容积。

接下来有 $N$ 行，每行两个整数 $v_i, w_i$，用空格隔开，分别表示第 $i$ 件物品的体积和价值。

#### 输出格式

输出一个整数，表示最大价值。

#### 数据范围

$0 \lt N, V \le 1000$
$0\lt v_i, w_i \le 1000$

#### 输入样例

```
4 5
1 2
2 4
3 4
4 5
```

#### 输出样例：

```
8
```


#### 解法

##### 解法一 dp

```c++
/*
f[i][j]: 只放前i个物品, 总体积是j的情况下总价值最大是多少

result = max(f[n][0~V])

f[i][j] = max(f[i-1][j], f[i-1][j-[i]] + w[i])

f[0][0] = 0
*/
#include <iostream>
#include <cstring>
#include <algorithm>

using namespace std;

const int N = 1010;

int n, m;
int f[N][N];
int v[N], w[N];

int main()
{
    cin >> n >> m;
    for (int i = 1; i <= n; i++) cin >> v[i] >> w[i];
    
    for (int i = 1; i <= n; i++) 
        for (int j = 0; j <= m; j++)
        {   
            f[i][j] = f[i-1][j];
            if (j >= v[i])
                f[i][j] = max(f[i][j], f[i-1][j-v[i]] + w[i]);
        }
    
    cout << f[n][m] << endl;
    return 0;
}
```

##### 解法二 dp 空间优化

空间优化关键是从大到小枚举

```c++
/*
f[i][j]: 只放前i个物品, 总体积是j的情况下总价值最大是多少

result = max(f[n][0~V])

f[i][j] = max(f[i-1][j], f[i-1][j-[i]] + w[i])

f[0][0] = 0
*/

#include <iostream>
#include <cstring>
#include <algorithm>

using namespace std;

const int N = 1010;

int n, m;
int f[N];
int v[N], w[N];

int main()
{
    cin >> n >> m;
    for (int i = 1; i <= n; i++) cin >> v[i] >> w[i];
    
    for (int i = 1; i <= n; i++) 
        for (int j = m; j >= v[i]; j--)
        {   
        		// 空间优化key: 若从小到大枚举, f[j-v[i]]使用的是f[i][j-v[i]], 所以从大到小枚举
          	f[j] = max(f[j], f[j-v[i]] + w[i]);
        }
    
    cout << f[m] << endl;
    return 0;
}
```

###[完全背包问题](https://www.acwing.com/problem/content/3/)

#### 题目描述

有 $N$ 种物品和一个容量是 $V$ 的背包，每种物品都有无限件可用。

第 $i$ 种物品的体积是 $v_i$，价值是 $w_i$。

求解将哪些物品装入背包，可使这些物品的总体积不超过背包容量，且总价值最大。
输出最大价值。

#### 输入格式

第一行两个整数，$N，V$，用空格隔开，分别表示物品种数和背包容积。

接下来有 $N$ 行，每行两个整数 $v_i, w_i$，用空格隔开，分别表示第 $i$ 种物品的体积和价值。

#### 输出格式

输出一个整数，表示最大价值。

#### 数据范围

$0 \lt N, V \le 1000$
$0 \lt v_i, w_i \le 1000$

#### 输入样例

```
4 5
1 2
2 4
3 4
4 5
```

#### 输出样例：

```
10
```


#### 解法

##### 解法一 
```python

```

##### 解法二
```python

```

### 53.最大子序和

#### 53.1.题目描述

给定一个整数数组 `nums` ，找到一个具有最大和的连续子数组（子数组最少包含一个元素），返回其最大和。

**示例:**

```
输入: [-2,1,-3,4,-1,2,1,-5,4],
输出: 6
解释: 连续子数组 [4,-1,2,1] 的和最大，为 6。
```

**进阶:**

如果你已经实现复杂度为 O(*n*) 的解法，尝试使用更为精妙的分治法求解。

#### 53.2.解法
##### 53.2.1 方法一 DP

state: 以i为连续子数组最后一个元素索引的和

```java
class Solution {
    public int maxSubArray(int[] nums) {
        int[] dp = new int[nums.length+1];
        int ans = Integer.MIN_VALUE;
        for(int i = 0; i < nums.length; i++){
            if(dp[i] <= 0){
                dp[i+1] = nums[i];
            }else{
                dp[i+1] = dp[i] + nums[i];
            }
            if(dp[i+1] > ans){
                ans = dp[i+1];
            }
        }
        return ans;
    }
}

class Solution:
    def maxSubArray(self, nums: List[int]) -> int:
        if not nums:
            return 0
        res = float('-inf')
        sumv = 0
        for num in nums:
            sumv += num
            res = max(sumv, res)
            if sumv < 0:
                sumv = 0
        return res
```

##### 53.2.2 方法二 D&C

```java
class Solution {
    public int maxSubArray(int[] nums) {
        return maxSubArray(nums, 0, nums.length-1);
    }
    
    private int maxSubArray(int[] nums, int left, int right){
        // 终止条件
        if(left == right){
            return nums[left];
        }
        int mid = (left + right) >> 1;
        int leftSum = maxSubArray(nums, left, mid);
        int rightSum = maxSubArray(nums, mid+1, right);
        int crossSum = crossSubArray(nums, left, right);
        return Math.max(Math.max(leftSum, rightSum), crossSum);
    }
    
    private int crossSubArray(int[] nums, int left, int right){
        int mid = (left + right) >> 1;
        // 最小值
        int leftSum = Integer.MIN_VALUE;
        int rightSum = Integer.MIN_VALUE;
        int sum = 0;
        for(int i = mid; i >= left; i--){
            sum += nums[i];
            leftSum = Math.max(leftSum, sum);
        }
        sum = 0;
        for(int i = mid+1; i <= right; i++){
            sum += nums[i];
            rightSum = Math.max(rightSum, sum);
        }
        return leftSum + rightSum;
        
    }
}

```

### 70.爬楼梯

#### 70.1.题目描述

假设你正在爬楼梯。需要 *n* 阶你才能到达楼顶。

每次你可以爬 1 或 2 个台阶。你有多少种不同的方法可以爬到楼顶呢？

**注意：**给定 *n* 是一个正整数。

**示例 1：**

```
输入： 2
输出： 2
解释： 有两种方法可以爬到楼顶。
1.  1 阶 + 1 阶
2.  2 阶
```

**示例 2：**

```
输入： 3
输出： 3
解释： 有三种方法可以爬到楼顶。
1.  1 阶 + 1 阶 + 1 阶
2.  1 阶 + 2 阶
3.  2 阶 + 1 阶
```

#### 70.2.解法
##### 70.2.1 方法一 DP

```java
class Solution {
    public int climbStairs(int n) {
        int[] dp = new int[n+1];
        dp[0] = 1;
        dp[1] = 1;
        for(int i = 2; i <= n; i++){
            dp[i] = dp[i-1] + dp[i-2];
        }
        return dp[n];
    }
}
```

### [72. 编辑距离](https://leetcode-cn.com/problems/edit-distance/)

#### 题目描述

给定两个单词 word1 和 word2，计算出将 word1 转换成 word2 所使用的最少操作数 。

你可以对一个单词进行如下三种操作：

插入一个字符
删除一个字符
替换一个字符
示例 1:

输入: word1 = "horse", word2 = "ros"
输出: 3
解释: 
horse -> rorse (将 'h' 替换为 'r')
rorse -> rose (删除 'r')
rose -> ros (删除 'e')
示例 2:

输入: word1 = "intention", word2 = "execution"
输出: 5
解释: 
intention -> inention (删除 't')
inention -> enention (将 'i' 替换为 'e')
enention -> exention (将 'n' 替换为 'x')
exention -> exection (将 'n' 替换为 'c')
exection -> execution (插入 'u')


#### 解法

##### 解法一 dp

动态规划：
dp[i][j] 代表 word1 到 i 位置转换成 word2 到 j 位置需要最少步数

所以，

当 word1[i] == word2[j]，dp\[i][j] = dp\[i-1][j-1]；

当 word1[i] != word2[j]，dp\[i][j] = min(dp\[i-1][j-1], dp\[i-1][j], dp\[i][j-1]) + 1

其中，dp\[i-1][j-1] 表示替换操作，dp\[i-1][j] 表示删除操作，dp\[i][j-1] 表示插入操作。

注意，针对第一行，第一列要单独考虑，我们引入 '' 下图所示：



第一行，是 word1 为空变成 word2 最少步数，就是插入操作

第一列，是 word2 为空，需要的最少步数，就是删除操作



①先删除字符串X 的第 i 个字符source[i] 再将源字符串X 的前 i-1 个字符 X[1...i-1] 转换成 目标字符串Y[1...j]， 

②先将 插入字符串Y的第 j 个字符 target[j]   ，然后再 

③先将 源字符串中的 第 i 个字符X[i] 替换为 目标字符串的第 j 个字符 Y[j]，然后 源字符串X[1...i-1] 转换成 目标字符串Y[1...j-1]

```python
class Solution:
    def minDistance(self, word1: str, word2: str) -> int:
        """
            动态规划:
        """
        res1, res2 = [], []
        m, n = len(word1), len(word2)
        dp = [[0] * (n + 1) for _ in range(m + 1)]
        for i in range(m + 1):
            dp[i][0] = i
        for j in range(n + 1):
            dp[0][j] = j
        
        for i in range(1, m + 1):
            for j in range(1, n + 1):
                if word1[i - 1] == word2[j - 1]:
                    dp[i][j] = dp[i - 1][j - 1]
                else:
                    dp[i][j] = min(dp[i - 1][j] + 1, dp[i][j - 1] + 1, dp[i - 1][j - 1] + 1)
                    
        return dp[m][n]
 
```

##### 解法二
```python

```

###95.不同的二叉搜索树II 不用DP

#### 题目描述

给定一个整数 n，生成所有由 1 ... n 为节点所组成的二叉搜索树。

示例:

输入: 3
输出:
[
  [1,null,3,2],
  [3,2,null,1],
  [3,1,null,null,2],
  [2,1,3],
  [1,null,2,null,3]
]
解释:
以上的输出对应以下 5 种不同结构的二叉搜索树：

   1         3     3      2      1
    \       /     /      / \      \
     3     2     1      1   3      2
    /     /       \                 \
   2     1         2                 3


#### 解法

##### 解法一 递归

我们从序列 `1 ..n` 中取出数字 `i`，作为当前树的树根。于是，剩余 `i - 1` 个元素可用于左子树，`n - i` 个元素用于右子树。

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None

class Solution:
    def generateTrees(self, n: int) -> List[TreeNode]:
        """
            动态规划
            从1...n选出一个数i作为root, 1...i-1做为左子树 i+1...n作为右子树
        """
        def helper(m, n):
            if m > n:
                return [None,]
            
            res = []
            for i in range(m, n + 1):
                left_trees = helper(m, i - 1)
                right_trees = helper(i + 1, n)
                for l in left_trees:
                    for r in right_trees:
                        root = TreeNode(i)
                        root.left = l
                        root.right = r
                        res.append(root)
            return res

        return helper(1, n) if n else []
        
```

##### 解法二
```python

```

### 120.三角形内角和

#### 120.1.题目描述

给定一个三角形，找出自顶向下的最小路径和。每一步只能移动到下一行中相邻的结点上。

例如，给定三角形：

```
[
     [2],
    [3,4],
   [6,5,7],
  [4,1,8,3]
]
```

自顶向下的最小路径和为 `11`（即，**2** + **3** + **5** + **1** = 11）。

**说明：**

如果你可以只使用 *O*(*n*) 的额外空间（*n* 为三角形的总行数）来解决这个问题，那么你的算法会很加分。

#### 120.2.解法

##### 120.2.1 方法一 TO(n)

state: 以该结点为根节点的

```java
class Solution {
    public int minimumTotal(List<List<Integer>> triangle) {
        int[] A = new int[triangle.size()+1];
        for(int i = triangle.size() - 1; i >= 0; i--){
            for(int j = 0; j < triangle.get(i).size(); j++){
                A[j] = Math.min(A[j], A[j+1]) + triangle.get(i).get(j);
            }
        }
        return A[0];
    }
}
```



##### 120.2.2 方法二

### 121. 买卖股票的最佳时机

#### 121.1.题目描述d

给定一个数组，它的第 *i* 个元素是一支给定股票第 *i* 天的价格。

如果你最多只允许完成一笔交易（即买入和卖出一支股票），设计一个算法来计算你所能获取的最大利润。

注意你不能在买入股票前卖出股票。

**示例 1:**

```
输入: [7,1,5,3,6,4]
输出: 5
解释: 在第 2 天（股票价格 = 1）的时候买入，在第 5 天（股票价格 = 6）的时候卖出，最大利润 = 6-1 = 5 。
     注意利润不能是 7-1 = 6, 因为卖出价格需要大于买入价格。
```

**示例 2:**

```
输入: [7,6,4,3,1]
输出: 0
解释: 在这种情况下, 没有交易完成, 所以最大利润为 0。
```

#### 121.2.解法
##### 121.2.1 方法一 DP

```java
class Solution {
    public int maxProfit(int[] prices) {
        // 记录到当前日期的股票最低价格
        // 前i天的最大价格 = max(前i-1天的最大价格, 第i天的价格-到当前日期的股票最低价格）
        if(prices.length == 0){
            return 0;
        }
        int[] dp = new int[prices.length];
        int minPrice = prices[0];
        for(int i = 1; i < prices.length; i++){
            dp[i] = Math.max(dp[i-1], prices[i] - minPrice);
            minPrice = Math.min(minPrice, prices[i]);
        }
        return dp[prices.length-1];
    }
}
```

##### 121.2.2 方法二

维持两个变量  "最小价格" "最大利润"

```java
class Solution {
    public int maxProfit(int[] prices) {
        int minprice = Integer.MAX_VALUE;
        int maxprofit = 0;
        for(int i = 0; i < prices.length; i++){
            if(prices[i] < minprice){
                minprice = prices[i];
            }else if(prices[i] - minprice > maxprofit){
                maxprofit = prices[i] - minprice
            }
        }
        return maxprofit;
    }
}
```

### 122. 买卖股票的最佳时机 II

#### 题目描述

给定一个数组，它的第 i 个元素是一支给定股票第 i 天的价格。

设计一个算法来计算你所能获取的最大利润。你可以尽可能地完成更多的交易（多次买卖一支股票）。

注意：你不能同时参与多笔交易（你必须在再次购买前出售掉之前的股票）。

示例 1:

输入: [7,1,5,3,6,4]
输出: 7
解释: 在第 2 天（股票价格 = 1）的时候买入，在第 3 天（股票价格 = 5）的时候卖出, 这笔交易所能获得利润 = 5-1 = 4 。
     随后，在第 4 天（股票价格 = 3）的时候买入，在第 5 天（股票价格 = 6）的时候卖出, 这笔交易所能获得利润 = 6-3 = 3 。
示例 2:

输入: [1,2,3,4,5]
输出: 4
解释: 在第 1 天（股票价格 = 1）的时候买入，在第 5 天 （股票价格 = 5）的时候卖出, 这笔交易所能获得利润 = 5-1 = 4 。
     注意你不能在第 1 天和第 2 天接连购买股票，之后再将它们卖出。
     因为这样属于同时参与了多笔交易，你必须在再次购买前出售掉之前的股票。
示例 3:

输入: [7,6,4,3,1]
输出: 0
解释: 在这种情况下, 没有交易完成, 所以最大利润为 0。

#### 解法

##### 解法一 贪心

找到局部最优就卖掉 

```python
class Solution:
    def maxProfit(self, prices: List[int]) -> int:
        """
            贪心 找到局部最优就卖掉 波峰波谷
        """
        if not prices:
            return 0
        min_value = prices[0]
        ans = 0
        for i in range(1, len(prices)):
            if min_value >= prices[i]:
                min_value = prices[i]
            else:
                if (i + 1 < len(prices) and prices[i] > prices[i+1]):
                    ans = ans + prices[i] - min_value
                    min_value = prices[i + 1]
                elif (i == len(prices) - 1 and prices[i] > min_value):
                    ans += prices[i] - min_value
        return ans      
```

##### 解法二 贪心优化

该解决方案遵循 方法二 的本身使用的逻辑，但有一些轻微的变化。在这种情况下，我们可以简单地继续在斜坡上爬升并持续增加从连续交易中获得的利润，而不是在谷之后寻找每个峰值。最后，我们将有效地使用峰值和谷值，但我们不需要跟踪峰值和谷值对应的成本以及最大利润，但我们可以直接继续增加加数组的连续数字之间的差值，如果第二个数字大于第一个数字，我们获得的总和将是最大利润。这种方法将简化解决方案。 这个例子可以更清楚地展现上述情况：

[1, 7, 2, 3, 6, 7, 6, 7]

与此数组对应的图形是：

![Profit Graph](/Users/zhengxinzhi/typora_img/LeetCode%E9%A2%98%E7%9B%AE%E6%80%BB%E7%BB%93/6eaf01901108809ca5dfeaef75c9417d6b287c841065525083d1e2aac0ea1de4-file_1555699697692.png)

```python
class Solution {
    public int maxProfit(int[] prices) {
        int maxprofit = 0;
        for (int i = 1; i < prices.length; i++) {
            if (prices[i] > prices[i - 1])
                maxprofit += prices[i] - prices[i - 1];
        }
        return maxprofit;
    }
}
```

### 139.单词拆分

#### 题目描述

给定一个非空字符串 s 和一个包含非空单词列表的字典 wordDict，判定 s 是否可以被空格拆分为一个或多个在字典中出现的单词。

说明：

拆分时可以重复使用字典中的单词。
你可以假设字典中没有重复的单词。
示例 1：

输入: s = "leetcode", wordDict = ["leet", "code"]
输出: true
解释: 返回 true 因为 "leetcode" 可以被拆分成 "leet code"。
示例 2：

输入: s = "applepenapple", wordDict = ["apple", "pen"]
输出: true
解释: 返回 true 因为 "applepenapple" 可以被拆分成 "apple pen apple"。
     注意你可以重复使用字典中的单词。
示例 3：

输入: s = "catsandog", wordDict = ["cats", "dog", "sand", "and", "cat"]
输出: false

#### 解法

##### 解法一 dp

dp[i] 表示i前面的字符串能否拆分

```python
class Solution:
    def wordBreak(self, s: str, wordDict: List[str]) -> bool:
        """
            dp[i] 表示i前面的字符串能否拆分
        """
        dp = [False] * (len(s) + 1)
        dp[0] = True
        wordset = set(wordDict)
        for i in range(1, len(s) + 1):
            for j in range(i - 1, -1, -1):
                if dp[j] and s[j: i] in wordset:
                    dp[i] = True
                    break
        return dp[len(s)]
```

复杂度分析

时间复杂度：O(n^2)。dp 数组需要两重循环

空间复杂度：O(n) 。dp 数组的长度是 n+1 。

##### 解法二 bruce

最简单的实现方法是用递归和回溯。为了找到解，我们可以检查字典单词中每一个单词的可能前缀，如果在字典中出现过，那么去掉这个前缀后剩余部分回归调用。同时，如果某次函数调用中发现整个字符串都已经被拆分且在字典中出现过了，函数就返回 true 。

```python
public class Solution {
    public boolean wordBreak(String s, List<String> wordDict) {
        return word_Break(s, new HashSet(wordDict), 0);
    }
    public boolean word_Break(String s, Set<String> wordDict, int start) {
        if (start == s.length()) {
            return true;
        }
        for (int end = start + 1; end <= s.length(); end++) {
            if (wordDict.contains(s.substring(start, end)) && word_Break(s, wordDict, end)) {
                return true;
            }
        }
        return false;
    }
}
```

复杂度分析

时间复杂度：O(n^n)。考虑最坏情况 s = \text{aaaaaaa}。每一个前缀都在字典中，此时回溯树的复杂度会达到 n^n。
空间复杂度：O(n) 。回溯树的深度最深达到 n 。

##### 解法三 记忆化回溯

由于前面单词不定长，所以后一步的递归有可能重复被调用，利用记忆化方法提交效率

```java
public class Solution {
    public boolean wordBreak(String s, List<String> wordDict) {
        // Boolean[] 初始化不是 false 而是 null
        return word_Break(s, new HashSet(wordDict), 0, new Boolean[s.length()]);
    }
    
    public boolean word_Break(String s, Set<String> wordDict, int start, Boolean[] memo) {
        if (start == s.length()) {
            return true;
        }
        // 利用记忆化
        if(memo[start] != null){
            return memo[start];
        }
        for (int end = start + 1; end <= s.length(); end++) {
            if (wordDict.contains(s.substring(start, end)) && word_Break(s, wordDict, end, memo)){
                // 返回前存储记忆化
                memo[start] = true;
                return true;
            }
        }
        // 返回前存储记忆化
        memo[start] = false;
        return false;
    }
}

```

复杂度分析

时间复杂度：O(n^2)。回溯树的大小最多达到 n^2。 

空间复杂度：O(n) 。回溯树的深度最深达到 n 

### 198.打家劫舍

#### 198.1.题目描述

你是一个专业的小偷，计划偷窃沿街的房屋。每间房内都藏有一定的现金，影响你偷窃的唯一制约因素就是相邻的房屋装有相互连通的防盗系统，**如果两间相邻的房屋在同一晚上被小偷闯入，系统会自动报警**。

给定一个代表每个房屋存放金额的非负整数数组，计算你**在不触动警报装置的情况下，**能够偷窃到的最高金额。

**示例 1:**

```
输入: [1,2,3,1]
输出: 4
解释: 偷窃 1 号房屋 (金额 = 1) ，然后偷窃 3 号房屋 (金额 = 3)。
     偷窃到的最高金额 = 1 + 3 = 4 。
```

**示例 2:**

```
输入: [2,7,9,3,1]
输出: 12
解释: 偷窃 1 号房屋 (金额 = 2), 偷窃 3 号房屋 (金额 = 9)，接着偷窃 5 号房屋 (金额 = 1)。
     偷窃到的最高金额 = 2 + 9 + 1 = 12 。
```

#### 198.2.解法
##### 198.2.1 方法一

```java
class Solution {
    public int rob(int[] nums) {
        if(nums.length == 0){
            return 0;
        }else if(nums.length == 1){
            return nums[0];
        }
        int[] dp = new int[nums.length];
        dp[0] = nums[0];
        dp[1] = Math.max(nums[1], nums[0]);
        for(int i = 2; i < nums.length; i++){
            dp[i] = Math.max(dp[i-1], dp[i-2] + nums[i]);
        }
        return dp[nums.length-1];
    }
}
```

##### 198.2.2 方法二 迭代法

```python
class Solution(object):
    def rob(self, nums):
        """
        :type nums: List[int]
        :rtype: int
        """
        last = 0 
        now = 0
        for i in nums: 
            #这是一个动态规划问题
            last, now = now, max(last + i, now)
        return now
```

###213.打家劫舍II

#### 题目描述

你是一个专业的小偷，计划偷窃沿街的房屋，每间房内都藏有一定的现金。这个地方所有的房屋都围成一圈，这意味着第一个房屋和最后一个房屋是紧挨着的。同时，相邻的房屋装有相互连通的防盗系统，如果两间相邻的房屋在同一晚上被小偷闯入，系统会自动报警。

给定一个代表每个房屋存放金额的非负整数数组，计算你在不触动警报装置的情况下，能够偷窃到的最高金额。

示例 1:

输入: [2,3,2]
输出: 3
解释: 你不能先偷窃 1 号房屋（金额 = 2），然后偷窃 3 号房屋（金额 = 2）, 因为他们是相邻的。
示例 2:

输入: [1,2,3,1]
输出: 4
解释: 你可以先偷窃 1 号房屋（金额 = 1），然后偷窃 3 号房屋（金额 = 3）。
     偷窃到的最高金额 = 1 + 3 = 4 。


#### 解法

##### 解法一 DP

```python
class Solution:
    def rob(self, nums: List[int]) -> int:
        """
            问题变成 max(House[1]...House[n-1], House[2]...House[n])
        """
        if not nums:
            return 0
        if len(nums) < 2:
            return nums[0]
        # House[1]...House[n-1]
        n = len(nums)
        dp = [0] * n
        dp[1] = nums[0]
        for i, num in enumerate(nums[1: n - 1]):
            dp[i + 2] = max(dp[i + 1], dp[i] + num)
        res = dp[n - 1]
    
        dp = [0] * n
        dp[1] = nums[1]
        for i, num in enumerate(nums[2:]):
            dp[i + 2] = max(dp[i + 1], dp[i] + num)
        res = max(dp[n - 1], res)
        return res
```

##### 解法二
```python
class Solution:
    def rob(self, nums: [int]) -> int:
        def my_rob(nums):
            cur, pre = 0, 0
            for num in nums:
                cur, pre = max(pre + num, cur), cur
            return cur
        return max(my_rob(nums[:-1]),my_rob(nums[1:])) if len(nums) != 1 else nums[0]

```

### 221.1 最大正方形

#### 1.题目描述

在一个由 0 和 1 组成的二维矩阵内，找到只包含 1 的最大正方形，并返回其面积。

**示例:**

```
输入: 

1 0 1 0 0
1 0 1 1 1
1 1 1 1 1
1 0 0 1 0

输出: 4
```

#### 2.解法

##### 2.1 方法一

流出第一行第一列作为初始化

state: 以i, j位置上元素为右下角的最大正方形边长

dp(i, j) = min{ dp(i-1, j-1), dp(i-1, j), dp(i, j-1) }

```java
public class Solution {
    public int maximalSquare(char[][] matrix) {
        int rows = matrix.length, cols = rows > 0 ? matrix[0].length : 0;
        int[][] dp = new int[rows + 1][cols + 1];
        int maxsqlen = 0;
        for (int i = 1; i <= rows; i++) {
            for (int j = 1; j <= cols; j++) {
                if (matrix[i-1][j-1] == '1'){
                    dp[i][j] = Math.min(Math.min(dp[i][j - 1], dp[i - 1][j]), dp[i - 1][j - 1]) + 1;
                    maxsqlen = Math.max(maxsqlen, dp[i][j]);
                }
            }
        }
        return maxsqlen * maxsqlen;
    }
}
```

- 时间复杂度：O(mn)*O*(*m**n*)。
- 空间复杂度：O(mn)*O*(*m**n*)，用了一个大小相同的矩阵 dp。

##### 2.2 方法二

![image-20191113190852244](/Users/zhengxinzhi/typora_img/LeetCode题目总结/image-20191113190852244.png)

```java
public class Solution {
    public int maximalSquare(char[][] matrix) {
        int rows = matrix.length, cols = rows > 0 ? matrix[0].length : 0;
        int[] dp = new int[cols + 1];
        int maxsqlen = 0, prev = 0;
        for (int i = 1; i <= rows; i++) {
            for (int j = 1; j <= cols; j++) {
                int temp = dp[j];
                if (matrix[i - 1][j - 1] == '1') {
                    dp[j] = Math.min(Math.min(dp[j - 1], prev), dp[j]) + 1;
                    maxsqlen = Math.max(maxsqlen, dp[j]);
                } else {
                    dp[j] = 0;
                }
                prev = temp;
            }
        }
        return maxsqlen * maxsqlen;
    }
}

```



### 300.最长上升子序列

#### 题目描述

给定一个无序的整数数组，找到其中最长上升子序列的长度。

示例:

输入: [10,9,2,5,3,7,101,18]
输出: 4 
解释: 最长的上升子序列是 [2,3,7,101]，它的长度是 4。
说明:

可能会有多种最长上升子序列的组合，你只需要输出对应的长度即可。
你算法的时间复杂度应该为 O(n2) 。
进阶: 你能将算法的时间复杂度降低到 O(n log n) 吗?

#### 解法 

##### 解法一 DP 

O(n ** 2)

```python
class Solution:
    def lengthOfLIS(self, nums: List[int]) -> int:
        """
            dp: 以nums[i]为上升子序列最后一个值的长度
            
            进阶： 修改logn时间找到待更新的点
        """
        if not nums:
            return 0
        res = 1
        dp = [1] * len(nums)
        for i, num in enumerate(nums):
            for j in range(i - 1, -1, -1):
                if nums[j] < num:
                    dp[i] = max(dp[j] + 1, dp[i])
            res = max(dp[i], res)
        return res
```

##### 解法二 贪心算法 + 二分查找 [记住]

O(nlogn)

**贪心算法的基本思想：**

算法的基本思想如下：

**如果前面的数越小，后面接上一个随机数，就会有更大的可能性构成一个更长的“上升子序列”。**

这个思想也不难理解，我们举例说明：如果前面的数是 11，后面接上一个随机数，能够构成长度为 22 的“上升子序列”的可能性，就远远大于前面的数是 1000010000，后面接上一个随机数，能够构成长度为 22 的“上升子序列”的可能性。

基于这个思想，我们先介绍算法的流程，然后再做总结，最后把其中关键的地方向大家指出

**算法的执行流程：**

**1**、设置一个有序数组 tail，初始时为空；

数组命名为 tail 大家先不用纠结，只要先有个印象，反正有序数组 tail 不是“最长上升子序列”（下文还会强调），不能命名为 LIS，有序数组 tail 是用于求解 LIS 问题的辅助数组，如果大家有更贴切的命名，可以在评论区向我指出。

2、在遍历数组 nums 的过程中，每来一个新数 num，如果这个数（严格）大于有序数组 tail 的最后一个元素，就把 num 放在有序数组 tail 的后面，否则进入第 3 点；

注意：这里的大于是“严格大于”，不包括等于的情况。

3、在有序数组 tail 中查找第 1 个大于等于 num 的那个数，试图让它变小；

“试图让它变小”的含义是：如果在有序数组 tail 中查找第 1 个等于 num 的那个数，等于就没有办法让它变小啦，这个企图失败；如果在有序数组 tail 中查找第 1 个（严格）大于 num 的那个数，就可以让它变小，企图成功；

在有序数组 tail 中找大于等于 num 的第 1 个数，可以使用“二分法”。

4、遍历新的数 num ，先尝试上述第 2 点，第 2 点行不通就执行第 3 点，直到遍历完整个数组 nums，最终有序数组 tail 的长度，就是所求的“最长上升子序列”的长度。

以上算法能够奏效的关键是：

根据最开始提到的“基本思想”，可以认为是一种“贪心选择”的思想：只要让前面的数尽量小，在算法的执行过程中，第 2 点被执行的机会就更多。

```python
class Solution:
    def lengthOfLIS(self, nums: List[int]) -> int:
        # key: 辅助队列 在j元素处 长度m是当前列表的最长子序列长度 内容是top least m
        # 目的是遍历下一个元素时能更快找到比它小的元素
        if len(nums) < 2:
            return len(nums)
        tail = []
        for num in nums:
            if not tail or num > tail[-1]:
                tail.append(num)
                continue
            # 使用二分查找法，在有序数组 tail 中
            # 找到第 1 个大于等于 nums[i] 的元素，尝试让那个元素更小
            left = 0
            right = len(tail) - 1
            while left < right:
                mid = left + ((right - left) >> 1)
                # 这样找的就是左边界, 即比num更大的数字中的最小值 
                if tail[mid] < num:
                    left = mid + 1
                else:
                    right = mid
            tail[left] = num
        return len(tail)
```

### 303.区域和检索-数组不可变

#### 303.1.题目描述

给定一个整数数组  *nums*，求出数组从索引 *i* 到 *j*  (*i* ≤ *j*) 范围内元素的总和，包含 *i,  j* 两点。

**示例：**

```
给定 nums = [-2, 0, 3, -5, 2, -1]，求和函数为 sumRange()

sumRange(0, 2) -> 1
sumRange(2, 5) -> -1
sumRange(0, 5) -> -3
```

**说明:**

1. 你可以假设数组不可变。
2. 会多次调用 *sumRange* 方法。

#### 303.2.解法
##### 303.2.1 方法一 DP

![image-20190512122106668](/Users/zhengxinzhi/typora_img/LeetCode题目总结/image-20190512122106668.png)

```java
class NumArray {
    private int[] dp;
    public NumArray(int[] nums) {
        dp = new int[nums.length+1];
        dp[0] = 0;
        for(int i = 0; i < nums.length; i++){
            dp[i+1] = dp[i] + nums[i];
        }
    }
    
    public int sumRange(int i, int j) {
        return dp[j+1] - dp[i];
    }
}

/**
 * Your NumArray object will be instantiated and called as such:
 * NumArray obj = new NumArray(nums);
 * int param_1 = obj.sumRange(i,j);
 */
```
##### 303.2.2 方法二

Imagine that *sumRange* is called one thousand times with the exact same arguments. How could we speed that up?

We could trade in extra space for speed. By pre-computing all range sum possibilities and store its results in a hash table, we can speed up the query to constant time.
```java
import javafx.util.Pair; // 类似于tuple
class NumArray {
    private Map<Pair<Integer, Integer>, Integer> map = new HashMap<>();

    public NumArray(int[] nums) {
        for (int i = 0; i < nums.length; i++) {
            int sum = 0;
            for (int j = i; j < nums.length; j++) {
                sum += nums[j];
                map.put(Pair.create(i, j), sum);
            }
        }
    }

    public int sumRange(int i, int j) {
        return map.get(Pair.create(i, j));
    }
}

/**
 * Your NumArray object will be instantiated and called as such:
 * NumArray obj = new NumArray(nums);
 * int param_1 = obj.sumRange(i,j);
 */
```

###309.[最佳买卖股票时机含冷冻期](https://leetcode-cn.com/problems/best-time-to-buy-and-sell-stock-with-cooldown/) [推荐]

#### 题目描述

给定一个整数数组，其中第 i 个元素代表了第 i 天的股票价格 。

设计一个算法计算出最大利润。在满足以下约束条件下，你可以尽可能地完成更多的交易（多次买卖一支股票）:

你不能同时参与多笔交易（你必须在再次购买前出售掉之前的股票）。
卖出股票后，你无法在第二天买入股票 (即冷冻期为 1 天)。
示例:

输入: [1,2,3,0,2]
输出: 3 
解释: 对应的交易状态为: [买入, 卖出, 冷冻期, 买入, 卖出]


#### 解法

##### 解法一 DP O(n**2) python超时
```python
class Solution:
    def maxProfit(self, prices: List[int]) -> int:
        """
            边界条件
            状态: 以第i天的最大利润dp[i]
            状态转移方程 dp[i]
        """
        if len(prices) < 2:
            return 0
            
        dp = [0] * (len(prices) + 1)
        for i in range(2, len(prices) + 1):
            dp[i] = dp[i - 1]
            for j in range(i - 1, 0, -1):
                if prices[i - 1] - prices[j - 1] > 0:
                    temp = dp[j - 2] if j > 2 else 0
                    dp[i] = max(dp[i], temp + prices[i - 1] - prices[j - 1])
                              
        return dp[-1]


```

##### 解法二 dp hold unhold

![image-20191024105512040](/Users/zhengxinzhi/typora_img/LeetCode题目总结/image-20191024105512040.png)

```python
class Solution:
    def maxProfit(self, prices: List[int]) -> int:
        """
            state:
                hold[i]: 第i天持有股票的最大profit
                unhold[i]: 第i天不持有股票的最大profit
            state transfer:
                hold[i] = max(hold[i-1], unhold[i-2] - prices[i])
                unhold[i] = max(unhold[i-1], hold[i-1] + prices[i])
            base case:
        """
        if len(prices) < 2:
            return 0

        hold = [0] * len(prices)
        unhold = [0] * len(prices)
        hold[0] = -prices[0]
        unhold[0] = 0
        for i in range(1, len(prices)):
            if i == 1:
                hold[i] = max(hold[i-1], -prices[i])
            else:
                # 有一天的冷冻时间
                hold[i] = max(hold[i-1], unhold[i-2] - prices[i])
            unhold[i] = max(unhold[i-1], hold[i-1] + prices[i])
        return unhold[-1]
        

```

空间优化

可用3个变量替换两个dp数组 unhold[i-1]  unhold[i-2]  hold[i-1]

```python

```

##### 2.3 方法三

![image-20191113192711202](/Users/zhengxinzhi/typora_img/LeetCode题目总结/image-20191113192711202.png)

##### 方法三

sell[i]表示截至第i天，最后一个操作是卖时的最大收益；
buy[i]表示截至第i天，最后一个操作是买时的最大收益；
cool[i]表示截至第i天，最后一个操作是冷冻期时的最大收益；

递推公式：
sell[i] = max(buy[i-1]+prices[i], sell[i-1]) (第一项表示第i天卖出，第二项表示第i天冷冻)
buy[i] = max(cool[i-1]-prices[i], buy[i-1]) （第一项表示第i天买进，第二项表示第i天冷冻）
cool[i] = max(sell[i-1], buy[i-1], cool[i-1])

```python
class Solution:
    def maxProfit(self, prices):
        n = len(prices)
        if n == 0:
            return 0     
        sell = [0 for _ in range(n)]
        buy = [0 for _ in range(n)]
        cool = [0 for _ in range(n)]
        buy[0] = -prices[0]
        for i in range(1,n):
            sell[i] = max(buy[i-1] + prices[i], sell[i-1])
            buy[i] = max(cool[i-1] - prices[i], buy[i-1])
            cool[i] = max(sell[i-1], buy[i-1],cool[i-1])
        return sell[-1]
```



### 322. 零钱兑换

#### 322. 1.题目描述

给定不同面额的硬币 coins 和一个总金额 amount。编写一个函数来计算可以凑成总金额所需的最少的硬币个数。如果没有任何一种硬币组合能组成总金额，返回 `-1`。

**示例 1:**

```
输入: coins = [1, 2, 5], amount = 11
输出: 3 
解释: 11 = 5 + 5 + 1
```

**示例 2:**

```
输入: coins = [2], amount = 3
输出: -1
```

**说明**:
你可以认为每种硬币的数量是无限的。

#### 322. 2.解法

##### 322. 2.1 方法一 DP

```java
class Solution {
    public int coinChange(int[] coins, int amount) {
        int[] dp = new int[amount+1];
        // 如果没有任何一种硬币组合能组成总金额，返回 -1。
        // 由上面初始化dp为-1
        for(int i = 0; i <= amount; i++){
            dp[i] = -1;
        }
        // 代表i金额最少硬币个数
        dp[0] = 0;
        for(int i = 1; i <= amount; i++){
            for(int j = 0; j < coins.length; j++){
                // 判断 i - coins[j] 是否有值
                if(i - coins[j] >= 0 && dp[i - coins[j]] != -1){
                    // 若有更小的硬币数 更新dp
                    if(dp[i] == -1 || dp[i] > dp[i -coins[j]] + 1){
                        dp[i] = dp[i - coins[j]] + 1;
                    }
                }    
            }
        }
        return dp[amount];
    }
}


```

### 337.打家劫舍III

#### 题目描述

在上次打劫完一条街道之后和一圈房屋后，小偷又发现了一个新的可行窃的地区。这个地区只有一个入口，我们称之为“根”。 除了“根”之外，每栋房子有且只有一个“父“房子与之相连。一番侦察之后，聪明的小偷意识到“这个地方的所有房屋的排列类似于一棵二叉树”。 如果两个直接相连的房子在同一天晚上被打劫，房屋将自动报警。

计算在不触动警报的情况下，小偷一晚能够盗取的最高金额。

示例 1:

输入: [3,2,3,null,3,null,1]

     3
    / \
   2   3
    \   \ 
     3   1

输出: 7 
解释: 小偷一晚能够盗取的最高金额 = 3 + 3 + 1 = 7.
示例 2:

输入: [3,4,5,1,3,null,1]

     3
    / \
   4   5
  / \   \ 
 1   3   1

输出: 9
解释: 小偷一晚能够盗取的最高金额 = 4 + 5 = 9.


#### 解法

##### 解法一 递归 + 记忆化
```python
import queue
class Solution:
    memo = dict()
    def rob(self, root: TreeNode) -> int:
        """
            bottom-to-up dp
            序数法
            递归
        """
        # 终止条件
        if not root:
            return 0
        if not root.left and not root.right:
            return root.val
        
        if root in self.memo:
            return self.memo[root]
        left = 0
        right = 0
        lleft = 0
        rleft = 0
        lright = 0
        rright = 0
        if root.left:
            left = self.rob(root.left)
            lleft = self.rob(root.left.left)
            rleft = self.rob(root.left.right)
        
        if root.right:
            right = self.rob(root.right)
            lright = self.rob(root.right.left)
            rright = self.rob(root.right.right)
        rootv = root.val + lleft + rleft + lright + rright
        childv = left + right
        self.memo[root] = max(rootv, childv)
        return self.memo[root]
```

##### 解法二 递归 + 记忆化
```python
"""
            选中root, 一定不可选他的左右孩子结点
            不选root, 不一定选他的左右孩子结点
"""
cache = {}


def max_with_root(node):
    return node.val + max_without_root(node.left) + max_without_root(node.right) if node else 0


def max_without_root(node):
    return helper(node.left) + helper(node.right) if node else 0


def helper(node):
    if node in cache:
        return cache[node]
    cache[node] = max(max_with_root(node), max_without_root(node)) if node else 0
    return cache[node]


class Solution(object):
    def rob(self, root):
        """
        :type root: TreeNode
        :rtype: int
        """
        return helper(root)

```

###  [343. 整数拆分](https://leetcode-cn.com/problems/integer-break/)

#### 题目描述

给定一个正整数 n，将其拆分为至少两个正整数的和，并使这些整数的乘积最大化。 返回你可以获得的最大乘积。

示例 1:

输入: 2
输出: 1
解释: 2 = 1 + 1, 1 × 1 = 1。
示例 2:

输入: 10
输出: 36
解释: 10 = 3 + 3 + 4, 3 × 3 × 4 = 36。
说明: 你可以假设 n 不小于 2 且不大于 58。

#### 解法

##### 解法一

```python
class Solution:
    def integerBreak(self, n: int) -> int:
        if n < 3:
            return 1
        dp = [0] * (n + 1)
        dp[1] = 1
        dp[2] = 1
        for i in range(3, n + 1):
            for j in range(1, i):
                # ****
                dp[i] = max(dp[i], j * dp[i - j], j * (i - j))
        return dp[n]
```

##### 解法二

```python

```

### [357. 计算各个位数不同的数字个数](https://leetcode-cn.com/problems/count-numbers-with-unique-digits/)

#### 题目描述

给定一个非负整数 n，计算各位数字都不同的数字 x 的个数，其中 0 ≤ x < 10n 。

示例:

输入: 2
输出: 91 
解释: 答案应为除去 11,22,33,44,55,66,77,88,99 外，在 [0,100) 区间内的所有数字。


#### 解法

##### 解法一 DP
```python
class Solution:
    def countNumbersWithUniqueDigits(self, n: int) -> int:
        """
            DP
                n=1: res=10

                n=2: res=9*9+10=91 # 两位数第一位只能为1-9，第二位只能为非第一位的数，加上一位数的所有结果

                n=3: res=9 * 9 * 8+91=739 # 三位数第一位只能为1-9，第二位只能为非第一位的数，第三位只能为非前两位的数，加上两位数的所有结果

                n=4: res=9 * 9 * 8 * 7+739=5275 # 同上推法
    
        """
        dp = [0] * (n + 1)
        dp[0] = 1
        for i in range(1, n + 1):
            dp[i] = 9
            for j in range(i - 1):
                dp[i] = dp[i] * (9 - j) 
            dp[i] += dp[i - 1]
        return dp[-1]
                
```

##### 解法二
```python
    def countNumbersWithUniqueDigits(self, n: int) -> int:
        if not n:
            return 1
        res, muls = 10, 9
        for i in range(1, min(n,10)):
            muls *= 10 - i
            res += muls
        return res

```

###368.最大整数子集 [推荐]

#### 题目描述

给出一个由无重复的正整数组成的集合，找出其中最大的整除子集，子集中任意一对 (Si，Sj) 都要满足：Si % Sj = 0 或 Sj % Si = 0。

如果有多个目标子集，返回其中任何一个均可。

 

示例 1:

输入: [1,2,3]
输出: [1,2] (当然, [1,3] 也正确)
示例 2:

输入: [1,2,4,8]
输出: [1,2,4,8]


#### 解法

##### 解法一 dp
dp[i] 存储的是到排序后的第i个最长子集

```python
class Solution(object):
    def largestDivisibleSubset(self, nums):
        """
        :type nums: List[int]
        :rtype: List[int]
        """

        if not nums: return nums
        if len(nums) == 1: return nums
        l = len(nums)
        nums.sort()

        dp = [[i] for i in nums]
        
        for i in range(1, l):
            for j in range(i-1, -1, -1):
                if nums[i]%nums[j] == 0:
                    dp[i] = max(dp[j] + [nums[i]], dp[i],key=len)

        return max(dp,key=len)
```

### 746.使用最小花费爬楼梯

#### 746.1.题目描述

数组的每个索引做为一个阶梯，第 `i`个阶梯对应着一个非负数的体力花费值 `cost[i]`(索引从0开始)。

每当你爬上一个阶梯你都要花费对应的体力花费值，然后你可以选择继续爬一个阶梯或者爬两个阶梯。

您需要找到达到楼层顶部的最低花费。在开始时，你可以选择从索引为 0 或 1 的元素作为初始阶梯。

**示例 1:**

```
输入: cost = [10, 15, 20]
输出: 15
解释: 最低花费是从cost[1]开始，然后走两步即可到阶梯顶，一共花费15。
```

 **示例 2:**

```
输入: cost = [1, 100, 1, 1, 1, 100, 1, 1, 100, 1]
输出: 6
解释: 最低花费方式是从cost[0]开始，逐个经过那些1，跳过cost[3]，一共花费6。
```

**注意：**

1. `cost` 的长度将会在 `[2, 1000]`。
2. 每一个 `cost[i]` 将会是一个Integer类型，范围为 `[0, 999]`。

#### 746.2.解法

题目不是很清楚，顶层不是最后一级台阶

##### 746.2.1 方法一 DP

```java
class Solution {
    public int minCostClimbingStairs(int[] cost) {
        if(cost.length == 0){
            return 0;
        }
        if(cost.length == 1){
            return cost[0];
        }
        int[] dp = new int[cost.length+1];
        dp[0] = 0;
        dp[1] = cost[0];
        for(int i = 2; i <= cost.length; i++){
            dp[i] = Math.min(dp[i-1], dp[i-2]) + cost[i-1];
        }
        return Math.min(dp[cost.length], dp[cost.length-1]);
    }
}
```

##### 746.2.2 方法二 

![image-20190512125230728](/Users/zhengxinzhi/typora_img/LeetCode题目总结/image-20190512125230728.png)

```java
class Solution {
    public int minCostClimbingStairs(int[] cost) {
        int f1 = 0, f2 = 0;
        for(int i = cost.length - 1; i >= 0; --i){
            int f0 = cost[i] + Math.min(f1, f2);
            f2 = f1;
            f1 = f0;
        }
        return Math.min(f1, f2);
    }
}
```

### 1025.除数博弈 看到这类题先试着打印结果 [找规律]

#### 1025.1.题目描述

爱丽丝和鲍勃一起玩游戏，他们轮流行动。爱丽丝先手开局。

最初，黑板上有一个数字 `N` 。在每个玩家的回合，玩家需要执行以下操作：

- 选出任一 `x`，满足 `0 < x < N` 且 `N % x == 0` 。
- 用 `N - x` 替换黑板上的数字 `N` 。

如果玩家无法执行这些操作，就会输掉游戏。

只有在爱丽丝在游戏中取得胜利时才返回 `True`，否则返回 `false`。假设两个玩家都以最佳状态参与游戏。

**示例 1：**

```
输入：2
输出：true
解释：爱丽丝选择 1，鲍勃无法进行操作。
```

**示例 2：**

```
输入：3
输出：false
解释：爱丽丝选择 1，鲍勃也选择 1，然后爱丽丝无法进行操作。
```

 

*提示：**

1. `1 <= N <= 1000`

#### 1025.2.解法
##### 1025.2.1 方法一 数学归纳法

```java
class Solution {
    public boolean divisorGame(int N) {
        //题解思路：两人选择的每一步都是要考虑选择此步是否对自己有利（后续每步的操作），分析可知，若N是偶数，在爱丽丝每步都是以最佳状态 
        // 选择的情况下，爱丽丝可以赢得比赛；若N是奇数，在鲍勃以最佳状态选择的情况下，鲍勃可以赢得比赛
				return N % 2 == 0;
    }
}
```

##### 1025.2.2 方法二 DP

```java
class Solution {
    public boolean divisorGame(int N) {
        // 初始化的值均为false
        boolean[] dp = new boolean[N+1];
        dp[0] = false;
        dp[1] = false;
        for(int i = 2; i <= N; i++){
            // 在某一个数中找到对方不利的结果
            for(int j = 1; j < i; j++){
                if(i % j == 0 && dp[i-j] == false){
                    dp[i] = true;
                    break;
                }else{
                    dp[i] = false; 
                }
            }
        }
        return dp[N];
    }
```

##### 

## 设计

### 146.LRU缓存机制 [高频面试题]

#### 题目描述

运用你所掌握的数据结构，设计和实现一个  LRU (最近最少使用) 缓存机制。它应该支持以下操作： 获取数据 get 和 写入数据 put 。

获取数据 get(key) - 如果密钥 (key) 存在于缓存中，则获取密钥的值（总是正数），否则返回 -1。
写入数据 put(key, value) - 如果密钥不存在，则写入其数据值。当缓存容量达到上限时，它应该在写入新数据之前删除最近最少使用的数据值，从而为新的数据值留出空间。

进阶:

你是否可以在 O(1) 时间复杂度内完成这两种操作？

示例:

LRUCache cache = new LRUCache( 2 /* 缓存容量 */ );

cache.put(1, 1);
cache.put(2, 2);
cache.get(1);       // 返回  1
cache.put(3, 3);    // 该操作会使得密钥 2 作废
cache.get(2);       // 返回 -1 (未找到)
cache.put(4, 4);    // 该操作会使得密钥 1 作废
cache.get(1);       // 返回 -1 (未找到)
cache.get(3);       // 返回  3
cache.get(4);       // 返回  4


#### 解法

##### 解法一 双向链表+哈希表
```python
class ListNode:
    def __init__(self, key=None, value=None):
        self.key = key
        self.value = value
        self.prev = None
        self.next = None


# 这题目重的删除最近最少使用不是按照次数，而是按照put和get的时间
# 解决方法 hashmap + bi_linked_list(双向链表)
class LRUCache:
    def __init__(self, capacity: int):
    	self.capacity = capacity
    	self.hashmap = dict()
    	# 新建两个结点 头结点 尾结点
    	self.head = ListNode()
    	self.tail = ListNode()
    	# 初始化链表为head <-> tail
    	self.head.next = self.tail
    	self.tail.prev = self.head

	# 因为get与put操作都可能需要将双向链表中的某个节点移到末尾，所以定义一个方法
	# key: ** O(1)
    def move_node_to_tail(self, key):
    	node = self.hashmap[key]
    	node.prev.next = node.next
    	node.next.prev = node.prev

    	node.prev = self.tail.prev
    	node.next = self.tail
    	self.tail.prev.next = node
    	self.tail.prev = node

    def get(self, key: int) -> int:
    	if key in self.hashmap:
    		self.move_node_to_tail(key)
    	res = self.hashmap.get(key, -1)
    	if res == -1:
    		return res
    	else:
    		return res.value


    def put(self, key: int, value: int) -> None:
        if key in self.hashmap:
            # 如果key本身已经在哈希表中了就不需要在链表中加入新的节点
            # 但是需要更新字典该值对应节点的value
            self.hashmap[key].value = value
            # 之后将该节点移到末尾
            self.move_node_to_tail(key)
        else:
            if len(self.hashmap) == self.capacity:
                # 去掉哈希表对应项
                self.hashmap.pop(self.head.next.key)
                # 去掉最久没有被访问过的节点，即头节点之后的节点
                self.head.next = self.head.next.next
                self.head.next.prev = self.head
            # 如果不在的话就插入到尾节点前
            new = ListNode(key, value)
            self.hashmap[key] = new
            new.prev = self.tail.prev
            new.next = self.tail
            self.tail.prev.next = new
            self.tail.prev = new

# Your LRUCache object will be instantiated and called as such:
# obj = LRUCache(capacity)
# param_1 = obj.get(key)
# obj.put(key,value)
```

##### 解法二
```python

```

