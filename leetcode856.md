# leetcode856 括号的分数

## 计数求解

```c++
class Solution {
public:
    int scoreOfParentheses(string s) {
        //用一个depth记录当前深度
        int ans = 0, depth = 0;
        for(int i = 0 ; i < s.length(); i++)
        {
            if(s[i] == '(')
            {
                depth++;
            }
            else
            {
                depth--;
                if(s[i-1] == '(')
                {
                    ans += 1<<depth;
                }
            }
        }
        return ans;
    }
};
```



//计数

根据题意 只有连续的完整括号"()"才能拿分，其余的都是用来计算能拿几分的

 "((),(()))"

在这个例子里面，只有两个是能拿分的

拆分一下

(()) 深度为2 2**(2-1) = 2

((())) 深度为3 2**(3-1) = 4