LeetCode专项训练

#### 字符串

常用API:

+ String : .
  + length()
  + charAt()
  + toCharArray()
  + String.valueOf() 整数转字符串
  + Integer.paresenint() 字符串转整数

1.[242. 有效的字母异位词](https://leetcode-cn.com/problems/valid-anagram/)

```java
class Solution {
    public boolean isAnagram(String s, String t) {
        if (s.length() != t.length()) {
            return false;
        }
        int[] array = new int[26];
        for (int i = 0; i < s.length(); i++) {
            char c = s.charAt(i);
            array[c - 'a']++;
        }
        for (int i = 0; i < t.length(); i++) {
            char c = t.charAt(i);
            array[c - 'a']--;
            if (array[c - 'a'] < 0) {
                return false;
            }
        }
        return true;
    }
}
```

#### [409. 最长回文串](https://leetcode-cn.com/problems/longest-palindrome/)

```
class Solution {
    public int longestPalindrome(String s) {
        int[] cnts = new int[128];
        //记录每个大小写字母的次数
        for (char c : s.toCharArray()) {
            cnts[c]++;
        }
        int palindrome = 0;
        //记录出现偶数次字母的次数
        for(int i : cnts) {
            palindrome +=  (i / 2)* 2;

        }
        //判断是否有单独的字符出现
        if (palindrome < s.length()) {
            palindrome++;
        }
        return palindrome;
    }
}
```

#### [647. 回文子串](https://leetcode-cn.com/problems/palindromic-substrings/)

```
class Solution {
    int cnt = 0;
    public int countSubstrings(String s) {
       for (int i = 0; i < s.length(); i++) {
          //奇数长度的字符串
          extend(s,i,i);
          //偶数长度的字符串
          extend(s,i,i+1); 
       }
       return cnt; 
    }
    //延伸字符串
    public void extend(String s,int begin,int end) {
        while (begin >= 0 && end < s.length() && (s.charAt(begin) == s.charAt(end))) {
            begin--;
            end++;
            cnt++;
        }
    }
}
```

#### [9. 回文数](https://leetcode-cn.com/problems/palindrome-number/)

```
class Solution {
    public boolean isPalindrome(int x) {
        //负数不是回文数
        if (x < 0) {
            return false;
        }
        //整数转为字符串
        String s = String.valueOf(x);
        int begin = 0;
        int end = s.length() - 1;
        //判断回文数
        while (begin <= end) {
            if (s.charAt(begin) == s.charAt(end)) {
                begin++;
                end--;
            } else {
                return false;
            } 
        }
        return true;
    }
}
```

#### [696. 计数二进制子串](https://leetcode-cn.com/problems/count-binary-substrings/)

```java
class Solution {
    public int countBinarySubstrings(String s) {
        int res = 0;
        int cnt = 1;
        int last = 0;
        for (int i = 1; i < s.length(); i++) {          
            if (s.charAt(i) == s.charAt(i-1)) {
                cnt++;
            } else {
                last = cnt;
                cnt = 1;
            }
            if (last >= cnt) {
                res++;
            }
        }
        return res;
    }
}
```

 
