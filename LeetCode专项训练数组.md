数组：

#### [283. 移动零](https://leetcode-cn.com/problems/move-zeroes/)

常规

思路：想象成有额外的数组。把非零的元素都复制到前面，后面元素都赋值为0

```java
class Solution {
    public void moveZeroes(int[] nums) {
        //思路：想象成有额外的数组。把非零的元素都复制到前面，后面元素都赋值为0
        int j = 0;
        //前面打的元素都赋值为非零的元素
        for (int i = 0; i < nums.length; i++) {
            if (nums[i] != 0) {
                nums[j++] = nums[i];
            }
        }
        //后面的元素都赋值为0
        while (j < nums.length) {
            nums[j++] = 0;
        }
    }
}
```

双指针: 左指针指向未处理打的数，右指针指向非零数，交换位置不改变非零元素的相对顺序

```java
class Solution {
    public void moveZeroes(int[] nums) {
        //双指针： 左指针指向未处理打的数，右指针指向非零数，交换位置不改变非零元素的相对顺序
        int left = 0, right = 0;
        while (right < nums.length) {
            //交换非零元素
            if (nums[right] != 0) {
                int temp = nums[left];
                nums[left] = nums[right];
                nums[right] = temp;
                left++;
            }
            //移动右指针
            right++;
        }
    }
}
```

#### [566. 重塑矩阵](https://leetcode-cn.com/problems/reshape-the-matrix/)

思路：把二维数组拍扁为一个一维数组，原数组与一维数组映射关系为  (i,j)→i×n+j ，也可以将元素x映射回二维数组 i = x /n , j = x % n。

```java
class Solution {
    public int[][] matrixReshape(int[][] mat, int r, int c) {
        //数组行数
        int m = mat.length;
        //列数
        int n = mat[0].length;
        if (m * n != r * c) {
            return mat;
        }
        int[][] res = new  int[r][c];
        for (int idx = 0; idx < r * c; idx ++) {
            //定位第x个元素在原数组的第几行第几列，应该放在新数组的第几行第几列
            res[idx / c][ idx % c] = mat[idx / n][idx % n];
        }
        return res;
    }
}
```

