# KickStart 2019

## 技巧

### 0.前250差不多可以通过Google在线笔试 各轮次去重后700人 最终招了20人 + 30实习生 = 50人左右

### 1.不能引用numpy

### 12.数组 前缀和 前缀积 技巧

前缀和、前缀积也称前缀和数组，前缀积数组。

给一数组A，

前缀和：新建一数组B，数组中每一项B[i]保存A中[0…i]的和；

后缀和：新建一数组B，数组中每一项B[i]保存A中[i…n-1]的和；

前缀积：新建一数组B，数组中每一项B[i]保存A中[0…i]的积；

后缀积：新建一数组B，数组中每一项B[i]保存A中[i…n-1]的积；

数组类问题 可以尝试 

- 先排序

- 后提前算法累加累乘
- O(1)的调用上面结果

### 3.最小值最大化问题 最大值最小化问题 二分答案法

经常有这样的问题，求xxx最小值的最大值，即求符合条件的值里的最大值，这种问题有个解法叫二分答案法。一听，什么，不知道的答案也能二分？嗯没错，关键在于这个答案是可以判断是不是符合条件的。

**将优化问题转化决策问题**

#### 算法思想

以求最小值的最大值（最小值最大化）为例，尝试一个可能的答案，如果这个答案符合题目条件，那么它肯定是“最小”（可行解），但不一定是“最大”（最优解），然后我们换个更大的可能答案，如果也符合条件，那这个新可行解就更优，不断重复即可。怎么找呢？这时就该二分上场了。

#### 二分前提

1.答案区间上下限确定，即最终答案在哪个范围是容易知道的。

2.检验某值是否可行是个简单活，即给你个值，你能很容易的判断是不是符合题目要求。

3.可行解满足区间单调性，即若x是可行解，则在答案区间内x+1（也可能是x-1）也可行。

#### 两种情况

下图中L,R为当前答案区间，M为中心点，根据二分思想判断M是否符合条件，再移动L或R，变成L'，R'，图中的T和F表示是否符合条件。

##### 1.最小值最大化

```c
int l = min_ans, r = max_ans;
while (l < r) {
	int mid = (l + r + 1) / 2;   //+1避免 r == l + 1 时mid一直等于l，从而死循环
	if (ok(mid))	//符合条件返回True
		l = mid;
	else
		r = mid - 1;
}
```

希望答案尽可能大，所以我们需要确保左区间L点符合题目条件（最小），至于R是否符合条件是不确定的，首先判断M点符合与否，符合则将L移到M点，维持了L的True属性，也增大了所要的最小值所在区间，如果不符合，没办法在保持L的True属性情况下移动L，那就移动R。

![img](/Users/zhengxinzhi/typora_img/KickStart2019总结/20160315160400082.png)

##### 2.最大值最小化

```c 
int l = min_ans, r = max_ans;
while (l < r) {
	int mid = (l + r) / 2;
	if (ok(mid))	//符合条件返回True
		r = mid;
	else
		l = mid + 1;
}
```


按同样道理分析，维持R的True属性即可。这里的mid就不需要加1了，因为 mid 跟 l 重合时，l = mid + 1;会自增，而当 mid 和 r 重合时 l 也跟 r 重合，结束循环了。

![img](/Users/zhengxinzhi/typora_img/KickStart2019总结/20160611153717723.png)

#### 注意点

\1. 每次循环都要确保L和R有一个被更新，否则死循环就呵呵了。

\2. 答案是浮点数的情况：区间更新不能加1，这样变动太大，直接

```cpp
l = mid;
r = mid;
```

### 4.三分法

