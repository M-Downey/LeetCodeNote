### [剑指 Offer 58 - II. 左旋转字符串](https://leetcode.cn/problems/zuo-xuan-zhuan-zi-fu-chuan-lcof/)

- 简单

字符串的左旋转操作是把字符串前面的若干个字符转移到字符串的尾部。请定义一个函数实现字符串左旋转操作的功能。比如，输入字符串`"abcdefg"`和数字`2`，该函数将返回左旋转两位得到的结果`"cdefgab"`。



**解法一：简单思路，直接赋值**

缺点：需要开辟额外空间

从第`k`个字符开始遍历字符串，往结果`res`里添加就可以了

```python
class Solution:
    def reverseLeftWords(self, s: str, n: int) -> str:
        res = []
        length = len(s)
        for i in range(length):
            res.append(s[(i+n) % length])
        res = ''.join(res)
        return res
```

**解法二：双指针**

利用双指针写一个`reverse`函数，用于翻转列表，然后将整个字符串翻转，以及用`k`分割的右，左两部分翻转，即可得到答案

```python
class Solution:
    def reverseLeftWords(self, s: str, n: int) -> str:
        # 翻转s中s[start: end+1]的部分，并返回这个子串
        def reverse(s, start, end):
            left, right = start, end
            while left < right:
                s[left], s[right] = s[right], s[left]
                left += 1
                right -= 1
            return s[start: end+1]
        s = list(s)
        
        length = len(s)
        s = reverse(s, 0, length-1)
        s[0:length-n] = reverse(s, 0, length-n-1)
        s[length-n:] = reverse(s, length-n, length-1)
        return ''.join(s)
```

