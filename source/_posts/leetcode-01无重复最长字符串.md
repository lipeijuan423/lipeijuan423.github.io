---
title: leetcode-无重复最长字符串
date: 2019-08-13 08:59:58
tags:
---
```js
  // 网络解答
  // 滑动窗口
/**
 * @param {string} s
 * @return {number}
 */
var lengthOfLongestSubstring = function(s) {
  let n = s.length;
  let hashMap = new Map();
  let ans = 0;
  for (let i = 0, j = 0; i < n; i++) {
    if (hashMap.has(s[i])) {
      j = Math.max(hashMap.get(s[i]), j)
    }
    ans = Math.max(ans, i - j + 1);
    hashMap.set(s[i], i + 1)
  }
  return ans;
}```