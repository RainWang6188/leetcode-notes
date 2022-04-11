# 332. Reconstruct Itinerary

## Description
You are given a list of airline `tickets` where `tickets[i] = [fromi, toi]` represent the departure and the arrival airports of one flight. Reconstruct the itinerary in order and return it.

All of the `tickets` belong to a man who departs from `"JFK"`, thus, the itinerary must begin with `"JFK"`. If there are multiple valid itineraries, you should return the itinerary that has the smallest lexical order when read as a single string.

For example, the itinerary `["JFK", "LGA"]` has a smaller lexical order than `["JFK", "LGB"]`.

You may assume all tickets form at least one valid itinerary. You must use all the tickets once and only once.

**Example:**
```
Input: tickets = [["MUC","LHR"],["JFK","MUC"],["SFO","SJC"],["LHR","SFO"]]
Output: ["JFK","MUC","LHR","SFO","SJC"]
```

## Classic Solution

### 1. Hierholzer Algorithm
> excerpt from [here](https://leetcode-cn.com/problems/reconstruct-itinerary/solution/zhong-xin-an-pai-xing-cheng-by-leetcode-solution/)

Hierholzer 算法用于在连通图中寻找欧拉路径，其流程如下：

从起点出发，进行深度优先搜索。

每次沿着某条边从某个顶点移动到另外一个顶点的时候，都需要删除这条边。

如果没有可移动的路径，则将所在节点加入到栈中，并返回。

当我们顺序地考虑该问题时，我们也许很难解决该问题，因为我们无法判断当前节点的哪一个分支是「死胡同」分支。

不妨倒过来思考。我们注意到只有那个入度与出度差为 11 的节点会导致死胡同。而该节点必然是最后一个遍历到的节点。我们可以改变入栈的规则，当我们遍历完一个节点所连的所有节点后，我们才将该节点入栈（即逆序入栈）。

对于当前节点而言，从它的每一个非「死胡同」分支出发进行深度优先搜索，都将会搜回到当前节点。而从它的「死胡同」分支出发进行深度优先搜索将不会搜回到当前节点。也就是说当前节点的死胡同分支将会优先于其他非「死胡同」分支入栈。

这样就能保证我们可以「一笔画」地走完所有边，最终的栈中逆序地保存了「一笔画」的结果。我们只要将栈中的内容反转，即可得到答案。

```C++
class Solution {
public:
    vector<string> findItinerary(vector<vector<string>>& tickets) {
        unordered_map<string, priority_queue<string, vector<string>, greater<string>>> flyTo;
        for(auto& ticket : tickets) {
            flyTo[ticket[0]].emplace(ticket[1]);
        }
        
        dfs(flyTo, "JFK");
        
        reverse(stk.begin(), stk.end());
        
        return stk;
    }
    
private:
    vector<string> stk;
    
    void dfs(unordered_map<string, priority_queue<string, vector<string>, greater<string>>>& flyTo, string curr) {
        while(flyTo.count(curr) > 0 && !flyTo[curr].empty()) {
            string next = flyTo[curr].top();
            flyTo[curr].pop();
            dfs(flyTo, next);
        }
        stk.push_back(curr);
    }
};
```


1. 由于题目中说必然存在一条有效路径(至少是半欧拉图)，所以算法不需要回溯（既加入到结果集里的元素不需要删除）
2. 整个图最多存在一个死胡同(出度和入度相差1），且这个死胡同一定是最后一个访问到的，否则无法完成一笔画。
3. DFS的调用其实是一个拆边的过程（既每次递归调用删除一条边，所有子递归都返回后，再将当前节点加入结果集保证了结果集的逆序输出），一定是递归到这个死胡同（没有子递归可以调用）后递归函数开始返回。所以死胡同是第一个加入结果集的元素。
4. 最后逆序的输出即可。


### 2. Backtracking
We cannot use `multiset` since the iterator will be broken after deletion.
```C++
class Solution {
public:
    vector<string> findItinerary(vector<vector<string>>& tickets) {
        unordered_map<string, map<string, int>> flyTo;
        for(auto& ticket : tickets)
            flyTo[ticket[0]][ticket[1]]++;

        vector<string> itinerary;
        itinerary.push_back("JFK");
        dfs(flyTo, itinerary, tickets.size());
        
        return itinerary;
    }
    
private:
    bool dfs(unordered_map<string, map<string, int>>& flyTo, vector<string>& itinerary, int ticketNum) {
        if(itinerary.size() == ticketNum + 1)
            return true;
        
        string curr = itinerary.back();
        for(auto& nextDest : flyTo[curr]) {
            if(nextDest.second <= 0)
                continue;
            nextDest.second--;
            itinerary.push_back(nextDest.first);
            if(dfs(flyTo, itinerary, ticketNum))
                return true;
            itinerary.pop_back();
            nextDest.second++;
        }
        
        return false;
    }
};
```