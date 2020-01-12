



## 对数器

### 步骤

1. 待测方法a
2. 实现一个绝对正确但是复杂度不好的方法b,
3. 实现一个随机样本产生器generateRandomArray
4. 实现比对的方法
5. 如果有一个样本使得比对出错,打印样本分析是哪个方法出
   错

### 注意

1.随机样本产生器根据题目要求(不同长度, 正负样本 0)

2.开始测试选择生成短数组或简单数组

3.准备 二叉树生成器模板 + 数组生成器模板

### 实现

#### Java实现

```java
    public static int[] generateRandomArray(int maxSize, int maxValue) {
    		// maxSize: 数组最大范围 maxValue: 数组最大值
        // 默认生成数组包含负数
        int[] arr = new int[(int) ((maxSize + 1) * Math.random())];
        for (int i = 0; i < arr.length; i++) {
            arr[i] = (int) ((maxValue + 1) * Math.random()) - (int) (maxValue * Math.random());
        }
        return arr;
    }

    // for test
    public static int[] copyArray(int[] arr) {
        if (arr == null) {
            return null;
        }
        int[] res = new int[arr.length];
        for (int i = 0; i < arr.length; i++) {
            res[i] = arr[i];
        }
        return res;
    }

    // for test
    public static boolean isEqual(int[] arr1, int[] arr2) {
        if ((arr1 == null && arr2 != null) || (arr1 != null && arr2 == null)) {
            return false;
        }
        if (arr1 == null && arr2 == null) {
            return true;
        }
        if (arr1.length != arr2.length) {
            return false;
        }
        for (int i = 0; i < arr1.length; i++) {
            if (arr1[i] != arr2[i]) {
                return false;
            }
        }
        return true;
    }

    public static void main(String[] args) {
        int testTime = 500;
        int maxSize = 100;
        int maxValue = 100;
        boolean succeed = true;
        for (int i = 0; i < testTime; i++) {
            int[] arr1 = generateRandomArray(maxSize, maxValue);
            int[] arr2 = copyArray(arr1);
            bubbleSort(arr1);
            // right and slow func
            comparator(arr2);
            if (!isEqual(arr1, arr2)) {
                // 打印出错的数组
                printArray(arr1);
                printArray(arr2);
                succeed = false;
                break;
            }
        }
        System.out.println(succeed ? "Nice!" : "Fucking fucked!");

        int[] arr = generateRandomArray(maxSize, maxValue);
        printArray(arr);
        bubbleSort(arr);
        printArray(arr);
    }
```

## 排序

### 快速排序 改进版 荷兰国旗

```python
def partition(arr, L, R):
  """
  		返回最后一个元素及相等元素的左右边界
  """
	less = L - 1
  # 待比较元素索引
	more = R
	while L < more:
		if arr[L] < arr{R}:
			less += 1
			arr[less], arr[L] = arr[L], arr[less]
			L += 1
		elif arr[L] > arr[R]:
			more -= 1
			arr[more], arr[L] = arr[L], arr[more]
		else:
			L += 1
		arr[more], arr[R] = arr[R], arr[more]
		return less + 1, more

def quicksort(arr, L, R):
	if L < R:
		l, r = partition(arr, L, R):
		quicksort(arr, L, l - 1)
		quicksort(arr, r + 1, R)
		
```

### 堆排序

```python
def heapify(data, index, size):
    left = index * 2 + 1
    while left < size:
        largest = left + 1 if left + 1 < size and data[left + 1] > data[left] else left
        largest = largest if data[largest] > data[index] else index
        if largest == index:
            break
        data[largest], data[index] = data[index], data[largest]
        index = largest
        left = largest * 2 + 1


def heap_sort(data):
    if not data or len(data) < 2:
        return data

    n = len(data)
    # 创建堆
    for i in range((n - 1)//2, -1, -1):
        heapify(data, i, n)

    # 堆排序 
    for i in range(n-1, -1, -1):    # 从大到小
        data[0], data[i] = data[i], data[0]     # 将最后一个值与父节点交互位置
        heapify(data, 0, i)


li = list(range(10))
random.shuffle(li)
print(li)
heap_sort(li)
print(li)
```

