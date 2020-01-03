# 剑指offer

## Java语法

##### 1.StringBuffer: setLength | charAt | setCharAt | replace | toString

##### 2.Arrays.copyOfRange 类似于Python切片

##### 3.Java中 ^ 表示异或，而不是幂， java.lang.Math.pow(arg0, arg1)

##### 4.-1 >> 1 == -1 死循环

##### 5.左移优先级低于比较  使用技巧 ((n>>1) == 1）

##### 6.判断两浮点数是否相等 

###### **比较double数据是否相等的方法**

**方法一：**若精度要求不高，比如因为传感器有误差，小于0.001的数都可以认为等于0，那么就定义epsilon = 0.001:

**方法二：**转换成字符串之后用equals方法比较 

如果要比较的两个double数据的字符串精度相等，可以将数据转换成String然后借助String的equals方法来间接实现比较两个double数据是否相等。

```java
Double.toString(double_x).equals(Double.toString(double_y))
```

注意：这种方法只适用于比较精度相同的数据，并且是只用用于比较是否相等的情况下，不能用来判断大小。
**方法三：**转换成Long之后用==方法比较

使用Sun提供的Double.doubleToLongBits()方法，该方法可以将double转换成long型数据，从而可以使double按照long的方法（<, >, ==）判断是否大小和是否相等。

```java
Double.doubleToLongBits(0.01) == Double.doubleToLongBits(0.01)
Double.doubleToLongBits(0.02) > Double.doubleToLongBits(0.01)
Double.doubleToLongBits(0.02) < Double.doubleToLongBits(0.01) 
```

**方法四：**使用BigDecimal类型的equals方法或compareTo方法

类加载：

```java
import java.math.BigDecimal;
```

使用**字符串形式**的float型和double型构造BigDecimal：BigDecimal(String val)。BigDecimal的euquals方法是先判断要比较的数据类型，如果对象类型一致前提下同时判断精确度(scale)和值是否一致；compareTo方法则不会比较精确度，把精确度低的那个对象转换为高精确度，只比较数值的大小。

```java
System.out.println(new BigDecimal("1.2").equals(new BigDecimal("1.20")));  //输出false  
System.out.println(new BigDecimal("1.2").compareTo(new BigDecimal("1.20")) == 0); //输出true  
System.out.println(new BigDecimal(1.2).equals(new BigDecimal("1.20"))); //输出false
System.out.println(new BigDecimal(1.2).compareTo(new BigDecimal("1.20")) == 0); //输出false  
System.out.println(new BigDecimal(1.2).equals(new BigDecimal(1.20))); //输出true  
```

##### 7.栈 Stack：push pop peek empty

##### 8.非线程安全队列 ArrayDeque(也可做Stack)

 isEmpty | offer offerFirst offerLast | poll pollFirst pollLast | peek peekFirst peekLast

##### 9.<>中不能有基本类型，包装类型

##### 10.随机 java.util.Random —> rand.nextInt(int bound)  [bound(exclusive)]

##### 11.最小堆 最大堆 java.util.PriorityQueue | java.util.Comparator

```java
// 默认实现了一个最小堆。
Queue<Integer> priorityQueue = new PriorityQueue<>(); 

// 实现最大堆 or 把值变为负数使用最小堆
Queue<ListNode> priorityQueue = new PriorityQueue<ListNode>(lists.size(),new Comparator<ListNode>(){
 
            @Override
            public int compare(ListNode o1, ListNode o2) {
                return o1.val-o2.val;
            }
 
        });
```

##### 12.HashMap put

**put**

```
public V put(K key,
             V value)
```

- **Returns:**

  the previous value associated with `key`, or `null` if there was no mapping for `key`. (A `null` return can also indicate that the map previously associated `null` with `key`.)



