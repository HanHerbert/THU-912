**个人笔记。**



# 第一章 绪论

## a.计算

**算法要素：**

- 输入与输出、
- 基本操作、确定性与可行性
- 有穷性与正确性
- 退化与鲁棒性
- 重用性

**好算法**：**正确**（处理简单的、大规模的、一般性的、退化的、合法的输入）、**健壮**、**可读**、**效率**（速度快、存储空间小）

**计算成本**T(n)：求解规模为n的问题所需基本操作数，在规模为n的所有实例中，只关注最坏（成本最高）者

## b.大O记号

T(n)定义为算法所执行基本操作的总次数。

关注T(n)的渐进上界。引入**大O记号**。

具体地，由T(n) < = c.f(n)，用f(n)代替T(n)，可忽略常系数和低次项。

```c++
//常数情况
2013×2013
//包含循环情况
for(i=0; i<n; i+= n/2013 + 1);
for(i=1; i<n; i=1 << i);
//包含分支
if((n+m)*(n+m) < 4*n*m) goto UNREACHABLE;
//包含递归
if(2==(n*n)% 5) 01(n);

```

**O(1)** ：常数

**O(logn)** 对数：此类算法非常有效，复杂度无限接近常数，可忽略常底数和常数次幂。

**O(n)** 线性及多项式：线性，从n到n2是编程题主要覆盖的范围。

**O(2^n)** 指数：无效，从多项式到指数被认为有效算法到无效算法的分水岭。指数复杂度算法无法真正应用于实际问题中，不是有效算法，甚至不能称之为算法。相应地，多项式复杂度的算法也被称作南街的(intractable)问题。

## c.常见算法分析

**两个任务**：**正确性**（不变性、单调性）+ **复杂度**
**复杂度分析**：猜测+验证，迭代（级数求和），递归（递归跟踪+递推方程）
**算数级数**：与末项平方同阶
**幂方级数**：比幂次高出一阶
**几何级数**(a>1)：与末项同阶
**收敛级数**：O(1)
**调和级数**：1+1/2+1/3+…+1/n=O(logn)
**对数级数**：log1+log2+…+logn=O(nlogn)

以冒泡排序为例：

```c++
void bubblesort(int A[], int n){
	for (bool sorted = false; sorted = !sorted; n--)
		for (int i=1; i<n; i++)
			if (A[i-1] > A[i]){
				swap(A[i-1], A[i]);
				sorted = false;
			}
}

```

**不变性**：经k轮交换，最大的k个元素就位
**单调性**：经k轮交换，问题规模缩减至n-k
**正确性**：经至多n趟扫描后，算法必然终止且给出正确解答

## d.递归及迭代

**分而治之**：划分为两个子问题，规模大体相当。

**减而治之**：划分为两个子问题，其一平凡，另一缩减。

1. **Fibonacci**数：迭代

动态规划：按规模自小而大求解各子问题的过程。

```c++
int fibI(int n){
    f = 0; g = 1;
    while (0 < n--){
	    g = g + f;
	    f = g - f;
    }
    return g;
}
```

仅使用两个中间变量f和g，记录当前的一对相邻Fibonacci数。整个算法仅需线性步的迭代.

**时间复杂度为O(n)**,**空间复杂度为O(1)**

1. **最长公共子序列**（可能有多个；可能有歧义）

对于序列A[0, n]和B[0, m]，LSC有三种情况：
（1）n = -1或m = -1，取空序列（“”）

（2）A[n] = ‘X’ = B[m]，取LSC(A[0, n),B[0, m)) + ‘X’ （减而治之）

（3）A[n] ≠ B[m]，则在 LCS(A[0, n], B[0, m))与 LCS(A[0, n), B[0, m])中取更长者 （分而治之）

**最好情况：O(n + m)**

**最坏情况：O(2^n),此时 (n = m)**

与斐波那契数列一样，有大量重复的递归实例；若采用动态规划，只需O(nm)时间即可计算所有子问题，为此只需:

（1）将所有子问题假想列成一张表；

（2）颠倒计算方向，从LCS(A[0], B[0])出发依次计算所有项。

## e.习题集

习题1-12

```c++
(unsigned long) -1 //在不知道最大无符号整数大小的情况下，只需将-1转换为此类型，轻松获得此类型的最大值
```

# 第二章 向量

**向量（vector）**是线性数组的一种抽象与泛化。

从向量最基本的接口出发，设计并实现与之对应的向量模板类。

