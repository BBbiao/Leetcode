# 数组与矩阵
<!-- GFM-TOC -->
### 数组

### 矩阵
[289.生命游戏(medium)](#289-生命游戏)

<!-- GFM-TOC -->

<div id="289-生命游戏"></div>
**289-生命游戏**(medium)  

　　给定一个包含 m × n 个格子的面板，每一个格子都可以看成是一个细胞。每个细胞具有一个初始状态 live（1）即为活细胞， 或 dead（0）即为死细胞。每个细胞与其八个相邻位置（水平，垂直，对角线）的细胞都遵循以下四条生存定律：  

　　如果活细胞周围八个位置的活细胞数少于两个，则该位置活细胞死亡；  
　　如果活细胞周围八个位置有两个或三个活细胞，则该位置活细胞仍然存活；  
　　如果活细胞周围八个位置有超过三个活细胞，则该位置活细胞死亡；  
　　如果死细胞周围正好有三个活细胞，则该位置死细胞复活；  

　　根据当前状态，写一个函数来计算面板上细胞的下一个（一次更新后的）状态。下一个状态是通过将上述规则同时应用于当前状态下的每个细胞所形成的，其中细胞的出生和死亡是同时发生的。  

```cpp
class Solution {
public:
    int dx[8]={-1,-1,-1, 0, 0,+1,+1,+1};
    int dy[8]={-1, 0,+1,-1,+1,-1, 0,+1};
    void gameOfLife(vector<vector<int>>& board) {
        int row= board.size();
        int col=board[0].size();
        //第一次遍历，活变死记-1， 死变活记2
        for(int i=0;i<row;i++){
            for(int j=0;j<col;j++){
                int cnt=count_cell(board,i,j,row,col);
                if(board[i][j]==1 && cnt<2) board[i][j]=-1;
                else if(board[i][j]==1 && cnt>3) board[i][j]=-1;
                else if(board[i][j]==0 && cnt==3) board[i][j]=2;
            }
        }
        for(int i=0;i<row;i++){
            for(int j=0;j<col;j++){
                if(board[i][j]==-1 ) board[i][j]=0;
                else if(board[i][j]==2) board[i][j]=1;
            }
        }
    }
    int count_cell(vector<vector<int>>& board,int x,int y,int row,int col){
        int cnt=0;
        for(int i=0;i<8;i++){
            if(x+dx[i]<0 || x+dx[i]==row || y+dy[i]<0 || y+dy[i]==col)continue;
            //-1和1 在初始状态都是活细胞
            if(board[x+dx[i]][y+dy[i]] ==1 || board[x+dx[i]][y+dy[i]] ==-1)cnt++;
        }
        return cnt;
    }
};
```