**values**
**return** Collection<V>
Returns a [`Collection`](https://docs.oracle.com/javase/8/docs/api/java/util/Collection.html) view of the values contained in this map.

##### 13.HashSet add remove

1. ```
   public boolean add(E e)
   ```

   Adds the specified element to this set if it is not already present. More formally, adds the specified element `e` to this set if this set contains no element `e2` such that `(e==null ? e2==null : e.equals(e2))`. If this set already contains the element, the call leaves the set unchanged and returns `false`.

##### 14.Integer.MAX_VALUE || Integer.MIN_VALUE

##### 15.Java中的tuple：Pair（javafx.util.Pair)

| Modifier and Type | Method and Description                                       |
| :---------------- | :----------------------------------------------------------- |
| `boolean`         | `equals(Object o)`Test this `Pair` for equality with another `Object`. |
| `K`               | `getKey()`Gets the key for this pair.                        |
| `V`               | `getValue()`Gets the value for this pair.                    |
| `int`             | `hashCode()`Generate a hash code for this `Pair`.            |
| `String`          | `toString()``String` representation of this `Pair`.          |

##### 16.String join(CharSequence delimiter, CharSequence... elements)

##### 17.双向链表 LinkedList

```
void          addFirst(E object)
void          addLast(E object)
E             get(int location)
E             getFirst()
E             getLast()
E             peek()
E             peekFirst()
E             peekLast()
E             poll()
E             pollFirst()
E             pollLast()
E             pop()
void          push(E e)
E             remove()
E             remove(int location)
boolean       remove(Object object)
E             removeFirst()
E             set(int location, E object)
int           size()
<T> T[]       toArray(T[] contents)
Object[]     toArray()
```

##### 18.Character.getNumericValue | String.valueOf |  'x' - '0' —> 数值型字符转int

##### 19.Character.isDigit(string.charAt(index)) 某一字符是否是数值型

##### 20.数值溢出问题 见将字符串转为整数

              // int型溢出问题, java中int型没有比Integer.MAX_VALUE更大的值
                // System.out.println(Integer.MAX_VALUE);2147483647
                // System.out.println(0x7fffffff);2147483647
                // System.out.println(Integer.MIN_VALUE);-2147483648
                // System.out.println(0x80000000);-2147483648
##### 21.ArrayList 遍历

```java
		for (Iterator iterator = arrayList.iterator(); iterator.hasNext();) {
			System.out.println(iterator.next());
		}

		for(Character c: arrayList){
      
    }
```

##### 22.数组转List

Arrays.asList(1, 2, 3)

## Python知识点

### [`defaultdict`](https://docs.python.org/2/library/collections.html#collections.defaultdict) Examples[¶](https://docs.python.org/2/library/collections.html#defaultdict-examples)

Using `list` as the `default_factory`, it is easy to group a sequence of key-value pairs into a dictionary of lists:

```
>>> s = [('yellow', 1), ('blue', 2), ('yellow', 3), ('blue', 4), ('red', 1)]
>>> d = defaultdict(list)
>>> for k, v in s:
...     d[k].append(v)
...
>>> d.items()
[('blue', [2, 4]), ('red', [1]), ('yellow', [1, 3])]
```

## 思路总结

##### 1.链表，树善用**递归**方法

##### 2.逐元素累乘 逐元素累加 见[238. 除自身以外数组的乘积] [leetcode209长度最小的子数组]

##### 3.双指针法

均从头开始遍历

或者从头和尾夹逼遍历

## 二维数组中的查找

### 题目描述

在一个二维数组中（每个一维数组的长度相同），每一行都按照从左到右递增的顺序排序，每一列都按照从上到下递增的顺序排序。请完成一个函数，输入这样的一个二维数组和一个整数，判断数组中是否含有该整数。

### 解法

#### 解法一 Bruce TO(n^2) SO(1) 

```java
public class Solution {
    public boolean Find(int target, int [][] array) {
        if(array == null || array.length == 0 ){
            return false;
        }
        for(int i = 0; i < array.length; i++){
            for(int j = 0; j < array[0].length; j++){
                if(array[i][j] == target){
                    return true;
                }
            }
        }
        return false;
    }
}
```

#### 解法二 一维数组的二分查找 TO(nlogn) SO(1)

```java
public class Solution {
    public boolean Find(int target, int [][] array) {
        /**
         * 边界条件: 空数组
         * 一维数组二分查找 TO(nlogn) SO(1)
         */
        if(array == null || array.length == 0 || array[0].length == 0){
            return false;
        }
        for(int i = 0; i < array.length; i++){
            // 二分查找
            int left = 0;
            int right = array.length - 1;
            while(left <= right){
                int mid = (left + right) >> 1;
                if(array[i][mid] == target){
                    return true;
                }else if(array[i][mid] > target){
                    right = mid - 1;
                }else{
                    left = mid + 1;
                }
            }
        }
        return false;
    }
}
```

#### 解法三 矩阵二分查找 TO() SO(1)

二分查找思想核心：找到中心点， 该矩阵的中心点即是左下角的元素，

当target > 待比较元素时，向右查找

当target < 带比较元素时，向上查找

```java
public class Solution {
    // 246
    public boolean Find(int target, int [][] array) {
        /**
         * 边界条件: 空数组
         * 二分查找
         * Bruce TO(nlogn) SO(1)
         */
        if(array == null || array.length == 0 || array[0].length == 0){
            return false;
        }
        int h = array.length - 1;
        int w = 0;
        while (h >= 0 && w < array[0].length){
            if(array[h][w] == target){
                return true;
            }else if(array[h][w] < target){
                w++;
            }else if(array[h][w] > target){
                h--;
            }
        }
        return false;
    }
}
```

## 替换空格
### 题目描述

请实现一个函数，将一个字符串中的每个空格替换成“%20”。例如，当字符串为We Are Happy.则经过替换之后的字符串为We%20Are%20Happy。

进阶：在原有字符串上做替换

### 解法

题目本身很简单，但要想有较好的效率， 需要注意到替换后和替换前的字符串长度不一样

#### 解法一 新开辟字符串做替换

运行时间：21ms

占用内存：9340k

```java
public class Solution {
    public String replaceSpace(StringBuffer str) {
        StringBuffer str2 = new StringBuffer();
        for(int i = 0; i < str.length(); i++){
            if(str.charAt(i) == ' '){
                str2.append("%20");
            }else{
                str2.append(str.charAt(i));
            }
        }
        return str2.toString();
    }
}
```



遍历一遍计算有几个空格，初始化一个origin_length+2*space_nums的空字符串，第二次遍历填补上去

#### 解法二 在原字符串上进行替换

运行时间：24ms

占用内存：9752k

在当前字符串替换，怎么替换才更有效率（不考虑java里现有的replace方法）。
      从前往后替换，后面的字符要不断往后移动，要多次移动，所以效率低下
      从后往前，先计算需要多少空间，然后从后往前移动，则每个字符只为移动一次，这样效率更高一点。


```java
public class Solution {
    public static String replaceSpace(StringBuffer str) {
        int cnt = 0;
        int oldLength = str.length();
        for(int i = 0; i < oldLength; i++){
            if(str.charAt(i) == ' '){
                cnt += 1;
            }
        }
        int newLength = oldLength + 2 * cnt;
        str.setLength(newLength);
        for(int i = oldLength - 1, j = newLength - 1; i >= 0; i--, j--){
            if(str.charAt(i) == ' '){
                // stringbuffer.replace start:end-str.length()  end:终止于前一位
                str.replace(j-2, j+1, "%20");
                j -= 2;
            }else{
                str.setCharAt(j, str.charAt(i));
            }
        }
        return str.toString();
    }
}
```

## 从尾到头打印链表
### 题目描述

输入一个链表，按链表值从尾到头的顺序返回一个ArrayList。

### 解法

逆转链表 遍历打印

遍历存入list，逆转list

存入stack中，遍历输出

#### 解法一 逆转list

```java
import java.util.ArrayList;
import java.util.Collections;
public class Solution {
    public ArrayList<Integer> printListFromTailToHead(ListNode listNode) {
        ArrayList list = new ArrayList();
        while(listNode != null){
            list.add(listNode.val);
            listNode = listNode.next;
        }
        Collections.reverse(list);
        return list;
    }
}
// ------ 自定义reverse ------
import java.util.ArrayList;
public class Solution {
    public ArrayList<Integer> printListFromTailToHead(ListNode listNode) {
        ArrayList list = new ArrayList();
        while(listNode != null){
            list.add(listNode.val);
            listNode = listNode.next;
        }
        // Collections.reverse(list);
        reverse(list);
        return list;
    }
    
    public void reverse(ArrayList<Integer> list){
        int length = list.size();
        for(int i = 0; i < length / 2; i++){
            int tmp = list.get(i);
            list.set(i, list.get(length - 1 - i));
            list.set(length - 1 - i, tmp);
        }
    }
}
```

#### 解法二 递归或stack

```java
import java.util.ArrayList;
public class Solution {
    public ArrayList<Integer> printListFromTailToHead(ListNode listNode) {
        if(listNode == null){
            return new ArrayList();
        }
        ArrayList list = printListFromTailToHead(listNode.next);
        list.add(listNode.val);
        return list;
    }
}
```

## 重建二叉树
### 题目描述

输入某二叉树的前序遍历和中序遍历的结果，请重建出该二叉树。假设输入的前序遍历和中序遍历的结果中都不含重复的数字。例如输入前序遍历序列{1,2,4,7,3,5,6,8}和中序遍历序列{4,7,2,1,5,3,8,6}，则重建二叉树并返回。

### 解法

前序排列：根结点在前 中左右

中序排列：根结点在中 左中右

#### 解法一 递归

善用数组索引



```java
public class Solution {
    public TreeNode reConstructBinaryTree(int [] pre,int [] in) {
        return reConstructBinaryTree(pre, 0, pre.length - 1, in, 0, in.length - 1);
    }
    private TreeNode reConstructBinaryTree(int[] pre, int preStart, int preEnd, int[] in, int inStart, int inEnd) {
        if (preStart > preEnd || inStart > inEnd){
            return null;
        }
        int rootVal = pre[preStart];
        TreeNode root = new TreeNode(rootVal);
        for(int i = inStart; i <= inEnd; i++){
            if(in[i] == rootVal){
                root.left = reConstructBinaryTree(pre, preStart + 1, preStart + (i - inStart), in, inStart, i - 1);
                root.right = reConstructBinaryTree(pre, preStart + (i - inStart) + 1, preEnd, in, i + 1, inEnd);
            }
        }
        return root;
    }
}
```



## 用两个栈实现队列
### 题目描述

用两个栈来实现一个队列，完成队列的Push和Pop操作。 队列中的元素为int类型。

### 解法
#### 解法一 

```java
import java.util.Stack;
public class Solution {
    Stack<Integer> stack1 = new Stack<Integer>();
    Stack<Integer> stack2 = new Stack<Integer>();
    
    public void push(int node) {
        while(!stack1.empty()){
            stack2.push(stack1.pop());
        }
        stack1.push(node);
        while(!stack2.empty()){
            stack1.push(stack2.pop());
        }
    }
    
    public int pop() {
        if(stack1.empty() && stack2.empty()){
            throw new RuntimeException("Error pop");
        }
        return stack1.pop();
    }
}
```



## 旋转数组的最小数字[可重复]
### 题目描述

把一个数组最开始的若干个元素搬到数组的末尾，我们称之为数组的旋转。 输入一个非减排序的数组的一个旋转，输出旋转数组的最小元素。 例如数组{3,4,5,1,2}为{1,2,3,4,5}的一个旋转，该数组的最小值为1。 NOTE：给出的所有元素都大于0，若数组大小为0，请返回0。

### 解法
#### 解法一 二分查找

```java
import java.util.ArrayList;
public class Solution {
    public int minNumberInRotateArray(int [] array) {
        if(array == null || array.length == 0){
            return 0;
        }
        if(array[array.length - 1] > array[0]){
            return array[0];
        }
        int left = 0;
        int right = array.length - 1;
        int mid = 0;
        while(left <= right){
            mid = (left + right) >> 1;
            if (array[mid] > array[mid+1]){
                return array[mid+1];
            }else if(array[mid] < array[right]){
                right = mid;
            }else{
                left = mid;
            }
        }
        return array[0];
    }
}
```

## 斐波那契数列
### 题目描述

大家都知道斐波那契数列，现在要求输入一个整数n，请你输出斐波那契数列的第n项（从0开始，第0项为0）。

n<=39

### 解法
#### 解法一 递归 最差的方法 

运行时间：1273ms

占用内存：9276k

```java
public class Solution {
    public int Fibonacci(int n) {
        if(n == 0 || n == 1){
            return n;
        }
        return Fibonacci(n-1) + Fibonacci(n-2);
    }
}
```

#### 解法二 迭代 DP思想 推荐 TO(n) SO(1)

运行时间：20ms

占用内存：9416k

```java
public class Solution {
    public int Fibonacci(int n) {
        if(n == 0 || n == 1){
            return n;
        }
        int a = 0, b = 1;
        for(int i = 2; i <= n; i++){
            b = a + b;
            a = b - a;
        }
        return b;
    }
}
```

#### 解法三 矩阵乘方+空间换时间 TO(logn) 更快但不实用

![image-20190430163510691](/Users/zhengxinzhi/typora_img/剑指offer/image-20190430163510691.png)

![image-20190430165211762](/Users/zhengxinzhi/typora_img/剑指offer/image-20190430165211762.png)

```c++
//时间复杂度为logN；
//参考程序猿代码面试指南；
class Solution {
public:
    int Fibonacci(int n) {
        if(n<1) return 0;
        if(n==1||n==2) return 1;
        vector<vector<int> > base = {{1,1},{1,0}};
        vector<vector<int> >  res=matrixPower(base, n-2);
        return res[0][0]+res[1][0];
    }
     
    //矩阵相乘
    vector<vector<int> > matrix_multiply(vector<vector<int> > arrA, vector<vector<int> > arrB)
    {
        int rowA=arrA.size();
        int colA=arrA[0].size();
        int colB=arrA[0].size();
        int rowB=arrA.size();
        vector<vector<int> > res (rowA,vector<int> (colB,0));
        if(colA!=rowB) return res;
        for(int i=0;i<rowA;i++)
        {
            for(int j=0;j<colB;j++)
            {
                for(int m=0;m<colA;m++)
                		res[i][j]+=arrA[i][m]*arrB[m][j];
            }
        }
        return res;
    }
     
    vector<vector<int> > matrixPower(vector<vector<int> > a,int p)
    {
        vector<vector<int> > res (a.size(),vector<int> (a[0].size(),0));
        for(int i=0;i<res.size();i++)
        {
            res[i][i]=1;
        }
        vector<vector<int> > tmp(a);
        for(;p!=0;p>>=1)
        {
            if((p&1)!=0)
            {
                res=matrix_multiply(res,tmp);
            }
            tmp=matrix_multiply(tmp,tmp);
        }
        return res;
    }
```



## 跳台阶

### 题目描述

一只青蛙一次可以跳上1级台阶，也可以跳上2级。求该青蛙跳上一个n级的台阶总共有多少种跳法（先后次序不同算不同的结果）。

### 解法

类似斐波那契，除初始条件不同（以1开始）

#### 解法一 迭代

```java
public class Solution {
    public int JumpFloor(int target) {
        if(target == 1 || target == 2){
            return target;
        }
        int a = 1, b = 2;
        for(int i = 3; i <= target; i++){
            b = a + b;
            a = b - a;
        }
        return b;
    }
}
```

## 变态跳青蛙
### 题目描述

一只青蛙一次可以跳上1级台阶，也可以跳上2级……它也可以跳上n级。求该青蛙跳上一个n级的台阶总共有多少种跳法。

### 解法

因为n级台阶，第一步有n种跳法：跳1级、跳2级、到跳n级跳1级，剩下n-1级，则剩下跳法是f(n-1)跳2级，剩下n-2级，则剩下跳法是f(n-2)所以f(n)=f(n-1)+f(n-2)+...+f(1)因为f(n-1)=f(n-2)+f(n-3)+...+f(1)所以f(n)=2*f(n-1)



每个台阶都有跳与不跳两种情况（除了最后一个台阶），最后一个台阶必须跳。所以共用2^(n-1)中情况

#### 解法一 

```java
import java.lang.Math;
public class Solution {
    public int JumpFloorII(int target) {
        return (int)Math.pow(2, target - 1);
    }
}
```

#### 解法二 左移 TO(1)

```java
import java.lang.Math;
public class Solution {
    public int JumpFloorII(int target) {
        return 1 << --target;
    }
}
```

## 矩形覆盖
### 题目描述

我们可以用2*1的小矩形横着或者竖着去覆盖更大的矩形。请问用n个2*1的小矩形无重叠地覆盖一个2*n的大矩形，总共有多少种方法？

### 解法

当第8行的小矩形如果竖着放，那么f(7)
当第8行的小矩形如果横着放，那么f(6)
f(8) = f(7) + f(6) 斐波那契

#### 解法一 
```java
public class Solution {
    public int RectCover(int target) {
        // 当第8行的小矩形如果竖着放，那么f(7)
        // 当第8行的小矩形如果横着放，那么f(6)
        // f(8) = f(7) + f(6)
        // 斐波那契
        if(target < 3){
            return target;
        }
        int a = 1, b = 2;
        for(int i = 3; i <= target; i++){
            b = a + b;
            a = b - a;
        }
        return b;
    }
}
```

## 二进制中1的个数
### 题目描述

输入一个整数，输出该数二进制表示中1的个数。其中负数用补码表示。

### 解法

测试用例：正数 负数 0

#### 解法一 只能处理正数 负数死循环 

-1 >> 1 == -1

```java
public class Solution {
    public int NumberOf1(int n) {
        // 负数补码 = 负数反码 + 1
        // 测试用例 正数 负数 0
        int cnt = 0;
        while(n > 0){
            if((n & 1) == 1){
                cnt += 1;
            }
            n = n >> 1;
        }
        return cnt;
    }
}
```

#### 解法二 

构造一个只有一个位为1的flag

```java
public class Solution {
    public int NumberOf1(int n) {
        // 负数补码 = 负数反码 + 1
        // 测试用例 正数 负数 0
        int flag = 1;
        int cnt = 0;
        while(flag != 0){
            if((n & flag) != 0){
                cnt += 1;
            }
            flag = flag << 1;
        }
        return cnt;
    }
}
```

#### 解法三 推荐

**分析 n & (n-1)的结果**

如果一个整数不为0，那么这个整数至少有一位是1。如果我们把这个整数减1，那么原来处在整数最右边的1就会变为0，原来在1后面的所有的0都会变成1(如果最右边的1后面还有0的话)。其余所有位将不会受到影响。    

​        举个例子：一个二进制数1100，从右边数起第三位是处于最右边的一个1。减去1后，第三位变成0，它后面的两位0变成了1，而前面的1保持不变，因此得到的结果是1011.我们发现减1的结果是把最右边的一个1开始的所有位都取反了。这个时候如果我们再把原来的整数和减去1之后的结果做与运算，从原来整数最右边一个1那一位开始所有位都会变成0。如1100&1011=1000.也就是说，把一个整数减去1，再和原整数做与运算，会把该整数最右边一个1变成0.那么一个整数的二进制有多少个1，就可以进行多少次这样的操作。

```java
public class Solution {
    public int NumberOf1(int n) {
        // 负数补码 = 负数反码 + 1
        // 测试用例 正数 负数 0
        int cnt = 0;
        while (n != 0){
            cnt += 1;
            n = n & (n - 1);
        }
        return cnt;
    }
}
```

## 数值的整数次方
### 题目描述

给定一个double类型的浮点数base和int类型的整数exponent。求base的exponent次方。

### 解法

测试用例：**底数**和**指数** 正数 负数 0

#### 解法一 

```java
public class Solution {
    public double Power(double base, int exponent) {
        // 底数为0 不可做分母
        if(base == 0){
            return 0;
        }
        if(exponent == 0){
            return 1;
        }
        if(exponent < 0){
            exponent = -1 * exponent;
            base = 1 / base;
        }
        double ans = 1.0;
        if ((exponent & 1) == 1){
            ans = base * ans;
        }
        double half = Power(base, exponent >> 1);
        return ans * half * half;
    }
}
```

## 调整数组顺序使奇数位于偶数前面
### 题目描述

输入一个整数数组，实现一个函数来调整该数组中数字的顺序，使得所有的奇数位于数组的前半部分，所有的偶数位于数组的后半部分。

进阶1:   原地调整，不使用额外的空间。

进阶2：并保证奇数和奇数，偶数和偶数之间的相对位置不变。

### 解法

#### 解法一 进阶1 不稳定

维护两指针，第一个指针放在数组第一个数字，从前向后遍历，第二个指针放在数组最后一个数字，从后向前遍历，两指针相遇前，第一个指针指向偶数，第二个指针指向奇数，交换。

```java
public class Solution {
    public void reOrderArray(int [] array) {
        if (array == null || array.length == 0){
            return;
        }
        int i = 0, j = array.length - 1;
        while(i < j){
            while(i < j && check(array[i])){
               i++; 
            }
            while(i < j && !check(array[j])){
                j--;
            }
            if(i < j){
                int tmp = array[i];
                array[i] = array[j];
                array[j] = tmp;
            }
        }
    }
    private boolean check(int num){
        return (num & 1) == 1;
    }
}
```

#### 解法二 进阶二 新建数组 或插入排序

```java
public class Solution {
    public void reOrderArray(int [] array) {
        if (array == null || array.length == 0){
            return;
        }
        int[] narray = new int[array.length];
        int odd_cnt = 0;
        for(int i = 0; i < array.length; i++){
            if((array[i] & 1) == 1){
                narray[odd_cnt] = array[i];
                odd_cnt++;
            }
        }
        for(int i = 0; i < array.length; i++){
            if((array[i] & 1) != 1){
                narray[odd_cnt] = array[i];
                odd_cnt++;
            }
        }
        // array = narray; 错误 该语句是让array指向narray的内存区域，而不是修改narray的内存区域
        for (int i = 0; i< array.length; i++){
            array[i] = narray[i];
        }
    }
}
```

```java
public class Solution {
    public void reOrderArray(int [] array) {
        /**
         * 不稳定法 从两侧逼近
         * 新建数组
         * 插入排序
         */
        if (array == null || array.length == 0){
            return;
        }
        int i = -1;
        for(int j = 0; j < array.length; j++){
            int key = array[j];
            if ((key & 1) == 1){
                int k = j - 1;
                while (k > i){
                    array[k + 1] = array[k];
                    k--;
                }
                array[k + 1] = key;
                i++;
            }
        }
    }
}
```



## 链表中倒数第k个结点

### 题目描述

输入一个链表，输出该链表中倒数第k个结点。

### 解法

双指针，第一个指针先走第k个结点，两个指针同时走，第一个指针走到尾结点，第二个指针即是需要的结点

#### 解法一 遍历一遍

```java
public class Solution {
    public ListNode FindKthToTail(ListNode head,int k) {
        if(k <= 0 || head == null){
            return null;
        }
        // k > lenght 
        ListNode first = head, second = head;
        for(int i = 0; i < k - 1; i++){
            if (first.next == null){
                return null;
            }
            first = first.next;
        }
        while(first.next != null){
            first = first.next;
            second = second.next;
        }
        return second;
    }
}
```

## 反转链表
### 题目描述

输入一个链表，反转链表后，输出新链表的表头。

### 解法
#### 解法一 

```java
public class Solution {
    public ListNode ReverseList(ListNode head) {
        ListNode nhead = null;
        while(head != null){
            ListNode tmp = head;
            head = head.next;
            tmp.next = nhead;
            nhead = tmp;
        }
        return nhead;
    }
}
```

## 合并两个排序的链表
### 题目描述

输入两个单调递增的链表，输出两个链表合成后的链表，当然我们需要合成后的链表满足单调不减规则。

### 解法
#### 解法一 非递归

```java
public class Solution {
    public ListNode Merge(ListNode list1,ListNode list2) {
        if(list1 == null){
            return list2;
        }
        if(list2 == null){
            return list1;
        }
        ListNode ans = null, head = null;
       
        int val = 0;
        while(list1 != null && list2 != null){
            if(list1.val <= list2.val){
                val = list1.val;
                list1 = list1.next;
            }else{
                val = list2.val;
                list2 = list2.next;
            }
            if(head == null){
                head = new ListNode(val);
                ans = head;
            }else{
                ans.next = new ListNode(val);
                ans = ans.next;
            }
        }
        if(list1 != null){
            ans.next = list1;
        }else{
            ans.next = list2;
        }
        return head;
    }
}
```

#### 解法二 递归版本

```java
public class Solution {
    public ListNode Merge(ListNode list1,ListNode list2) {
        if(list1 == null){
            return list2;
        }
        if(list2 == null){
            return list1;
        }
        ListNode head = null;
        if(list1.val <= list2.val){
            head = new ListNode(list1.val);
            head.next = Merge(list1.next, list2);
        }else{
            head = new ListNode(list2.val);
            head.next = Merge(list1, list2.next);
        }
        return head;
    }
}
```

## [收藏]树的子结构
### 题目描述

输入两棵二叉树A，B，判断B是不是A的子结构。（ps：我们约定空树不是任意一个树的子结构）

### 解法

第一步：在树A中找到和树B的根结点的值一样的结点R

第二步：判断树A以R为结点的子树是否包含和树B一样的结构(树A中R是和树B中根结点对应的结点)

#### 解法一 

```java
public class Solution {
    public boolean HasSubtree(TreeNode root1,TreeNode root2) {
        if(root1 == null || root2 == null){
            return false;
        }
        boolean ans = false;
        if(root1.val == root2.val){
            ans = DoesTree1HasEqualRootTree2(root1, root2);
        }
        if(!ans){
            ans = HasSubtree(root1.left, root2);
        }
        if(!ans){
            ans = HasSubtree(root1.right, root2);
        }
        return ans;
    }
    
    private boolean DoesTree1HasEqualRootTree2(TreeNode root1, TreeNode root2){
        /**
         * 同根的树1是否包含同根的树二
         */
        if(root2 == null){
            return true;
        }
        if(root1 == null){
            return false;
        }
        return root1.val == root2.val && DoesTree1HasEqualRootTree2(root1.left, root2.left) 
            && DoesTree1HasEqualRootTree2(root1.right, root2.right);
    }
}
```

#### 解法二 修改1使之易于理解

```java
public class Solution {
    public boolean HasSubtree(TreeNode root1,TreeNode root2) {
        if(root1 == null || root2 == null){
            return false;
        }

        return DoesTree1HasEqualRootTree2(root1, root2) ||
               HasSubtree(root1.left, root2) ||
               HasSubtree(root1.right, root2);
    }

    private boolean DoesTree1HasEqualRootTree2(TreeNode root1, TreeNode root2){
        /**
         * 同根的树1是否包含同根的树二
         */
        if(root2 == null){
            return true;
        }
        if(root1 == null){
            return false;
        }
        return root1.val == root2.val && DoesTree1HasEqualRootTree2(root1.left, root2.left) 
            && DoesTree1HasEqualRootTree2(root1.right, root2.right);
    }
}
```



## 二叉树的镜像
### 题目描述

操作给定的二叉树，将其变换为源二叉树的镜像。

### 输入描述:

```
二叉树的镜像定义：源二叉树 
    	    8
    	   /  \
    	  6   10
    	 / \  / \
    	5  7 9 11
    	镜像二叉树
    	    8
    	   /  \
    	  10   6
    	 / \  / \
    	11 9 7  5
```

### 解法
#### 解法一 递归

```java
public class Solution {
    public void Mirror(TreeNode root) {
        MirrorHelp(root);
    }
    public TreeNode MirrorHelp(TreeNode root){
        if(root == null){
            return null;
        }
        TreeNode left = MirrorHelp(root.right);
        TreeNode right = MirrorHelp(root.left);
        root.left = left;
        root.right = right;
        return root;
    }
}
```

```c++
import java.util.Stack;
public class Solution {
    public void Mirror(TreeNode root) {
        if(root == null){
            return;
        }
        TreeNode tmp = root.left;
        root.left = root.right;
        root.right = tmp;
        Mirror(root.left);
        Mirror(root.right);   
    }
}
```

#### 解法二 DFS

```java
import java.util.Stack;
public class Solution {
    public void Mirror(TreeNode root) {
        if(root == null){
            return;
        }
        Stack<TreeNode> stack = new Stack<>();
        stack.push(root);
        while(!stack.empty()){
            TreeNode node = stack.pop();
            TreeNode temp = node.left;
            node.left = node.right;
            node.right = temp;
            if(node.left != null){
                stack.push(node.left);
            }
            if(node.right != null){
                stack.push(node.right);
            }
        }
    }
}
```

## [收藏]顺时针打印矩阵 
### 题目描述

输入一个矩阵，按照从外向里以顺时针的顺序依次打印出每一个数字，例如，如果输入如下4 X 4矩阵： 1 2 3 4 5 6 7 8 9 10 11 12 13 14 15 16 则依次打印出数字1,2,3,4,8,12,16,15,14,13,9,5,6,7,11,10.

### 解法

画图，发现结果由一个个同心圈组成

测试案例，多行多列 单行 单列 单行单列

思路：题目分成两个小问题：

1. 找到每一圈的其实坐标 i < (Math.min(rows, cols)+1)/2 
2. 找到循环终止条件

#### 解法一 

```java
import java.util.ArrayList;
import java.lang.Math;
public class Solution {
    public ArrayList<Integer> printMatrix(int [][] matrix) {
        if(matrix == null || matrix.length == 0){
            return new ArrayList<Integer>();
        }
        int rows = matrix.length;
        int cols = matrix[0].length;
        ArrayList<Integer> printList = new ArrayList<>(rows * cols);
        // (i, i)代表每一圈的起始元素
        for(int i = 0; i < (Math.min(rows, cols)+1)/2; i++){
            printCircle(matrix, rows, cols, i, printList);
        }
        return printList;
    }
    private void printCircle(int[][] matrix, int rows, int cols, int start, ArrayList<Integer> list){
        // 终止列索引
        int endX = cols - 1 - start;
        // 终止行索引
        int endY = rows - 1 - start;
        // 从左到右打印一行
        for(int j = start; j <= endX; j++){
            list.add(matrix[start][j]);
        }
        // 从上到下打印一列
        for(int j = start + 1; j <= endY; j++){
            list.add(matrix[j][endX]);
        }
        // 从右到左打印一行
        for(int j = endX - 1; (j >= start) && (start < endX && start < endY); j--){
            list.add(matrix[endY][j]);
        }
        // 从下到上打印一列
        for(int j = endY - 1; (j > start) && (start < endX && start < endY - 1) ; j--){
            list.add(matrix[j][start]);
        }
    }
}
```

```java
import java.util.ArrayList;
import java.lang.Math;
public class Solution {
    public ArrayList<Integer> printMatrix(int [][] matrix) {
        if(matrix == null || matrix.length == 0){
            return new ArrayList<Integer>();
        }
        int rows = matrix.length;
        int cols = matrix[0].length;
        ArrayList<Integer> printList = new ArrayList<>(rows * cols);
        // (i, i)代表每一圈的起始元素
        for(int i = 0; i < (Math.min(rows, cols)+1)/2; i++){
            printCircle(matrix, rows, cols, i, printList);
        }
        return printList;
    }
    private void printCircle(int[][] matrix, int rows, int cols, int start, ArrayList<Integer> list){
        int endX = rows - start - 1;
        int endY = cols - start - 1;
        for(int i = start;i <= endY; i++){
            list.add(matrix[start][i]);
        }
        for(int i = start + 1; i <= endX; i++){
            list.add(matrix[i][endY]);
        }
        if (start != endX){
            for(int i = endY - 1; i >= start; i--){
                list.add(matrix[endX][i]);
            }
        }
        if (start != endY){
            for(int i = endX - 1; i > start; i--){
                list.add(matrix[i][start]);
            }        
        }
    }
}
```



## 包含min函数的栈

### 题目描述

定义栈的数据结构，请在该类型中实现一个能够得到栈中所含最小元素的min函数（时间复杂度应为O（1））。

### 解法

看到O(1)想到，hash或者数组，或者一一对应的数据结构(本题为辅助栈)

定义一个辅助栈，每次把一个元素推入栈，都把当前时刻的最小值存入辅助栈

#### 解法一 

```java
import java.util.Stack;
import java.lang.Math;
public class Solution {

       private Stack<Integer> mainStack = new Stack<>();
        private Stack<Integer> secondStack = new Stack<>();
        public void push(int node) {
            mainStack.push(node);
            if(!secondStack.empty()){
                secondStack.push(Math.min(node, secondStack.peek()));
            }else{
                secondStack.push(node);
            }

        }

        public void pop() {
            if(mainStack.empty()){
                throw new RuntimeException("null");
            }
            secondStack.pop();
            mainStack.pop();
        }

        public int top() {
            if(mainStack.empty()){
                throw new RuntimeException("null");
            }
            return mainStack.peek();
        }

        public int min() {
            if(mainStack.empty()){
                throw new RuntimeException("null");
            }
            return secondStack.peek();
        }
}
```

## 栈的压入和弹出序列
### 题目描述

输入两个整数序列，第一个序列表示栈的压入顺序，请判断第二个序列是否可能为该栈的弹出顺序。假设压入栈的所有数字均不相等。例如序列1,2,3,4,5是某栈的压入顺序，序列4,5,3,2,1是该压栈序列对应的一个弹出序列，但4,3,5,1,2就不可能是该压栈序列的弹出序列。（注意：这两个序列的长度是相等的）

### 解法
#### 解法一 用ArrayList

```java
import java.util.ArrayList;

public class Solution {
    public boolean IsPopOrder(int [] pushA,int [] popA) {
        ArrayList<Integer> list = new ArrayList<>(pushA.length);
        int j = 0;
        for(int i = 0; i < pushA.length; i++){
            list.add(pushA[i]);
            // 判断索引校验
            while(j < popA.length && list.get(list.size()-1) == popA[j]){
                list.remove(list.size()-1);
                j++;
            }
        }
        return j == popA.length;
    }
}
```

#### 解法二 用Stack 推荐

```java
import java.util.Stack;

public class Solution {
    public boolean IsPopOrder(int [] pushA,int [] popA) {
        Stack<Integer> stack = new Stack<>();
        int j = 0;
        for(int i = 0; i < pushA.length; i++){
            stack.push(pushA[i]);
            // 判断索引校验
            while(j < popA.length && stack.peek() == popA[j]){
                stack.pop();
                j++;
            }
        }
        return stack.empty();
    }
}
```

## 从上往下打印二叉树
### 题目描述

从上往下打印出二叉树的每个节点，同层节点从左至右打印。

### 解法

BFS

#### 解法一 BFS

```java
import java.util.ArrayList;
import java.util.ArrayDeque;
/**
public class TreeNode {
    int val = 0;
    TreeNode left = null;
    TreeNode right = null;

    public TreeNode(int val) {
        this.val = val;

    }

}
*/
public class Solution {
    public ArrayList<Integer> PrintFromTopToBottom(TreeNode root) {
        // BFS
        if(root == null){
            return new ArrayList<Integer>();
        }
        ArrayList<Integer> list = new ArrayList<>();
        ArrayDeque<TreeNode> queue = new ArrayDeque();
        queue.offer(root);
        TreeNode node = null;
        while(!queue.isEmpty()){
            node = queue.poll();
            list.add(node.val);
            if(node.left != null){
                queue.offer(node.left);
            }
            if(node.right != null){
                queue.offer(node.right);
            }
        }
        return list;
    }
}
```

## [收藏]二叉搜索树的后序遍历序列
### 题目描述

输入一个整数数组，判断该数组是不是某二叉搜索树的后序遍历的结果。如果是则输出Yes,否则输出No。假设输入的数组的任意两个数字都互不相同。

### 解法
#### 解法一 递归

**已知条件**：**后序序列最后一个值为root；二叉搜索树左子树值都比root小，右子树值都比root大。** 

   **1、确定root；** 

   **2、遍历序列（除去root结点），找到第一个大于root的位置，则该位置左边为左子树，右边为右子树；** 

   **3、遍历右子树，若发现有小于root的值，则直接返回false；** 

   **4、分别判断左子树和右子树是否仍是二叉搜索树（即递归步骤1、2、3）。**

```java
import java.util.Arrays;
public class Solution {
    public boolean VerifySquenceOfBST(int [] sequence) {
        // [] 为 false
        if(sequence == null || sequence.length == 0){
            return false;
        }
        return verifyHelper(sequence);
    }
    
    private boolean verifyHelper(int [] sequence){
        if(sequence.length == 0){
            return true;
        }
        int root = sequence[sequence.length-1];
        int left = 0;
        // 找到比root小的元素
        while(left < sequence.length - 1 && sequence[left] < root){
            left++;
        }
        // 判断right做面都小于root，右面都大于root
        int right = left;
        while(right < sequence.length - 1){
            if(sequence[right] < root){
                return false;
            }
            right++;
        }
        // 递归判断子树是否是二叉搜索树的后序遍历
        return verifyHelper(Arrays.copyOfRange(sequence, 0, left)) && 
               verifyHelper(Arrays.copyOfRange(sequence, left, right));
    }
}
```

#### 解法二 非递归 推荐 后序遍历的规律

```c++
//非递归 
//非递归也是一个基于递归的思想：
//左子树一定比右子树小，因此去掉根后，数字分为left，right两部分，right部分的
//最后一个数字是右子树的根他也比左子树所有值大，因此我们可以每次只看有子树是否符合条件
//即可，即使到达了左子树左子树也可以看出由左右子树组成的树还想右子树那样处理
 
//对于左子树回到了原问题，对于右子树，左子树的所有值都比右子树的根小可以暂时把他看出右子树的左子树
//只需看看右子树的右子树是否符合要求即可
class Solution {
public:
    bool VerifySquenceOfBST(vector<int> sequence) {
        int size = sequence.size();
        if(0==size)return false;
 
        int i = 0;
        while(--size)
        {
            while(sequence[i++]<sequence[size]);
            while(sequence[i++]>sequence[size]);
 
            if(i<size)return false;
            i=0;
        }
        return true;
    }
```

## 二叉树中和为某一值的路径
### 题目描述

输入一颗二叉树的根节点和一个整数，打印出二叉树中结点值的和为输入整数的所有路径。路径定义为从树的根结点开始往下一直到叶结点所经过的结点形成一条路径。(注意: 在返回值的list中，数组长度大的数组靠前)

### 解法
#### 解法一 递归

```java
public class Solution {
    public ArrayList<ArrayList<Integer>> FindPath(TreeNode root,int target) {
        if(root == null){
            return new ArrayList<ArrayList<Integer>>();
        }

        ArrayList<ArrayList<Integer>> result = new ArrayList<>();
        ArrayList<Integer> item = new ArrayList<>();
        dfsHelper(item, result, root, target);
        return result;
    }

    private void dfsHelper(ArrayList<Integer> item, ArrayList<ArrayList<Integer>> result,
                      TreeNode root, int target){
        item.add(root.val);
        if(target == root.val && root.left == null && root.right == null){
            result.add(item);
            return;
        }
        if(root.left != null){
            dfsHelper(new ArrayList<Integer>(item), result, root.left, target - root.val);
        }
        if(root.right != null){
            dfsHelper(new ArrayList<Integer>(item), result, root.right, target - root.val);
        }
    }
}
```

```java
public class Solution {
    private ArrayList<ArrayList<Integer>> listAll = new ArrayList<ArrayList<Integer>>();
    private ArrayList<Integer> list = new ArrayList<Integer>();
    public ArrayList<ArrayList<Integer>> FindPath(TreeNode root,int target) {
        if(root == null) return listAll;
        list.add(root.val);
        target -= root.val;
        if(target == 0 && root.left == null && root.right == null)
            listAll.add(new ArrayList<Integer>(list));
        FindPath(root.left, target);
        FindPath(root.right, target);
        list.remove(list.size()-1);
        return listAll;
    }
}
```



#### 解法二 非递归版本

```c++
//非递归版本
//思路：
1.按先序遍历把当前节点cur的左孩子依次入栈同时保存当前节点，每次更新当前路径的和sum；
2.判断当前节点是否是叶子节点以及sum是否等于expectNumber，如果是，把当前路径放入结果中。
3.遇到叶子节点cur更新为NULL，此时看栈顶元素，如果栈顶元素的把栈顶元素保存在last变量中，同时弹出栈顶元素，当期路径中栈顶元素弹出，sum减掉栈顶元素，这一步骤不更改cur的值；
4.如果步骤3中的栈顶元素的右孩子存在且右孩子之前没有遍历过，当前节点cur更新为栈顶的右孩子，此时改变cur=NULL的情况。
#include <iostream>
#include <vector>
 
using namespace std;
 
struct TreeNode{
    int val;
    TreeNode *left;
    TreeNode *right;
    TreeNode(int x) : val(x), left(NULL), right(NULL){}
}
 
vector<vector<int> > FindPath(TreeNode *root, int expectNumber){
    vector<vector<int> > res;   
    if (root == NULL)
        return res;
    stack<TreeNode *> s;
    s.push(root);
    int sum = 0; //当前和
    vector<int> curPath; //当前路径
    TreeNode *cur = root; //当前节点
    TreeNode *last = NULL; //保存上一个节点
    while (!s.empty()){
        if (cur == NULL){
            TreeNode *temp = s.top();
            if (temp->right != NULL && temp->right != last){
                cur = temp->right; //转向未遍历过的右子树
            }else{
                last = temp; //保存上一个已遍历的节点
                s.pop();
                curPath.pop_back(); //从当前路径删除
                sum -= temp->val;
            }  }
        else{
            s.push(cur);
            sum += cur->val;
            curPath.push_back(cur->val);
            if (cur->left == NULL && cur->right == NULL && sum == expectNum){
                res.push_back(curPath);
            }
            cur = cur->left; //先序遍历，左子树先于右子树
        }
    }
    return res;
}

```

## 复杂链表的复制
### 题目描述

输入一个复杂链表（每个节点中有节点值，以及两个指针，一个指向下一个节点，另一个特殊指针指向任意一个节点），返回结果为复制后复杂链表的head。（注意，输出结果中请不要返回参数中的节点引用，否则判题程序会直接返回空）

### 解法

Hash

#### 解法一 两个Map

// 构建 旧结点 -> 旧结点索引 map

 // 构建 新结点索引 -> 新结点 map

```java
/*
public class RandomListNode {
    int label;
    RandomListNode next = null;
    RandomListNode random = null;

    RandomListNode(int label) {
        this.label = label;
    }
}
*/
import java.util.HashMap;
public class Solution {
    public RandomListNode Clone(RandomListNode pHead)
    {
        RandomListNode cur = pHead, newHead = null, newCur = null;
        HashMap<RandomListNode, Integer> nodeMap = new HashMap<>();
        HashMap<Integer, RandomListNode> intMap = new HashMap<>();
        
        // 构建 旧结点 -> 旧结点索引 map
        // 构建 新结点索引 -> 新结点 map
        int i = 0;
        while(cur != null){
            if(newHead == null){
                newCur = new RandomListNode(cur.label);
                newHead = newCur;
            }else{
                newCur.next = new RandomListNode(cur.label);
                newCur = newCur.next;
            }
            intMap.put(i, newCur);
            nodeMap.put(cur, i);
            cur = cur.next;
            i++;
        }
        // 构建随机结点
        cur = pHead;
        newCur = newHead;
        while(cur != null){
            newCur.random = intMap.get(nodeMap.get(cur.random));
            cur = cur.next;
            newCur = newCur.next;
        }
        return newHead;
    }
}
```

#### 解法二 一个map

// 旧结点 -> 新结点 map

```java
/*
public class RandomListNode {
    int label;
    RandomListNode next = null;
    RandomListNode random = null;

    RandomListNode(int label) {
        this.label = label;
    }
}
*/
import java.util.HashMap;
public class Solution {
    public RandomListNode Clone(RandomListNode pHead)
    {
        RandomListNode cur = pHead, newHead = null, newCur = null;
        HashMap<RandomListNode, RandomListNode> nodeMap = new HashMap<>();
        // 构建 旧结点 -> 新结点 map
        while(cur != null){
            if(newHead == null){
                newCur = new RandomListNode(cur.label);
                newHead = newCur;
            }else{
                newCur.next = new RandomListNode(cur.label);
                newCur = newCur.next;
            }
            nodeMap.put(cur, newCur);
            cur = cur.next;
        }
        // 构建随机结点
        cur = pHead;
        newCur = newHead;
        while(cur != null){
            newCur.random = nodeMap.get(cur.random);
            cur = cur.next;
            newCur = newCur.next;
        }
        return newHead;
    }
}
```

解法三 剑指 offer

![img](/Users/zhengxinzhi/typora_img/剑指offer/412362_1489225139482_4A47A0DB6E60853DEDFCFDF08A5CA249.png)

```java
/*
*解题思路：
*1、遍历链表，复制每个结点，如复制结点A得到A1，将结点A1插到结点A后面；
*2、重新遍历链表，复制老结点的随机指针给新结点，如A1.random = A.random.next;
*3、拆分链表，将链表拆分为原链表和复制后的链表
*/
public class Solution {
    public RandomListNode Clone(RandomListNode pHead) {
        if(pHead == null) {
            return null;
        }
         
        RandomListNode currentNode = pHead;
        //1、复制每个结点，如复制结点A得到A1，将结点A1插到结点A后面；
        while(currentNode != null){
            RandomListNode cloneNode = new RandomListNode(currentNode.label);
            RandomListNode nextNode = currentNode.next;
            currentNode.next = cloneNode;
            cloneNode.next = nextNode;
            currentNode = nextNode;
        }
         
        currentNode = pHead;
        //2、重新遍历链表，复制老结点的随机指针给新结点，如A1.random = A.random.next;
        while(currentNode != null) {
            currentNode.next.random = currentNode.random==null?null:currentNode.random.next;
            currentNode = currentNode.next.next;
        }
         
        //3、拆分链表，将链表拆分为原链表和复制后的链表
        currentNode = pHead;
        RandomListNode pCloneHead = pHead.next;
        while(currentNode != null) {
            RandomListNode cloneNode = currentNode.next;
            currentNode.next = cloneNode.next;
            cloneNode.next = cloneNode.next==null?null:cloneNode.next.next;
            currentNode = currentNode.next;
        }
         
        return pCloneHead;
    }
}

```

```c++
class Solution {
public:
    /*
        1、复制每个节点，如：复制节点A得到A1，将A1插入节点A后面
        2、遍历链表，A1->random = A->random->next;
        3、将链表拆分成原链表和复制后的链表
    */
     
    RandomListNode* Clone(RandomListNode* pHead)
    {
        if(!pHead) return NULL;
        RandomListNode *currNode = pHead;
        while(currNode){
            RandomListNode *node = new RandomListNode(currNode->label);
            node->next = currNode->next;
            currNode->next = node;
            currNode = node->next;
        }
        currNode = pHead;
        while(currNode){
            RandomListNode *node = currNode->next;
            if(currNode->random){               
                node->random = currNode->random->next;
            }
            currNode = node->next;
        }
        //拆分
        RandomListNode *pCloneHead = pHead->next;
        RandomListNode *tmp;
        currNode = pHead;
        while(currNode->next){
            tmp = currNode->next;
            currNode->next =tmp->next;
            currNode = tmp;
        }
        return pCloneHead;
    }
};

```

## 二叉搜索树与双向链表
### 题目描述

输入一棵二叉搜索树，将该二叉搜索树转换成一个排序的双向链表。要求不能创建任何新的结点，只能调整树中结点指针的指向。

### 解法
#### 解法一 前序遍历 非递归

```java
/**
public class TreeNode {
    int val = 0;
    TreeNode left = null;
    TreeNode right = null;

    public TreeNode(int val) {
        this.val = val;

    }

}
*/
import java.util.ArrayList;
import java.util.Stack;
public class Solution {
    public TreeNode Convert(TreeNode pRootOfTree) {
        if(pRootOfTree == null){
            return null;
        }
        ArrayList<TreeNode> nodes = new ArrayList<>();
        Stack<TreeNode> stack = new Stack<>();
        // 中序遍历
        TreeNode node = pRootOfTree, nHead = pRootOfTree;
        while(node != null || !stack.empty()){
            if(node != null){
                stack.push(node);
                node = node.left;
            }else{
                node = stack.pop();
                nodes.add(node);
                node = node.right;
            }
        }
        // 正向连接
        nHead = null;
        for(int i = 0; i < nodes.size(); i++){
            if(i == 0){
                node = nodes.get(i);
                nHead = node;
            }else{
                node.right = nodes.get(i);
                node = node.right;
            }
        }
        node.right = null;
        // 反向连接
        for(int i = nodes.size() - 1; i >= 0 ; i--){
            if(i == (nodes.size() - 1)){
                node = nodes.get(i);
            }else{
                node.left = nodes.get(i);
                node = node.left;
            }
        }
        node.left = null;
        return nHead;
    }
}
```

#### 解法二 前序遍历 非递归 推荐

```java
import java.util.Stack;
public class Solution {
    public TreeNode Convert(TreeNode pRootOfTree) {
        if(pRootOfTree == null){
            return null;
        }
        Stack<TreeNode> stack = new Stack<>();
        // 中序遍历
        TreeNode node = pRootOfTree, nHead = null, cur = null;
        while(node != null || !stack.empty()){
            if(node != null){
                stack.push(node);
                node = node.left;
            }else{
                node = stack.pop();
                // --- 修改指针 --
                if(nHead == null){
                    cur = node;
                    nHead = cur;
                }else{
                    cur.right = node;
                    cur.right.left = cur;
                    cur = cur.right;
                }
                // --- 修改指针 --
                node = node.right;
            }
        }
        return nHead;
    }
}
```

#### 解法三 递归

```java
解题思路：
1.将左子树构造成双链表，并返回链表头节点。
2.定位至左子树双链表最后一个节点。
3.如果左子树链表不为空的话，将当前root追加到左子树链表。
4.将右子树构造成双链表，并返回链表头节点。
5.如果右子树链表不为空的话，将该链表追加到root节点之后。
6.根据左子树链表是否为空确定返回的节点。

public class Solution {
    public TreeNode Convert(TreeNode pRootOfTree) {
        if(pRootOfTree == null){
            return null;
        }
        if(pRootOfTree.left == null && pRootOfTree.right == null){
            return pRootOfTree;
        }
        TreeNode leftLink = Convert(pRootOfTree.left);
        TreeNode rightLink = Convert(pRootOfTree.right);
        TreeNode nHead = leftLink, cur = leftLink;
        if(leftLink != null){
            while(cur.right != null){
                cur = cur.right;
            }
            cur.right = pRootOfTree;
            cur.right.left = cur;
        }else{
            nHead = pRootOfTree;
        }
        if(rightLink != null){
            pRootOfTree.right = rightLink;
            pRootOfTree.right.left = pRootOfTree;
        }

        return nHead;
    }
}
```

#### 方法四 中序遍历 递归版

```c++
class Solution {
public:
    TreeNode* Convert(TreeNode* pRootOfTree)
    {
        if(pRootOfTree == nullptr) return nullptr;
        TreeNode* pre = nullptr;
         
        convertHelper(pRootOfTree, pre);
         
        TreeNode* res = pRootOfTree;
        while(res ->left)
            res = res ->left;
        return res;
    }
     
    void convertHelper(TreeNode* cur, TreeNode*& pre)
    {
        if(cur == nullptr) return;
         
        convertHelper(cur ->left, pre);
         
        cur ->left = pre;
        if(pre) pre ->right = cur;
        pre = cur;
         
        convertHelper(cur ->right, pre);
    }
};
```

```java
//递归调用 左 根 右 遍历
public class Solution {
     //双向链表的左边头结点和右边头节点
    TreeNode leftHead = null;
    TreeNode rightHead = null;
    public TreeNode Convert(TreeNode pRootOfTree) {
         //递归调用叶子节点的左右节点返回null
        if(pRootOfTree==null) return null;
          //第一次运行时，它会使最左边叶子节点为链表第一个节点
        Convert(pRootOfTree.left);
        if(rightHead==null){
            leftHead= rightHead = pRootOfTree;
        }else{
            //把根节点插入到双向链表右边，rightHead向后移动
           rightHead.right = pRootOfTree;
           pRootOfTree.left = rightHead;
           rightHead = pRootOfTree;
        }
         //把右叶子节点也插入到双向链表（rightHead已确定，直接插入）
        Convert(pRootOfTree.right);
         //返回左边头结点
        return leftHead;
    }
}
```

## 字符串的排列

### 题目描述

输入一个字符串,按字典序打印出该字符串中字符的所有排列。例如输入字符串abc,则打印出由字符a,b,c所能排列出来的所有字符串abc,acb,bac,bca,cab和cba。

##### 输入描述:

```
输入一个字符串,长度不超过9(可能有字符重复),字符只包括大小写字母。
```

### 解法
#### 解法一 递归

第一步：交换第一个元素和后面所有字符交换

第二步：固定第一个字符，递归得到后面元素的所有排列

```java
import java.util.ArrayList;
import java.util.HashSet;
import java.util.Arrays;
public class Solution {
    public ArrayList<String> Permutation(String str) {
        if(str == null || "".equals(str)){
            return new ArrayList<String>();
        }
        char[] array = str.toCharArray();
        // 字典序输出
        Arrays.sort(array);
        return helper(array);
    }
    
    private ArrayList<String> helper(char[] array){
        ArrayList<String> result = new ArrayList<String>();
        // 这个是为什么不写在主函数的原因，如果主函数是""， 那么返回的不是"", 而是不返回
        if("".equals(new String(array))){
            result.add("");
        }else{
            ArrayList<String> ans = null;
            HashSet<Character> set = new HashSet();
            for(int i = 0; i < array.length; i++){
                // 防止相同元素和第一个元素交换
                if(set.contains(array[i])){
                    continue;
                }
                set.add(array[i]);
                // 交换第一个字符和后面的字符
                char temp = array[0];
                array[0] = array[i];
                array[i] = temp;
                ans = helper(Arrays.copyOfRange(array, 1, array.length));
                // 结果加入
                for(String s: ans){
                    result.add(array[0] + s);
                }
            }
        }
        return result;
    }
}

```

#### 解法二 回溯

```java
import java.util.List;
import java.util.Collections;
import java.util.ArrayList;
 
public class Solution {
 
    public ArrayList<String> Permutation(String str) {
        List<String> res = new ArrayList<>();
        if (str != null && str.length() > 0) {
            PermutationHelper(str.toCharArray(), 0, res);
            // 对结果做一次排序
            Collections.sort(res);
        }
        return (ArrayList)res;
    }
 
    public void PermutationHelper(char[] cs, int i, List<String> list) {
        if (i == cs.length - 1) {
            String val = String.valueOf(cs);
            // 防止字符串有重复字符导致的结果有重合
            if (!list.contains(val))
                list.add(val);
        } else {
            for (int j = i; j < cs.length; j++) {
                swap(cs, i, j);
                PermutationHelper(cs, i+1, list);
                swap(cs, i, j);
            }
        }
    }
 
    public void swap(char[] cs, int i, int j) {
        char temp = cs[i];
        cs[i] = cs[j];
        cs[j] = temp;
    }
}
```

## 数组中出现次数超过一半的数字
### 题目描述

数组中有一个数字出现的次数超过数组长度的一半，请找出这个数字。例如输入一个长度为9的数组{1,2,3,2,2,2,5,4,2}。由于数字2在数组中出现了5次，超过数组长度的一半，因此输出2。如果不存在则输出0。

### 解法
#### 解法一  Hash

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

#### 解法二 Patition

我们回到题目本身分析，就会发现前面的思路并没有考虑到                **数组的特性**              ：数组中有一个数字出现的次数超过了数组长度的一半。如果我把这个数组排序，那么排序之后位于数组中间的数字一定就是那个出现次数超过数组一半的数字。也就是说，这个数字就是统计学上的中位数，即长度为n的数组中第n/2的数字。                **我们有成熟的O(n)的算法得到数组中任意第K大的数字         。**                

​                        这种算法是受快速排序算法的启发。在随机快速排序算法中，我们现在数组中随机选择一个数字，然后调整数组中数字的顺序，使得比选中的数字小的数字都排在它的左边，比选中的数字大的数字都排在它的右边。如果这个选中的数字的下标刚好是n/2，那么这个数字就是数组的中位数。如果它的下标大于n/2，那么中位数应该位于它的左边，我们可以接着在它的左边部分的数组中查找。如果它的下标小于n/2，那么中位数应该位于它的右边，我们可以接着在它的右边部分的数组中查找。这是一个典型的递归过程，实现代码如下：

```java
import java.util.Random;
public class Solution {
    public int MoreThanHalfNum_Solution(int [] array) {
        if(array == null || array.length == 0){
            return 0;
        }
        int mid = array.length >> 1;
        int start = 0;
        int end = array.length - 1;
        int index = random_partition(array, 0, array.length-1);
        while(index != mid){
            if(index >mid){
                end = index - 1;
                index = random_partition(array, start, end);
            }else{
                start = index + 1;
                index = random_partition(array, start, end);
            }
        }
        int ans = array[mid];
        // 判断该元素次数是否超过一半
        int cnt = 0;
        for(int num: array){
            if(num == ans){
                cnt++;
                if(cnt > mid){
                    return num;
                }
            }
        }
        return 0;
    }
    
    private int random_partition(int[] array, int start, int end){
        // random
        Random rand = new Random();
        int index = rand.nextInt(end + 1);
        exchange(array, index, end);
        int i = start - 1;
        for(int j = start; j < end; j++){
            if(array[j] < array[end]){
                exchange(array, ++i, j);
            }
        }
        exchange(array, ++i, end);
        return i;
    }
    
    private void exchange(int[] array, int first, int second){
        int temp = array[first];
        array[first] = array[second];
        array[second] = temp;
    }
}
```

#### 解法三 Boyer-Moore Voting Algorithm(摩尔投票法) + 校验

找出一组数字序列中出现次数大于总数1/2的数字（并且假设这个数字一定存在）。显然这个数字只可能有一个。**摩尔投票算法是基于这个事实：每次从序列里选择两个不相同的数字删除掉（或称为“抵消”），最后剩下一个数字或几个相同的数字，就是出现次数大于总数一半的那个**。请首先认同这个事实，这里不证明了~

```java
import java.util.Random;
public class Solution {
    public int MoreThanHalfNum_Solution(int [] array) {
        if(array == null || array.length == 0){
            return 0;
        }
        int temp = array[0];
        int count = 0;
        for(int num: array){
            if(count == 0){
                temp = num;
                count = 1;
            }else if(num == temp){
                count++;
            }else{
                count--;
            }
        }
        // 判断该元素次数是否超过一半
        count = 0;
        for(int num: array){
            if(num == temp){
                count++;
                if(count > array.length/2){
                    return num;
                }
            }
        }
        return 0;
    }

}
```

## 最小的K个数
### 题目描述

输入n个整数，找出其中最小的K个数。例如输入4,5,1,6,2,7,3,8这8个数字，则最小的4个数字是1,2,3,4,。

### 解法

Bruce O(n**2)

#### 解法一 partition O(n)

限制：修改原数组

```java
import java.util.Random;
import java.util.ArrayList;
public class Solution {
    public ArrayList<Integer> GetLeastNumbers_Solution(int [] input, int k) {
        if(input == null || input.length == 0 || k > input.length){
            return new ArrayList<Integer>();
        }
        // 找到第k个值，且使前k个值都小于等于第k个值
        int start = 0, end = input.length - 1;
        int index = random_partition(input, start, end);
        while(index != k - 1){
            if(index > k - 1){
                end = index - 1;
                index = random_partition(input, start, end);
            }else{
                start = index + 1;
                index = random_partition(input, start, end);
            }
        }
        // 输出前k个值
        ArrayList<Integer> result = new ArrayList<>();
        for (int i = 0; i < k; i++) {
            result.add(input[i]);
        }
        return result;
    }
    
    private int random_partition(int[] array, int start, int end){
        // random
        Random rand = new Random();
        int index = rand.nextInt(end + 1);
        exchange(array, index, end);
        int i = start - 1;
        for(int j = start; j < end; j++){
            if(array[j] < array[end]){
                exchange(array, ++i, j);
            }
        }
        exchange(array, ++i, end);
        return i;
    }
    
    private void exchange(int[] array, int first, int second){
        int temp = array[first];
        array[first] = array[second];
        array[second] = temp;
    }
}
```

#### 解法二 堆 O(nlogk)

```java
import java.util.ArrayList;
import java.util.PriorityQueue;
public class Solution {
    public ArrayList<Integer> GetLeastNumbers_Solution(int [] input, int k) {
        if(input == null || input.length == 0 || k > input.length || k == 0){
            return new ArrayList<>();
        }
        // 默认小顶堆， 想要大顶堆把值变负数
        PriorityQueue<Integer> heap = new PriorityQueue<>(k);
        int [] negInput = new int[input.length];
        for(int i = 0; i < input.length; i++){
            negInput[i] = input[i] * -1;
            if(heap.size() == k){
                if(negInput[i] > heap.peek()){
                    heap.poll();
                    heap.add(negInput[i]);
                }
            }else {
                heap.add(negInput[i]);
            }
        }

        ArrayList<Integer> result = new ArrayList<>();
        for (int i = 0; i < k; i++) {
            result.add(heap.poll() * -1);
        }
        return result;
    }
}
```

```java
import java.util.ArrayList;
import java.util.PriorityQueue;
import java.util.Comparator;
public class Solution {
   public ArrayList<Integer> GetLeastNumbers_Solution(int[] input, int k) {
       ArrayList<Integer> result = new ArrayList<Integer>();
       int length = input.length;
       if(k > length || k == 0){
           return result;
       }
        PriorityQueue<Integer> maxHeap = new PriorityQueue<Integer>(k, new Comparator<Integer>() {
            @Override
            public int compare(Integer o1, Integer o2) {
                return o2.compareTo(o1);
            }
        });
     
        for (int i = 0; i < length; i++) {
            if (maxHeap.size() != k) {
                maxHeap.offer(input[i]);
            } else if (maxHeap.peek() > input[i]) {
                Integer temp = maxHeap.poll();
                temp = null;
                maxHeap.offer(input[i]);
            }
        }
        for (Integer integer : maxHeap) {
            result.add(integer);
        }
        return result;
    }
}
```

## 连续子数组的最大和
### 题目描述

HZ偶尔会拿些专业问题来忽悠那些非计算机专业的同学。今天测试组开完会后,他又发话了:在古老的一维模式识别中,常常需要计算连续子向量的最大和,当向量全为正数的时候,问题很好解决。但是,如果向量中包含负数,是否应该包含某个负数,并期望旁边的正数会弥补它呢？例如:{6,-3,-2,7,-15,1,2,2},连续子向量的最大和为8(从第0个开始,到第3个为止)。给一个数组，返回它的最大连续子序列的和，你会不会被他忽悠住？(子向量的长度至少是1)

### 解法

测试用例

负数 最大也是负数

#### 解法一 分而治之

```java
import java.lang.Math;
import java.util.Arrays;
		public static int FindGreatestSumOfSubArray(int[] array) {
        if(array.length == 0){
            return 0;
        }
        if (array.length == 1) {
            return array[0];
        }
        int mid = (array.length - 1) / 2;
        int leftmax = array[mid], left = array[mid], rightmax = array[mid+1], right = array[mid+1], ans = 0;
        for (int i = mid - 1; i >= 0; i--) {
            left += array[i];
            if (leftmax < left) {
                leftmax = left;
            }
        }
        for (int i = mid + 2; i < array.length; i++) {
            right += array[i];
            if (rightmax < right) {
                rightmax = right;
            }
        }

        left = FindGreatestSumOfSubArray(Arrays.copyOfRange(array, 0, mid + 1));
        right = FindGreatestSumOfSubArray(Arrays.copyOfRange(array, mid + 1, array.length));
        ans = Math.max(left, right);
        ans = Math.max(ans, rightmax + leftmax);
        return ans;
    }
```

#### 解法二 时间窗口

```java
import java.lang.Math;
import java.util.Arrays;
public class Solution {
    public static int FindGreatestSumOfSubArray(int[] array) {
        if(array.length == 0){
            return 0;
        }
        int totalMax = array[0], curMax = array[0];
        for(int i = 1; i < array.length; i++){
            if(curMax <= 0){
                curMax = array[i];
            }else{
                curMax += array[i];
            }
            totalMax = Math.max(totalMax, curMax);
        }
        return totalMax;
    }
}
```

#### 解法三 dp

```java
import java.lang.Math;
import java.util.Arrays;
public class Solution {
    public int FindGreatestSumOfSubArray(int[] array) {
        if(array.length == 0){
            return 0;
        }
        // 以第i个元素为结尾的最大和
        int[] dp = new int[array.length];
        dp[0] = array[0];
        for(int i = 1; i < array.length; i++){
            if(dp[i-1] > 0){
                dp[i] = dp[i-1] + array[i];
            }else{
                dp[i] = array[i];
            }
        }
        return max(dp);
    }
    private int max(int[] array){
        if(array.length == 0){
            return 0;
        }
        int ans = array[0];
        for(int i = 1; i < array.length; i++){
            if(ans < array[i]){
                ans = array[i];
            }
        }
        return ans;
    }
}
```

## [todo]正数中1出现的次数
### 题目描述

求出1~13的整数中1出现的次数,并算出100~1300的整数中1出现的次数？为此他特别数了一下1~13中包含1的数字有1、10、11、12、13因此共出现6次,但是对于后面问题他就没辙了。ACMer希望你们帮帮他,并把问题更加普遍化,可以很快的求出任意非负整数区间中1出现的次数（从1 到 n 中1出现的次数）。

### 解法
#### 解法一 
#### 解法二

## [todo]把数组排成最小的数
### 题目描述

输入一个正整数数组，把数组里所有数字拼接起来排成一个数，打印能拼接出的所有数字中最小的一个。例如输入数组{3，32，321}，则打印出这三个数字能排成的最小数字为321323。

### 解法
#### 解法一 
#### 解法二

## 丑数
### 题目描述

把只包含质因子2、3和5的数称作丑数（Ugly Number）。例如6、8都是丑数，但14不是，因为它包含质因子7。 习惯上我们把1当做是第一个丑数。求按从小到大的顺序的第N个丑数。

### 解法
#### 解法一 
#### 解法二

## 第一个只出现一次的字符
### 题目描述

在一个字符串(0<=字符串长度<=10000，全部由字母组成)中找到第一个只出现一次的字符,并返回它的位置, 如果没有则返回 -1（需要区分大小写）.

### 解法
#### 解法一 TO(n) SO(n)

```java
import java.util.HashMap;
public class Solution {
    public int FirstNotRepeatingChar(String str) {
        if(str == null || "".equals(str)){
            return -1;
        }
        HashMap<Character, Integer> map  = new HashMap<>();
        for(int i = 0; i < str.length(); i++){
            if(map.containsKey(str.charAt(i))){
                map.put(str.charAt(i), map.get(str.charAt(i)) + 1);
            }else{
                map.put(str.charAt(i), 1);
            }
        }
        for(int i = 0; i < str.length(); i++){
            if(map.get(str.charAt(i)) == 1){
                return i;
            }
        }
        return -1;
    }
}s
```

## [收藏]数组中的逆序对
### 题目描述

在数组中的两个数字，如果前面一个数字大于后面的数字，则这两个数字组成一个逆序对。输入一个数组,求出这个数组中的逆序对的总数P。并将P对1000000007取模的结果输出。 即输出P%1000000007

### 输入描述:

```
题目保证输入的数组中没有的相同的数字数据范围：	对于%50的数据,size<=10^4	对于%75的数据,size<=10^5	对于%100的数据,size<=2*10^5
```

示例1

##### 输入

复制

```
1,2,3,4,5,6,7,0
```

##### 输出

复制

```
7
```

### 解法
#### 解法一 Divide and Conquer



#### 解法二

## 两个链表的第一个公共结点

### 题目描述

输入两个链表，找出它们的第一个公共结点。

### 解法
#### 解法一 HashSet

```java
 import java.util.HashSet;
public class Solution {
    public ListNode FindFirstCommonNode(ListNode pHead1, ListNode pHead2) {
        HashSet<ListNode> nodeSet = new HashSet<>();
        ListNode p = pHead1;
        while(p != null){
            nodeSet.add(p);
            p = p.next;
        }
        p = pHead2;
        while(p != null){
            if(nodeSet.contains(p)){
                return p;
            }
            p = p.next;
        }
        return p;
    }
}
```

#### 解法二 空间复杂度O(1) 长度差值

```
找出2个链表的长度，然后让长的先走两个链表的长度差，然后再一起走
（因为2个链表用公共的尾部）
```

```java
import java.util.HashSet;
import java.lang.Math;
public class Solution {
    public ListNode FindFirstCommonNode(ListNode pHead1, ListNode pHead2) {
        int len1 = 0, len2 = 0;
        ListNode p = pHead1;
        while(p != null){
            len1++;
            p = p.next;
        }
        p = pHead2;
        while(p != null){
            len2++;
            p = p.next;
        }
        int diff = len1 - len2;
        for(int i = 0; i < Math.abs(diff); i++){
            if(diff > 0){
                pHead1 = pHead1.next;
            }else{
                pHead2 = pHead2.next;
            }
        }
        while(pHead1 != null){
            if(pHead1 == pHead2){
                return pHead2;
            }
            pHead1 = pHead1.next;
            pHead2 = pHead2.next;
        }
        return null;
    }
}
```

#### 解法三 双指针 

我们可以使用两次迭代来做到这一点。

在第一次迭代中，我们将在到达尾节点之后将一个链表的指针重置到另一个链表的头部。

在第二次迭代中，我们将移动两个指针，直到它们指向同一个节点。我们在第一次迭代中的操作将帮助我们抵消差异。

因此，如果两个链表相交，则第二次迭代中的会合点必须是交点。

如果两个链表完全没有交集，那么第二次迭代中的会议指针必须是两个列的尾节点，即null



```java
public class Solution {
    public ListNode FindFirstCommonNode(ListNode pHead1, ListNode pHead2) {
        int len1 = 0, len2 = 0;
        ListNode p1 = pHead1, p2 = pHead2;
        while(p1 != p2){
            if(p1 != null){
                p1 = p1.next;
            }else{
                p1 = pHead2;
            }
            if(p2 != null){
                p2 = p2.next;
            }else{
                p2 = pHead1;
            }
        }
        return p1;
    }
}
```

## 数字在排序数组中的应用

### 题目描述

统计一个数字在排序数组中出现的次数。

### 解法
#### 解法一 二分法+扩散

```java
public class Solution {
    public int GetNumberOfK(int [] array , int k) {
        if(array == null || array.length == 0){
            return 0;
        }
        int left = 0, right = array.length-1;
        int mid = 0;
        int index = -1, ans = 0;
        while(left <= right){
            mid = (left + right) >> 1;
            if(array[mid] == k){
                index = mid;
                ans++;
                break;
            }else if(array[mid] < k){
                left = mid + 1;
            }else{
                right = mid - 1;
            }
        }
        for(int i = index - 1; i >= 0; i--){
            if(array[i] == k){
                ans++;
            }else{
                break;
            }
        }
        for(int i = index + 1; i < array.length; i++){
            if(array[i] == k){
                ans++;
            }else{
                break;
            }
        }
        return ans;
    }
}
```

#### 解法二 相同数字的索引上下界

**二分法插入**

```java
//由于数组有序，所以使用二分查找方法定位k的第一次出现位置和最后一次出现位置
class Solution {
public:
    int GetNumberOfK(vector<int> data ,int k) {
        int lower = getLower(data,k);
        int upper = getUpper(data,k);
         
        return upper - lower + 1;
    }
     
    //获取k第一次出现的下标
    int getLower(vector<int> data,int k){
        int start = 0,end = data.size()-1;
        int mid = (start + end)/2;
         
        while(start <= end){
            if(data[mid] < k){
                start = mid + 1;
            }else{
                end = mid - 1;
            }
            mid = (start + end)/2;
        }
        return start;
    }
    //获取k最后一次出现的下标
    int getUpper(vector<int> data,int k){
         int start = 0,end = data.size()-1;
        int mid = (start + end)/2;
         
        while(start <= end){
            if(data[mid] <= k){
                start = mid + 1;
            }else{
                end = mid - 1;
            }
            mid = (start + end)/2;
        }
         
        return end;
    }
};

```

```c++
//因为data中都是整数，所以可以稍微变一下，不是搜索k的两个位置，而是搜索k-0.5和k+0.5
//这两个数应该插入的位置，然后相减即可。
class Solution {
public:
    int GetNumberOfK(vector<int> data ,int k) {
        return biSearch(data, k+0.5) - biSearch(data, k-0.5) ;
    }
private:
    int biSearch(const vector<int> & data, double num){
        int s = 0, e = data.size()-1;     
        while(s <= e){
            int mid = (e - s)/2 + s;
            if(data[mid] < num)
                s = mid + 1;
            else if(data[mid] > num)
                e = mid - 1;
        }
        return s;
    }
};
```

## 二叉树的深度

### 题目描述

输入一棵二叉树，求该树的深度。从根结点到叶结点依次经过的结点（含根、叶结点）形成树的一条路径，最长路径的长度为树的深度。

### 解法
#### 解法一 递归

```java
public class Solution {
    public int TreeDepth(TreeNode root) {
        if(root == null){
            return 0;
        }
        return Math.max(TreeDepth(root.left), TreeDepth(root.right)) + 1;
    }
}
```

#### 解法二 dfs

```java
import java.util.Stack;
public class Solution {
    public int TreeDepth(TreeNode root) {
        if(root == null){
            return 0;
        }
        Stack<TreeNode> stack = new Stack<>();
        Stack<Integer> depthStack = new Stack<>();
        stack.push(root);
        depthStack.push(1);
        TreeNode node = null;
        int depth = 0, ans = 0;
        while(!stack.empty()){
            node = stack.pop();
            depth = depthStack.pop();
            if(node.left != null){
                stack.push(node.left);
                depthStack.push(depth + 1);
            }
            if(node.right != null){
                stack.push(node.right);
                depthStack.push(depth + 1);
            }
            ans = Math.max(ans, depth);
        }
        return ans;
    }
}
```

#### 解法三 bfs 层次遍历

```java
/**
public class TreeNode {
    int val = 0;
    TreeNode left = null;
    TreeNode right = null;

    public TreeNode(int val) {
        this.val = val;

    }

}
*/
import java.util.ArrayDeque;
public class Solution {
    public int TreeDepth(TreeNode root) {
        if(root == null){
            return 0;
        }
        ArrayDeque<TreeNode> deque = new ArrayDeque<>();
        deque.offer(root);
        int depth = 0, count = 0, nextCount = 1;
        while(!deque.isEmpty()){
            TreeNode top = deque.poll();
            count++;
            if(top.left != null){
                deque.add(top.left);
            }
            if(top.right != null){
                deque.add(top.right);
            }
            if(count == nextCount){
                nextCount = deque.size();
                count = 0;
                depth++;
            }
        }
        return depth;
    }
}
```

## 平衡二叉树

### 题目描述

输入一棵二叉树，判断该二叉树是否是平衡二叉树。

### 解法
#### 解法一  递归 自顶向下

```java
public class Solution {
    public boolean IsBalanced_Solution(TreeNode root) {
        if(root == null){
            return true;
        }
        int leftH = getHeight(root.left);
        int rightH = getHeight(root.right);
        return Math.abs(leftH - rightH) <= 1;
    }
    
    private int getHeight(TreeNode root){
        if(root == null){
            return 0;
        }
        return Math.max(getHeight(root.left), getHeight(root.right)) + 1;
    }
}
```



#### 解法二 递归优化 自底向上

这种做法有很明显的问题，在判断上层结点的时候，会多次重复遍历下层结点，增加了不必要的开销。如果改为从下往上遍历，如果子树是平衡二叉树，则返回子树的高度；如果发现子树不是平衡二叉树，则直接停止遍历，这样至多只对每个结点访问一次。

```java
public class Solution {
    public boolean IsBalanced_Solution(TreeNode root) {
        if(root == null){
            return true;
        }
        return getHeight(root) != -1;
    }
    
    private int getHeight(TreeNode root){
        if(root == null){
            return 0;
        }
        int leftH = getHeight(root.left);
        if(leftH == -1){
            return -1;
        }
        int rightH = getHeight(root.right);
        if(rightH == -1){
            return -1;
        }
        if(Math.abs(leftH - rightH) > 1){
            return -1;
        }
        return Math.max(leftH, rightH) + 1;
    }
}
```

## 数组中只出现一次的数字（两个数）
### 题目描述

一个整型数组里除了两个数字之外，其他的数字都出现了两次。请写程序找出这两个只出现一次的数字。

### 解法
#### 解法一 Hash

```java
//num1,num2分别为长度为1的数组。传出参数
//将num1[0],num2[0]设置为返回结果
import java.util.HashSet;
import java.util.ArrayList;
public class Solution {
    public void FindNumsAppearOnce(int [] array,int num1[] , int num2[]) {
        HashSet<Integer> set = new HashSet<>();
        
        for(int i = 0; i < array.length; i++){
            if(set.contains(array[i])){
                set.remove(array[i]);
            }else{
                set.add(array[i]);
            }
        }
        ArrayList<Integer> list = new ArrayList<>(set);
        num1[0] = list.get(0);
        num2[0] = list.get(1);
    }
}
```

#### 解法二 位运算 lowbit(x) = x & (-x) == x & (~x + 1) —> 要的是你从末尾开始第1个 1(其他位置都是0) 所代表的值

```java
public class Solution {
    public void FindNumsAppearOnce(int [] array,int num1[] , int num2[]) {
        int diff = 0;
        for(int i = 0; i < array.length; i++){
            diff ^= array[i];
        }
        int bitFlag = diff & (-diff);
        for(int i = 0; i < array.length; i++){
            if((array[i] & bitFlag) == 0){
                num1[0] ^= array[i];
            }else{
                num2[0] ^= array[i];
            }
        }
    }
}
```



## 和为S的连续正数序列
### 题目描述

小明很喜欢数学,有一天他在做数学作业时,要求计算出9~16的和,他马上就写出了正确答案是100。但是他并不满足于此,他在想究竟有多少种连续的正数序列的和为100(至少包括两个数)。没多久,他就得到另一组连续正数和为100的序列:18,19,20,21,22。现在把问题交给你,你能不能也很快的找出所有和为S的连续正数序列? Good Luck!

### 输出描述:

```
输出所有和为S的连续正数序列。序列内按照从小至大的顺序，序列间按照开始数字从小到大的顺序
```

### 解法
#### 解法一 计算平均值

1）由于我们要找的是和为S的连续正数序列，因此这个序列是个公差为1的等差数列，而这个序列的中间值代表了平均值的大小。假设序列长度为n，那么这个序列的中间值可以通过（S / n）得到，知道序列的中间值和长度，也就不难求出这段序列了。 

  2）满足条件的n分两种情况： 

  n为奇数时，序列中间的数正好是序列的平均值，所以条件为：(n & 1) == 1 && sum % n == 0；


  n为偶数时，序列中间两个数的平均值是序列的平均值，而这个平均值的小数部分为0.5，所以条件为：(sum % n) * 2 == n.


  3）由题可知n >= 2，那么n的最大值是多少呢？我们完全可以将n从2到S全部遍历一次，但是大部分遍历是不必要的。为了让n尽可能大，我们让序列从1开始， 

  根据等差数列的求和公式：S = (1 + n) * n / 2，得到![img](/Users/zhengxinzhi/typora_img/剑指offer/5073898_1515927185012_equation). 

​    最后举一个例子，假设输入sum = 100，我们只需遍历n = 13~2的情况（按题意应从大到小遍历），n = 8时，得到序列[9, 10, 11, 12, 13, 14, 15, 16]；n  = 5时，得到序列[18, 19, 20, 21, 22]。 

  完整代码：时间复杂度为![img](/Users/zhengxinzhi/typora_img/剑指offer/5073898_1516868312189_equation)

```java
import java.util.ArrayList;
public class Solution {
    public ArrayList<ArrayList<Integer> > FindContinuousSequence(int sum) {
        ArrayList<ArrayList<Integer>> ans = new ArrayList<>();
        // 输出所有和为S的连续正数序列。序列内按照从小至大的顺序，序列间按照开始数字从小到大的顺序
        // 所以逆序
        for(int i = (int) Math.sqrt(2 * sum); i >= 2; i--){
            // n为奇数时，序列中间的数正好是序列的平均值，所以条件为：(n & 1) == 1 && sum % n == 0；
            // n为偶数时，序列中间两个数的平均值是序列的平均值，而这个平均值的小数部分为0.5，所以条件为：(sum % n) * 2 == n.
            if((sum % i == 0 && (i & 1) == 1) || sum % i * 2 == i){
                ArrayList<Integer> list = new ArrayList<>();
                for(int j = 0, k = sum / i - (i - 1)/ 2; j < i; j++, k++){
                    list.add(k);
                }
                ans.add(list);
            }
        }
        return ans;
    }
}
```

#### 解法二 双指针法 O(n)

```
/*
用两个数字begin和end分别表示序列的最大值和最小值，
首先将begin初始化为1，end初始化为2.
如果从begin到end的和大于s，我们就从序列中去掉较小的值(即增大begin),
相反，只需要增大end。
终止条件为：一直增加begin到(1+sum)/2并且end小于sum为止
*/
```

```java
import java.util.ArrayList;
public class Solution {
    public ArrayList<ArrayList<Integer> > FindContinuousSequence(int sum) {
        ArrayList<ArrayList<Integer>> ans = new ArrayList<>();
        int s = 1, t = 2;
        while(s < t){
            int stSum = (s + t) * (t - s + 1) / 2;
            if(stSum < sum){
                t++;
            }else if(stSum > sum){
                s++;
            }else{
                ArrayList<Integer> list = new ArrayList<>();
                for(int i = s; i <= t; i++){
                    list.add(i);
                }
                ans.add(list);
                s++;
            }
        }
        return ans;
    }
}
```

## 和为S的两个数字
### 题目描述

输入一个递增排序的数组和一个数字S，在数组中查找两个数，使得他们的和正好是S，如果有多对数字的和等于S，输出两个数的乘积最小的。

### 解法
#### 解法一 Hash TO(n) SO(n)

```java
import java.util.HashMap;
import java.util.ArrayList;
public class Solution {
    public ArrayList<Integer> FindNumbersWithSum(int [] array,int sum) {
        ArrayList<Integer> ans = new ArrayList<>();
        HashMap<Integer, Integer> map = new HashMap<>();
        int mulVal = Integer.MAX_VALUE;
        int low = -1, high = -1;
        for(int i = 0; i < array.length - 1; i++){
            if(array[i] == array[i+1] && array[i] * 2 == sum){
                mulVal = array[i] * array[i+1];
                low = array[i];
                high = array[i+1];
            }
        }
        for(int i = 0; i < array.length; i++){
            map.put(array[i], i);
        }
        
        for(int i = 0; i < array.length; i++){
            if(map.containsKey(sum - array[i]) && map.get(sum - array[i]) != i){
                if(array[i] * (sum - array[i]) < mulVal){
                    low = array[i];
                    high = sum - array[i];
                    break;
                }
            }
        }
        if(low != -1){
            ans.add(low);
            ans.add(high);
        }
        return ans;
    }
}
```

#### 解法二 双指针 夹逼 TO(n) SO(1)

不要被题目误导了！证明如下，清晰明了： 

  //输出两个数的乘积最小的。这句话的理解？
 **假设：**若b>a,且存在， 

  a + b = s; 

  (a - m ) + (b + m) = s 

  **则**：(a - m )(b + m)=ab **- (b-a)m - m\*m** < ab；**说明外层的乘积更小**
 也就是说**依然是左右夹逼法**！！！只需要2个指针 

  1.**left开头**，**right指向结尾**
 2.如果和**小于sum**，说明**太小了**，**left右移**寻找更**大**的数
 3.如果和大**于sum**，说明**太大了**，**right左移**寻找更**小**的数
 4.和**相等**，**把left和right的数返回**

```java
import java.util.ArrayList;
public class Solution {
    public ArrayList<Integer> FindNumbersWithSum(int [] array,int sum) {
        ArrayList<Integer> ans = new ArrayList<>();
        int s = 0, t = array.length - 1;
        while(s < t){
            if(array[s] + array[t] == sum){
                ans.add(array[s]);
                ans.add(array[t]);
                break;
            }else if(array[s] + array[t] < sum){
                s++;
            }else if(array[s] + array[t] > sum){
                t--;
            }
        }
        return ans;
    }
}
```

## 左旋转字符串
### 题目描述

汇编语言中有一种移位指令叫做循环左移（ROL），现在有个简单的任务，就是用字符串模拟这个指令的运算结果。对于一个给定的字符序列S，请你把其循环左移K位后的序列输出。例如，字符序列S=”abcXYZdef”,要求输出循环左移3位后的结果，即“XYZdefabc”。是不是很简单？OK，搞定它！

### 解法
#### 解法一 

```java
public class Solution {
    public String LeftRotateString(String str,int n) {
        if(str == null || str.length() == 0 || str.length() < n){
            return str;
        }
        return str.substring(n, str.length()) + str.substring(0, n);
    }
}
```



#### 解法二

这道题考的核心是应聘者是不是可以灵活利用字符串翻转。假设字符串abcdef，n=3，设X=abc，Y=def，所以字符串可以表示成XY，如题干，问如何求得YX。假设X的翻转为XT，XT=cba，同理YT=fed，那么YX=(XTYT)T，三次翻转后可得结果。

```java
public class Solution {
    public String LeftRotateString(String str,int n) {
        char[] chars = str.toCharArray();
        if(chars.length < n){
            return "";
        }
        reverse(chars, 0, n - 1);
        reverse(chars, n, chars.length - 1);
        reverse(chars, 0, chars.length - 1);
        return new String(chars);
    }
    
    private void reverse(char[] chars, int low, int high){
        char temp;
        while(low < high){
            temp = chars[low];
            chars[low] = chars[high];
            chars[high] = temp;
            low++;
            high--;
        }
    }
}
```

## 翻转单词顺序

### 题目描述

牛客最近来了一个新员工Fish，每天早晨总是会拿着一本英文杂志，写些句子在本子上。同事Cat对Fish写的内容颇感兴趣，有一天他向Fish借来翻看，但却读不懂它的意思。例如，“student. a am I”。后来才意识到，这家伙原来把句子单词的顺序翻转了，正确的句子应该是“I am a student.”。Cat对一一的翻转这些单词顺序可不在行，你能帮助他么？

### 解法
#### 解法一 Java String的函数

```java
public class Solution {
    public String ReverseSentence(String str) {
        // 例如 " "
        if(str.trim().equals("")){
            return str;
        }
        String[] words = str.split(" ");
        int s = 0, t = words.length - 1;
        String temp;
        while(s < t){
            temp = words[s];
            words[s] = words[t];
            words[t] = temp;
            s++;
            t--;
        }
        
        return String.join(" ", words);
    }
}
```

#### 解法二

这个个题目本意是进行每个单词倒转，再倒转整个句子

```java
import java.util.Arrays;
public class Solution {
    public String ReverseSentence(String str) {
        // 例如 " "
        if(str.trim().equals("")){
            return str;
        }
        // 为了下面中循环条件保持一致
        char[] chars = (str + " ").toCharArray();
        
        int low = 0, high = 0;
        for(high = 0; high < chars.length; high++)
            if(chars[high] == ' '){
                reverse(chars, low, high - 1);
                low = high + 1;
        }
        reverse(chars, 0 , str.length() - 1);
        return new String(Arrays.copyOfRange(chars, 0, str.length()));
    }
    
    private void reverse(char[] chars, int low, int high){
        char temp;
        while(low < high){
            temp = chars[low];
            chars[low] = chars[high];
            chars[high] = temp;
            low++;
            high--;
        }
    }
}
```

## 扑克牌顺子
### 题目描述

LL今天心情特别好,因为他去买了一副扑克牌,发现里面居然有2个大王,2个小王(一副牌原本是54张^_^)...他随机从中抽出了5张牌,想测测自己的手气,看看能不能抽到顺子,如果抽到的话,他决定去买体育彩票,嘿嘿！！“红心A,黑桃3,小王,大王,方片5”,“Oh My God!”不是顺子.....LL不高兴了,他想了想,决定大\小 王可以看成任何数字,并且A看作1,J为11,Q为12,K为13。上面的5张牌就可以变成“1,2,3,4,5”(大小王分别看作2和4),“So Lucky!”。LL决定去买体育彩票啦。 现在,要求你使用这幅牌模拟上面的过程,然后告诉我们LL的运气如何， 如果牌能组成顺子就输出true，否则就输出false。为了方便起见,你可以认为大小王是0。

### 解法
#### 解法一 排序 TO(blogs)

```java
import java.util.Arrays;
public class Solution {
    public boolean isContinuous(int [] numbers) {
        if(numbers == null || numbers.length != 5){
            return false;
        }
        // 排序
        Arrays.sort(numbers);
        // 癞子总数
        int changeCnt = 0;
        for(int i = 0; i < numbers.length - 1; i++){
            if(numbers[i] == 0){
                changeCnt += 1;
                // 若癞子有numbers.length-1个，返回true
                if(changeCnt == numbers.length - 1){
                    return true;
                }
            }else{
                // 有相等元素， 返回false
                if(numbers[i + 1] == numbers[i]){
                    return false;
                }
                // 相邻元素间隔用癞子消掉，判断癞子个数，若小于0，false
                changeCnt = changeCnt - (numbers[i + 1] - numbers[i] - 1);
                if(changeCnt < 0){
                    return false;
                }
            }
        }
        return true;
    }

}
```



#### 解法二 定规则 TO(n) 推荐

max 记录 最大值
 min 记录  最小值
 min ,max 都不记0
 满足条件 1 max - min   <5
                2 除0外没有重复的数字(牌)
                3 数组长度 为5

```java
import java.util.HashSet;
public class Solution {
    public boolean isContinuous(int [] numbers) {
        if(numbers == null || numbers.length != 5){
            return false;
        }
        HashSet<Integer> set = new HashSet<>();
        int min = 14, max = -1;
        for(int i = 0; i < numbers.length; i++){
            if(numbers[i] != 0){
                if(set.contains(numbers[i]))
                    return false;
                set.add(numbers[i]);
                min = Math.min(numbers[i], min);
                max = Math.max(numbers[i], max);
            }
        }
        return (max - min) <= 4;
    }
}
 
```

## 孩子们的游戏（圆圈中最后剩下的数）
### 题目描述

每年六一儿童节,牛客都会准备一些小礼物去看望孤儿院的小朋友,今年亦是如此。HF作为牛客的资深元老,自然也准备了一些小游戏。其中,有个游戏是这样的:首先,让小朋友们围成一个大圈。然后,他随机指定一个数m,让编号为0的小朋友开始报数。每次喊到m-1的那个小朋友要出列唱首歌,然后可以在礼品箱中任意的挑选礼物,并且不再回到圈中,从他的下一个小朋友开始,继续0...m-1报数....这样下去....直到剩下最后一个小朋友,可以不用表演,并且拿到牛客名贵的“名侦探柯南”典藏版(名额有限哦!!^_^)。请你试着想下,哪个小朋友会得到这份礼品呢？(注：小朋友的编号是从0到n-1)， 若n == 0 返回-1

### 解法
#### 解法一 训练链表

```java
class CycleLinkedList{
    int val;
    CycleLinkedList next;
    
    public CycleLinkedList(int val){
        this.val = val;
        this.next = null;
    }
}
public class Solution {
    public int LastRemaining_Solution(int n, int m) {
        if(n == 0){
            return -1;
        }
        // 循环链表的构建
        CycleLinkedList dummy = new CycleLinkedList(-1);
        CycleLinkedList p = dummy;
        for(int i = 0; i < n; i++){
            p.next = new CycleLinkedList(i);
            p = p.next;
        }
        p.next = dummy.next;
        p = dummy.next;
        // 报数
        while(p.next != p){
            for(int i = 0; i < m - 2; i++){
                p = p.next;
            }
            p.next = p.next.next;
            p = p.next;
        }
        return p.val;
    }
}
```

```java
import java.util.ArrayList;
public class Solution {
    public int LastRemaining_Solution(int n, int m) {
        if(n == 0 || m == 0){
            return -1;
        }
        // 也可换成LinkedList
        ArrayList<Integer> list = new ArrayList<>();
        for(int i = 0; i < n; i++){
            list.add(i);
        }
        int index = 0;
        while(list.size() > 1){
            index = (index + m - 1) % list.size();
            list.remove(index);
        }
        return list.get(0);
    }
}
```



#### 解法二 找规律 递归解法

问题描述：n个人（编号0~(n-1))，从0开始报数，报到(m-1)的退出，剩下的人               继续从0开始报数。求胜利者的编号。   