当二分的函数值不是递增/减，而是先增后减或者先减后增时二分就挂了。此时需要三分法，这里直接盗用[hihocoder Problem 1142](http://hihocoder.com/problemset/problem/1142)的图

![img](/Users/zhengxinzhi/typora_img/KickStart2019总结/14281364244078.png)

如图这种情况先减后增有极小，若lm比rm低（即lm对应的函数值 < rm函数值）则极小点（图中最低点）肯定在[ left, rm ] ，反之在[ lm, right ]，剩下就跟二分一样根据大小关系调整区间就行了。那lm和rm取值多少？一个不错的取值是lm为整个区间的1/3点，rm为2/3点，即
lmid = l + (r - l)/3;
rmid = r - (r - l)/3;
嗯三分就这样完了。

然后另外一种情况，先增后减有极大：

![img](/Users/zhengxinzhi/typora_img/KickStart2019总结/20160430000457502.png)

##### [HDU 2899 Strange fuction](http://acm.hdu.edu.cn/showproblem.php?pid=2899)

##### [hihocoder 1142 ](http://hihocoder.com/problemset/problem/1142?sid=785562)

### 5.数组 偏移量向量 N皇后

```c++
// 上右下左 钟表序
// dx代表列 dy代表行
int dx[4] = {-1, 0, 1, 0};
int dy[4] = {0, 1, 0, -1};
```

### 6.线段树

### 7.sweep line algorithm 扫描线算法

### 8.贪心是最难的题目

想证明太难 思路很难 但容易蒙对 可以采用模拟case的办法





## C++语法

### 1.vector map set

![image-20190530161548653](/Users/zhengxinzhi/typora_img/KickStart2019总结/image-20190530161548653.png)

![image-20190530161643870](/Users/zhengxinzhi/typora_img/KickStart2019总结/image-20190530161643870.png)

![image-20190530161758169](/Users/zhengxinzhi/typora_img/KickStart2019总结/image-20190530161758169.png)



![image-20190530162629805](/Users/zhengxinzhi/typora_img/KickStart2019总结/image-20190530162629805.png)



![image-20190530162805042](/Users/zhengxinzhi/typora_img/KickStart2019总结/image-20190530162805042.png)

![image-20190530162848960](/Users/zhengxinzhi/typora_img/KickStart2019总结/image-20190530162848960.png)

![image-20190530170257249](/Users/zhengxinzhi/typora_img/KickStart2019总结/image-20190530170257249.png)

![image-20190530170327343](/Users/zhengxinzhi/typora_img/KickStart2019总结/image-20190530170327343.png)

![image-20190530170413210](/Users/zhengxinzhi/typora_img/KickStart2019总结/image-20190530170413210.png)

![image-20190530193638978](/Users/zhengxinzhi/typora_img/KickStart2019总结/image-20190530193638978.png)

![image-20190530194254048](/Users/zhengxinzhi/typora_img/KickStart2019总结/image-20190530194254048.png)



![image-20190530194334521](/Users/zhengxinzhi/typora_img/KickStart2019总结/image-20190530194334521.png)

![image-20190530194400884](/Users/zhengxinzhi/typora_img/KickStart2019总结/image-20190530194400884.png)



![image-20190530194951623](/Users/zhengxinzhi/typora_img/KickStart2019总结/image-20190530194951623.png)

### 2.c++ 1s可计算10E7 ~10E8的计算量

### 3.int 最大值#include <limits.h> INT_MAX

### 4.#include \<algorithm> sort

### 5.初始化函数 memset ( void * ptr, int value, size_t num );

memset(dist, -1, sizeof dist)

### 6.#include <string.h>

### 7.queue #include \<queue>

| [front](https://en.cppreference.com/w/cpp/container/queue/front) | access the first element  (public member function)           |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| [back](https://en.cppreference.com/w/cpp/container/queue/back) | access the last element  (public member function)            |
| Capacity                                                     |                                                              |
| [empty](https://en.cppreference.com/w/cpp/container/queue/empty) | checks whether the underlying container is empty  (public member function) |
| [size](https://en.cppreference.com/w/cpp/container/queue/size) | returns the number of elements  (public member function)     |
| Modifiers                                                    |                                                              |
| [push](https://en.cppreference.com/w/cpp/container/queue/push) | inserts element at the end  (public member function)         |
| [emplace](https://en.cppreference.com/w/cpp/container/queue/emplace)(C++11) | constructs element in-place at the end  (public member function) |
| [pop](https://en.cppreference.com/w/cpp/container/queue/pop) 返回None | removes the first element  (public member function)          |
| [swap](https://en.cppreference.com/w/cpp/container/queue/swap) | swaps the contents  (public member function)                 |

### 8.stack #include \<stack>

1. s.push(item);       //将item压入栈顶  
2. s.pop();            //删除栈顶的元素，但不会返回  
3. s.top();            //返回栈顶的元素，但不会删除  
4. s.size();           //返回栈中元素的个数  
5. s.empty();          //检查栈是否为空，如果为空返回true，否则返回false  

### 8.pair

构建法1: {i, j}

构建法2: make_pair(i, j)

| member variable | definition                   |
| --------------- | ---------------------------- |
| `first`         | The first value in the pair  |
| `second`        | The second value in the pair |

### 9.template

这个是C++中的模板..template<typename T> 这个是定义模板的固定格式,规定了的..模板应该可以理解到它的意思吧.. 比如你想求2个int float 或double型变量的值,只需要定义这么一个函数就可以了,假如不用模板的话,你就必须针对每种类型都定义一个[sum函数](https://www.baidu.com/s?wd=sum函数&tn=44039180_cpr&fenlei=mv6quAkxTZn0IZRqIHckPjm4nH00T1Y4njmdn1msm1bdPyuBP1640ZwV5Hcvrjm3rH6sPfKWUMw85HfYnjn4nH6sgvPsT6KdThsqpZwYTjCEQLGCpyw9Uz4Bmy-bIi4WUvYETgN-TLwGUv3EPHR3njRdnjT3)..int sum(int, int);float sum(float, float);double sum(double, double);

### 10.优先级

![image-20190601153824322](/Users/zhengxinzhi/typora_img/KickStart2019总结/image-20190601153824322.png)

### 11.int的范围2.1**(10E9)

### 12.数据范围

abs(INT_MIN) == INT_MIN (溢出)

```
Value of INT_MAX is +2147483647.
Value of INT_MIN is -2147483648.
```

    -- -- char                            8 -2^7 ~ 2^7-1 %c %c、%d、%u
    
    signed -- char                    8 -2^7 ~ 2^7-1 %c %c、%d、%u
    
    unsigned -- char                8 0 ~ 2^8-1 %c %c、%d、%u
    
    [signed] short [int]            16 -2^15 ~ 2^15-1 %hd
    
    unsigned short [int]           16 0 ~ 2^16-1 %hu、%ho、%hx
    
    [signed] -- int                    32 -2^31 ~ 2^31-1 %d   -2147483648 ～ 2147483647
    
    unsigned -- [int]                 32 0 ~ 2^32-1 %u、%o、%x
    
    [signed] long [int]              32 -2^31 ~ 2^31-1 %ld
    
    unsigned long [int]             32 0 ~ 2^32-1 %lu、%lo、%lx
    
    [signed] long long [int]       64 -2^63 ~ 2^63-1 %I64d
    
    unsigned long long [int]      64 0 ~ 2^64-1 %I64u、%I64o、%I64x
    
    -- -- float                            32 +/- 3.40282e+038 %f、%e、%g
    
    -- -- double                        64 +/- 1.79769e+308 %lf、%le、%lg %f、%e、%g
    
    -- long double                    96 +/- 1.79769e+308 %Lf、%Le、%Lg
## Round A

### Training 公平足球队

```
作为当地学校的足球教练，你的任务是挑选一支完全由P学生组成的团队代表你的学校。有N名学生供您选择。第i名学生的技能等级为Si，这是一个正整数，表示他们的技术水平。

如果它有P个学生，并且他们都具有相同的技能等级，你可以确定这个团队是公平的。最初，可能无法选择一个公平的团队，因此您将为一些学生提供一对一的辅导。需要一个小时的辅导才能将任何学生的技能等级提高1。

比赛季很快就开始了（事实上，第一场比赛已经开始了！），所以你想找到你需要提供的最少训练小时数才能选出一支公平的球队。

输入
输入的第一行给出了测试用例的数量，T。T测试用例如下。每个测试用例分别以包含两个整数N和P，学生数和您需要选择的学生数的行开头。然后，另一行包含N个整数Si;这些中的第i个是第i个学生的技能。

产量
对于每个测试用例，输出一行包含Case #x：y，其中x是测试用例编号（从1开始），y是所需的最小教练小时数，然后才能选择一个公平的P学生团队。

范围
时间限制：每个测试集15秒。
内存限制：1 GB。
1≤T≤100。
对于所有i，1≤Si≤10000。
2≤P≤N。

测试装置1（可见）
2≤N≤1000。

测试装置2（隐藏）
2≤N≤105。

样品

输入

产量

3
4 3
3 1 9 100
6 2
5 5 1 2 3 4
5 5
7 7 1 7 7

案例＃1：14
案例＃2：0
案例＃3：6

在样本案例＃1中，您可以花费总共6个小时训练第一个学生，8个小时训练第二个学生。这使得第一，第二和第三学生的技能水平为9.这是您可以花费的最短时间，因此答案是14。

在示例案例＃2中，您可以选择一个公平的团队（第一个和第二个学生）而无需进行任何指导，因此答案为0。

在示例案例＃3中，P = N，因此每个学生都将加入您的团队。你必须花6个小时训练第三个学生，这样他们就像其他人一样拥有7级技能。这是您可以花费的最短时间，所以答案是6。
```

#### 解法

##### 解法1  ✅ + TLE

time O(TPN)

```python
T = int(input().strip())
for i in range(1, T + 1):
    N, P = map(int, input().strip().split())
    team = map(int, input().strip().split())
    team = sorted(team)
    min_val = float('inf')
    for j in range(N-P+1):
        val = team[j+P-1]*P - sum(team[j: j+P])
        min_val = min(val, min_val)
    print("Case #{}: {}".format(i, min_val))

```

##### 解法2 ✅ + ✅ 前缀和法 

time O(NlogN)

```python
T = int(input().strip())
for i in range(1, T + 1):
    N, P = map(int, input().strip().split())
    team = map(int, input().strip().split())
    team = sorted(team)
    # key: cumsum 计算 节省了时间复杂度
    cumsum_team = [0]
    for x in team:
        cumsum_team.append(cumsum_team[-1] + x)
    min_val = float('inf')
    for j in range(N-P+1):
        val = team[j+P-1]*P - (cumsum_team[j+P] - cumsum_team[j])
        min_val = min(val, min_val)
    print("Case #{}: {}".format(i, min_val))
```

```c++
#include <iostream>
#include <algorithm>
#include <limits.h>

using namespace std;

// 题目数据范围10^5 多10个防止越界
const int N = 100010;

int n, p;
int skill[N], sum[N];

int main() {
  int T;
  cin >> T;
  for(int i = 1; i <= T; i++){
      cin >> n >> p;
      // 从1开始
      for(int i = 1; i <= n; i++) cin >> skill[i];
      // algorithm::sort
      sort(skill + 1, skill + 1 + n);
      // 前缀和
      for(int i = 1; i <= n; i++) sum[i] = sum[i-1] + skill[i];
      int res = INT_MAX;
      for(int i = p; i <= n; i++){
          res = min(res, p * skill[i] - (sum[i] - sum[i-p]));
      }
      // 输出结果
      cout << "Case #%d: %d\n" << endl;
  }
  return 0;
}
```



#### Analysis

```
为了建立一个公平的团队，我们必须培训团队成员，使其达到与团队中最熟练的成员相同的技能水平。
对于我们选择的任何P学生，组建公平团队所需的时间是=Σ（max（Si，Si + 1 ... SP） -  Si），对于团队中所有学生i = 1到P.我们的目标是尽量减少这个价值。
一种可能的方法是从给定的N个学生中查看P学生的所有可能子集。但是存在NCP这样的子集（这里符号C表示组合）。这些子集的数量将按因子（N）的顺序排列，因此所有这些子集的枚举都不符合时限。

测试装置1（可见）
我们可以从观察开始，一旦我们确定了具有最高技能水平x的学生，为了最小化我们的目标，我们应该总是选择具有尽可能高但是小于或等于x的技能的学生。因此，我们可以按照降序排列技能水平对学生进行排序，并假设他们拥有团队中最高的技能水平，并对每个学生进行迭代。比如说，这名学生处于排序顺序的第i位置;该团队将由位置i，i + 1，...，i + P-1（即大小为P的连续子阵列）的学生组成。
对于排序数组中每个P大小的子阵列，我们可以使用上述公式计算建立公平团队所需的训练时间。存在大小为P的N-P + 1个子阵列，并且计算子阵列大小P的训练时间的复杂度为O（P）。因此，该方法的总体复杂度为O（N×P），这对于测试集1是足够的。

测试装置2（隐藏）
我们需要经历所有子阵列，但是我们能否更快地计算出子阵列的训练时间？
让我们假设数组按递减顺序排序。从位置i开始的子阵列的训练时间公式是
=Σ（S [i]  -  S [j]）其中j = i至i + P -1
= P×S [i]  - Σ（S [j]）其中j = i至i + P-1
由于我们总是需要一个连续子阵列的总和，我们可以预先计算整个数组的前缀和，并在O（1）时间内得到任何子阵列的总和，这样就可以计算训练时间O（1） 。
因此，这种方法的总体复杂性是O（NlogN）。
```



### 快递公司最大距离最小值

```
您最近被聘为着名包裹投递公司的首席决策者（CDM），恭喜！客户喜欢快速交付包裹，您决定减少在全球范围内交付包裹以赢得客户所需的时间。你已经向当局介绍了这个想法，他们已经为你分配了足够的预算来建立一个新的交付办公室。

世界可以分为R×C方格。每个广场都包含一个送货办公室，或者没有。您可以选择一个尚未包含交付办公室的网格广场，并在那里建立一个新的交付办公室。

如果该正方形包含交付局，则包裹到正方形的交货时间为0。否则，它被定义为该广场与包含交付局的任何其他广场之间的最小曼哈顿距离。**总交货时间是所有方格的交货时间的最大值**。您最多可以通过建立一个新的交付办公室获得的最短总交货时间是多少？【所有放个交货时间的最大值的最小时间】

注意：两个正方形（r1，c1）和（r2，c2）之间的曼哈顿距离定义为| r1  -  r2 | + | c1  -  c2 |，其中| * |运算符表示绝对值。

输入
输入的第一行给出了测试用例的数量，T。T测试用例如下。每个测试用例的第一行包含行数R和网格列数C.接下来的R行中的每一行包含从集合{0,1}中选择的C字符串，其中0表示没有递送办公室，1表示在广场中存在递送办公室。

产量
对于每个测试用例，输出一行包含Case #x：y，其中x是测试用例编号（从1开始），y是在最多添加一个额外的递送办公室后可以获得的最小总交付时间。

范围
时间限制：每个测试集15秒。
内存限制：1GB。
1≤T≤100。
初始网格中至少有一个交付局。

测试装置1（可见）
1≤R≤10。
1≤C≤10。

测试装置2（隐藏）
1≤R≤250。
1≤C≤250。

样品

输入

产量

3
3 3
101
000
101
1 2
11
5 5
10001
00000
00000
00000
10001

案例＃1：1
案例＃2：0
案例＃3：2

在样本案例＃1中，通过在没有交付办公室的五个方块中的任何一个中建立新的交付办公室，您可以获得最小的总交付时间1。

在示例案例＃2中，所有正方形都已经有一个交付办公室，因此最小总交付时间为0.请注意，您必须添加最多一个交付办公室。

在样本案例＃3中，要获得最小的总交付时间2，您可以在以下任何方格中建立新的交付办公室：（2,3），（3,2），（3,3），（3， 4），或（4,3）。任何其他可能性导致总交付时间比2更高。
```



#### Analysis

```
测试装置1（可见）
对于测试装置1，我们能够检查新配送办公室的所有可能位置，以便找到最小化交货时间的位置。我们将分两个阶段完成。首先，我们将根据现有的交付办公室计算每个广场的交付时间。其次，我们将尝试新办公室的所有可能位置。第一部分中的预计算将允许我们更有效地在第二部分中找到交货时间。

对于第一阶段，我们通过迭代整个网格并找到到具有交付局的正方形的最小曼哈顿距离来计算正方形的交付时间。对于O（（RC）2）的总时间复杂度，这具有每平方O（RC）的时间复杂度。

对于第二阶段，我们遍历新交付局和每个位置的所有可能位置，在整个网格中搜索具有最大新交付时间的方块。新的交货时间是在第一部分计算的交货时间和到新交货办事处的曼哈顿距离的最小值。对于总时间复杂度为O（（RC）2），这也具有每个传送位置的O（RC）的时间复杂度，这对于测试装置1是足够的。

或者，如果我们使用更快的方式计算每个方格的交付时间（例如广度优先搜索），我们可以跳过第一阶段。有关详细信息，请参阅下一节。


测试装置2（隐藏） 最大问题最小化 二分答案法+BFS
我们可以对测试集2使用类似的方法，但是我们需要一种更有效的方法来计算新交付地点的最大交付时间。我们将通过解决以下子问题来实现这一点：给定K值，我们是否可以添加新的交付办公室，以便最大交付时间最多为K？该子问题的解决方案与测试集1类似，但是目标值为K允许我们确切地确定新交付局需要为哪些方块提供服务。我们可以利用这种差异来创建更快的解决方案。

请注意，如果对原始问题的答案为K，则对于[K，1]范围内的值，我们的子问题的答案为“否”，对于[K，无穷大]范围内的值，答案为“是”。对于这些类型的子问题，我们可以使用二分搜索：如果给定的值有效，则它是答案的上限;否则它是答案的严格下限。因此，一旦我们有了子问题的解决方案，我们就可以使用二分搜索来解决原始问题。这是将优化问题转化为决策问题的常用技术。

首先，我们通过反转问题有效地计算每个广场的现有交货时间：我们找到每个广场到达每个广场的最短距离，而不是找到每个广场的最短距离。这可以使用从所有交付办公室开始的多源，广度优先搜索来完成。多源BFS与常规BFS相同，只是您使用多个起始位置而不是一个。此搜索最多访问每个方块一次，这给了我们O（RC）的时间复杂度。

其次，我们识别所有具有大于K的传递时间的正方形，然后确定是否存在与这些正方形中的每一个相距K的距离内的位置。为了有效地做到这一点，我们注意到曼哈顿距离具有相同的公式：

dist（（x1，y1），（x2，y2））= max（abs（x1 + y1  - （x2 + y2）），abs（x1  -  y1  - （x2  -  y2）））

该公式基于以下事实：对于任何点，曼哈顿距离K内的点集形成旋转45度的正方形。这个公式的好处是，如果我们修正（x2，y2），当x1 + y1和x1-y1最大化或最小化时，距离将最大化。

因此，我们可以为交付时间大于K的所有方格计算x1 + y1和x1-y1的最大值和最小值。然后，我们可以尝试新交付局的所有可能位置，并检查最大距离是否为当前交货时间大于K的正方形的位置在恒定时间内至多为K.因此，我们可以检查答案是否最多为K，时间复杂度为O（RC）。

使用二进制搜索，时间复杂度变为O（RClog（R + C）），这对于测试集来说已足够。有一种方法可以通过在网格上单次通过计算上述所有可能K的最小/最大值，然后使用个案工作来确定每个K是否存在可行的新交付办公地点，从而将其提高到O（RC）时间，但这种优化是不必要的
```

![image-20190531132117526](/Users/zhengxinzhi/typora_img/KickStart2019总结/image-20190531132117526.png)

#### 解法

##### 解法二 BFS + 二分答案答案法（最大问题最小化）

将优化问题转化为决策问题

1.选取K作为答案判断是否是最大问题的结果（不一定是最小结果）

2.由于求问题的最小化结果，则[1, K)答案为No，[K, R+C]答案Yes

3.最大值最小化二份答案法，**O（log（R+C）） **

```c 
int l = min_ans, r = max_ans;
while (l < r) {
	int mid = (l + r) / 2;
	if (ok(mid))	//符合条件返回True
		r = mid;
	else
		l = mid + 1;
}
```

4.ok函数 用bfs， 即从所有1出发 遍历K层，**O(RC) **

5.曼哈顿距离性质

**dist((x1，y1)，(x2，y2))= max(abs((x1 + y1)  - （x2 + y2))，abs((x1  -  y1) - (x2  -  y2)))**

6.找到一个(x_k, y_k)点满足所有未遍历过的点的距离均小于等于K，即dist((x1, y1), (x_k, y_k)) <= K **O(RC)**，可推出（性质）

x + y de min max

x - y de min max

当x1 + y1和x1-y1最大化或最小化时，距离将最大化。(边界点)

因为如果遍历所有k点要O(RC)，而固定K点计算未遍历点也要O(RC), 共O(R^2C^2)，复杂度太高，我们找边界点即上面四个数

![image-20190531141601690](/Users/zhengxinzhi/typora_img/KickStart2019总结/image-20190531141601690.png)

7.计算所有可能的交付办公室到几个边界点的距离，找到最小的 O(RC)**

O(RC*log(R+C))

```c++
#include <iostream>
#include <stdio.h>
#include <algorithm>
#include <limits.h>
#include <string.h>
#include <queue>

using namespace std;

// 题目数据范围250 多5个防止越界
const int N = 255;
// r c
int n, m;
// g 整个地图
string g[N];
// dist 每个空地到达补给站的距离
int dist[N][N];

// 多源点bfs
int bfs(int k){
    // 从所有1出发，把所有K步以内能遍历到的点全部mark一遍, 具体的距离数值可以不用精确 即原先点标记过的K步内点可忽略 因为已经被标记了
    queue<pair<int, int>> q;
    // 初始化为-1
    memset(dist, -1, sizeof dist);
    // 用所有1的点初始化queue
    for(int i = 0; i < n; i++){
        for(int j = 0; j < m; j++){
            if(g[i][j] == '1'){
                dist[i][j] = 0;
                q.push({i, j});
            }
        }
    }
    // 上右下左 钟表序
    // dx代表列 dy代表行
    int dx[4] = {-1, 0, 1, 0};
    int dy[4] = {0, 1, 0, -1};  
    while(q.size()){
        auto t = q.front();
        q.pop();
        int x = t.first, y = t.second;
        int distance = dist[x][y];
        // 若当前已经是第k层，不可以这个点继续扩散
        if(distance == k){
            continue;
        }
        // 枚举四个方向
        for(int i = 0; i < 4; i++){
            int a = x + dx[i];
            int b = y + dy[i];
            // 坐标不要出界且该单元未遍历过
            if(a >= 0 && a < n && b >= 0 && b < m && dist[a][b] == -1){
                dist[a][b] = distance + 1;
                q.push({a, b});
            }
        }
    }
}

bool check(int k){
    bfs(k);
    // bfs后 dist里所有-1的点都是未遍历的点, 即到最近1的点大于K
    // min(x + y)定义为min_sum 以此类推
    int min_sum = INT_MAX, max_sum = INT_MIN;
    int min_sub = INT_MAX, max_sub = INT_MIN;

    for(int i = 0; i < n; i++){
        for(int j = 0; j < m; j++){
            if(dist[i][j] == -1){
                min_sum = min(min_sum, i + j);
                max_sum = max(max_sum, i + j);
                min_sub = min(min_sub, i - j);
                max_sub = max(max_sub, i - j);
            }
        }
    }
    // 若所有点都被标记过 符合结果要求 返回true
    if(min_sum == INT_MAX) return true;

    for(int i = 0; i < n; i++){
        for(int j = 0; j < m; j++){
            if(g[i][j] == '0'){
                // 曼哈顿距离的 特点dist((x1，y1)，(x2，y2))= max(abs((x1 + y1)  - （x2 + y2))，abs((x1  -  y1) - (x2  -  y2)))
                int sum = max(abs(i + j - min_sum), abs(i + j - max_sum));
                int sub = max(abs(i - j - min_sub), abs(i - j - max_sub));
                if(max(sum, sub) <= k){
                    return true;
                }
            }
        }
    }
    return false;
}

int main(){
    int T;
    cin >> T;
    for(int c = 1; c <= T; c++){
        cin >> n >> m;
        for(int i = 0; i < n; i++) cin >> g[i];
        // 二分答案法
        int l = 0, r = n + m;
        while(l < r){
          int mid = (l + r) >> 1;
          if(check(mid)){
              r = mid;
          }else{
              l = mid + 1;
          }
        }
        printf("Case #%d: %d\n", c, r);
    }
    return 0;
}




```

### 电影票门票预定



```
您正在出售电影院前排座位的门票。前排有N个座位，从左到右编号为1到N.你上周离开了办公室，回来后，Q座位预订已经堆积！第i个预订要求从Li到Ri的所有座位。您现在无聊地将每个预订输入系统，一次一个。
由于某些预订可能会重叠，因此系统可能无法完全满足每个预订。当您在系统中输入预订时，它会将预订所请求的未被分配的座位分配出去
最大整数k是各个订单分配的最小座位数，题目要求k的最大值
输入
输入的第一行给出了测试用例的数量，T。T测试用例如下。每个测试用例都以包含两个整数N和Q，座位数和预订数量的行开头。然后，还有Q行，其中第i行包含两个整数Li和Ri，表示第i个预订想要预订从Li到Ri的所有座位，包括在内。
产量
对于每个测试用例，输出一行包含Case #x：y，其中x是测试用例编号（从1开始），y是最大值k，如上所述。
范围
时间限制：每个测试集30秒。
内存限制：1GB。
T = 100。
1≤N≤106。
1≤Li≤Ri≤N。
测试装置1（可见）
1≤Q≤300。
测试装置2（隐藏）
1≤Q≤30000。
对于至少85个测试用例，Q≤3000。
样品
输入

产量

3
5 3
1 2
3 4
2 5
30 3
10 11
10 10
11 11
10 4
1 8
4 5
3 6
2 7

案例＃1：1
案例＃2：0
案例＃3：2
在样本案例＃1中，有N = 5个席位和Q = 3个预订。一个可能的顺序是：
在第二次预订中，系统将预订2个座位（3和4）。
在第一次预订中，系统将预订2个座位（1和2）。
在第三次预订中，系统将预订1个座位（仅限座位5，因为座位1,2,3和4已经预订）。
每个预订至少分配1个座位，并且没有订单为每个预订分配至少2个座位，因此答案是1。

在样本案例＃2中，有N = 30个席位且Q = 3个预订。无论您分配的座位是什么，至少有一个预订将没有分配座位。所以答案是0.请注意，可以有座位不属于任何预订！

在样本案例＃3中，有N = 10个席位且Q = 4个预订。一个可能的顺序是：
在第二次预订中，系统将预订2个座位（4和5）。
在第三次预订中，系统将预订2个座位（3和6，因为4和5已经预订）。请注意，预订的座位不一定彼此相邻。
进入第四次预订，系统将预订2个座位（2和7）。
在第一次预订时，系统将预订2个座位（1和8）。
每个预订至少分配2个座位，并且没有订单为每个预订分配至少3个座位，因此答案是2。

注意：我们不建议对此问题的Large数据集使用解释/慢速语言
```

#### Analysis

一般最大问题最小化（最小问题最大化）用二分答案法

```
分析
请求的可能排序数量是Factorial(Q),对于任何一个测试集都不够快。我们可以观察到，对于所选择的请求排序，系统在上一个请求中预订的席位数量不依赖于先前Q-1请求的排序。因此，我们可以从找到最后处理的请求开始，然后向后移向先前的请求。答案是Q步骤中预订的最低座位数。

另一个观察是，在每一步中，我们总是可以从剩余的集合中贪心地选择最后一个请求：我们可以预订最大座位数。一个直观的证明，我们可以使用交换参数来证明这一观察结果，因为选择预订较少座位的请求不会给我们更好的答案。

测试装置1（可见）
一个简单的实现将是O（N×Q）的顺序，其中我们使用针对每个Q步骤的扫描线算法sweep line algorithm 重新计算O（N）中的剩余请求的座位数。我们可以通过另一个观察来加快速度：请求覆盖的唯一范围的数量最大为2 * Q，这将使我们的解决方案在每个测试用例的O（Q2）中运行。

测试装置2（隐藏） 贪心 + 线段树
让我们尝试加快重新计算每个步骤可以预订的座位数量的缓慢过程对于每个座位，让我们将座位的价值表示为试图预订此座位的剩余请求数量。每当座位的价值降至1时，我们会增加我们可以为包含此座位预订的相应请求预订的座位数。

由于请求由间隔表示，因此我们可以使用间隔树来支持在树的初始构造之后移除间隔的操作。区间树的每个节点都存储覆盖它的间隔集，以及其范围内任何座位的最小值。每当移除操作使最小值变为1时，我们沿树向下走，找到成为一个的座位，然后向上走回树，找出哪个间隔是现在覆盖该座位的唯一间隔。我们现在将座位的值设置为无穷大，以便我们不再处理它。每个座位只发生一次，总摊还成本为O（NlogN）。总的来说，如果你使用一个恒定的时间集（比如一个hashset），这个算法就是O（Nlog（Q + N））。
```



#### 解法

##### 解法一 贪心 + 线段树

1.由于系统在第Q个订单预定的席位数不依赖于前Q-1个订单的排序，只取决于哪些座位被占了，因此我们逆序处理

2.可用贪心策略找出第Q个订单，即第Q个订单相比于其他的订单（即前面的Q-1个订单）没有被前面订单覆盖的最多的订单

![image-20190531202615020](/Users/zhengxinzhi/typora_img/KickStart2019总结/image-20190531202615020.png)

3.



![image-20190531194850099](/Users/zhengxinzhi/typora_img/KickStart2019总结/image-20190531194850099.png)

##### 解法二 二分答案法 最小问题最大化 [todo]

二分。
重点在于判断是否可行阶段。
首先，对于两个预定，它们只有两种执行顺序，a先b后，或b先a后。
现在判断的思路是，
首先根据区间左右端点排序，第一关键字为起始点，第二关键字为− -−终止点，
要使得这些预定尽可能满足最小值k的需求，每次求得第i 个区间满足需求后，最后一个座位后一个座位，设为ed 
对于两个区间，进行以下讨论：

两个区间相离，无需要进行特别讨论；
两个区间相交，
如果后一个区间起始点在ed 之后，那么则可以在执行的时候，让后者先执行；
如果后一个区间起始点在ed之前，那么只能先将后者起始点偏移到ri+1，否则第i 个区间就满足不了了；
如果前者包含了后者，那么，后者必须先执行，因此在计算前者有效位置的时候，需要跳过后者；
根据上述规则，可以在O(Q2)的时间内完成判断，算法总复杂度为O(Q2logN) 

---

首先我们可以观察到，对于一个 booking 来说，如果它放在最后，那么它能确定的seat 数目一定是固定的，与前面 booking 的顺序无关。

同时我们 可以证明这样一个**贪心的做法**一定是正确的:

倒着确定booking 的顺序，每次取能booking 到的seat 最多的去掉，然后在剩余集合中如法炮制

---

```c++
// In the name of God

#include <iostream>
#include <algorithm>
#include <fstream>
#include <vector>
#include <deque>
#include <assert.h>
#include <queue>
#include <stack>
#include <set>
#include <map>
#include <stdio.h>
#include <string.h>
#include <utility>
#include <math.h>
#include <bitset>
#include <iomanip>
#include <complex>

using namespace std;

#define rep(i, a, b) for (int i = (a), i##_end_ = (b); i < i##_end_; ++i)
#define debug(...) fprintf(stderr, __VA_ARGS__)
#define mp make_pair
#define x first
#define y second
#define pb push_back
#define SZ(x) (int((x).size()))
#define ALL(x) (x).begin(), (x).end()

template<typename T> inline bool chkmin(T &a, const T &b) { return a > b ? a = b, 1 : 0; }
template<typename T> inline bool chkmax(T &a, const T &b) { return a < b ? a = b, 1 : 0; }
template<typename T> inline bool smin(T &a, const T &b)   { return a > b ? a = b : a;    }
template<typename T> inline bool smax(T &a, const T &b)   { return a < b ? a = b : a;    }

typedef long long LL;

const int N = (int) 1e6 + 6, mod = (int) 0;
int n, q, xl[N], xr[N], mvl[N];
pair<int, int> sr[N];

int check(int k) {
	for (int j = 0; j < q; ++j)
		xl[j] = sr[j].first, xr[j] = -sr[j].second, mvl[j] = xl[j];
	for (int j = 0; j < q; ++j) {
		int l = xl[j], r = xr[j];	
		int st = mvl[j];
		int allowed_after = r;
		int cnt = 0;
		for (int i = j + 1; i < q; ++i) {
			if (xr[i] <= r) {
				if (xl[i] <= st) {
					st = max(st, xr[i]);	
				} else {
					cnt += xl[i] - st;
					st = max(st, xr[i]);
					if (cnt >= k) {
						allowed_after = xl[i] - (cnt - k);
						break;
					}
				}
			}
		}
		if (cnt < k) {
			cnt += r - st;
			if (cnt < k) return 0;
			allowed_after = r - (cnt - k);
		}
		for (int i = j + 1; i < q; ++i) {
			if (xl[i] >= allowed_after) break;
			if (xr[i] > r) {
				mvl[i] = max(mvl[i], r);
			}
		}
	}
	return 1;
}
int main() {
	int tc;
	cin >> tc;
	for (int tt = 1; tt <= tc; ++tt) {
		cout << "Case #" << tt << ": ";
		cin >> n >> q;
		for (int j = 0; j < q; ++j) {
			cin >> xl[j] >> xr[j], --xl[j];
			sr[j] = mp(xl[j], -xr[j]);	
		}
		sort(sr, sr + q);
		int bl = 0, br = n + 1;
		while (bl < br - 1) {
			int bm = bl + br >> 1;
			if (check(bm)) {
				bl = bm;
			} else {
				br = bm;
			}
		}
		cout << bl << '\n';

	}
}
```



## Round C

### Wiggle Walk

#### 问题描述

```
问题
Banny刚买了一台新的可编程机器人。为了测试他的编码技能，他将机器人放置在一个正方形网格中，其中R行（从北到南编号为1到R）和C列（从西到东编号为1到C）。行r和列c中的方块表示为（r，c）。

最初机器人在方块（SR，SC）开始。班尼将给予机器人N指令。每条指令是N，S，E或W中的一条，指示机器人分别向北，向南，向东或向西移动一个方格。

如果机器人移动到之前所处的方块，机器人将继续沿同一方向移动，直到它到达之前未曾进入过的方块。 Banny永远不会给机器人一个指令，使它移出网格。

你可以帮助Banny按照N指令确定机器人将完成哪个方格？

输入
输入的第一行给出了测试用例的数量，T。T测试用例如下。每个测试用例以包含五个整数N，R，C，SR和SC，指令数，行数，列数，机器人的起始行和起始列的行开始。

然后，另一行包含一个包含N个字符的字符串;这些字符中的第i个是Banny给机器人（N，S，E或W中的一个，如上所述）的第i个指令。

产量
对于每个测试用例，输出一行包含Case #x：r c，其中x是测试用例编号（从1开始），r是机器人完成的行，c是机器人完成的列。

范围
内存限制：1GB。
1≤T≤100。
1≤R≤5×104。
1≤C≤5×104。
1≤SR≤R
1≤SC≤C
说明不会导致机器人移出网格。

测试装置1（可见）
时间限制：20秒。
1≤N≤100。

测试装置2（隐藏）
时间限制：60秒。
1≤N≤5×104。

样品

输入
 
产量
Input  
3
5 3 6 2 3
EEWNS
4 3 3 1 1
SESE
11 5 8 3 4
NEESSWWNESE


  
案例＃1：3 2
案例＃2：3 3
案例＃3：3 7

  
样本案例＃1对应于左上图，样本案例＃2对应于右上图，样本案例＃3对应于下图。在每个图中，黄色方块是机器人开始的方格，而绿色方块是机器人完成的方格。
```

![image-20190530115112052](/Users/zhengxinzhi/typora_img/KickStart2019总结/image-20190530115112052.png)

#### 解法

##### bruce 模拟法 O(n^2)

```python
"""
small dataset
"""
def getPos(N, R, C, SR, SC, instructions):
    iset = {(SR, SC)}
    for c in instructions:
        if c == 'N':
            while((SR - 1, SC) in iset):
                SR -= 1
            SR -= 1

        elif c == 'S':
            while((SR + 1, SC) in iset):
                SR += 1
            SR += 1

        elif c == 'W':
            while((SR, SC - 1) in iset):
                SC -= 1
            SC -= 1

        elif c == 'E':
            while((SR, SC + 1) in iset):
                SC += 1
            SC += 1
        iset = iset | {(SR, SC)}
    return SR, SC

T = int(input().strip())
for i in range(1, T + 1):
    N, R, C, SR, SC = map(int, input().strip().split())
    instructions = map(str, input())
    ans1, ans2 = getPos(N, R,W C, SR, SC, instructions)
    print("Case #{}: {} {}".format(i, ans1, ans2))


```

##### 间隔集 O(nlogn)

```

测试装置1（可见）
人们可能试图简单地模拟问题陈述中提到的内容，即继续沿指定方向移动机器人直到它到达未访问的方格。这种方法将具有O（N2）的时间复杂度，尽管对于测试集1来说足够好。

测试装置2（隐藏）
上述方法的问题在于它访问了许多已经访问过的方块，在最坏的情况下，这些方块将是所有先前访问过的方块（考虑输入，在整个区域中给出交替的E和W）。如果我们以某种方式更快地到达每个指令的目标方块，我们可能会降低复杂性。

Let's say the robot is in some row r and received an instruction W. Now, all the already visited squares (if any) it will pass before reaching an unvisited square have to form a contiguous interval in row r. This suggests that we may use intervals to represent all the visited squares in the same row.
假设机器人在某行r中并且接收到指令W.现在，在到达未访问的正方形之前它将通过的所有已经访问的正方形（如果有的话）必须在行r中形成连续的间隔。这表明我们可以使用区间来表示同一行中的所有访问过的方块。

考虑到这一点，考虑我们为网格的每一行和每一列都有一组间隔来表示在该特定行或列中访问了哪些单元格，让我们称它们为间隔集。最初，除了对应于具有单个间隔（SC，SC）的行SR的集合以及对应于具有单个间隔（SR，SR）的列SC的集合之外，所有这些集合都是空的。

现在，使用这个数据结构，让我们尝试找到机器人的目标方块。假设它在方形（r，c）并得到指令W.为此，首先我们搜索对应于行r的区间集。我们将尝试找出此集合中的哪个区间包含c（必须确定为一个！）。一旦我们找到它，我们立即知道什么是机器人的新位置！很明显，同样的方法也适用于所有其他方向。

现在剩下的就是找到一种方法来适当地更新我们的数据结构，以便包括新访问的广场。这可以通过简单地在两者中找到该方块的相邻间隔，相应的列间隔集和相应的行间隔集来以非常标准的方式完成，然后通过扩展其中一个间隔或合并它们或添加它们来更新它们一个新的1长度间隔。

由于我们在每种情况下最多添加一个间隔，因此间隔的数量是O（N）。由于所有操作都是关于查找/插入/删除单个间隔，因此所有这些操作都可以在O（log（N））时间内轻松处理。因此，这种方法的所有运行时间都是O（Nlog（N））。使用哈希表也存在针对此问题的O（N）解决方案。它留给读者练习。
```

## Round D

### X or what 异或问题

#### 问题描述

```
Steven has an array of N non-negative integers. The i-th integer (indexed starting from 0) in the array is Ai.

Steven really likes subintervals of A that are xor-even. Formally, a subinterval of A is a pair of indices (L, R), denoting the elements AL, AL+1, ..., AR-1, AR. The xor-sum of this subinterval is AL xor AL+1 xor ... xor AR-1 xor AR, where xor is the bitwise exclusive or.

A subinterval is xor-even if its xor-sum has an even number of set bits in its binary representation.

Steven would like to make Q modifications to the array. The i-th modification changes the Pi-th (indexed from 0) element to Vi. Steven would like to know, what is the size of the xor-even subinterval of A with the most elements after each modification?

Input
The first line of the input gives the number of test cases, T. T test cases follow.

Each test case starts with a line containing two integers N and Q, denoting the number of elements in Steven's array and the number of modifications, respectively.

The second line contains N integers. The i-th of them gives Ai indicating the i-th integer in Steven's array.

Then, Q lines follow, describing the modifications. The i-th line contains Pi and Vi, The i-th modification changes the Pi-th element to Vi. indicating that the i-th modification changes the Pi-th (indexed from 0) element to Vi.

Output
For each test case, output one line containing Case #x: y_1 y_2 ... y_Q, where x is the test case number (starting from 1) and y_i is the number of elements in the largest xor-even subinterval of A after the i-th modification. If there are no xor-even subintervals, then output 0.

Limits
Time limit: 40 seconds per test set.
Memory limit: 1GB.
1 ≤ T ≤ 100.
0 ≤ Ai < 1024.
0 ≤ Pi < N.
0 ≤ Vi < 1024.

Test set 1 (Visible)
1 ≤ N ≤ 100.
1 ≤ Q ≤ 100.

Test set 2 (Hidden)
1 ≤ N ≤ 105.
1 ≤ Q ≤ 105.

Sample

Input 
 	
Output 
 
2
4 3
10 21 3 7
1 13
0 32
2 22
5 1
14 1 15 20 26
4 26

  
Case #1: 4 3 4
Case #2: 4
  
In Sample Case 1, N = 4 and Q = 3.
After the 1st modification, A is [10, 13, 3, 7]. The subinterval (0, 3) has xor-sum 10 xor 13 xor 3 xor 7 = 3. In binary, the xor-sum is 112, which has an even number of 1 bits, so the subinterval is xor-even. This is the largest subinterval possible, so the answer is 4.
After the 2nd modification, A is [32, 13, 3, 7]. The largest xor-even subinterval is (0, 2), which has xor-sum 32 xor 13 xor 3 = 46. In binary, this is 1011102.
After the 3rd modification, A is [32, 13, 22, 7]. The largest xor-even subinterval is (0, 3) again, which has xor-sum 32 xor 13 xor 22 xor 7 = 60. In binary, this is 1111002.
In Sample Case 2, N = 5 and Q = 1. After the 1st modification, A is [14, 1, 15, 20, 26]. The largest xor-even subinterval is (1, 4), which has xor sum 1 xor 15 xor 20 xor 26 = 0. In binary, this is 02.

史蒂文有一组N个非负整数。数组中的第i个整数（从0开始索引）是Ai。

史蒂文真的很喜欢A的子区间。形式上，A的子区间是一对索引（L，R），表示元素AL，AL + 1，...，AR-1，AR。该子区间的xor-sum是AL xor AL + 1 xor ... xor AR-1 xor AR，其中xor是按位异或。

子区间的xor-even表示子区间xor-sum在其二进制表示中具有偶数个1。

史蒂文想对阵列进行Q修改。第i次修改将Pi-th（索引自0）元素更改为Vi。 Steven想知道，每次修改后，A的xor-even子区间的最大范围大小是多少？

输入
输入的第一行给出了测试用例的数量，T。T测试用例如下。

每个测试用例以包含两个整数N和Q的行开始，分别表示Steven数组中的元素数和修改数。

第二行包含N个整数。其中的第i个给出Ai表示Steven阵列中的第i个整数。

然后，Q行跟随，描述修改。第i行包含Pi和Vi，第i次修改将第Pi个元素更改为Vi。指示第i个修改将Pi-th（从0索引）元素改变为Vi。

产量
对于每个测试用例，输出一行包含Case #x：y_1 y_2 ... y_Q，其中x是测试用例编号（从1开始），y_i是A之后的最大xor-even子区间中的元素数。第i次修改。如果没有xor-even子区间，则输出0。

范围
时间限制：每个测试集40秒。
内存限制：1GB。
1≤T≤100。
0≤Ai<1024。
0≤Pi<N。
0≤Vi<1024。

测试装置1（可见）
1≤N≤100。
1≤Q≤100。

测试装置2（隐藏）
1≤N≤105。
1≤Q≤105。

样品

输入
 
产量
 
2
4 3
10 21 3 7
1 13
0 32
2 22
5 1
14 1 15 20 26
4 26

  
案例＃1：4 3 4
案例＃2：4
  
在样本案例1中，N = 4且Q = 3。
在第一次修改后，A是[10,13,3,7]。子区间（0,3）具有xor-sum 10 x或13 xor 3 xor 7 = 3.在二进制中，xor-sum为112，其偶数为1位，因此子区间为xor-even。这是最大的子区间，所以答案是4。
在第二次修改后，A是[32,13,3,7]。最大的xor-even子区间是（0,2），其xor-sum为32 xor 13 xor 3 = 46.在二进制中，这是1011102。
在第3次修改后，A是[32,13,22,7]。最大的xor-even子区间再次为（0,3），其xor-sum为32 xor 13 xor 22 xor 7 = 60.二进制中，这是1111002。
在样本情况2中，N = 5且Q = 1.在第一次修改之后，A是[14,1,15,20,26]。最大的xor-even子区间是（1,4），其xor和1 x或15 x或20 xor 26 = 0.在二进制中，这是02。
```

### 解法

#### 解法一 利用异或的性质 + 逐元素 xor 小数据集pass

```python
# 求二进制1的个数 O(c)
def _count1(n):
    c = 0
    while n:
        n &= (n - 1)
        c += 1
    return c


# 二进制数中的1的个数的奇偶性: 如果一个二进制数中的1的个数为奇数，那么返回1；如果为偶数，那么返回0。
# k+y-(x-k)=2*k+y-x
def get_numbers(A, Ch):

    # 二进制个数数组
    # Bi = []
    # for i, num in enumerate(A):
    #     # 偶数位为正 奇数位位负
    #     Bi.append(_count1(num) if i & 1 == 0 else -_count1(num))
    forward = [0]
    for i, num in enumerate(A):
        forward.append(forward[-1] + _count1(num) * (1 if i & 1 == 0 else -1))

    ans = []
    for i, (Pi, Vi) in enumerate(Ch):
        ori = forward[Pi + 1] - forward[Pi]
        diff = _count1(Vi) * (1 if Pi & 1 == 0 else -1) - ori
        for j in range(Pi + 1, len(forward)):
            forward[j] += diff
        # 计算第一个奇数 第一个偶数 最后一个奇数 最后一个偶数
        first_odd = -1
        first_even = 0
        last_odd = -1
        last_even = -1
        for j in range(len(forward)):
            if forward[j] & 1 == 1:
                first_odd = j
                break
            # if forward[j] & 1 == 0 and first_even == -1:
            #     first_even = j
            # if first_odd != -1 and first_even != -1:
            #     break
        for j in range(len(forward) - 1, -1, -1):
            if forward[j] & 1 == 1 and last_odd == -1:
                last_odd = j
            if forward[j] & 1 == 0 and last_even == -1:
                last_even = j
            if last_odd != -1 and last_even != -1:
                break
        print((first_even, first_odd, last_even, last_odd))
        temp = max(last_odd - first_odd, last_even - first_even)
        temp = temp if temp > 0 else 0
        ans.append(str(temp))

    return ans



T = int(input().strip())
for i in range(1, T + 1):
    N, Q = map(int, input().strip().split())
    A = list(map(int, input().strip().split()))
    Ch = []
    for _ in range(Q):
        fix = tuple(map(int, input().strip().split()))
        Ch.append(fix)

    print("Case #{}: ".format(i) + " ".join(get_numbers(A, Ch)))
```

#### 解法二 利用set的值有序性 + 奇数相减为偶数 

**set按值有序**

```c++
#include <cstdio>
#include <cstring>
#include <algorithm>
#include <iostream>
#include <set>
using namespace std;

const int maxn = 1e5 + 5;

int N, Q;
int a[maxn];
set<int> s;
int main(){
    int T;
    int cas = 0;
    cin >> T;
    while(T--){
        printf("Case #%d:", ++cas);
        cin >> N >> Q;
        s.clear();
        for(int i = 0;i < N;i++){
            scanf("%d", a + i);
            if(__builtin_popcount(a[i]) & 1){
                s.insert(i);
            }
        }    
        for(int i = 0;i < Q;i++){
            int p, v;
            scanf("%d%d", &p, &v);
            if(__builtin_popcount(v) & 1){
                s.insert(p);
            }else{
                s.erase(p);
            }
            if(s.size() & 1){
                int ans = min(*s.begin() + 1, N - *s.rbegin());
                printf(" %d", N - ans);
            }else{
                printf(" %d", N);
            } 
        } 
        puts("");
    } 
    return 0;
}

```



## Round G

###Book Reading

#### 题目描述

Supervin is a librarian handling an ancient book with **N** pages, numbered from 1 to **N**. Since the book is too old, unfortunately **M** pages are torn out: page number **P1**, **P2**, ..., **PM**.

Today, there are **Q** lazy readers who are interested in reading the ancient book. Since they are lazy, each reader will not necessarily read all the pages. Instead, the i-th reader will only read the pages that are numbered multiples of **Ri** and not torn out. Supervin would like to know the sum of the number of pages read by each reader.

### Input

The first line of the input gives the number of test cases, **T**. **T** test cases follow. Each test case begins with a line containing the three integers **N**, **M**, and **Q**, the number of pages in the book, the number of torn out pages in the book, and the number of readers, respectively. The second line contains **M** integers, the i-th of which is **Pi**. The third line contains **Q** integers, the i-th of which is **Ri**.

### Output

For each test case, output one line containing `Case #x: y`, where `x` is the test case number (starting from 1) and `y` is the total number of pages that will be read by all readers.

### Limits

Time limit: 40 seconds per test set.
Memory limit: 1GB.
1 ≤ **T** ≤ 100.
1 ≤ **P1** < **P2** < ... < **PM** ≤ **N**.
1 ≤ **Ri** ≤ **N**, for all i.

#### Test set 1 (Visible)

1 ≤ **M** ≤ **N** ≤ 1000.
1 ≤ **Q** ≤ 1000.

#### Test set 2 (Hidden)

1 ≤ **M** ≤ **N** ≤ 105.
1 ≤ **Q** ≤ 105.

### Sample

| Input                                                        | Output                                  |
| ------------------------------------------------------------ | --------------------------------------- |
| `3 11 1 2 8 2 3 11 11 11 1 2 3 4 5 6 7 8 9 10 11 1 2 3 4 5 6 7 8 9 10 11 1000 6 1 4 8 15 16 23 42 1   ` | `Case #1: 7 Case #2: 0 Case #3: 994   ` |

In sample case #1, the first reader will read the pages numbered 2, 4, 6, and 10. Note that the page numbered 8 will not be read since it is torn out. The second reader will read the pages numbered 3, 6, and 9. Therefore, the total number of pages that will be read by all readers is 4 + 3 = 7.

In sample case #2, all pages are torn out so all readers will read 0 pages.

In sample case #3, the first reader will read all the pages other than the six given pages.


#### 解法

##### 解法一
```python
T = int(input().strip())
for i in range(1, T + 1):
    N, M, Q = map(int, input().strip().split())
    torns = set(map(int, input().strip().split()))
    persons = map(int, input().strip().split())
    if len(torns) == N:
        print("Case #{}: {}".format(i, 0))
        continue

    res = 0
    # key: 通过大数据集的关键是加上记忆化 : C++直接过
    momo = dict()
    for p in persons:
        if p in momo:
            res += momo[p]
        else:
            temp = sum([1 if i * p not in torns else 0 for i in range(1, N // p + 1) ])
            momo[p] = temp
            res += momo[p]
    print("Case #{}: {}".format(i, res))
```

##### 解法二
```python

```



# KickStart 2018

![image-20190719150120726](/Users/zhengxinzhi/typora_img/KickStart2019总结/image-20190719150120726.png)



## Round A

### Even Digits 偶数

Determine the minimum number of button presses to make with no odd digits. a button press can increase
the number by 1 or decrease the number by 1

EXamples
42=>0(42 has no odd digits)
11=>3(11-3=8)
1=>1(1-1=00r1+1=2)
2018=>2(2018+2=2020

Limits
o Small dataset:1≤N≤105
o Large dataset:1sN$106

## Round C 74

### Planet Distance 

#### 问题描述

```
There are n planets and N tubes through which you can travel from one planet toanother. The tubes are bidirectional, and no two tubes connect the same pair ofplanets. You can travel from any planet to any other planet using tubes. There exists exactly one cycle in the universe
Your task is to find the minimum distance(in terms of the number of vacuumtubes)between each planet and a planet that is part of the cycle. Planets that are part of the cycle are assumed to be at distance 0

有n个行星和N个管道，您可以通过它们从一个行星到另一个行星。这些管道是双向的，没有两个管道连接同一行星。您可以使用管道从任何行星到任何其他行星。 宇宙中存在一个循环
您的任务是找到每个行星与作为环的行星之间的最小距离（就真空管的数量而言）。作为环一部分的行星被假定为距离0
```

#### 解法

1.无向图拓扑排序找到这个环

2.从这个环出发BFS找到距离

```python
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

# O(n + e)
def bfs(g, cycle_set, ans):
    que = queue.Queue()
    for v in cycle_set:
        que.put((v, 0))

    while not que.empty():
        v, dist = que.get()
        for adj_v in g.adj_list[v]:
            if ans[adj_v] != 0 or adj_v in cycle_set:
                continue
            ans[adj_v] = dist + 1
            que.put((adj_v, ans[adj_v]))



def solve(N, paths):
    # 1.构建图和度字典 以1开始

    g = Graph(N)
    for s, t in paths:
        g.addEdge(s - 1, t - 1)

    # 2.拓扑排序 + bfs
    ans = [0] * N
    if not g.topological_sort():
        cycle_set = g.not_visited
        bfs(g, cycle_set, ans)
    return ans


path = 'A-large-practice.in'
fw = open('result_a_samll.out', 'w')
with open(path, 'r') as f:
    T = int(f.readline().strip())
    for i in range(1, T + 1):
        N = int(f.readline().strip())
        paths = []
        for _ in range(N):
            paths.append(map(int, f.readline().strip().split()))
        ans = solve(N, paths)
        print("Case #{}: {}".format(i, ans))
fw.close()
```

```c++
#include <iostream>
#include <vector>
#include <queue>

#define MAX_N 1005

using namespace std;

int main() {
    int T;
    cin >> T;
    for (int tc = 1; tc <= T; tc++){
        int N;
        cin >> N;
        // 1.构建图（邻接表）
        vector<int> G[MAX_N];
        for (int i = 0; i < N; i++){
            int x, y;
            cin >> x >> y;
            G[x].push_back(y);
            G[y].push_back(x);
        }

        // 2.计算每个结点的度
        queue<int> q;
        vector<int> degree(N + 1);
        for(int i = 1; i <= N; i++){
            degree[i] = G[i].size();
            if (degree[i] == 1){
                q.push(i);
            }
        }

        // 3.拓扑排序
        vector<int> dis(N + 1);
        while (! q.empty()){
            int v = q.front();
            q.pop();
            dis[v] = -1;
            for(int i = 0; i < G[v].size(); i++){
                int adj_v = G[v][i];
                degree[adj_v]--;
                if (degree[adj_v] == 1){
                    q.push(adj_v);
                }
            }
        }

        // 4.把环中结点放入队列
        for (int i = 1; i <= N; i++){
            if (dis[i] == 0){
                q.push(i);
            }
        }

        // 5.BFS
        while (! q.empty()) {
            int v = q.front();
            q.pop();
            for (int i = 0; i < G[v].size(); i++) {
                int adj_v = G[v][i];
                if (dis[adj_v] == -1) {
                    dis[adj_v] = dis[v] + 1;
                    q.push(adj_v);
                }
            }
        }
        cout << "Case #" << tc << ": ";
        for (int i = 1; i <= N; i++){
            cout << dis[i] << " ";
        }
        cout << endl;
    }
    return 0;
}
```



### Kickstart Alarm

### 问题描述

```
Use a Parameter Array A,, A2,..., An to compute an Alarm Array P,, P2,.,PK
P: is just the summation of the i-th exponential-power of all the contiguous
subarrays of the Parameter Array. The i-th exponential-power of subarray Aj, Aj+1
…, A_h is defined as 
Our task is to calculate(P, +P,+.+Pk) modulo 1000000007
```

![image-20190731153828980](/Users/zhengxinzhi/typora_img/KickStart2019总结/image-20190731153828980.png)

![image-20190731153850970](/Users/zhengxinzhi/typora_img/KickStart2019总结/image-20190731153850970.png)

![image-20190731154052443](/Users/zhengxinzhi/typora_img/KickStart2019总结/image-20190731154052443.png)

### 解法

![image-20190731154142728](/Users/zhengxinzhi/typora_img/KickStart2019总结/image-20190731154142728.png)

![image-20190731154205050](/Users/zhengxinzhi/typora_img/KickStart2019总结/image-20190731154205050.png)

![image-20190731154247371](/Users/zhengxinzhi/typora_img/KickStart2019总结/image-20190731154247371.png)

**快速幂** + **费马小定理**

![fa image-20190731154318098](/Users/zhengxinzhi/typora_img/KickStart2019总结/image-20190731154318098.png)

![image-20190731154331648](/Users/zhengxinzhi/typora_img/KickStart2019总结/image-20190731154331648.png)

![image-20190731154433685](/Users/zhengxinzhi/typora_img/KickStart2019总结/image-20190731154433685.png)

![image-20190731154701459](/Users/zhengxinzhi/typora_img/KickStart2019总结/image-20190731154701459.png)

```c++
#include <iostream>
#include <math.h>

#define MAX_N 1000000

using namespace std;
const long long MOD = 1000000007;

long long fast_pow(long long x, long long n, long long MOD){
    // 计算 x ** n  O(log n)
    long long ans = 1;
    x = x % MOD;
    while (n > 0){
        if (n & 1){
            ans = ans * x % MOD;
        }
        n = n / 2;
        x = x * x % MOD;
    }
    return ans;
}

int main(){
    long long A[MAX_N + 2];
    int tcase;
    cin >> tcase;
    for (int tc = 1; tc <= tcase; tc++){
        long long N, K, x, y, C, D, E1, E2, F;
        cin >> N >> K >> x >> y >> C >> D >> E1 >> E2 >> F;
        A[1] = (x + y) % F;

        for (int i = 2; i <= N; i++){
            int x_t = x, y_t = y;
            x = (C * x_t + D * y_t + E1) % F;
            y = (D * x_t + C * y_t + E2) % F;
            A[i] = (x + y) % F;
        }
        long long result = 0;
        long long last_sum = 0;
        for(int i = 1; i <= N; i++){
            if (i == 1){
                last_sum += K;
            } else{
                // 等比数列 快速幂 + 费马小定理
                last_sum += i * (fast_pow(i, K, MOD) - 1) % MOD * fast_pow(i - 1, MOD - 2, MOD) % MOD;
            }
            last_sum %= MOD;
            long long temp = last_sum * (N - i + 1) % MOD;
            result += temp * A[i] % MOD;
            result %= MOD;
        }
        cout << "Case #" << tc << ": " << result << endl;
    }
}
```

## Round D 48

### Candies

#### 题目描述

![image-20190731164630948](/Users/zhengxinzhi/typora_img/KickStart2019总结/image-20190731164630948.png)

![image-20190731172808795](/Users/zhengxinzhi/typora_img/KickStart2019总结/image-20190731172808795.png)



