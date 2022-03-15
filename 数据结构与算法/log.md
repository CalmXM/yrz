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

## 题解  
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
   

   