​                我们知道第一个人(编号一定是m%n-1) 出列之后，剩下的n-1个人组成了一个新               的约瑟夫环（以编号为k=m%n的人开始）:   

​                      k  k+1  k+2  ... n-2, n-1, 0, 1, 2, ... k-2并且从k开始报0。   

​               现在我们把他们的编号做一下转换：   

​               k     --> 0   

​               k+1   --> 1   

​               k+2   --> 2   

​               ...   

​               ...   

​               k-2   --> n-2   

​               k-1   --> n-1   

​               变换后就完完全全成为了(n-1)个人报数的子问题，假如我们知道这个子问题的解：               例如x是最终的胜利者，那么根据上面这个表把这个x变回去不刚好就是n个人情               况的解吗？！！变回去的公式很简单，相信大家都可以推出来：x'=(x+k)%n。   

​               令f[i]表示i个人玩游戏报m退出最后胜利者的编号，最后的结果自然是f[n]。   

​               递推公式   

​                    f[1]=0;   

​                    f[i]=(f[i-1]+m)%i;  (i>1)   

​               有了这个公式，我们要做的就是从1-n顺序算出f[i]的数值，最后结果是f[n]。                    因为实际生活中编号总是从1开始，我们输出f[n]+1。

```java
public class Solution {
    public int LastRemaining_Solution(int n, int m) {
        if(n == 0){
            return -1;
        }else if(n == 1){
            return 0;
        }else{
            return (LastRemaining_Solution(n - 1, m) + m) % n;
        }
    }
}
```

