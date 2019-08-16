---
title: leetcode-04z字形变换
date: 2019-08-16 09:57:34
tags:
---
官方解法： 
  1. 按行排序
  可以使用当前行和当前方向这两个变量对合适的行进行跟踪
  2. 按行访问
首先访问 行 0 中的所有字符，接着访问 行 1，然后 行 2...


  ```js
  /*
   * 整体的思路是遍历字符串，遍历过程中将每行都看成新的字符串构成字符串数组，最后再将该数组拼接起来即可
    如果 numRows=1numRows=1 则说明当前字符串即为结果，直接返回
    否则整个字符串需要经历，向下向右，向下向右，这样的反复循环过程，设定 downdown 变量表示是否向下，locloc 变量表示当前字符串数组的下标
    如果 downdown 为 truetrue，则 loc+=1loc+=1，字符串数组下标向后移动，将当前字符加入当前字符串中
    如果 downdown 为 falsefalse，则表示向右，则 loc-=1loc−=1，字符串数组下标向前移动，将当前字符加入当前字符串中
  */
  // 按行排序
  var convert = function(s, numRows) {
      if(numRows == 1)
          return s;

      const len = Math.min(s.length, numRows);
      const rows = [];
      for(let i = 0; i< len; i++) rows[i] = "";
      let loc = 0;
      let down = false;

      for(const c of s) {
          rows[loc] += c;
          if(loc == 0 || loc == numRows - 1)
              down = !down;
          loc += down ? 1 : -1;
      }
      // rows  ["ld", "eoe", "eco", "to"]
      let ans = "";
      for(const row of rows) {
          ans += row;
      }
      return ans;
  };
  ```
  ```js
  // 按行排序
  var convert = function(s, numRows) {
    // 通过一个数组来存储numRows个字符串
    let arr = [];
    // 记录当前下标
    let count = 0;
    // 当前应该是下标递增或是递减
    let add = true;
    // 如果numRows为1，直接返回字符串s
    if (numRows === 1) {
        return s;
    }
    // 遍历给定字符串
    for (let i = 0; i < s.length; i++) {
        // 需要判断arr[count]是否存在，防止出现undefined + 'a'这类情况
        arr[count] = arr[count] ? arr[count] + s[i] : s[i];
        // 当count计数到0时，接下来需要递增
        // 当count计数到numRows - 1时，接下来需要递减
        if (count === 0) {
            add = true;
        } else if (count === numRows - 1) {
            add = false;
        }
        // 根据递增递减情况，对下标count进行对应操作
        count = add ? count + 1 : count - 1;
    }
    
    // 拼接多个元素并返回结果
    return arr.join('');
};
  ```

  ```js
  // 按行访问 - 根据规律
  class Solution {
  public:
      string convert(string s, int numRows) {

          if (numRows == 1) return s;

          string ret;
          int n = s.size();
          int cycleLen = 2 * numRows - 2;

          for (int i = 0; i < numRows; i++) {
              for (int j = 0; j + i < n; j += cycleLen) {
                  ret += s[j + i];
                  if (i != 0 && i != numRows - 1 && j + cycleLen - i < n)
                      ret += s[j + cycleLen - i];
              }
          }
          return ret;
      }
  };
  ```