## a.向量ADT支持的操作接口

| 操作接口      | 功能                                               | 适用对象 |
| ------------- | -------------------------------------------------- | -------- |
| size()        | 报告向量当前的规模（元素总数）                     | 向量     |
| get(r)        | 获取秩为r的元素                                    | 向量     |
| put(r, e)     | 用e替换秩为r的元素                                 | 向量     |
| insert(r, e)  | e作为秩为r元素插入，后继元素依次右移               | 向量     |
| remove(r)     | 删除秩为r的元素，返回该元素中原存放的对象          | 向量     |
| disordered()  | 判断所有元素是否已按非降序排列                     | 向量     |
| sort()        | 调整各元素位置，使之按非降序排列                   | 向量     |
| find(e)       | 查找等于e且秩最大的元素                            | 向量     |
| search(e)     | 查找目标元素e，返回不大于e且秩最大的元素           | 有序向量 |
| deduplicate() | 剔除重复元素                                       | 向量     |
| uniquify()    | 剔除重复元素                                       | 有序向量 |
| traverse()    | 遍历向量并统一处理所有元素，处理方法由函数对象指定 | 向量     |

## b.Vector模板类

```c++
typedef int Rank; //秩
#define DEFAULT_CAPACITY  3 //默认的初始容量（实际应用中可设置为更大）

template <typename T> class Vector { //向量模板类
protected:
   Rank _size; int _capacity;  T* _elem; //规模、容量、数据区
   void copyFrom ( T const* A, Rank lo, Rank hi ); //复制数组区间A[lo, hi)
   void expand(); //空间不足时扩容
   void shrink(); //装填因子过小时压缩
   bool bubble ( Rank lo, Rank hi ); //扫描交换
   void bubbleSort ( Rank lo, Rank hi ); //起泡排序算法
   Rank max ( Rank lo, Rank hi ); //选取最大元素
   void selectionSort ( Rank lo, Rank hi ); //选择排序算法
   void merge ( Rank lo, Rank mi, Rank hi ); //归并算法
   void mergeSort ( Rank lo, Rank hi ); //归并排序算法
   void heapSort ( Rank lo, Rank hi ); //堆排序（稍后结合完全堆讲解）
   Rank partition ( Rank lo, Rank hi ); //轴点构造算法
   void quickSort ( Rank lo, Rank hi ); //快速排序算法
   void shellSort ( Rank lo, Rank hi ); //希尔排序算法
public:
// 构造函数
   Vector ( int c = DEFAULT_CAPACITY, int s = 0, T v = 0 ) //容量为c、规模为s、所有元素初始为v
   { _elem = new T[_capacity = c]; for ( _size = 0; _size < s; _elem[_size++] = v ); } //s<=c
   Vector ( T const* A, Rank n ) { copyFrom ( A, 0, n ); } //数组整体复制
   Vector ( T const* A, Rank lo, Rank hi ) { copyFrom ( A, lo, hi ); } //区间
   Vector ( Vector<T> const& V ) { copyFrom ( V._elem, 0, V._size ); } //向量整体复制
   Vector ( Vector<T> const& V, Rank lo, Rank hi ) { copyFrom ( V._elem, lo, hi ); } //区间
// 析构函数
   ~Vector() { delete [] _elem; } //释放内部空间
// 只读访问接口
   Rank size() const { return _size; } //规模
   bool empty() const { return !_size; } //判空
   Rank find ( T const& e ) const { return find ( e, 0, _size ); } //无序向量整体查找
   Rank find ( T const& e, Rank lo, Rank hi ) const; //无序向量区间查找
   Rank search ( T const& e ) const //有序向量整体查找
   { return ( 0 >= _size ) ? -1 : search ( e, 0, _size ); }
   Rank search ( T const& e, Rank lo, Rank hi ) const; //有序向量区间查找
// 可写访问接口
   T& operator[] ( Rank r ); //重载下标操作符，可以类似于数组形式引用各元素
   const T& operator[] ( Rank r ) const; //仅限于做右值的重载版本
   Vector<T> & operator= ( Vector<T> const& ); //重载赋值操作符，以便直接克隆向量
   T remove ( Rank r ); //删除秩为r的元素
   int remove ( Rank lo, Rank hi ); //删除秩在区间[lo, hi)之内的元素
   Rank insert ( Rank r, T const& e ); //插入元素
   Rank insert ( T const& e ) { return insert ( _size, e ); } //默认作为末元素插入
   void sort ( Rank lo, Rank hi ); //对[lo, hi)排序
   void sort() { sort ( 0, _size ); } //整体排序
   void unsort ( Rank lo, Rank hi ); //对[lo, hi)置乱
   void unsort() { unsort ( 0, _size ); } //整体置乱
   int deduplicate(); //无序去重
   int uniquify(); //有序去重
// 遍历
   void traverse ( void (* ) ( T& ) ); //遍历（使用函数指针，只读或局部性修改）
   template <typename VST> void traverse ( VST& ); //遍历（使用函数对象，可全局性修改）
}; //Vector
```