## [收藏]求1+2+…+n
### 题目描述

求1+2+3+...+n，要求不能使用乘除法、for、while、if、else、switch、case等关键字及条件判断语句（A?B:C）。

### 解法
#### 解法一 递归 错误 使用 if 语句了

```java
public class Solution {
    public int Sum_Solution(int n) {
      	// 利用短路去替换
        if(n < 1){
            return 0;
        }
        return n + Sum_Solution(n - 1);
    }
}
```

#### 解法二 递归+短路

```java
//其实只要先看我们手里有什么牌就能一步一步想到利用短路特性了
//我们手里现在可以使用（按优先级高低）单目运算符：++和--,双目运算符：+,-，移位运算符<<和>>，关系运算符>,<等，逻辑运算符&&，||,&,|,^，赋值=
//单目和双目的作用是一样的，移位显然没有规律性，因为一个二进制位并不能区分某个数和其他数，这也就排除了&,|,^,因为不需要做位运算了
//关系运算符要和if匹配，但这是不行的，这时看看剩下的运算符只能选&&,||了
//如果做过Java笔试题，会对这两个运算符非常敏感，他们有短路特性，前面的条件判真（或者假）了，就不会再执行后面的条件了
//这时就能联想到--n,直到等于0就能返回值。
public class Solution {
    public int Sum_Solution(int n) {
        int sum = n;
        boolean flag = (sum>0)&&((sum+=Sum_Solution(--n))>0);
        return sum;
    }
}
```

