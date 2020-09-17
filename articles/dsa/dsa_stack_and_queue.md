# DSA - 栈和队列

数组是 random access 的数据结构，读取数组数据的时间复杂度是 $O(1)$，就好像翻开一页书，想翻开哪一页就翻开哪一页，没有“打开第 250 页必须先打开第 249 页”这种限制。

链表是 sequential access 数据结构，数组读取操作是线性的，就好像一个竹简，你想要知道竹简最后写了什么，必须先卷开它前面的部分。

而栈和队列都属于 sequentail access 数据结构下的一个分支 limited access 数据结构。

## 栈

### 概念

简单复习一下，栈是一种后进先出(LIFO)的数据结构。它是一种受限的数据结构，也就是说，元素只能往栈顶添加，也只能从栈顶移除。就好像一摞盘子，只能往顶部加盘子，也只能从顶部拿走盘子，如果想拿最底下的盘子，就得先把它上面的盘子都移走。

### 应用

-   反转字符串
-   实现“撤销”操作功能时，可以把所有操作都存在栈中
-   在编译中，将 infix 表达式转换成 prefix 或者 postfix 形式再进行计算。[more on this topic.](https://www.tutorialride.com/data-structures/applications-of-stack-in-data-structure.htm) 语法检查的过程中也用到栈来检查括号的有效性。
-   在代码执行过程中，参数和变量都是存放在调用栈中的。能够使用递归也是因为调用栈使用了栈这种数据结构。
-   在浏览器中，“后退”按钮的实现也是因为历史记录被存在栈中。
-   回溯算法中，比如迷宫问题，我们是通过尝试不同路径来寻找问题的解，当遇上 deadend 时，我们就要回溯，回到上一步的位置，这其实有点像“撤销”操作。
-   DFS
-   内存管理

### 实现

栈是一种 adapter class，意思是栈是基于其他数据结构实现的，它内部使用的数据结构可以是数组，也可以是链表，等等。虽然底层使用的数据结构不固定，但栈的实现必须提供统一的 API：

-   push()
-   pop()
-   peek()
-   isEmpty()

关于栈的实现还有一个要求，就是各个 API 操作都必须是 $O(1)$ 时间复杂度的。

## 队列

### 概念

队列是一种先进先出(FIFO)的数据结构，可以想一下饭堂排队打饭的同学们，先来的同学可以先打饭离开，后来的同学只能排在队伍后面等着。

### 实现

队列也是一种 adapter class，队列的 API：

-   enqueue()
-   dequeue()
-   isEmpty()
-   getFront()
-   clear()

各操作的时间复杂度都必须是 $O(1)$。

### 应用

-   BFS
-   CPU 调度、Disk 调度
-   两个进程之间进行异步传输数据时，队列 is used for synchronization。eg: IO Buffers, pipes, file IO...
-   handling of iterrupts in real-time systems
-   call center phone systems uses queues to hold people calling them in an order

### 分类

1. 简单队列
2. 循环队列：最后一个元素会执行第一个元素。循环队列相对普通队列的优势是它更好地利用了内存，如果队列尾部空间满了而队列头部还有空间的话，使用循环队列我们可以充分利用队头的空间。
3. 优先队列：每个元素都有一个优先级，元素出列顺序取决于它们的优先级。
4. Double Ended Queue or Deque：入列和出列操作可以发生在队头也可以在队尾，这种数据结构就不遵循 FIFO 原则了。

### 循环队列

在一个用数组实现的普通队列中，一旦一个队列满了，我们就不能插入下一个元素，即使在队列前面仍有空间。但是使用循环队列，我们就能使用这些空间去储存新的值。

**循环队列的实现**

-   用两个指针，一个表示队头 `front`，一个表示队尾 `rear`。
-   初始化时，把两个指针都设为 -1。
-   入列时，先检查队列是否满了；接着对 `rear` 指针进行**循环递增**操作 `rear = (rear + 1) % k`, k 是队列元素总数，再把新元素放到 `rear` 的位置。
-   出列时，先检查队列是否空了；接着将 `front` 指针的元素出列，再对 `front` 指针进行循环递增。
-   入列第一个元素时还需要将 `front` 指针更新为 0。
-   出列最后一个元素时需要将 `front` 和 `rear` 指针都重置为 -1。
-   判断队列是否满了有两种情况：
    -   `front === 0 && rear === size - 1`
    -   `front === rear + 1`
-   判断队列是否为空：`front === -1 && rear === -1`

相关题目：[622. 设计循环队列](https://leetcode-cn.com/problems/design-circular-queue/)

**循环队列的应用**

-   CPU 调度
-   内存管理
-   交通管理