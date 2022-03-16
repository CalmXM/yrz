# 数组
## 存在重复元素
```
给你一个整数数组 nums 。如果任一值在数组中出现 至少两次 ，返回 true ；如果数组中每个元素互不相同，返回 false 。

 

示例 1：

输入：nums = [1,2,3,1]
输出：true

```

***题解***
```
关键条件在于重复值，这恰恰与Set的特性相吻合。即可以通过将数组中的元素添加到一个HashSet对象中，根据add方法的返回值来判断正在添加的元素是否已存在于集合中。若返回值为true，说明添加成功，暂时不存在相同值；若返回值为false，说明添加失败，集合中存在相同值。
```

***代码***
```java
class Solution {
    public boolean containsDuplicate(int[] nums) {
        HashSet hash = new HashSet();
        for(int n:nums){
            if(!hash.add(n)){
                return true;
            }
        }
        return false;
    }
}
```



## 最大子数组和
```
给你一个整数数组 nums ，请你找出一个具有最大和的连续子数组（子数组最少包含一个元素），返回其最大和。

子数组 是数组中的一个连续部分。

 

示例 1：

输入：nums = [-2,1,-3,4,-1,2,1,-5,4]
输出：6
解释：连续子数组 [4,-1,2,1] 的和最大为 6 。

```

***题解***  
1. 刚开始的思路是，从开头出发，通过逐一选取数组中的元素，然后以它为中心选取子数组，最后选出和最大的子数组。 如果按这个思路来，我们需要求出以当前元素开头的子数组的和，并记录下来。其他元素重复此动作。最后经过比对，从记录中选取一个最大值。实现如下：
   ```java
   class Solution {
    public int maxSubArray(int[] nums) {
        ArrayList<Integer> list = new ArrayList<Integer>();
        
        int temp = 0;
        for(int i = 0 ; i < nums.length ; i++){
            temp = nums[i];
            list.add(temp);
            for(int j = i+1; j < nums.length;j++){
                temp += nums[j];
                list.add(temp);
            }
        }

        return Collections.max(list);
    }
   }
   ```
   在测试用例200/209中，超出了内存限制，但我觉得仍旧具有可行性，只是时间复杂度太大。

2. 利用动态规划求解，将复杂的问题分解成多个子问题，最终选出最优解
   1. 子问题定义
     逐一把每个元素当做是最后一个元素——即求以当前元素结尾的子数组的和
   2. 状态转移方程（描述子问题之间的联系）
     如果以当前元素i结尾的子数组<=0，那么后面元素的求解就没有必要再加入这个子数组——一个数加上0或者负数，不可能比当前数大。
     ![](https://s3.bmp.ovh/imgs/2022/03/849139261e9441a9.png)
    
```java
1. 第一版
class Solution {
    public int maxSubArray(int[] nums) {
        int[] dp = new int[nums.length];
        dp[0] = nums[0];

        for(int i = 1 ; i < nums.length ; i++){
            if(dp[i-1] <= 0){
                dp[i] = nums[i];
            } else {
                dp[i] = nums[i] + dp[i-1];
            }
        }

        Arrays.sort(dp);
        return dp[dp.length-1];
    }
}

2. 第二版
class Solution {
    public int maxSubArray(int[] nums) {
        int[] dp = new int[nums.length];
        dp[0] = nums[0];

        int res = dp[0];
        for(int i = 1 ; i < nums.length ; i++){
            if(dp[i-1] <= 0){
                dp[i] = nums[i];
            } else {
                dp[i] = nums[i] + dp[i-1];
            }

            res = Math.max(res,dp[i]);
        }
        return res;
    }
} 
```

## 买股票的最佳时机
```
给定一个数组 prices ，它的第 i 个元素 prices[i] 表示一支给定股票第 i 天的价格。

你只能选择 某一天 买入这只股票，并选择在 未来的某一个不同的日子 卖出该股票。设计一个算法来计算你所能获取的最大利润。

返回你可以从这笔交易中获取的最大利润。如果你不能获取任何利润，返回 0 。

 

示例 1：
输入：[7,1,5,3,6,4]
输出：5
解释：在第 2 天（股票价格 = 1）的时候买入，在第 5 天（股票价格 = 6）的时候卖出，最大利润 = 6-1 = 5 。
     注意利润不能是 7-1 = 6, 因为卖出价格需要大于买入价格；同时，你不能在买入前卖出股票。


示例 2：
输入：prices = [7,6,4,3,1]
输出：0
解释：在这种情况下, 没有交易完成, 所以最大利润为 0。
```

***题解***
> 思路：可利用与最大子数组和相同的思路求解。即求出以某一天结尾的利润的值，选取最大的出来
1. 定义子问题
   >以某天出售股票的最大利润为多少
2. 状态转移方程
   >第i天的利润 = i-1天的利润  +  i和i-1的差