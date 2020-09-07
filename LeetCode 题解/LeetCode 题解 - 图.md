# 图  

主要使用BFS和DFS  

BFS使用栈。 

DFS主要是建立好邻接表，例如1->2,1->3, 那么graph[1]=[2,3]；放入两个元素。

<!-- GFM-TOC -->
[17. 电话号码的字母组合(medium)](#17-电话号码的字母组合)    

[743. 网络延迟时间(medium)](#743-网络延迟时间)  

<!-- GFM-TOC -->

<div id="17-电话号码的字母组合"></div>  
**17. 电话号码的字母组合**（medium）[题目](https://leetcode-cn.com/problems/letter-combinations-of-a-phone-number/)  
题目：  
　　给定一个仅包含数字 2-9 的字符串，返回所有它能表示的字母组合。给出数字到字母的映射如下（与电话按键相同）。注意 1 不对应任何字母。  
示例:  
　　输入："23"  
　　输出：["ad", "ae", "af", "bd", "be", "bf", "cd", "ce", "cf"].  

思路：  
　　直接使用DFS，和回溯

```cpp
class Solution {
public:    

    vector<string> letterCombinations(string digits) {
        //dfs
        set<string> st;
        vector<string> table= vector<string>(10,"");
        table[2]="abc";
        table[3]="def";
        table[4]="ghi";
        table[5]="jkl";
        table[6]="mno";
        table[7]="pqrs";
        table[8]="tuv";
        table[9]="wxyz";

        int size=digits.length();
        if(size ==0 )return  vector<string>(st.begin(),st.end());;
        string tmp;
        dfs(st,tmp,table,digits,size,0);
        return vector<string>(st.begin(),st.end());
    }
    void dfs(set<string>& st,string &tmp,vector<string> &table,string& digits,int size,int num){
        if(size==num){
            st.insert(tmp);
            return;
        }
        for(char c: table[digits[num]-'0']){    //遍历表内的字符
            tmp+=c;
            dfs(st,tmp,table,digits,size,num+1);
            tmp=tmp.substr(0,tmp.length()-1);
        }
    }
};

```
<div id="529-扫雷游戏"></div>
**529. 扫雷游戏**(medium) [力扣](https://leetcode-cn.com/problems/minesweeper/)  
题目：  
　　让我们一起来玩扫雷游戏！  

　　给定一个代表游戏板的二维字符矩阵。 'M' 代表一个未挖出的地雷，'E' 代表一个未挖出的空方块，'B' 代表没有相邻（上，下，左，右，和所有4个对角线）地雷的已挖出的空白方块，数字（'1' 到 '8'）表示有多少地雷与这块已挖出的方块相邻，'X' 则表示一个已挖出的地雷。

　　现在给出在所有未挖出的方块中（'M'或者'E'）的下一个点击位置（行和列索引），根据以下规则，返回相应位置被点击后对应的面板：  

　　1.如果一个地雷（'M'）被挖出，游戏就结束了- 把它改为 'X'。  
　　2.如果一个没有相邻地雷的空方块（'E'）被挖出，修改它为（'B'），并且所有和其相邻的未挖出方块都应该被递归地揭露。  
　　3..如果一个至少与一个地雷相邻的空方块（'E'）被挖出，修改它为数字（'1'到'8'），表示相邻地雷的数量。  
　　4.如果在此次点击中，若无更多方块可被揭露，则返回面板。  
思路：最好看一下网页上的示例，规则1直接先判断，使用DFS，不用回溯，判断下面的。
```
class Solution {
public:
    int dir_x[8]={-1,-1,-1, 0, 0,+1,+1,+1};
    int dir_y[8]={-1, 0,+1,-1,+1,-1, 0,+1};
    void dfs(vector<vector<char>>& board, int x,int y){
        int cnt=0;  //计算周围几个蛋
        for(int i=0;i<8;++i){
            int tx=x+dir_x[i];
            int ty=y+dir_y[i];
            if(tx<0 || tx>=board.size() || ty<0 ||ty>=board[0].size()){
                continue;
            }
            cnt+= (board[tx][ty]=='M');
        }
        // 规则3
        if(cnt>0) board[x][y]=cnt+'0';
        else{
            board[x][y]='B';
            for(int i=0;i<8;++i){
                int tx=x+dir_x[i];
                int ty=y+dir_y[i];
                if(tx<0 || tx>=board.size() || ty<0 ||ty>=board[0].size() ||board[tx][ty]!='E')
                    continue;
                dfs(board,tx,ty);
                }
        }
    }
    vector<vector<char>> updateBoard(vector<vector<char>>& board, vector<int>& click) {
        int x=click[0],y=click[1];
        // 规则1
        if(board[x][y]=='M') 
            board[x][y]='X';
        // 规则4
        else if(board[x][y]=='B' || (board[x][y]>'0' & board[x][y]<'9'))
            return board;
        else dfs(board,x,y);
        return board;
    }
};
```


<div id="743-网络延迟时间"></div>  
**743. 网络延迟时间**(medium) [力扣](https://leetcode-cn.com/problems/network-delay-time/)   
题目：  
　　有 N 个网络节点，标记为 1 到 N。  
　　给定一个列表 times，表示信号经过有向边的传递时间。 times[i] = (u, v, w)，其中 u 是源节点，v 是目标节点， w 是一个信号从源节点传递到目标节点的时间。  
　　现在，我们从某个节点 K 发出一个信号。需要多久才能使所有节点都收到信号？如果不能使所有节点收到信号，返回 -1。  
示例：  
　　输入：times = [[2,1,1],[2,3,1],[3,4,1]], N = 4, K = 2  
　　输出：2  
```cpp
class Solution {
public:
    int networkDelayTime(vector<vector<int>>& times, int N, int K) {
        // dfs
        vector<int> dp(N+1,101);
        vector<pair<int,int>> graph(N+1);
        for(int i=0;i<times.size();++i){    //建立邻接表
            graph[times[i][0]].emplace_back(times[i][0]);
        }
        dp[K]=0;
        dfs(times,dp);
        int max_time=0;
        for(int i=1;i<=N;++i){
            if(dp[i]>max_time){
                max_time=dp[i];
            }
            if(dp[i]>100)return -1;
        }
        return max_time;
    }
    void dfs(vector<vector<int>>& times,vector<int> &dp){
        for(vector<int> a:times){
            int u=a[0],v=a[1],w=a[2];
            if(dp[v]>dp[u]+w)  dp[v]=dp[u]+w;
            cout<<dp[v]<<endl;
        }
    }
};
```
