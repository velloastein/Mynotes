每日一题：

[954. 二倍数对数组](https://leetcode-cn.com/problems/array-of-doubled-pairs/)

```java
class Solution {
    public boolean canReorderDoubled(int[] arr) {
        HashMap<Integer, Integer> maps = new HashMap<>();
        //统计个数
        for (int i : arr) {
            maps.put(i,maps.getOrDefault(i,0) + 1);
        }
        // 数组里0的个数只能是偶数
        if (maps.getOrDefault(0,0) % 2 != 0) {
            return false;
        }
        //键进行排序
        ArrayList<Integer>  keys = new ArrayList<>();
        for (int i : maps.keySet()) {
            keys.add(i);
        }
        Collections.sort(keys, new Comparator<Integer>() {
            @Override
            public int compare(Integer a, Integer b) {
                int a1 = Math.abs(a);
                int b1 = Math.abs(b);
                return  a1 - b1 ;
            }
        });
        //比较
        for (int i : keys) {
            if (maps.get(i) > maps.getOrDefault(2 * i,0)) {
                return false;
            } 
            maps.put(2 * i , maps.getOrDefault(2 * i,0) - maps.get(i));
        }
        return true;
    }
}
```



#### [744. 寻找比目标字母大的最小字母](https://leetcode-cn.com/problems/find-smallest-letter-greater-than-target/)

```java
class Solution {
    public char nextGreatestLetter(char[] letters, char target) {
        for (char i : letters) {
            if (i <= target ) {
                continue;
            } else {
                return i;
            }
        }
        //溢出的话
        return letters[0];
    }
}

```

二分查找

```
class Solution {
    public char nextGreatestLetter(char[] letters, char target) {
        int low = 0;
        int high = letters.length - 1;
        if (letters[high] <= target) {
            return letters[0];
        }
        //二分查找
        while (low < high) {
            int mid = low + (high - low) / 2;
            if (letters[mid] == target) {
                low = mid + 1;
            } else if (letters[mid] >= target) {
                high = mid;
            } else {
                low = mid + 1;
            }
        }
        return letters[low];
    }
}
```

