# 哈希表  
<!-- GFM-TOC -->
[1.两数之和(easy)](#1-两数之和)  
[面试题03.数组中重复的数字(easy)](#面试题03-数组中重复的数字)  
[1160.拼写单词(easy)](#1160-拼写单词)   
<!-- GFM-TOC -->

<div id="1-两数之和"></div>

**1-两数之和**(easy)  

　　给定一个整数数组 nums 和一个目标值 target，请你在该数组中找出和为目标值的那 两个 整数，并返回他们的数组下标。你可以假设每种输入只会对应一个答案。但是，你不能重复利用这个数组中同样的元素。  
　　　示例:  
　　　给定 nums = [2, 7, 11, 15], target = 9  
　　　因为 nums[0] + nums[1] = 2 + 7 = 9  
　　　所以返回 [0, 1]  

[way1](#1-way1): 两遍哈希，nums中的每个数是key，给每个key一个value，就是防止重复利用这个数。  
[way2](#1-way2)：一遍哈希，在给map中增加元素时，一遍查找target-nums[i],是否存在。一般数组遍  历到较后边位置时才会有结果，所以i为较大的元素。  
<div id="1-way1"></div>

```cpp
/******************************* way1 ****************************************/
class Solution {
public:
    vector<int> twoSum(vector<int>& nums, int target) {
        unordered_map<int,int> m;
        int size = nums.size();
        for(int i=0;i<size;i++){
            m.insert(make_pair(nums[i],i));
        }
        for(int i=0;i<size;i++){
			//find这个key值不为空，并且这个key对应的value不等于i，要不就重复使用了。
            if( m.find(target-nums[i])!=m.end() &&  m[target-nums[i]]!=i)
                return {i,m[target-nums[i]]};
        }
        return {};
    }
```

<div id="1-way2"></div>

```cpp
/******************************* way2 ****************************************/
class Solution {
public:
    vector<int> twoSum(vector<int>& nums, int target) {
        unordered_map<int,int> m;
        for(int i=0;i<nums.size();i++)
        {
            if(m.find(target-nums[i]) != m.end())//m中存在对应的键值
            //因为i为较大的元素，所以i在后面
                return {m[target-nums[i]],i};
            m[nums[i]]=i;  //向map中添加元素
        }

        return {};
    }
};
```
<div id="面试题03-数组中重复的数字"></div>

**面试题03-数组中重复的数字**(easy)[力扣](#https://leetcode-cn.com/problems/shu-zu-zhong-zhong-fu-de-shu-zi-lcof/)  

　　找出数组中重复的数字。  
　　在一个长度为 n 的数组 nums 里的所有数字都在 0～n-1 的范围内。数组中某些数字是重复的，但不知道有几个数字重复了，也不知道每个数字重复了几次。请找出数组中任意一个重复的数字。2 <= n <= 100000  

　示例 1：  
　　输入：[2, 3, 1, 0, 2, 5, 3]  
　　输出：2 或 3   
　思路：直接unordered_map，没出现过就存入，出现过直接输出

```cpp
class Solution {
public:
    int findRepeatNumber(vector<int>& nums) {
        unordered_map<int,int> m;
        int size=nums.size();
        for(int i=0;i<size;i++){
            if(m.find(nums[i]) ==m.end())
                m[nums[i]]++;
            else return nums[i];
        }
        return 0;
    }
};
```


<div id="1160-拼写单词"></div>

**1160-拼写单词**(easy)  

　　给你一份『词汇表』（字符串数组） words 和一张『字母表』（字符串） chars。  
　　假如你可以用 chars 中的『字母』（字符）拼写出 words 中的某个『单词』（字符串），那么我们就认为你掌握了这个单词。  
　　注意：每次拼写时，chars 中的每个字母都只能用一次。  
　　　返回词汇表 words 中你掌握的所有单词的 长度之和。  
　　示例 1：  
　　　输入：words = ["cat","bt","hat","tree"], chars = "atach"  
　　　输出：6  
　　　解释：可以形成字符串 "cat" 和 "hat"，所以答案是 3 + 3 = 6。  

way1: 用一个哈希表存chars的字母表每个出现次数，然后遍历每个单词时也用一个哈希表存，对比一下。。  

```cpp
class Solution {
public:
    int countCharacters(vector<string>& words, string chars) {
        unordered_map<char, int> chars_cnt;
        for (char c : chars)
            ++chars_cnt[c];
        int ans = 0;
        for (string word : words) {
            unordered_map<char, int> word_cnt;
            for (char c : word)
                ++word_cnt[c];
            bool is_ans = true;
            for (char c : word)
                if (chars_cnt[c] < word_cnt[c]) {
                    is_ans = false;
                    break;
                }
            if (is_ans)
                ans += word.size();
        }
        return ans;
    }
};
```