```java

public class Code_03_HeapSort {

	public static void heapSort(int[] arr) {
		if (arr == null || arr.length < 2) {
			return;
		}

		for (int i = (arr.length - 1) / 2; i >= 0; i--){
			heapify(arr, i, n);
		}
		int size = arr.length;
		swap(arr, 0, --size);
		while (size > 0) {
			heapify(arr, 0, size);
			swap(arr, 0, --size);
		}
	}

	public static void heapInsert(int[] arr, int index) {
		while (arr[index] > arr[(index - 1) / 2]) {
			swap(arr, index, (index - 1) / 2);
			index = (index - 1) / 2;
		}
	}

	public static void heapify(int[] arr, int index, int size) {
		int left = index * 2 + 1;
		while (left < size) {
			int largest = left + 1 < size && arr[left + 1] > arr[left] ? left + 1 : left;
			largest = arr[largest] > arr[index] ? largest : index;
			if (largest == index) {
				break;
			}
			swap(arr, largest, index);
			index = largest;
			left = index * 2 + 1;
		}
	}



	public static void swap(int[] arr, int i, int j) {
		int tmp = arr[i];
		arr[i] = arr[j];
		arr[j] = tmp;
	}
```

### 桶排序  (计数排序)

```python
def bucketSort(arr):
    if not arr or len(arr) < 2:
        return arr

    # 1.最大值
    maxV = max(arr)

    # 2.词频表
    bucket = [0] * (maxV + 1)
    for i in range(len(arr)):
        bucket[arr[i]] += 1
    # 3.根据词频表排序
    i = 0
    for j in range(len(bucket)):
        while bucket[j] > 0:
            arr[i] = j
            i += 1
            bucket[j] -= 1

```

```java
import java.util.Arrays;

class test {

	public static void bucketSort(int[] arr){
		if (arr == null || arr.length < 2){
			return;
		}
		// 1.最大值
		int max = Integer.MIN_VALUE;
		for(int i = 0; i < arr.length; i++){
			max = Math.max(max, arr[i]);
		}
		
		// 2.词频表
		int[] bucket = new int[max + 1];
		for (int i = 0; i < arr.length; i++){
			bucket[arr[i]]++;
		}

		// 3.根据词频表进行安置排序
		int i = 0;
		for(int j = 0; j < bucket.length; j++){
			while(bucket[j]-- > 0){
				arr[i++] = j;
			}
		}
	}

```

#### 热门题目

给定一个数组,求如果排序之后,相邻两数的最大差值,要求时间复杂度0(N),且要求不能用非基于比较的排序。

##### 思路

1.找到最大值最小值, 构建 N + 1个桶, 把最小值放入第一个桶, 最大值放入最后一个桶

2.剩余N - 1个桶, 把最大值最小值的差值 (N - 1) 等分

3.遍历数组, 把值放入不同的范围(桶), N 个数放入 N + 1个桶一定会空余一个桶

4.最大差值一定在这个空桶的前一个桶最大值和后一个桶最小值

[注意]每个桶只更新进入这个桶的最小值和最大值, 并维护一个是否是空桶的标记

```java
	public static int maxGap(int[] nums) {
		if (nums == null || nums.length < 2) {
			return 0;
		}
		int len = nums.length;
		int min = Integer.MAX_VALUE;
		int max = Integer.MIN_VALUE;
		for (int i = 0; i < len; i++) {
			min = Math.min(min, nums[i]);
			max = Math.max(max, nums[i]);
		}
		if (min == max) {
			return 0;
		}
		boolean[] hasNum = new boolean[len + 1];
		int[] maxs = new int[len + 1];
		int[] mins = new int[len + 1];
		int bid = 0;
		for (int i = 0; i < len; i++) {
			bid = bucket(nums[i], len, min, max);
			mins[bid] = hasNum[bid] ? Math.min(mins[bid], nums[i]) : nums[i];
			maxs[bid] = hasNum[bid] ? Math.max(maxs[bid], nums[i]) : nums[i];
			hasNum[bid] = true;
		}
		int res = 0;
		int lastMax = maxs[0];
		int i = 1;
		for (; i <= len; i++) {
			if (hasNum[i]) {
				res = Math.max(res, mins[i] - lastMax);
				lastMax = maxs[i];
			}
		}
		return res;
	}

	public static int bucket(long num, long len, long min, long max) {
		// key: 判断num属于哪个bucket 
		// (num-min)/(max-min)就是占所有的比例
		return (int) ((num - min) * len / (max - min));
	}
```

## 队列和栈

### 用数组结构实现大小固定的队列和栈

key: 维护一个索引

