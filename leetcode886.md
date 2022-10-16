#### [886. 可能的二分法](https://leetcode.cn/problems/possible-bipartition/)

```c++
class Solution {
public:
    bool dfs(int currnode,int currnode_color,vector<int>& color, vector<vector<int>>& corr)
    {
        //currnode 当前待搜索的人
        //currnode_color 给当前的人染色颜色
        //color 记录每个人染色情况 0 未分组 1 红色组 2 蓝色组
        //corr每个人 不能与其分在同组的人
        //深度优先搜索
        color[currnode] = currnode_color;
        for(auto& nextnode: corr[currnode])//遍历当前的人，所有不能在同组的人
        {
            if(color[nextnode] && color[nextnode] == color[currnode])
            {
                //如果某一个不能分在同组的人，已经染色了，且与当前人同样颜色，说明冲突了，分组失败
                return false;
            }
            
            if(!color[nextnode] && !dfs(nextnode, 3^currnode_color, color, corr)){
                //不能分在同组的人没有染色，就给他染不用颜色，然后继续深度优先搜索
                //使用3^currnode_color 因为我们用 0 1 2表示颜色 
                //而3^1=(11)^(01)=(10) = 2 3^2 =(11)^(10)=(01) = 1 就很快 不用再去判断
                return false;
            }
        }
        return true;
    }
    bool possibleBipartition(int n, vector<vector<int>>& dislikes) {
        vector<vector<int>> corr(n+1); //1~n 用来存放每个人 不能与其分在同组的人
        vector<int> color(n+1,0);//1~n 每个人的颜色分组，0 未分组 1 红色组 2 蓝色组
        for(auto& dislike : dislikes)
        {
            corr[dislike[0]].push_back(dislike[1]);
            corr[dislike[1]].push_back(dislike[0]);
        }
        for(int i = 1; i <= n; i++)
        {
            //第i个人没染色，先给他染个红(1)， 然后往下搜索
            if(color[i] == 0 && !dfs(i, 1, color, corr)){
                return false;
            }
        }
        return true;
    }
};
```

