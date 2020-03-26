# 动态规划

```text	
动态规划一般可分为线性动规，区域动规，树形动规，背包动规四类。
举例\:
+ 线性动规
　　[53.最大子序和](#53-最大子序和)
　　[70.爬楼梯](#70-爬楼梯)
　　[96.不同的二叉搜索树](#96-不同的二叉搜索树)
　　[121.买卖股票的最佳时机](#121-买卖股票的最佳时机)
　　[198.打家劫舍](#198-打家劫舍)
　　[213.打家劫舍II](#213-打家劫舍II)
　　[322.零钱兑换](#322-零钱兑换)
+ 区域动规
　　[62.不同路径](#62-不同路径)
+ 树形动规
+ 背包问题
```

## **1.线性动规**
**53. 最大子序和**<div  id="53-最大子序和"> </div> 

```text	
给定一个整数数组 nums ，找到一个具有最大和的连续子数组（子数组最少包含一个元
	素），返回其最大和。
　　示例:
　　　　输入: [-2,1,-3,4,-1,2,1,-5,4],
　　　　输出: 6
　　　　解释: 连续子数组 [4,-1,2,1] 的和最大，为 6。
　　注意：贪心与动态规划的方向是反的。
　　[*way1*](#53-way1): 先遍历一遍确认存在正数，若不存在输出最大的负数，如存在，则用一个tmp存和，如果tmp小于0，说明这个子列不能提供一点帮助，那么让他等于0，然后用一个max来存中间最大的和。
　　[*way2*](#53-way2)：分治。先求两边最大的，再求一个这两边跨中线的最大子列，看看三个数哪个大。然后逐渐往上层走。
　　[*way3*](#53-way3)：贪心
　　[*way4*](#53-way4)：动态规划，dp[i]表示nums中以nums[i]结尾的最大子序和，子问题公式　dp[i] = max(dp[i - 1] + nums[i], nums[i]);	
```
<div  id="53-way1"> </div>

```cpp
/******************************* way1 ****************************************/
//执行用时 :8 ms, 在所有 C++ 提交中击败了82.01% 的用户
//内存消耗 :14.7 MB, 在所有 C++ 提交中击败了5.10%的用户
class Solution {
public:
    int maxSubArray(vector<int>& nums) {
        int size = nums.size();
        int sum=0,tmp=nums[0];
        bool negflag = 1;   //默认是nums全是负数
        //如果全是负数
        for(int i=0;i<size;i++){
            if(nums[i]>0){
                negflag =0;
                break;
            } 
            else tmp = nums[i]>tmp? nums[i]:tmp;
        }
        if(negflag) return tmp;
        //存在正数
        tmp=0;
        for(int i=0;i<size;i++){
            tmp +=nums[i];
            if(tmp>sum) sum=tmp;
            if(tmp<0)tmp=0; 
        }
        return sum;
    }
};
```
<div  id="53-way2"> </div>

```cpp
/******************************* way2 ****************************************/

class Solution
{
public:
	int maxSubArray(vector<int> &nums)
	{
		//类似寻找最大最小值的题目，初始值一定要定义成理论上的最小最大值
		int result = INT_MIN;
		int numsSize = int(nums.size());
		result = maxSubArrayHelper(nums, 0, numsSize - 1);
		return result;
	}

	int maxSubArrayHelper(vector<int> &nums, int left, int right)
	{
		if (left == right)
		{
			return nums[left];
		}
		int mid = (left + right) / 2;
		int leftSum = maxSubArrayHelper(nums, left, mid);
		//注意这里应是mid + 1，否则left + 1 = right时，会无线循环
		int rightSum = maxSubArrayHelper(nums, mid + 1, right);
		int midSum = findMaxCrossingSubarray(nums, left, mid, right);
		int result = max(leftSum, rightSum);
		result = max(result, midSum);
		return result;
	}

	int findMaxCrossingSubarray(vector<int> &nums, int left, int mid, int right)
	{
		int leftSum = INT_MIN;
		int sum = 0;
		for (int i = mid; i >= left; i--)
		{
			sum += nums[i];
			leftSum = max(leftSum, sum);
		}

		int rightSum = INT_MIN;
		sum = 0;
		//注意这里i = mid + 1，避免重复用到nums[i]
		for (int i = mid + 1; i <= right; i++)
		{
			sum += nums[i];
			rightSum = max(rightSum, sum);
		}
		return (leftSum + rightSum);
	}
};
```
<div  id="53-way3"> </div>

