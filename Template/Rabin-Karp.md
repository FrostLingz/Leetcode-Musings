# Rabin-Karp

```Java
// 文本串
String txt;
// 模式串
String pat;


// 需要寻找的子串长度为模式串 pat 的长度
int L = pat.length();
// 仅处理 ASCII 码字符串，可以理解为 256 进制的数字
int R = 256;
// 存储 R^(L - 1) 的结果
int RL = (int) Math.pow(R, L - 1);
// 维护滑动窗口中字符串的哈希值
int windowHash = 0;
// 计算模式串的哈希值
long patHash = 0;
for (int i = 0; i < pat.length(); i++) {
    patHash = R * patHash + pat.charAt(i);
}

// 滑动窗口代码框架
int left = 0, right = 0;
while (right < txt.length()) {
    // 扩大窗口，移入字符（在最低位添加数字）
    windowHash = R * windowHash + txt[right];
    right++;

    // 当子串的长度达到要求时
    if (right - left == L) {
        // 根据哈希值判断窗口中的子串是否匹配模式串 pat
        if (patHash == windowHash) {
            // 找到模式串
            print("找到模式串，起始索引为", left);
            return left;
        }
        
        // 缩小窗口，移出字符（删除最高位的数字）
        windowHash = windowHash - txt[left] * RL;
        left++;
    }
}
// 没有找到模式串
return -1;
```
