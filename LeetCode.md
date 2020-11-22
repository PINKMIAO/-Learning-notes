# LeetCode

## 数组

#### 解题方法

1. 求两数之和为目标数字：（输出对应下标）
   1. 将数组放进map，值为k，下标为v
   2. 目标减下标对应的数字
   3. 余数在通过hashMap.containsKey()找

11. 盛最多水的容器：
    1. 双指针
    2. Math.max

15. 三数之和为零不重复有几种可能，输出数字：
    1. 先排序。
    2. 去重
    3. L = i + 1, R = len - 1 ; i + L + R;
    4. 去重

16. 最接近目标数的三数之和：

    1. Math.abs( target - ans ) < Math.abs( target -sum )

    

18. 四数之和：