```cpp
/******************************* way3 ****************************************/
//执行用时 :12 ms, 在所有 C++ 提交中击败了47.25% 的用户
//内存消耗 :14.6 MB, 在所有 C++ 提交中击败了5.10%的用户
class Solution {
public:
	int maxSubArray(vector<int>& nums) {
		int size = nums.size();
		int max_sum=nums[0],tmp_sum=nums[0];
		for(int i=1;i<size;i++){
			tmp_sum = max(nums[i],tmp_sum+nums[i]);
			max_sum = max(tmp_sum,max_sum);
		}
		return max_sum;
	}
};
```

<div  id="53-way4"> </div>

```cpp
/******************************* way4 ****************************************/
class Solution
{
public:
	int maxSubArray(vector<int> &nums)
	{
		//类似寻找最大最小值的题目，初始值一定要定义成理论上的最小最大值
		int result = INT_MIN;
		int numsSize = int(nums.size());
		//dp[i]表示nums中以nums[i]结尾的最大子序和
		vector<int> dp(numsSize);
		dp[0] = nums[0];
		result = dp[0];
		for (int i = 1; i < numsSize; i++)
		{
			dp[i] = max(dp[i - 1] + nums[i], nums[i]);		//子问题公式
			result = max(result, dp[i]);
		}
		return result;
	}
};
```
**70.爬楼梯**(easy)<div  id="70-爬楼梯"> </div>

```text	
假设你正在爬楼梯。需要 n 阶你才能到达楼顶。
每次你可以爬 1 或 2 个台阶。你有多少种不同的方法可以爬到楼顶呢？
　　注意：给定 n 是一个正整数。
way1: 动态规划，F[i]表示到达底i个台阶，有几种方式，F[i] = F[i-1]+F[i-2];
```

```cpp
class Solution {
public:
    int climbStairs(int n) {
        //F[i]表示到达底i个台阶，有几种方式
        //F[i] = F[i-1]+F[i-2];
        int F[n+1];
        F[0]=1;
        F[1]=1;
        for(int i=2;i<=n;i++){
            F[i] = F[i-1]+F[i-2];
        }
        return F[n];
    }
};
```
	
**96.不同的二叉搜索树**(medium)<div  id="96-不同的二叉搜索树"> </div>

```text
　　给定一个整数 n，求以 1 ... n 为节点组成的二叉搜索树有多少种？
　　示例:
　　　输入: 3
　　　输出: 5
　　　解释:给定 n = 3, 一共有 5 种不同结构的二叉搜索树:
　　[way1](#96-way1): 动态规划问题，1,2...i...n,我们可以看做以数字i为根，然后前i-1个为左子树，后n-i个为右子树。
　　　可以定义两个函数：
　　　　G(n): 长度为n的序列的不同二叉搜索树个数。
　　　　F(i,n): 以i为根的不同二叉搜索树个数(1≤i≤n)。
		　　	　　　所以	G(n)=∑F(i,n)
		　　举例而言，F(3,7)，以3为根的不同二叉搜索树个数。为了以3为根从序列[1,2,3,4,5,6,7]构建二叉搜索树，我们需要从左子序列 [1,2] 构建左子树，从右子序列 [4,5,6,7] 构建右子树，然后将它们组合(即笛卡尔积)。
　　巧妙之处在于，我们可以将[1,2]构建不同左子树的数量表示为 G(2)。从[4,5,6,7]构建不同右子树的数量表示为 G(4)。这是由于 G(n)和序列的内容无关，只和序列的长度有关。于是F(3,7)=G(2)⋅G(4)。概括而言，我们可以得到以下公式：
					F(i,n)=G(i−1)⋅G(n−i)
					G(n)=∑G(i−1)⋅G(n−i) 
		　　为了计算函数结果，我们从小到大计算，因为G(n)的值依赖于 G(0)…G(n−1)。所以要挨个计算。
　　时间复杂度: O(N^2)
　　空间复杂度:上述算法的空间复杂度主要是存储所有的中间结果，因此为O(N)
　　[way2](#96-way2)：上面G(n)函数的值被称为 卡塔兰数 C(n)​。卡塔兰数更便于计算的定义如下:
　　　　　C(0)=1,C(n+1)=2(2n+1)*C(n)/(n+2)
		
<div  id="96-way1"> </div>
```

