## nSum
[一个函数秒杀nSum](https://mp.weixin.qq.com/s/fSyJVvggxHq28a0SdmZm6Q)     
相关题目: [Leetcode. 15 三数之和](https://leetcode.cn/problems/3sum/) &nbsp; | &nbsp; [Leetcode. 18 四数之和](https://leetcode.cn/problems/4sum/)

#### 三数之和
> 现在我们想找和为 target 的三个数字，那么对于第一个数字，可能是什么？nums 中的每一个元素 nums[i] 都有可能！
> 那么，确定了第一个数字之后，剩下的两个数字可以是什么呢？其实就是和为 target - nums[i] 的两个数字，就是 twoSum 函数解决的问题。

#### 四数之和
> 4Sum 完全就可以用相同的思路：穷举第一个数字，然后调用 3Sum 函数计算剩下三个数，最后组合出和为 target 的四元组。

#### nSum函数
> n == 2 时是 twoSum 的双指针解法，n > 2 时就是穷举第一个数字，然后递归调用计算 (n-1)Sum，组装答案。  
> 需要注意的是，调用这个 nSum 函数之前一定要先给 nums 数组排序，因为 nSum 是一个递归函数，如果在 nSum 函数里调用排序函数，那么每次递归都会进行没有必要的排序，效率会非常低。

```java
public List<List<Integer>> fourSum(int[] nums, int target) {
        Arrays.sort(nums);
        return nSum(nums, n, 0, target);
    }

private List<List<Integer>> nSum(int[] nums, int n, int start, int target) {
    int len = nums.length;
    List<List<Integer>> res = new ArrayList<List<Integer>>();
    if (n < 2 || len < n)
        return res;
    if (n == 2) {
        int small = start, big = len - 1;
        while (small < big) {
            int left = nums[small], right = nums[big];
            int sum = left + right;
            if (sum == target) {
                res.add(new ArrayList<Integer>(Arrays.asList(left, right)));
                while (small < big && nums[small] == left)
                    small++;
                while (small < big && nums[big] == right)
                    big--;
            } else if (sum > target) {
                while (small < big && nums[big] == right)
                    big--;
            } else if (sum < target) {
                while (small < big && nums[small] == left)
                    small++;
            }
        }
    } else {
        // n > 2 时，递归计算 (n-1)Sum 的结果
        int i = start;
        while (i < len) {
            int now = nums[i];
            List<List<Integer>> sub = nSum(nums, n - 1, i + 1, target - now);
            for (List<Integer> s : sub) {
                s.add(now);
                res.add(s);
            }
            while (i < len && nums[i] == now)
                i++;
        }
    }
    return res;
}
```