## [收藏]不用加减乘除做加法
### 题目描述

写一个函数，求两个整数之和，要求在函数体内不得使用+、-、*、/四则运算符号。

### 解法
#### 解法一 位运算按位于得到进位位，按位异或得到非进位位，循环执行，直到无进位位

> step1:按位与是查看两个数哪些二进制位都为1，这些都是进位位，结果需左移一位，表示进位后的结果 
>
> step2:异或是查看两个数哪些二进制位只有一个为1，这些是非进位位，可以直接加、减，结果表示非进位位进行加操作后的结果

首先看十进制是如何做的： 5+7=12，三步走
第一步：相加各位的值，不算进位，得到2。
第二步：计算进位值，得到10. 如果这一步的进位值为0，那么第一步得到的值就是最终结果。

第三步：重复上述两步，只是相加的值变成上述两步的得到的结果2和10，得到12。

同样我们可以用三步走的方式计算二进制值相加： 5-101，7-111 第一步：相加各位的值，不算进位，得到010，二进制每位相加就相当于各位做异或操作，101^111。

第二步：计算进位值，得到1010，相当于各位做与操作得到101，再向左移一位得到1010，(101&111)<<1。

第三步重复上述两步， 各位相加 010^1010=1000，进位值为100=(010&1010)<<1。
     继续重复上述两步：1000^100 = 1100，进位值为0，跳出循环，1100为最终结果。

