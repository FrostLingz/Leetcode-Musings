## 二分查找
[二分查找的细节](https://leetcode.cn/problems/search-insert-position/solution/te-bie-hao-yong-de-er-fen-cha-fa-fa-mo-ban-python-/)
> 二分查找只有一个思想，那就是：**逐步缩小搜索区间**。  
> 需要分析清楚题目的意思，分析清楚要找的答案需要满足什么性质。应该清楚模板具体的用法，不可以盲目套用、死记硬背。

### 常见问题

#### 问题 1：while (left < right) or while (left <= right)?

> 看到 while (left < right) **不表示搜索区间为「左闭右开」**，这一点没有根据。真正应该看边界如何设置，这一点完全是人为定义。  
>
> 只看到 while (left < right) 里的 < ，不能说明右边界不能取到。真正看区间表示应该看左右边界到底如何设置，如果我们分析出下一轮搜索的范围是 [left..mid] 而设置 right = mid + 1，才表示搜索区间为「左闭右开」区间 。这是因为 [left..right) = [left..mid + 1) = [left..mid]。

#### 问题 2：二分查找是不是要求数组一定是有序的？

> 不一定 「二分」不是单纯指从有序数组中快速找某个数，这只是「二分」的一个应用。  
> 「二分」的本质是**两段性**，只要一段满足某个性质，另外一段不满足某个性质，就可以用「二分」。

典型案例:[力扣33](https://leetcode.cn/problems/search-in-rotated-sorted-array/) &nbsp;  解析:[力扣33解析](https://leetcode.cn/problems/search-in-rotated-sorted-array/solution/shua-chuan-lc-yan-ge-ologn100yi-qi-kan-q-xifo/)

#### 问题 3：为什么有一些二分查找取 mid 的时候，括号里要加 1?  
> 这是因为 int mid = (left + right) / 2 在区间里有偶数个元素的时候，mid 只能取到位于左边的中间数，要想取到位于右边的中间数，就需要在括号里加 1。  

> 为什么要取右边中间数呢？这是因为在区间里 只有 2 个元素的时候，把 [left..right] 划分成 [left..mid - 1] 和 [mid..right] 这两个区间，int mid = (left + right) / 2 这种取法不能把搜索区间缩小。

> 如果划分区间的逻辑是 l**eft = mid + 1**; 和 **right = mid;** 时，while(left < right) 退出循环以后 left == right 成立，此时 mid 中间数正常下取整就好；
如果划分区间的逻辑是 **left = mid**; 和**right = mid - 1**; 时，while(left < right) 退出循环以后 left == right 成立，此时为了避免死循环，**mid 中间数需要改成上取整(mid + 1)**。

典型案例:[力扣69](https://leetcode.cn/problems/sqrtx/) &nbsp;  解析:[力扣69解析](https://leetcode.cn/problems/sqrtx/solution/er-fen-cha-zhao-niu-dun-fa-python-dai-ma-by-liweiw/)

#### 问题 4：什么时候 right 取 len ？什么时候 right = len - 1？
> 取决于题目设置的搜索区间是什么
