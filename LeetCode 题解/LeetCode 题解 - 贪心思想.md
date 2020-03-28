# 贪心思想  
保证每次操作都是局部最优的，并且最后得到的结果是全局最优的。  
<!-- GFM-TOC -->  
[455.分发饼干(Easy)](#455-分发饼干)  
[55.跳跃游戏(medium)](#55-跳跃游戏)  
[134.加油站(medium)](#134-加油站)   
<!-- GFM-TOC -->

保证每次操作都是局部最优的，并且最后得到的结果是全局最优的。

**455-分发饼干**(Easy) [力扣](https://leetcode-cn.com/problems/assign-cookies/description/)  

　　题目描述：每个孩子都有一个满足度 grid，每个饼干都有一个大小size，只有饼干的大小大于等于一个孩子的满足度，该孩子才会获得满足。求解最多可以获得满足的孩子数量。
　　　Input: grid[1,3], size[1,2,4]  
　　　Output: 2  
　　　a. 给一个孩子的饼干应当尽量小并且又能满足该孩子，这样大饼干才能拿来给满足度比较大的孩子。  
　　　b. 因为满足度最小的孩子最容易得到满足，所以先满足满足度最小的孩子。  
　　在以上的解法中，我们只在每次分配时饼干时选择一种看起来是当前最优的分配方法，但无法保证这种局部最优的分配方法最后能得到全局最优解。我们假设能得到全局最优解，并使用反证法进行证明，即假设存在一种比我们使用的贪心策略更优的最优策略。如果不存在这种最优策略，表示贪心策略就是最优策略，得到的解也就是全局最优解。  
　　证明：假设在某次选择中，贪心策略选择给当前满足度最小的孩子分配第 m 个饼干，第 m 个饼干为可以满足该孩子的最小饼干。假设存在一种最优策略，可以给该孩子分配第 n 个饼干，并且 m < n。我们可以发现，经过这一轮分配，贪心策略分配后剩下的饼干一定有一个比最优策略来得大。因此在后续的分配中，贪心策略一定能满足更多的孩子。也就是说不存在比贪心策略更优的策略，即贪心策略就是最优策略。  

```cpp
		class Solution {
		public:
		    int findContentChildren(vector<int>& g, vector<int>& s) {
		        sort(g.begin(),g.end());
		        sort(s.begin(),s.end());
		        int res=0;
		        vector<int>::iterator it = s.begin();
		        if(it==s.end()) return res;;
		        for(int child :g ){
		            while(*it < child) { //直到找到比child胃口大的饼干       
		                it++; 
		                if(it==s.end())return res;;
		            }
		            res++;  it++;
		            if(it==s.end())break;
		        }
		        return res;
		    }
};
```

**55-跳跃游戏**(medium)  
　　[力扣](https://leetcode-cn.com/problems/jump-game/)  
　　给定一个非负整数数组，你最初位于数组的第一个位置。  
　　数组中的每个元素代表你在该位置可以跳跃的最大长度。  
　　判断你是否能够到达最后一个位置。  
　　　示例 1:  
　　　　输入: [2,3,1,1,4]  
　　　　输出: true  
　　　　解释: 我们可以先跳 1 步，从位置 0 到达 位置 1, 然后再从位置 1 跳 3 步到达最后一个位置。  

```cpp
//执行用时 :20 ms, 在所有 C++ 提交中击败了25.72% 的用户
//内存消耗 :9.1 MB, 在所有 C++ 提交中击败了100.00%的用户
class Solution {
public:
    bool canJump(vector<int>& nums) {
        int n = nums.size();
        int last = n - 1;
        for(int i = n - 1; i >= 0; --i){
            if(i + nums[i] >= last){
                last = i;
            }
        }
        return !last;
    }
};```


**134-加油站** (medium)   
　　在一条环路上有 N 个加油站，其中第 i 个加油站有汽油 gas[i] 升。  
　　你有一辆油箱容量无限的的汽车，从第 i 个加油站开往第 i+1 个加油站需要消耗汽油 cost[i] 升。你从其中的一个加油站出发，开始时油箱为空。  
　　如果你可以绕环路行驶一周，则返回出发时加油站的编号，否则返回 -1。  
　说明:   
　　如果题目有解，该答案即为唯一答案。  
　　输入数组均为非空数组，且长度相同。  
　　输入数组中的元素均为非负数。  
　示例 1:  
　　　输入:   
　　　gas  = [1,2,3,4,5]  
　　　cost = [3,4,5,1,2]  
　　　输出: 3  
　思路：贪心算法，从0(0是个哑结点，加油站从1开始)开始算gas的可用量，例如在1位置可用的油为-2,累加到2位置时，可用的油为-4，一直累加，当累加和最少的位置的下一个位置就是出发点。  
　　　　　加油站位置：0  1  2  3  4  5  
　　可用汽油量累加和：0 -2 -4 -6 -3  0  
　　当然如果可用汽油量累加和最后小于0，肯定到不了重点。  

```cpp
class Solution {
public:
    int canCompleteCircuit(vector<int>& gas, vector<int>& cost) {
        //从0开始gas的可用量，gas_sum += gas[i]-cost[i];
        int gas_sum=0;
        int min_sum =0, min_i=0;
        int size=gas.size();
        for(int i=1;i<=size;i++){
            gas_sum+=gas[i-1]-cost[i-1];
            if(min_sum>gas_sum){
                min_sum= gas_sum;
                min_i=i;
            }
        };
        return gas_sum >=0?min_i :-1;
    }
};```