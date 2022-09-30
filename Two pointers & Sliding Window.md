## Two pointers
#### 1. 快慢指针
> 数组问题中比较常见的快慢指针技巧，是原地修改数组。
> 链表问题常用快慢指针，比如判断链表成环。

#### 2. 左右指针
> 一头一尾从两端向中间锁进。

#### 3. 中间指针
> 从中心向两端扩散的双指针技巧  
 
[5. 最长回文字符串](https://leetcode.cn/problems/longest-palindromic-substring/)

> 找回文串的难点在于，回文串的的长度可能是奇数也可能是偶数，解决该问题的核心是从中心向两端扩散的双指针技巧。  
> 如果回文串的长度为奇数，则它有一个中心字符；如果回文串的长度为偶数，则可以认为它有两个中心字符。
 ```java
 class Solution {
    public String longestPalindrome(String s) {
        String res = " ";
        for (int i = 0; i < s.length(); i++) {
            String s1 = palindrome(s, i, i);
            // 以 s[i] 和 s[i+1] 为中心的最长回文子串
            String s2 = palindrome(s, i, i + 1);
            // res = longest(res, s1, s2)
            res = res.length() > s1.length() ? res : s1;
            res = res.length() > s2.length() ? res : s2;
        }
        return res;
    }

    String palindrome(String s, int l, int r) {
        while (l >= 0 && r < s.length() && s.charAt(l) == s.charAt(r)) {
            l--;
            r++;
        }
        return s.substring(l + 1, r);
    }
  }
 ```
 
## Sliding Window
**「相关问题」**   
[3. 无重复字符的最长子串](https://leetcode.cn/problems/longest-substring-without-repeating-characters/)  
[76. 最小覆盖子串](https://leetcode.cn/problems/minimum-window-substring/)  
[438. 找到字符串中所有字母异位词](https://leetcode.cn/problems/find-all-anagrams-in-a-string/)  
[567. 字符串的排列](https://leetcode.cn/problems/permutation-in-string/)


#### Template
```java
void slidingWindow(string s) {
    Map<Character, Integer> window = new HashMap<>();
    
    int left = 0, right = 0;
    while (right < s.size()) {
        // c 是将移入窗口的字符
        char c = s[right];
        // 增大窗口
        right++;
        // 进行窗口内数据的一系列更新
        ...

        /*** debug 输出的位置 ***/
        printf("window: [%d, %d)\n", left, right);
        /********************/
        
        // 判断左侧窗口是否要收缩
        while (window needs shrink) {
            // d 是将移出窗口的字符
            char d = s[left];
            // 缩小窗口
            left++;
            // 进行窗口内数据的一系列更新
            ...
        }
    }
}
```

**时间复杂度：O(n)** n 是输入字符串/数组的长度。
> 字符串/数组中的每个元素都只会进入窗口一次，然后被移出窗口一次，不会说有某些元素多次进入和离开窗口，  
> 所以算法的时间复杂度就和字符串/数组的长度成正比。

现在开始套模板，只需要思考以下几个问题：

 1、什么时候应该移动 right 扩大窗口？窗口加入字符时，应该更新哪些数据？

 2、什么时候窗口应该暂停扩大，开始移动 left 缩小窗口？从窗口移出字符时，应该更新哪些数据？

 3、我们要的结果应该在扩大窗口时还是缩小窗口时进行更新？
