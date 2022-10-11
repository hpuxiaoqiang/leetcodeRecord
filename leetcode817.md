#### [817. 链表组件](https://leetcode.cn/problems/linked-list-components/) 

nums中的每一个元素都可以算是Head的一个链表，但是我们需要的是一个尽可能常的连续子链表
 例如 
 链表为：0->1->2->3->4->5
 nums [0,1,2,4,5]
 例如0，1，2都可以是子链表，但是它们能构成一个连续的子链表1->2->3,这就是一个组件，
 而单独的1，2，3，或者1-2，2-3就不是，因为它们还能变长


```c++
class Solution {
public:
    int numComponents(ListNode* head, vector<int>& nums) {
        int ans = 0;
        unordered_set<int> set(nums.begin(), nums.end());
        while(head){
            // 当前节点的值 在nums中
            if(set.find(head->val) != set.end()){
                //链表遍历结束 或者当前节点的下一个值不在nums中 那么当前部分就是一个组件
                if(head->next == NULL || set.find(head->next->val) == set.end())
                {
                    ans++;
                }
            }
            head = head->next;
        }
        return ans;
    }
};
```

