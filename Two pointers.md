## Two pointers
#### 1. 快慢指针
> 数组问题中比较常见的快慢指针技巧，是原地修改数组。

#### 2. 左右指针
> 一头一尾从两端向中间锁进。

#### 3. 中间指针
> 从中心向两端扩散的双指针技巧  
 
[Leetcode 5. 最长回文字符串](https://leetcode.cn/problems/longest-palindromic-substring/)

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
