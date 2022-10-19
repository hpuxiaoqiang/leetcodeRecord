#### [902. 最大为 N 的数字组合](https://leetcode.cn/problems/numbers-at-most-n-given-digit-set/)

**hard**

```c++
class Solution {
public:
    int atMostNGivenDigitSet(vector<string>& digits, int n) {
        int ans = 0;
        int len_d = digits.size();
        string s = to_string(n);
        int len_s = s.length();
        for(int i = 1; i < len_s; i++) ans += pow(len_d, i); // 1. 统计所有位数小于len_s的
        for(int i = 0; i < len_s; i++)
        {
            int j  = 0;
            while(j < len_d)
            {
                if(s[i] - '0' > stoi(digits[j])) ++j;//统计比n的当前第i位为小的数字有几个
                else break; //digits是升序的，当前第j位都大于等于s[i]后续的肯定大
            }
            ans += j * pow(len_d, len_s - i -1);//当前位小，后续位数任意选
            //后面这两行！！！
            
            if(j >= len_d || stoi(digits[j]) != s[i] - '0') break;
            //1. j >= len_d 说明所有位均比当前第i(初始第0)位小，任意组合即可
            //2. stoi(digits[j]) != s[i] - '0' 说明当前第i位是不可选的 而 比第i位小的已经统计了 所有直接返回结果 
            // 否则能继续循环就说明第i位是可以在数组中选择的
            if(i == len_s-1 && j < len_d && stoi(digits[j]) == s[i] - '0') ++ans;//全部相同 也算一个 结果加一
            //到这里说明前面的都能再数组中找到，看最后一位是不是也能找到，能找到就说明有完全相同的数字
        }
        return ans;
    }
};

/*
假设数字n的位数为m, 数字digits数组长度为k
情况1：构成的数字位数小于m, 那么无论如何都要比n小 这种情况下的结果为sum(k^i) i = 1,...,m-1

情况2：构成的数字位数等于m，那么就要从高位开始逐位开始比较 

例如 digits={1,3,5,7} n = 5721


然后 先看小于最高位的，严格小于 5 的有 2 个，或者说 (1***, 3***) ：ans += 2 * 4^3

再看最高位能不能从数组中选择到

最高位 5 可以选择，再看次高位，可能有低位剩余，或者说 (5???)。求 721，看最高位，严格小于 7 的有 3 个，或者说 (1**, 3**, 5**) ： ans += 3 * 4^2

7 可以选择，求 21，看最高位，严格小于 2 的有 1 个：ans += 1 * 4^1

2不可以选择，或者说 (573* > 5721, 571* ∈ 57**)。结束

由于严格小于，如果全部相同要 +1
*/
```