```java
public class Solution {
    public int Add(int num1,int num2) {
        while(num2 != 0){
            // 非进位位
            int temp = num1 ^ num2;
            // 进位位
            num2 = (num1 & num2) << 1;
            num1 = temp;
        }
        return num1;
    }
}

```

## 将字符串转为整数
### 题目描述

将一个字符串转换成一个整数(实现Integer.valueOf(string)的功能，但是string不符合数字要求时返回0)，要求不能使用字符串转换整数的库函数。 数值为0或者字符串不是一个合法的数值则返回0。

#### 输入描述:

```
输入一个字符串,包括数字字母符号,可以为空
```

#### 输出描述:

```
如果是合法的数值表达则返回该数字，否则返回0
```

#### 示例1

#### 输入

复制

```
+2147483647
    1a33
```

#### 输出

复制

```
2147483647
    0
```

### 解法

注意: 数字上下界溢出

Java 如果数值超出int的范围，要特殊处理；比int的最大值还要大，已经上溢，这肯定不能通过数字的大小比较，所以需要在字符串的状态下判断是否上溢或下溢。

                // int型溢出问题, java中int型没有比Integer.MAX_VALUE更大的值
                // System.out.println(Integer.MAX_VALUE);2147483647
                // System.out.println(0x7fffffff);2147483647
                // System.out.println(Integer.MIN_VALUE);-2147483648
                // System.out.println(0x80000000);-2147483648