```java
package class_03;

public class Code_01_Array_To_Stack_Queue {

	public static class ArrayStack {
		private Integer[] arr;
		private Integer size;

		public ArrayStack(int initSize) {
			if (initSize < 0) {
				throw new IllegalArgumentException("The init size is less than 0");
			}
			arr = new Integer[initSize];
			size = 0;
		}

		public Integer peek() {
			if (size == 0) {
				return null;
			}
			return arr[size - 1];
		}

		public void push(int obj) {
			if (size == arr.length) {
				throw new ArrayIndexOutOfBoundsException("The queue is full");
			}
			arr[size++] = obj;
		}

		public Integer pop() {
			if (size == 0) {
				throw new ArrayIndexOutOfBoundsException("The queue is empty");
			}
			return arr[--size];
		}
	}

	public static class ArrayQueue {
		private Integer[] arr;
		private Integer size;
		private Integer first;
		private Integer last;

		public ArrayQueue(int initSize) {
			if (initSize < 0) {
				throw new IllegalArgumentException("The init size is less than 0");
			}
			arr = new Integer[initSize];
			size = 0;
			first = 0;
			last = 0;
		}

		public Integer peek() {
			if (size == 0) {
				return null;
			}
			return arr[first];
		}

		public void push(int obj) {
			if (size == arr.length) {
				throw new ArrayIndexOutOfBoundsException("The queue is full");
			}
			size++;
			arr[last] = obj;
			last = last == arr.length - 1 ? 0 : last + 1;
		}

		public Integer poll() {
			if (size == 0) {
				throw new ArrayIndexOutOfBoundsException("The queue is empty");
			}
			size--;
			int tmp = first;
			first = first == arr.length - 1 ? 0 : first + 1;
			return arr[tmp];
		}
	}

	public static void main(String[] args) {

	}
}
```

### 猫狗队列

实现一种狗猫队列的结构,要求如下:用户可以调用add方法将cat类或dog类的实例放入队列中;用户可以调用pollAll方法,将队列中所有的实例按照进队列的先后顺序依次弹出;用户可以调用 pol l Dog方法,将队列中dog类的实例按照进队列的先后顺序依次弹出;用户可以调用 pol ICat方法,将队列中cat类的实例按照进队列的先后顺序依次弹出;用户可以调用 isEmpty方法,检查队列中是否还有dog或cat的实例;用户可以调用 isDogEmpty方法,检查队列中是否有dog类的实例;用户可以调用 i sCatEmpty方法,检查队列中是否有cat类的实例。 

#### 思路

dog 队列

cat 队列

时间戳

## 链表题目

### 判断链表是否回文, TO(N), SO(1)

快慢指针 找到中点后把后半部分逆序

### 将单向链表按某值划分成左边小、中间相等、右边大的形式

构建指针 less eq more less_p eq_p more_p

重点考虑边界条件: 某区域为空

### 复制含有随机指针节点的链表

#### 双Hash

不用Hash