```cpp
/******************************* way1 ****************************************/
class Solution {
public:
    int numTrees(int n) {
		int G[n+1];
		G[0]=1;
		G[1]=1;

        for(int i=2;i<=n;i++){
            int cal = 0;
			for(int j=1;j<=i;j++){
				cal += G[j-1] * G[i-j];		//注意在这个地方防止内存重复读写
			}
            G[i] = cal;						//用一个变量，就是对寄存器重复读写
		}
		return G[n];
    }
};
```

<div  id="96-way2"> </div>

```cpp
/******************************* way2 ****************************************/
//执行用时 :0 ms, 在所有 C++ 提交中击败了100.00% 的用户
//内存消耗 :7.5 MB, 在所有 C++ 提交中击败了100.00%的用户
class Solution {
  public int numTrees(int n) {
    // Note: we should use long here instead of int, otherwise overflow
    long C = 1;
    for (int i = 0; i < n; ++i) {
      C = C * 2 * (2 * i + 1) / (i + 2);
    }
    return (int) C;
  }
};
```
**121.买卖股票的最佳时机**<div  id="121-买卖股票的最佳时机"> </div>

```text	
给定一个数组，它的第 i 个元素是一支给定股票第 i 天的价格。
如果你最多只允许完成一笔交易（即买入和卖出一支股票一次），设计一个算法来计算你所能获取的最大利润。
	注意你不能在买入股票前卖出股票。
　　示例 1:
　　　输入: [7,1,5,3,6,4]
　　　输出: 5
　　　解释: 在第 2 天（股票价格 = 1）的时候买入，在第 5 天（股票价格 = 6）的时候卖出，最大利润 = 6-1 = 5 。
　　　注意利润不能是 7-1 = 6, 因为卖出价格需要大于买入价格。

[way1](#121-way1): 动态规划，low_price表示在前i天最低价格，dp[i]表示如果第i天卖出的收益与dp[i-1]相比，最大的收益
<div  id="121-way1"> </div>
```

```cpp
/******************************* way1 ****************************************/
//执行用时 :16 ms, 在所有 C++ 提交中击败了29.11% 的用户
//内存消耗 :14.5 MB, 在所有 C++ 提交中击败了5.14%的用户
class Solution {
public:
    int maxProfit(vector<int>& prices) {
        int size = prices.size();
        if(size<2) return 0;
        int low_price=prices[0];    //在前i天最低价格
        int dp[size+1];     //表示如果第i天卖出的收益，与dp[i-1]相比，最大的收益
        dp[0]=0;dp[1]=0;
        for(int i=2;i<=size;i++){   //prices[i-1] 代表第i天的价格
            dp[i]=max(dp[i-1],prices[i-1]-low_price);
            low_price = low_price>prices[i-1]? prices[i-1] :low_price;
        }
        return dp[size];
    }
};
```
**198.打家劫舍**<div  id="198-打家劫舍"> </div>

```text	
你是一个专业的小偷，计划偷窃沿街的房屋。每间房内都藏有一定的现金，影响你偷窃的唯一制约因素就是相邻的房屋装有相互连通的防盗系统，如果两间相邻的房屋在同一晚上被小偷闯入，系统会自动报警。给定一个代表每个房屋存放金额的非负整数数组，计算你在不触动警报装置的情况下，能够偷窃到的最高金额。
　　示例 1:
　　　输入: [1,2,3,1]
　　　输出: 4
　　　解释: 偷窃 1 号房屋 (金额 = 1) ，然后偷窃 3 号房屋 (金额 = 3)。偷窃到的最高金额 = 1 + 3 = 4 。

[way1](#198-way1): 动态规划，F[i]表示如果有i个房屋，可以得到的最大收入，F[i] = max(F[i-1],F[i-2]+nums[i-1]);

<div  id="198-way1"> </div>
```

