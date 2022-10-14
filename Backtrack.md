# Backtrack
回溯算法和我们常说的 DFS 算法非常类似，本质上就是一种**暴力穷举**算法。回溯算法和 DFS 算法的细微差别是：回溯算法是在遍历「树枝」，DFS 算法是在遍历「节点」  

解决一个回溯问题，实际上就是一个决策树的遍历过程，站在回溯树的一个节点上，你只需要思考 3 个问题：  

1、**路径**：也就是已经做出的选择。  

2、**选择列表**：也就是你当前可以做的选择。  

3、**结束条件**：也就是到达决策树底层，无法再做选择的条件。  

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
## 排列-组合-子集
### 形式一、元素无重不可复选，即 nums 中的元素都是唯一的，每个元素最多只能被使用一次。
```Java
/* 组合/子集问题回溯算法框架 */
void backtrack(int[] nums, int start) {
    // 回溯算法标准框架
    for (int i = start; i < nums.length; i++) {
        // 做选择
        track.addLast(nums[i]);
        // 注意参数
        backtrack(nums, i + 1);
        // 撤销选择
        track.removeLast();
    }
}

/* 排列问题回溯算法框架 */
void backtrack(int[] nums) {
    for (int i = 0; i < nums.length; i++) {
        // 剪枝逻辑
        if (used[i]) {
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

### 形式二、元素可重不可复选，即 nums 中的元素可以存在重复，每个元素最多只能被使用一次，其关键在于排序和剪枝。

```Java
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


### 形式三、元素无重可复选，即 nums 中的元素都是唯一的，每个元素可以被使用若干次，只要删掉去重逻辑即可。
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