![image-20190823200547199](http://spaceplayer.oss-cn-beijing.aliyuncs.com/spaceplayer/typora_img/ACM/image-20190823200547199.png)



### 判断单链表是否有环

快慢指针

set (额外空间)

### 两单链表相交

![image-20190823204533138](http://spaceplayer.oss-cn-beijing.aliyuncs.com/spaceplayer/typora_img/ACM/image-20190823204533138.png)

set法: 先把head1所有的结点放入set, 遍历head2是否在set中存在

长度法: 记录head1和head2长度, 较长的链表先走|len(head1) - len(head2) | 之后两链表一起走

[注]关于环: 一个有环一个无环不相交, 两个环不一样不相交 , 有一个相同的环相交(同一个入口, 两个入口)

![image-20190824093052050](http://spaceplayer.oss-cn-beijing.aliyuncs.com/spaceplayer/typora_img/ACM/image-20190824093052050.png)

```java
	public static class Node {
		public int value;
		public Node next;

		public Node(int data) {
			this.value = data;
		}
	}

	public static Node getIntersectNode(Node head1, Node head2) {
		if (head1 == null || head2 == null) {
			return null;
		}
		Node loop1 = getLoopNode(head1);
		Node loop2 = getLoopNode(head2);
		if (loop1 == null && loop2 == null) {
			return noLoop(head1, head2);
		}
		if (loop1 != null && loop2 != null) {
			return bothLoop(head1, loop1, head2, loop2);
		}
		return null;
	}

	public static Node getLoopNode(Node head) {
		if (head == null || head.next == null || head.next.next == null) {
			return null;
		}
		Node n1 = head.next; // n1 -> slow
		Node n2 = head.next.next; // n2 -> fast
		while (n1 != n2) {
			if (n2.next == null || n2.next.next == null) {
				return null;
			}
			n2 = n2.next.next;
			n1 = n1.next;
		}
		n2 = head; // n2 -> walk again from head
		while (n1 != n2) {
			n1 = n1.next;
			n2 = n2.next;
		}
		return n1;
	}

	public static Node noLoop(Node head1, Node head2) {
		if (head1 == null || head2 == null) {
			return null;
		}
		Node cur1 = head1;
		Node cur2 = head2;
		int n = 0;
		while (cur1.next != null) {
			n++;
			cur1 = cur1.next;
		}
		while (cur2.next != null) {
			n--;
			cur2 = cur2.next;
		}
		if (cur1 != cur2) {
			return null;
		}
		cur1 = n > 0 ? head1 : head2;
		cur2 = cur1 == head1 ? head2 : head1;
		n = Math.abs(n);
		while (n != 0) {
			n--;
			cur1 = cur1.next;
		}
		while (cur1 != cur2) {
			cur1 = cur1.next;
			cur2 = cur2.next;
		}
		return cur1;
	}

	public static Node bothLoop(Node head1, Node loop1, Node head2, Node loop2) {
		Node cur1 = null;
		Node cur2 = null;
		if (loop1 == loop2) {
			cur1 = head1;
			cur2 = head2;
			int n = 0;
			while (cur1 != loop1) {
				n++;
				cur1 = cur1.next;
			}
			while (cur2 != loop2) {
				n--;
				cur2 = cur2.next;
			}
			cur1 = n > 0 ? head1 : head2;
			cur2 = cur1 == head1 ? head2 : head1;
			n = Math.abs(n);
			while (n != 0) {
				n--;
				cur1 = cur1.next;
			}
			while (cur1 != cur2) {
				cur1 = cur1.next;
				cur2 = cur2.next;
			}
			return cur1;
		} else {
			cur1 = loop1.next;
			while (cur1 != loop1) {
				if (cur1 == loop2) {
					return loop1;
				}
				cur1 = cur1.next;
			}
			return null;
		}
	}
```

## 二叉树题目

前序中序后序

```java
	public static class Node {
		public int value;
		public Node left;
		public Node right;

		public Node(int data) {
			this.value = data;
		}
	}

	public static void preOrderRecur(Node head) {
		if (head == null) {
			return;
		}
		System.out.print(head.value + " ");
		preOrderRecur(head.left);
		preOrderRecur(head.right);
	}

	public static void inOrderRecur(Node head) {
		if (head == null) {
			return;
		}
		inOrderRecur(head.left);
		System.out.print(head.value + " ");
		inOrderRecur(head.right);
	}

	public static void posOrderRecur(Node head) {
		if (head == null) {
			return;
		}
		posOrderRecur(head.left);
		posOrderRecur(head.right);
		System.out.print(head.value + " ");
	}

	public static void preOrderUnRecur(Node head) {
		System.out.print("pre-order: ");
		if (head != null) {
			Stack<Node> stack = new Stack<Node>();
			stack.add(head);
			while (!stack.isEmpty()) {
				head = stack.pop();
				System.out.print(head.value + " ");
				if (head.right != null) {
					stack.push(head.right);
				}
				if (head.left != null) {
					stack.push(head.left);
				}
			}
		}
		System.out.println();
	}



	public static void inOrderUnRecur(Node head) {
		System.out.print("in-order: ");
		if (head != null) {
			Stack<Node> stack = new Stack<Node>();
			while (!stack.isEmpty() || head != null) {
				if (head != null) {
					stack.push(head);
					head = head.left;
				} else {
					head = stack.pop();
					System.out.print(head.value + " ");
					head = head.right;
				}
			}
		}
		System.out.println();
	}



	public static void posOrderUnRecur1(Node head) {
		System.out.print("pos-order: ");
		if (head != null) {
			Stack<Node> s1 = new Stack<Node>();
			Stack<Node> s2 = new Stack<Node>();
			s1.push(head);
			while (!s1.isEmpty()) {
				head = s1.pop();
				s2.push(head);
				if (head.left != null) {
					s1.push(head.left);
				}
				if (head.right != null) {
					s1.push(head.right);
				}
			}
			while (!s2.isEmpty()) {
				System.out.print(s2.pop().value + " ");
			}
		}
		System.out.println();
	}

	public static void posOrderUnRecur1(Node head) {
		if (head == null){
			return;
		}
		Stack<Node> stack = new Stack<Node>();
		stack.push(head)
		Stack<Node> revRes = new Stack<Node>();
		// MRL 前序变种
		while (!stack.isEmpty()){
			head = stack.pop();
			revRes.push(head);
			if (head.left != null){
				stack.push(head.left);
			}
			if (head.right != null){
				stack.push(head.right);
			}
		}
		while (!revRes.isEmpty()){
			System.out.print(revRes.pop().value + " ");
		}
	}



	public static void posOrderUnRecur2(Node h) {
		System.out.print("pos-order: ");
		if (h != null) {
			Stack<Node> stack = new Stack<Node>();
			stack.push(h);
			Node c = null;
			while (!stack.isEmpty()) {
				c = stack.peek();
				if (c.left != null && h != c.left && h != c.right) {
					stack.push(c.left);
				} else if (c.right != null && h != c.right) {
					stack.push(c.right);
				} else {
					System.out.print(stack.pop().value + " ");
					h = c;
				}
			}
		}
		System.out.println();
	}
```

### [福利函数] 如何直观的打印二叉树

刷题时观察树的结构

```java
package class_04;

public class Code_02_PrintBinaryTree {

	public static class Node {
		public int value;
		public Node left;
		public Node right;

		public Node(int data) {
			this.value = data;
		}
	}

	public static void printTree(Node head) {
		System.out.println("Binary Tree:");
		printInOrder(head, 0, "H", 17);
		System.out.println();
	}

	public static void printInOrder(Node head, int height, String to, int len) {
		if (head == null) {
			return;
		}
		printInOrder(head.right, height + 1, "v", len);
		String val = to + head.value + to;
		int lenM = val.length();
		int lenL = (len - lenM) / 2;
		int lenR = len - lenM - lenL;
		val = getSpace(lenL) + val + getSpace(lenR);
		System.out.println(getSpace(height * len) + val);
		printInOrder(head.left, height + 1, "^", len);
	}

	public static String getSpace(int num) {
		String space = " ";
		StringBuffer buf = new StringBuffer("");
		for (int i = 0; i < num; i++) {
			buf.append(space);
		}
		return buf.toString();
	}

```



### 在二叉树中找到一个结点的后继结点

![image-20190824113049890](http://spaceplayer.oss-cn-beijing.aliyuncs.com/spaceplayer/typora_img/ACM/image-20190824113049890.png)

如果该结点有右子树,那么后继结点是它的右子树的最左结点

如果该结点没有右子树,  那么向上查找直到找到一个根节点: 该结点是在这个根节点的左子树上, 这个点就是后继结点

![image-20190824132855423](http://spaceplayer.oss-cn-beijing.aliyuncs.com/spaceplayer/typora_img/ACM/image-20190824132855423.png)

```java
	public static class Node {
		public int value;
		public Node left;
		public Node right;
		public Node parent;

		public Node(int data) {
			this.value = data;
		}
	}

	public static Node getSuccessorNode(Node node) {
		if (node == null) {
			return node;
		}
		if (node.right != null) {
			return getLeftMost(node.right);
		} else {
			Node parent = node.parent;
			while (parent != null && parent.left != node) {
				node = parent;
				parent = node.parent;
			}
			return parent;
		}
	}

	public static Node getLeftMost(Node node) {
		if (node == null) {
			return node;
		}
		while (node.left != null) {
			node = node.left;
		}
		return node;
	}
```

### 二叉树的序列化和反序列化

#### 先序序列化(null结点也序列化, 用#表示)

![image-20190824161735514](http://spaceplayer.oss-cn-beijing.aliyuncs.com/spaceplayer/typora_img/ACM/image-20190824161735514.png)



![image-20190824161813323](http://spaceplayer.oss-cn-beijing.aliyuncs.com/spaceplayer/typora_img/ACM/image-20190824161813323.png)

```java
	public static class Node {
		public int value;
		public Node left;
		public Node right;

		public Node(int data) {
			this.value = data;
		}
	}

	public static String serialByPre(Node head) {
		if (head == null) {
			return "#!";
		}
		String res = head.value + "!";
		res += serialByPre(head.left);
		res += serialByPre(head.right);
		return res;
	}
```

```python
	def serialByPre(root):
		if not root:
			return '#_'
		res = root.val + "_"
		res += serialByPre(root.left)
		res += serialByPre(root.right)
		return res
  
  
  def reconByPreString(preStr):
		values = preStr.split("_")
		# 逆序目的方便取出
		stack = valueOf[::-1]
		return _reconPreOrder(stack);

	def _reconPreOrder(stack):
		val = stack.pop()
		if val == '#':
			return
		root = Node(int(val))
		root.left = _reconPreOrder(stack)
		root.right = _reconPreOrder(stack)
		return root
```

#### 层次遍历序列化及反序列化

```java
	public static String serialByLevel(Node head) {
		if (head == null) {
			return "#!";
		}
		String res = head.value + "!";
		Queue<Node> queue = new LinkedList<Node>();
		queue.offer(head);
		while (!queue.isEmpty()) {
			head = queue.poll();
			if (head.left != null) {
				res += head.left.value + "!";
				queue.offer(head.left);
			} else {
				res += "#!";
			}
			if (head.right != null) {
				res += head.right.value + "!";
				queue.offer(head.right);
			} else {
				res += "#!";
			}
		}
		return res;
	}



	public static Node reconByLevelString(String levelStr) {
		String[] values = levelStr.split("!");
		int index = 0;
		Node head = generateNodeByString(values[index++]);
		Queue<Node> queue = new LinkedList<Node>();
		if (head != null) {
			queue.offer(head);
		}
		Node node = null;
		while (!queue.isEmpty()) {
			node = queue.poll();
			node.left = generateNodeByString(values[index++]);
			node.right = generateNodeByString(values[index++]);
			if (node.left != null) {
				queue.offer(node.left);
			}
			if (node.right != null) {
				queue.offer(node.right);
			}
		}
		return head;
	}


	public static Node generateNodeByString(String val) {
		if (val.equals("#")) {
			return null;
		}
		return new Node(Integer.valueOf(val));
	}
```

```python
	def serialByLevel(root):
		if not root:
			return "#_"
		res = root.val
		que = queue.Queue()
		que.put(root)
		while queue:
			root = queue.poll()
			if head.left:
				res += root.left.val + "_"
			else:
				res += "#_"
			if head.right:
				res += root.right.val + "_"
			else:
				res += "#_"
		return res
  
  	def generateNodeByString(val):
		if val == '#':
			return
		return Node(int(val))

	def _reconByLevelString(levelStr):
		values = levelStr.split("_")
		index = 0
		root = generateNodeByString(valueOf[index])
		index += 1
		que = queue()
		if root:
			queue.put(root)
		node = None
		while !que.empty():
			node = que.get()
			node.left = _reconByLevelString(values[index])
			index += 1
			node.right = _reconByLevelString(values[index])
			index += 1
			if node.left:
				que.put(node.left)
			if node.right:
				que.put(node.right)
		return root

```

### 判断一棵二叉树是否是平衡二叉树

自底向上 helper返回深度

### 判断一颗二叉树是否是完全二叉树

若左结点有空有节点不为空返回false

then 如果左结点不空右结点空或左右均为空那么后面必须均是叶节点(层次遍历)

```java
	public static boolean isCBT(Node head) {
		if (head == null) {
			return true;
		}
		Queue<Node> queue = new LinkedList<Node>();
		boolean leaf = false;
		Node l = null;
		Node r = null;
		queue.offer(head);
		while (!queue.isEmpty()) {
			head = queue.poll();
			l = head.left;
			r = head.right;
			if ((leaf && (l != null || r != null)) || (l == null && r != null)) {
				return false;
			}
			if (l != null) {
				queue.offer(l);
			}
			if (r != null) {
				queue.offer(r);
			} else {
				leaf = true;
			}
		}
		return true;
	}
```

### 已知一棵完全二叉树,求其节点的个数

要求:时间复杂度低于0(N),N为这棵树的节点个数

低于O(N) 不能遍历求结点个数

1.遍历左结点 计算高度 mostLeftLevel

2.遍历右子树的左结点判断是否到达最底层

​	2.1如果到达最底层, 左子树是满二叉树, 递归调用右子树

​	2.2如果没到到最底层,右子树是满二叉树, 递归调用左子树

```java
	public static int nodeNum(Node head) {
		if (head == null) {
			return 0;
		}
		return bs(head, 1, mostLeftLevel(head, 1));
	}

	public static int bs(Node node, int l, int h) {
		if (l == h) {
			return 1;
		}
		if (mostLeftLevel(node.right, l + 1) == h) {
			return (1 << (h - l)) + bs(node.right, l + 1, h);
		} else {
			return (1 << (h - l - 1)) + bs(node.left, l + 1, h);
		}
	}

	public static int mostLeftLevel(Node node, int level) {
		while (node != null) {
			level++;
			node = node.left;
		}
		return level - 1;
	}
```

