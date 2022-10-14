# Backtrack

回溯法，一般可以解决如下几种问题：

**组合问题**：N个数里面按一定规则找出k个数的集合   
**切割问题**：一个字符串按一定规则有几种切割方式  
**子集问题**：一个N个数的集合里有多少符合条件的子集  
**排列问题**：N个数按一定规则全排列，有几种排列方式  
**棋盘问题**：N皇后，解数独等等  


[回溯算法解题套路](https://labuladong.github.io/algo/1/8/)
```Java
result = []
def backtrack(path, selection list):
    //base case
    if (Meet the end condition) {
        result.add(path);
        return;
    }

    for (selection in selection list) {
        //make a selection
        Remove this selection from the selection list
        path.add(selection);
        backtrack(selection, selection list);
        //withdraw selection
        path.remove(selection);
        Add the selection back to the selection list
    }
  ```
[回溯秒杀排列-组合-子集](https://labuladong.github.io/algo/1/9/)
## Combination/Subset/Permutation
### Form I. Elements are non-reselectable, i.e. the elements in nums are unique and each element can only be used at most once
```Java
/* Combination/Subset */
void backtrack(int[] nums, int start) {
    // backtrack
    for (int i = start; i < nums.length; i++) {
        // make a selection
        track.addLast(nums[i]);
        // Note that the parameter i + 1 ensures that each element will only be used once
        backtrack(nums, i + 1);
        // withdraw selection
        track.removeLast();
    }
}

/* Permutation */
void backtrack(int[] nums) {
    for (int i = 0; i < nums.length; i++) {
        // Pruning
        if (used[i]) {
            continue;
        }
        // make a selection
        used[i] = true;
        track.addLast(nums[i]);

        backtrack(nums);
        // withdraw selection
        track.removeLast();
        used[i] = false;
    }
}
```

### Form II. Elements can be re-elected and not re-selected, i.e. elements in nums can be duplicated and each element can only be used at most once, the key to this is sorting and pruning

```Java
//数组排序 方便剪枝去重
Arrays.sort(nums);
/* 组合/子集问题回溯算法框架 */
void backtrack(int[] nums, int start) {
    // 回溯算法标准框架
    for (int i = start; i < nums.length; i++) {
        // 剪枝逻辑，跳过值相同的相邻树枝
        if (i > start && nums[i] == nums[i - 1]) {
            continue;
        }
        // 做选择
        track.addLast(nums[i]);
        // 注意参数
        backtrack(nums, i + 1);
        // 撤销选择
        track.removeLast();
    }
}

Arrays.sort(nums);
/* 排列问题回溯算法框架 */
void backtrack(int[] nums) {
    for (int i = 0; i < nums.length; i++) {
        // 剪枝逻辑
        if (used[i]) {
            continue;
        }
        // 剪枝逻辑，固定相同的元素在排列中的相对位置
        // 对于[2, 2'] 对于2'来说，只有2用过(used[i - 1])，才可以选择
        if (i > 0 && nums[i] == nums[i - 1] && !used[i - 1]) {
            continue;
        }
        // 做选择
        used[i] = true;
        track.addLast(nums[i]);
        backtrack(nums);
        // 撤销选择
        track.removeLast();
        used[i] = false;
    }
}
```


### Form 3, elements are non-reselectable, i.e. the elements in nums are unique and each element can be used several times
```Java
/* 组合/子集问题回溯算法框架 */
void backtrack(int[] nums, int start) {
    // 回溯算法标准框架
    for (int i = start; i < nums.length; i++) {
        // 做选择
        track.addLast(nums[i]);
        // 注意参数
        backtrack(nums, i);
        // 撤销选择
        track.removeLast();
    }
}

/* 排列问题回溯算法框架 */
void backtrack(int[] nums) {
    for (int i = 0; i < nums.length; i++) {
        // 做选择
        track.addLast(nums[i]);
        backtrack(nums);
        // 撤销选择
        track.removeLast();
    }
}
```
