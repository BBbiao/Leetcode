# 动态规划

动态规划一般可分为线性动规，区域动规，树形动规，背包动规四类。
举例\:
+ 线性动规
　　[最大子序和](# 53\. 最大子序和)
* [1. 用栈实现队列](#1-用栈实现队列)
+ 区域动规
+ 树形动规
+ 背包问题

**1.线性动规**
<div  id="53\. 最大子序和"> </div>53\. 最大子序和
	给定一个整数数组 nums ，找到一个具有最大和的连续子数组（子数组最少包含一个元
	素），返回其最大和。

	示例:
		输入: [-2,1,-3,4,-1,2,1,-5,4],
		输出: 6
		解释: 连续子数组 [4,-1,2,1] 的和最大，为 6。

	注意：贪心与动态规划的方向是反的。

	way1: 先遍历一遍确认存在正数，若不存在输出最大的负数，如存在，则用一个tmp存和，
		如果tmp小于0，说明这个子列不能提供一点帮助，那么让他等于0，然后用一个max来
		存中间最大的和。
	way2：分治。先求两边最大的，再求一个这两边跨中线的最大子列，看看三个数哪个大。然后逐渐往上层走。
	way3：贪心
	way4：动态规划，dp[i]表示nums中以nums[i]结尾的最大子序和，
		dp[i] = max(dp[i - 1] + nums[i], nums[i]);	子问题公式

	/******************************* way1 ****************************************/
	//执行用时 :8 ms, 在所有 C++ 提交中击败了82.01% 的用户
	//内存消耗 :14.7 MB, 在所有 C++ 提交中击败了5.10%的用户
	```class Solution {
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
	};```

	/******************************* way2 ****************************************/
	```
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
	};```
	/******************************* way3 ****************************************/
	//执行用时 :12 ms, 在所有 C++ 提交中击败了47.25% 的用户
	//内存消耗 :14.6 MB, 在所有 C++ 提交中击败了5.10%的用户
	```
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
	};```
	/******************************* way4 ****************************************/
	```
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
	};```
	96\.不同的二叉搜索树
	题目：
		给定一个整数 n，求以 1 ... n 为节点组成的二叉搜索树有多少种？
	示例:
		输入: 3
		输出: 5
		解释:给定 n = 3, 一共有 5 种不同结构的二叉搜索树:
		   1         3     3      2      1
			\       /     /      / \      \
			 3     2     1      1   3      2
			/     /       \                 \
		   2     1         2                 3
	注意：
		
	way1: 
		动态规划问题，1,2...i...n,我们可以看做以数字i为根，然后前i-1个为左子树，后n-i个为右子树。
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
	way2：
		上面G(n)函数的值被称为 卡塔兰数 C(n)​。卡塔兰数更便于计算的定义如下:
				C(0)=1,C(n+1)=2(2n+1)*C(n)/(n+2)
		

