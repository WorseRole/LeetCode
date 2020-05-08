# LeetCode

### 最大正方形

* 难度：中等

>  题目：在一个由 0 和 1 组成的二维矩阵内，找到只包含 1 的最大正方形，并返回其面积。
>
> 示例:
>
> 输入: 
>
> 1 0 1 0 0
> 1 0 1 1 1
> 1 1 1 1 1
> 1 0 0 1 0
>
> 输出: 4
>

~~~java
//暴力搜索  现在的水平菜的暴力搜索都写不了噗一口血喷出来
class Solution {
    public int maximalSquare(char[][] matrix) {
        int maxSide = 0;
        if(matrix == null || matrix.length == 0 || matrix[0].length==0) {
            return maxSide;
        }
        int rows = matrix.length;
        int cols = matrix[0].length;
        for(int i=0; i<rows; i++) {
            for(int j=0; j<cols; j++) {
                if(matrix[i][j] == '1') {
                    maxSide = Math.max(maxSide, 1);
                //可能最大的边长
                    int currentSide = Math.min(rows-i, cols-j);
                    //取证
                    for(int k=1; k<currentSide; k++) {
                        boolean flag = true;
                        if(matrix[i+k][j+k] == '0') {
                            break;
                        }
                        for(int m=0; m<k; m++) {
                            if(matrix[i+m][j+k] == '0' || matrix[i+k][j+m] == '0') {
                                flag = false;
                            }
                        }
                        if(flag) {
                            maxSide = Math.max(maxSide, k+1);
                        }else {
                            break;
                        }
                    }
                }
            }
        }
        return maxSide * maxSide;
    }
}
~~~

**暴力搜索解题思路：**

> 首先从二维数组的第一个数开始找，等于1时，因为是正方形我们可以通过(rows-i cols-j )取最小值得到可能是最大变长的值，当然我们只是先定义一个值的边界，好去接下来的循环查找。
>
> 循环时，因为第一次是对i+k，j+k的树进行判断，就是斜下方的值进行判断，没有对上或者下的值进行判断是否为1。因此需要定m 然后（i+k，j+m）（i+m，j+k）对上方下方的数值进行判断。每判断一组，就可以在下面为真正的边长进行max 也就是+1；【这些都是在第一个i，j的值为1的基础上进行的】

> > 主要的技术点其实就是，仔细的通过二维数组一个个的遍历下去，拿到最大的边长值。然后边长*边长就是正方形的面积。

> > > > > 当然即使这样，写不出的自己仍写不出来。lower......



这题要是从暴力角度来搞的话，死。

下面，还是来说一下精准而优雅的动态规划(dp)

**动态规划解题思路：**

> 用dp(i, j) 来表示为右下角，且往上数只包含1的正方形的最大边长数。（i，j）就是坐标。
>
> 如果该位置是0，那么dp(i, j)为0，因为当前的位置已经不是1了，所以不可能存在于包含1 的正方形中
>
> 如果该位置是1，那么dp(i, j) 由它的上方，左方，左上方的值来决定。通过dp方程来进行计算.                        dp(i, j) = min(dp(i, j-1), dp(i-1, j), dp(i-1, j-1)) +1  几个的最小值加1 来取此时dp(i, j)的值
>
> 最后我们再考虑一手边界情况，就是最左边一行和最上边一行，以这个位置的dp(i, j)最大也就是1了，因为已经到边界了。也就是 i=0 和 j=0 的情况 为1 dp(i, j)为1

上一个很清晰的例子：

> > 原始数据如下：

```java
0 1 1 1 0
1 1 1 1 0
0 1 1 1 1
0 1 1 1 1
0 0 1 1 1
```

> > 对应的 dp 值如下：

~~~java
0 1 1 1 0
1 1 2 2 0
0 1 2 3 1
0 1 2 3 2
0 0 1 2 3
~~~

![动态规划dp示例图](/Users/liyanyan/Desktop/personal/leetcode/最大正方形面积/img/动态规划dp示例图.png)

好了，优雅的动态规划就整出来了。当然还是多刷刷题，才能灵活掌握：

~~~java
class Solution {
    public int maximalSquare(char[][] matrix) {
        int maxSide = 0;
        if(matrix == null || matrix.length==0 || matrix[0].length==0) {
            return maxSide;
        }
        int rows = matrix.length;
        int cols = matrix[0].length;
        int[][] dp = new int[rows][cols];
        for(int i=0; i<rows; i++) {
            for(int j=0; j<cols; j++) {
                if(matrix[i][j] == '1') {
                    //靠边的数值为1 dp也为1
                    if(i==0 || j==0) {
                        dp[i][j] = 1;
                    }else {
                        dp[i][j] = Math.min(Math.min(dp[i-1][j-1], dp[i][j-1]), dp[i-1][j]) + 1;
                    }
                }
                maxSide = Math.max(maxSide, dp[i][j]);
            }
        }
        return maxSide*maxSide;
    }
}
~~~

