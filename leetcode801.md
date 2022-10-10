#### [801. 使序列递增的最小交换次数](https://leetcode.cn/problems/minimum-swaps-to-make-sequences-increasing/)

首先用一个数组`dp[i][j]`表示状态，

- `i`表示`nums1` 和`nums2`上第`i`个数字
- `j`表示交换的状态 只有0 和 1  ，0表示不交换，1表示交换

因此

- `dp[i][0]`就表示当前位置**不交换**的，已经累计的交换次数

- `dp[i][1]`就表示当前位置**交换**的，已经累计的交换次数

根据题意，我们只能交换`nums1`和nums2对应位置上的数字，那么能交换的条件必须一下列情况中一种

1. `nums1[i-1] < nums1[i] && nums2[i-1] < nums2[i]`
   - `nums1[i]` 和`nums2[i]`可以随意互换，`num1[i-1]` 和`nums2[i-1]`也可以随意互换

> nums1: ...2 6... -> ...4 6... 
>
> nums2: ...4 5... -> ...2 5...

   - `nums1` 和`nums2`都递增，但是互换后不递增，要换只能一起换
> nums1: ...2 3... -> ...4 5... 
>
> nums2: ...4 5... -> ...2 3...

2. `nums1[i-1] < nums2[i] && nums2[i-1] < nums1[i]`

> nums1: ...5 9... -> ...5 6... 
>
> nums2: ...7 6... -> ...7 9...

这种情况下，只需要交换`i-1`或者`i`一个位置上的即可

## DP数组

```c++
//使用DP数组
class Solution {
public:
    int minSwap(vector<int>& nums1, vector<int>& nums2) {
        int n = nums1.size();
        int dp[n][2];
        dp[0][0] = 0;
        dp[0][1] = 1;//当前位置进行交换
        for(int i = 1; i < n; i++)
        {
            if(nums1[i-1] < nums1[i] && nums2[i-1] < nums2[i])
            {
                if(nums1[i-1] < nums2[i] && nums2[i-1] < nums1[i])
                {
                    //这种情况 当前位置可以随机交换 
                    dp[i][0] = min(dp[i-1][0],dp[i-1][1]); //i不交换 i-1任意，因此选择最小的
                    dp[i][1] = dp[i][0]+1; //i交换,i-1任意 等价于min(dp[i-1][0],dp[i-1][1])+1
                }
                else
                {
                    //要换，两者必须都交换，要么都不换
                    dp[i][0] = dp[i-1][0];
                    dp[i][1] = dp[i-1][1] + 1;
                }
            }
            else if(nums1[i-1] < nums2[i] && nums2[i-1] < nums1[i])
            {
                //两者只能由一个交换
                dp[i][0] = dp[i-1][1];//i不换，i-1必须换
                dp[i][1] = dp[i-1][0] + 1;//i换，i-1不换
            }
        }
        return min(dp[n-1][0], dp[n-1][1]);
    }
};
```



## 状态压缩

```c++

//只涉及两个状态的转换，使用a，b替代       
class Solution {
public:
    int minSwap(vector<int>& nums1, vector<int>& nums2) {
        int n = nums1.size();
        int dp[n][2];
        int max = n;
        int a = 0;
        int b = 1;//当前位置进行交换
        for(int i = 1; i < n; i++)
        {
            if(nums1[i-1] < nums1[i] && nums2[i-1] < nums2[i])
            {
                if(nums1[i-1] < nums2[i] && nums2[i-1] < nums1[i])
                {
                    //这种情况 当前位置可以随机交换 
                    a = min(a,b); //i不交换 i-1任意，因此选择最小的
                    b = a+1; //i交换,i-1任意，但是要取最小的 等价于min(a,b)+1
                }
                else
                {
                    //要换，两者必须都交换，要么都不换
                    a = a;
                    b = b+1;
                }
            }
            else if(nums1[i-1] < nums2[i] && nums2[i-1] < nums1[i])
            {
                int cur = a;
                a = b;//i不换，i-1必须换
                b = cur+1;//i换，i-1不换
            }
        }
        return min(a, b);
    }
};

```