## c.构造和析构

**基于复制的构造方法**

```c++
template <typename T> //元素类型
void Vector<T>::copyFrom ( T const* A, Rank lo, Rank hi ) { //以数组区间A[lo, hi)为蓝本复制向量
   _elem = new T[_capacity = 2 * ( hi - lo ) ]; _size = 0; //分配空间，规模清零
   while ( lo < hi ) //A[lo, hi)内的元素逐一
      _elem[_size++] = A[lo++]; //复制至_elem[0, hi - lo)
}
```

需强调的是，由于向量内部含有动态分配的空间，默认的运算符“=”不足以支持向量之间的直接赋值。

故重载向量的赋值运算符。

```c++
template <typename T> Vector<T>::operator= (Vector<T> const& V){ //重载
	if (_elem) delete [] _elem; //释放原有内容
	copyFrom ( V._elem, 0, V.size() ); //整体复制
	return *this; //返回当前对象的引用，以便链式赋值
}
```

## c.动态空间管理

### 1.静态空间管理

内部数组所占物理空间的容量，若在向量的生命期内不允许调整，则为静态管理策略。

缺点：因容量固定，总有可能在此后的某一时刻，无法加入更多的新元素--即导致所谓的上溢（overflow）。

也有可能拥有少量元素的数组长时间占据大量物理空间，造成空间浪费。

### 2.可扩充向量

```c++
template <typename T> void Vector<T>::expand() { //向量空间不足时扩容
   if ( _size < _capacity ) return; //尚未满员时，不必扩容
   if ( _capacity < DEFAULT_CAPACITY ) _capacity = DEFAULT_CAPACITY; //不低于最小容量
   T* oldElem = _elem;  _elem = new T[_capacity <<= 1]; //容量加倍
   for ( int i = 0; i < _size; i++ )
      _elem[i] = oldElem[i]; //复制原向量内容（T为基本类型，或已重载赋值操作符'='）
   delete [] oldElem; //释放原空间
}
```

该扩容策略是**容量加倍策略**

**分析：**最坏情况：在初始容量为1的满向量中，连续插入n=2 ^ m>>2个元素，在第1、2、4、8、16次插入都需要扩容，每次扩容中复制原向量的时间成本分别为1,2,4,8，…,2^m = n，总体耗时O(n)，每次扩容的分摊成本为O(1)
**平均分析复杂度**：根据数据结构各种操作出现概率的分布，将对应的成本加权平均，各种可能的操作作为独立事件分别考查，割裂了操作之间的相关性和连贯性，不能准确评判数据结构和算法的真实性能；
**分摊复杂度**：对数据结构连续实施足够多次操作，所需总成本分摊至单次操作，对一系列操作做整体的考量。

另外，**追加固定数目的单元的策略**，无论采用的固定常数多大，在最坏的情况下，此类数组单次操作的分摊时间复杂度都将高达**Ω(n)**。

### 3.缩小容量

```c++
template <typename T> void Vector<T>::shrink() { //装填因子过小时压缩向量所占空间
   if ( _capacity < DEFAULT_CAPACITY << 1 ) return; //不致收缩到DEFAULT_CAPACITY以下
   if ( _size << 2 > _capacity ) return; //以25%为界
   T* oldElem = _elem;  _elem = new T[_capacity >>= 1]; //容量减半
   for ( int i = 0; i < _size; i++ ) _elem[i] = oldElem[i]; //复制原向量内容
   delete [] oldElem; //释放原空间
}
```

## d.常规向量

### 1.直接引用元素

```c++
template <typename T> 
T& Vector::operator[] ( Rank r ) const //重载下标操作符
{   return _elem[r]; } //assert: 0 <= r < _size
```

此后对外的V[r]对应内部V._elem[r]
**右值**：T x = V[r] + U[s] * W[t];
**左值**：V[r] = (T)(2*x + 3)