#### 解法一 关键是溢出

```java
import java.lang.Character;
public class Solution {
    public int StrToInt(String str) {
        if(str == null || "".equals(str)){
            return 0;
        }
        char[] chars = str.toCharArray();
        int i = 0, ans = 0;
        boolean isPos = true;
        // 判断第一位是否是符号位,若是, i从第一位开始遍历(以0位起点)
        if(chars[0] == '+' || chars[0] == '-'){
            i = 1;
            isPos = chars[0] == '+';
        }
        for(; i < chars.length; i++){
            if (chars[i] < '9' && chars[i] > '0'){
                // int型溢出问题, java中int型没有比Integer.MAX_VALUE更大的值
                // System.out.println(Integer.MAX_VALUE);2147483647
                // System.out.println(0x7fffffff);2147483647
                // System.out.println(Integer.MIN_VALUE);-2147483648
                // System.out.println(0x80000000);-2147483648
                if(ans == Integer.MAX_VALUE / 10){
                    if((isPos && chars[i] > '7') || (!isPos && chars[i] > '8')){
                        return 0;
                    }
                }else if(ans > Integer.MAX_VALUE / 10){
                    return 0;
                }
                // Character.getNumbericValue
                ans = ans * 10 + (chars[i] - '0');

            }else{
                return 0;
            }
        }
        return ans * (isPos ? 1 : -1);
    }
}

```

