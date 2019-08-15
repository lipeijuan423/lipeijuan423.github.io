---
title: leetcode-03最长回文子串
date: 2019-08-15 09:31:13
tags:
---
<!-- 
  * 回文： 正读和反读都相同的字符串
  * 动态规划
  * 字符串处理
 -->
 1. 最长公共子串 
 > 反转 SS，使之变成 S'S ′。找到 SS 和 S'S ′之间最长的公共子串，这也必然是最长的回文子串。都需要检查子串的索引是否与反向子串的原始索引相同
 > S=“abacdfgdcaba”, S' = {“abacdgfdcaba”}S ′=“abacdgfdcaba”：SS 以及 S'S ′之间的最长公共子串为{“abacd”}“abacd”
 2. 暴力法
 > 时间复杂度： O(n3)
 > 空间复杂度： O(1)
 列举所有回文串的可能，取得最长
 3.动态规划
 因为它的左首字母和右尾字母是相同的 
 4. 中心扩展
 时间复杂度： O(n2)
 空间复杂度： O(1)
 5. Manacher 算法
 ----- 未完成 -----
 ```js
 // 中心扩展
 var longestPalindrome = function (s) {
  if (s.length < 1) { return s; }
  var ss = s.split('');
  var start = 0;
  var end = 0;
  for (var i = 0; i < s.length; i++) {
    var len1 = expandCenter(ss, i, i);
    var len2 = expandCenter(ss, i, i + 1);
    var len = Math.max(len1, len2);
    if (len > end - start) {
      start = Math.ceil(i - (len - 1) / 2);
      end = Math.floor(i + len / 2);
    }
    console.log('len', len);
  }
  //因为slice是截止到end +1的前一位，就是会提取到end位置
  var result = ss.slice(start, end + 1).join('');
  return result;
}
var expandCenter = function (ss, L, R) {
  var left = L;
  var right = R;
  while (left >= 0 && right < ss.length && ss[left] === ss[right]) {
    left--;
    right++;
  }
  return right - left - 1;
}
 ```
