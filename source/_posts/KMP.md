---
title: KMP
categories:
- 数据结构
- 字符串
tags:
- 数据结构
---

<escape><!--more--></escape>

### Preface

> 给定一个模式串 S，以及一个模板串 P，所有字符串中只包含大小写英文字母以及阿拉伯数字。
>
> 模板串 P 在模式串 S 中多次作为子串出现，求出模板串 P 在模式串 S 中所有出现的位置的起始下标。

字符串匹配算法，比较容易想到的是模拟匹配，一般会出现 `TLE`，因此不可取。

非常出名的 `KMP` 算法，一直认为比较难，经过学习，其实发现并不是特别难，只不过是纸老虎。

在学习 `KMP` 之前，也需要回顾是如何暴力匹配的，才能明白为何 `KMP` 缩短了时间复杂度。

<strong>当然参考一些变量的定义，将 `s` 重名为 `haystack`，`p` 重名为 `needle`，也就是大海捞针。</strong>

类似题目可以参考：https://leetcode-cn.com/problems/implement-strstr/submissions/

### Details

#### 暴力匹配

遍历 `haystack`，匹配 `needle`，如果当前位置不满足，就移动至下一位，直至匹配成功。

![](暴力.png)

不难看出时间复杂度很高，那么造成其原因就在于，很多匹配其实是无效的，并且重复匹配。

举个例子：`abcbdefbcbdc` 和 `bcbd`，在暴力模拟匹配的过程中，可以看到以下两个问题：

1. 其实很多位置最开始都不满足，但还是遍历了；
2. 每次遍历匹配的过程中，都对 `bcbd` 重头匹配。

而 `KMP` 算法解决了这两个问题，当然这是我个人的理解，不得不说 `KMP` 真的精妙。

#### KMP

<strong>原论文地址：[Knuth77.pdf (jhu.edu)](https://www.cs.jhu.edu/~misha/ReadingSeminar/Papers/Knuth77.pdf)</strong>

`KMP` 的核心就在于其构造了一个 `next` 数组，其作用是返回每个位置的最长对应前缀。

个人觉得先理解这个数组，其实便于更好地去理解这个算法，如果先去理解算法，容易晕，

就好比学习树状数组的时候，先学习 `lowbit` 函数，对于后续整个处理都有益处。

举个例子说明 `next` 数组，如下图所示：

![](next.png)

接下来介绍 `KMP` 是如何进行匹配的，还是回到 `needle` 和 `haystack` 中，

与暴力匹配不同的是，当在 `haystack` 中发现此时不匹配时，不是重新开始，

因为从前面已经知道，在匹配失败之前都是匹配成功的，不妨设最后成功的位置为 `j`，

那么下次就从 `next[j]` 开始进行匹配，重复此过程，直至相等。

<strong><em>（这里就不画图模拟了，网上有很多，主要是记录自己的理解）</em></strong>

这一步就避免了暴力所带来的时间开销，从而优化了时间复杂度。

那么如何构造 `next` 数组呢？其实和匹配过程是非常相似的，唯一不同的就在于最后的判断，

匹配过程中是寻找完全相等的情况，而构造 `next` 只是寻找前缀的过程，需要理解。

### Code

```java
private void kmp(String s, String p) throws IOException {
    BufferedWriter bw = new BufferedWriter(new OutputStreamWriter(System.out));
    
    // 字符串转为数组便于处理
    char[] needle = s.toCharArray();
    char[] haystack = p.toCharArray();
    int n = needle.length;
    int m = haystack.length;
    
    // 避免增加数组的开销 从 -1 开始
    int[] next = new int[n];
    next[0] = -1;
    
    // 构建 next 数组
    for (int i = 1, j = -1; i < n; i++) {
        while (j >= 0 && needle[i] != needle[j + 1]) {
            j = next[j];
        }
        if (needle[i] == needle[j + 1]) {
            j++;
        }
        next[i] = j;
    }
    
    // kmp 查询
    for (int i = 0, j = -1; i < m; i++) {
        while (j >= 0 && haystack[i] != needle[j + 1]) {
            j = next[j];
        }
        if (haystack[i] == needle[j + 1]) {
            j++;
        }
        // 如果匹配成功
        if (j == n - 1) {
            bw.write(i - n + 1 + " ");
            j = next[j];
        }
    }
    
    bw.flush();
    bw.close();
}
```

