#### [1790. 仅执行一次字符串交换能否使两个字符串相等](https://leetcode.cn/problems/check-if-one-string-swap-can-make-strings-equal/)

```c++
class Solution {
public:
    bool areAlmostEqual(string s1, string s2) {
        if(s1 == s2) return true;
        int n = s1.length();
        int pre = -1;
        int count = 0;
        for(int i = 0 ; i < n; i++)
        {
            if(s1[i] != s2[i] && pre < 0)
            {
                //pre 表示s1和s2第一次出现不同的位置
                pre = i;
            }
            else if(s1[i] != s2[i]){
                //第二次遇到不同的，判断与第一次不相等的字符交换后的是否相等 
                //不相等说明一次交换无法使两个字符串相等
                // 相等的话 则清空pre，交换次数加一（这里count）
                if(s1[pre] == s2[i] && s1[i] == s2[pre]) pre = -1,count++;
                else return false;
            }
        }
        //能通过只交换一次，是的字符串相等
        if(count <= 1 && pre == -1) return true;
        return false;
    }
};

/*
s1 = "qgqag"
s2 = "gqgaq"
*/
```