## 数组中重复的数字
### 题目描述

在一个长度为n的数组里的所有数字都在0到n-1的范围内。 数组中某些数字是重复的，但不知道有几个数字是重复的。也不知道每个数字重复几次。请找出数组中任意一个重复的数字。 例如，如果输入长度为7的数组{2,3,1,0,2,5,3}，那么对应的输出是第一个重复的数字2。

### 解法
#### 解法一 Hash

```java
public class Solution {
    // Parameters:
    //    numbers:     an array of integers
    //    length:      the length of array numbers
    //    duplication: (Output) the duplicated number in the array number,length of duplication array is 1,so using duplication[0] = ? in implementation;
    //                  Here duplication like pointor in C/C++, duplication[0] equal *duplication in C/C++
    //    这里要特别注意~返回任意重复的一个，赋值duplication[0]
    // Return value:       true if the input is valid, and there are some duplications in the array number
    //                     otherwise false
    public boolean duplicate(int numbers[],int length,int [] duplication) {
        if(numbers == null || length == 0){
            return false;
        }
        int[] nums = new int[length];
        for(int i = 0; i < length; i++){
            if(nums[numbers[i]] > 0){
                duplication[0] = numbers[i];
                return true;
            }
            nums[numbers[i]] += 1;
        }
        return false;
    }
}
```



#### 解法二 数字数组+索引数组（一个数组）

不需要额外的数组或者hash table来保存，题目里写了数组里数字的范围保证在0 ~ n-1   之间，所以可以利用现有数组设置标志，当一个数字被访问过后，可以设置对应位上的数 +   n，之后再遇到相同的数时，会发现对应位上的数已经大于等于n了，那么直接返回这个数即可。

这个数组同时有两个作用：数字数组 索引数组 需要脑海不断的转换

```java
    public boolean duplicate(int numbers[],int length,int [] duplication) {
        if(numbers == null || length == 0){
            return false;
        }
        for(int i = 0; i < length; i++){
            // 遇到的数字 不是索引
            int index = numbers[i];
            // 碰到之前做过标记的索引，临时改变index，未改变该元素在数组中的值
            if(index >= length){
                index -= length;
            }
            // 遇到该标记 返回
            if(numbers[index] >= length){
                return index;
            }
            // 把数字变为索引，其中该索引上的数字做个标记
            numbers[index] = numbers[index] + length;
        }
        return false;
    }
}
```

## [收藏]构建乘积数组
### 题目描述

给定一个数组A[0,1,...,n-1],请构建一个数组B[0,1,...,n-1],其中B中的元素B[i]=A[0]*A[1]*...*A[i-1]*A[i+1]*...*A[n-1]。不能使用除法。

### 解法
#### 解法一 双向逐元素累乘

```java
import java.util.ArrayList;
public class Solution {
    public int[] multiply(int[] A) {
        int[] sums = new int[A.length+1], B = new int[A.length];
        sums[0] = 1;
        for(int i = 1; i <= A.length; i++){
            sums[i] = sums[i-1] * A[i-1];
        }
        int backSum = 1;
        for(int i = A.length - 1; i >= 0; i--){
            B[i] = sums[i] * backSum;
            backSum *= A[i];
        }
        return B;
    }
}
```

## [todo]正则表达式匹配
### 题目描述

请实现一个函数用来匹配包括'.'和'*'的正则表达式。模式中的字符'.'表示任意一个字符，而'*'表示它前面的字符可以出现任意次（包含0次）。 在本题中，匹配是指字符串的所有字符匹配整个模式。例如，字符串"aaa"与模式"a.a"和"ab*ac*a"匹配，但是与"aa.a"和"ab*a"均不匹配

### 解法



#### 解法一 
#### 解法二



## [todo]表示数值的字符串
### 题目描述

请实现一个函数用来判断字符串是否表示数值（包括整数和小数）。例如，字符串"+100","5e2","-123","3.1416"和"-1E-16"都表示数值。 但是"12e","1a3.14","1.2.3","+-5"和"12e+4.3"都不是。

### 解法
#### 解法一 
#### 解法二 