```cpp
/******************************* way1 ****************************************/
//执行用时 :4 ms, 在所有 C++ 提交中击败了66.42% 的用户
//内存消耗 :9.2 MB, 在所有 C++ 提交中击败了5.18%的用户
class Solution {
public:
    int rob(vector<int>& nums) {
        int size = nums.size();
        if(size==0) return 0;
		if(size==1)return nums[0];
        //F[i]表示如果有i个房屋，可以得到的最大收入
        int F[size+1];          //序号从1开始存，
        F[0]=0;
        F[1]=nums[0];
		
        for(int i=2;i<=size;i++){
            F[i] = max(F[i-1],F[i-2]+nums[i-1]);
        }
        return F[size];
    }
};
```
**213.打家劫舍II**(medium)<div  id="213-打家劫舍II"> </div>

```text	
　　你是一个专业的小偷，计划偷窃沿街的房屋，每间房内都藏有一定的现金。这个地方所有的房屋都围成一圈，这意味着第一个房屋和最后一个房屋是紧挨着的。同时，相邻的房屋装有相互连通的防盗系统，如果两间相邻的房屋在同一晚上被小偷闯入，系统会自动报警。
　　给定一个代表每个房屋存放金额的非负整数数组，计算你在不触动警报装置的情况下，能够偷窃到的最高金额。
　　示例 1:
　　　输入: [2,3,2]
　　　输出: 3
　　　解释: 你不能先偷窃 1 号房屋（金额 = 2），然后偷窃 3 号房屋（金额 = 2）, 因为他们是相邻的。
　　注意：
　　　其实与I相比，就需要考虑三种情况，
　　　1). 1号被偷+最后1家没偷
　　　2). 1号没偷+最后1家被偷
　　　3). 1号没偷+最后1家也没偷
[way1](#213-way1): 上面其实只用考虑1和2，第三种被包括进去了，所以用两遍打家劫舍I的方法就行。
[way2](#213-way2)：跟上面思路一样，在一个for循环里，计算两个dp数组，一个算nums[0:n-1],一个算nums[1:n]
```
<div  id="213-way1"> </div>

```cpp
/******************************* way1 ****************************************/
class Solution {
public:
    int rob(vector<int>& nums) {
        int size = nums.size();
        if(size==0) return 0;
        if(size==1)return nums[0];
        int i= rob_helper(vector<int>(nums.begin(), nums.end() - 1) );
        int j= rob_helper(vector<int>(nums.begin()+1, nums.end()));
        return i>j? i:j;
    }
    int rob_helper(vector<int> nums) {
        int size = nums.size();
        if(size==0) return 0;
        if(size==1)return nums[0];
        //F[i]表示如果有i个房屋，可以得到的最大收入
        int F[size+1];          //序号从1开始存，
        F[0]=0;
        F[1]=nums[0];
        
        for(int i=2;i<=size;i++){
            F[i] = max(F[i-1],F[i-2]+nums[i-1]);
        }
        return F[size];
    }
};
```
<div  id="213-way2"> </div>

```cpp
/******************************* way2 ****************************************/
class Solution {
public:
    int rob(vector<int>& nums) {
        if(nums.size()==1)
            return nums[0];
        int n=nums.size();
        vector<int>a(n+1),b(n+1);
        for(int i=2;i<n+1;i++){
            a[i]=max(a[i-1],a[i-2]+nums[i-2]);
            b[i]=max(b[i-1],b[i-2]+nums[i-1]);
        }
        return max(a[n],b[n]);
    }
};
```

## 区域动规
**62. 不同路径**(easy)<div  id="62-不同路径"> </div>

```text	
　　一个机器人位于一个 m x n 网格的左上角 （起始点在下图中标记为“Start” ）。
　　机器人每次只能向下或者向右移动一步。机器人试图达到网格的右下角（在下图中标记为“Finish”）。
　　问总共有多少条不同的路径？
```

```cpp
/******************************* way1 ****************************************/
//执行用时 :0 ms, 在所有 C++ 提交中击败了100.00% 的用户
//内存消耗 :7.5 MB, 在所有 C++ 提交中击败了100.00%的用户
class Solution {
public:
    int uniquePaths(int m, int n) {
        double F[n][m] ;//n行m列
        
        //二位动态规划要从左到右，从上到下
        for(int i=0;i<n;i++){       //行
            for(int j=0;j<m;j++){   //列
                if(i==0 || j==0){   //第一行与第一列都是1
                    F[i][j] = 1;
                    continue;
                    }
                F[i][j]=F[i][j-1]+F[i-1][j];
            }        
        }
        return (int)F[n-1][m-1];
    }
};
```
