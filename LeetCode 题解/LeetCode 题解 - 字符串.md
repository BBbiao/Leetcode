# 字符串  
<!-- GFM-TOC -->
[面试题05. 替换空格(easy)](#面试题05-替换空格)   
[3. 无重复字符的最长子串(medium)](#3-无重复字符的最长子串)  
[28. 实现 strStr()(easy)](#28-实现 strStr())  
[214. 最短回文串(hard)](#214-最短回文串)  
[459. 重复的子字符串(easy)](#459-重复的子字符串)  
[1392. 最长快乐前缀(hard)](#1392-最长快乐前缀)  
<!-- GFM-TOC -->

****
重点题：459！(其中KMP 可解 28,459,1392)
****
[ ](# )
<div id="-way1"></div>  
<div id="-way2"></div>  

<div id="面试题05-替换空格"></div> 

**面试题05-替换空格**(easy)[力扣-剑指offer](#https://leetcode-cn.com/problems/ti-huan-kong-ge-lcof/)  
　题目：  
　　请实现一个函数，把字符串 s 中的每个空格替换成"%20"。  
　示例 1：  
　　输入：s = "We are happy."  
　　输出："We%20are%20happy."  
　思路：这道题只要就是熟悉C++的string操作，要利用C++的特性。

[way1](#面试题05-way1)：熟悉一下操作，使用源字符串进行操作。  
**erase函数**，第一个参数pos(默认0)，第二个是删除几个字符(默认是pos之后所有)。  
**insert函数**，插入一个字符，只能是一个字符，这个操作能返回一个迭代器，指向插入这个字符的前一位置。  
**find函数**，这个返回一个size_t类型，如果找不到，返回string::nops(其值为-1)。  

[way2](#面试题05-way2)：直接在定义一个string

<div id="面试题05-way1"></div> 

```cpp
    string replaceSpace(string s) {
        if(s.size()==0) return s;
        string::iterator it=s.begin();
        int pos= s.find(' ',0) ;
        while(pos!=-1){
            s.erase(pos,1);
            it=s.begin()+pos;
            it=s.insert(it,'%')+1;
            it=s.insert(it,'2')+1;
            it=s.insert(it,'0')+1;
            
            pos = s.find(' ',pos+1);
        }        
        return s;
    }
```

<div id="面试题05-way2"></div>  

```cpp
    string replaceSpace(string s) {
        if(s.size()==0) return s;
        string str;
        for(auto c:s){
            if(c==' ')str+="%20";
            else str+=c;
        }
        return str;
    }
```

<div id="3-无重复字符的最长子串"></div> 

**3-无重复字符的最长子串**[力扣](#https://leetcode-cn.com/problems/longest-substring-without-repeating-characters/)/[力扣-剑指offer](#https://leetcode-cn.com/problems/zui-chang-bu-han-zhong-fu-zi-fu-de-zi-zi-fu-chuan-lcof/solution/)  
　题目：  
　　给定一个字符串，请你找出其中不含有重复字符的 最长子串 的长度。  
　示例:  
　　输入: "abcabcbb"  
　　输出: 3   
　　解释: 因为无重复字符的最长子串是 "abc"，所以其长度为 3。  
　思路：  
[way1](#3-way1)：题目中要求答案必须是`子串`的长度，意味着子串内的字符在原字符串中一定是连续的。因此我们可以将答案看作原字符串的一个滑动窗口，并维护窗口内不能有重复字符，同时更新窗口的最大值。使用左右指针，加上string::find函数辅助。

[way2](#3-way2)：不使用find函数辅助，使用hashmap，key为字符，value为该字符位置，这样直接找到重复的那个字符的位置。(但提交速度没有快0.0)

<div id="3-way1"></div>  

```cpp
    int lengthOfLongestSubstring(string s) {
        int start=0,end=0;
        string str;
        int size = s.size(),res=0;
        for(int i=0;i<size;i++){
            int m=str.find(s[i]);        //使用find来str中重复的那个字符的位置
            if(m==string::npos)str+=s[i];
            else{
                res=res>str.size()?res:str.size();
                str+=s[i];
                str.erase(0,m+1);
            } 
            res=res>str.size()?res:str.size();
        }
        return res;
    }
```

<div id="3-way2"></div>  

```cpp
    int lengthOfLongestSubstring(string s) {
      map<char, int> m;
        int res = 0, start = 0, end = 0;
        while (end < s.size()) {
            if (m.find(s[end]) != m.end()) {
                start = max(start, m[s[end]] + 1);
            }
            m[s[end++]] = end;
            res = max(end - start, res);
        }
        return res;
    }
```

<div id="28-实现 strStr()"></div>    
**28. 实现 strStr()**(easy) [题目](https://leetcode-cn.com/problems/implement-strstr/)    
　题目：  
　　实现 strStr() 函数。给定一个 haystack 字符串和一个 needle 字符串，在 haystack 字符串中找出 needle 字符串出现的第一个位置 (从0开始)。如果不存在，则返回  -1。  
　示例:  
　　输入: haystack = "hello", needle = "ll"  
　　输出: 2  
　思路：KMP（具体看459，思路4）  

```cpp
class Solution {
public:
    int strStr(string haystack, string needle) {
    //KMP
        int m=haystack.size(),n=needle.size();
        vector<int> next(n+1,-1);
        int i=0,j=-1;
        while(i<n){
            if(j==-1 || needle[i]==needle[j]){
                i++;j++;
                next[i]=j;
            }
            else j=next[j];
        }
        i=0;j=0;
        while(i<m && j<n){
            if(j==-1 || haystack[i]==needle[j]){
                i++;j++;
            }
            else j=next[j];
        }
        if(j==n) return i-j;
        return -1;
    }
};
```


<div id="459-重复的子字符串"></div>
**459. 重复的子字符串**(easy) [题目](https://leetcode-cn.com/problems/repeated-substring-pattern/)    
　题目：  
　　给定一个非空的字符串，判断它是否可以由它的一个子串重复多次构成。给定的字符串只含有小写英文字母，并且长度不超过10000。  
　示例:  
　　输入: "abab"  
　　输出: True  
　　解释: 可由子字符串 "ab" 重复两次构成。  
　思路1：直接遍历，使用双指针  
　　使用双指针，left=0，right向右走，使用一个len计录子串长度，如果s[left]==s[right]，len--，两个指针都向右移接着比对。如果在比对时出现s[left]！=s[right]，两个指针要回退，right指针回退为 right=right-left+1（也就是开始遍历比对位置的下一个位置），left直接为0，然后len直接等于right即可。  

```cpp
class Solution {
public:
    bool repeatedSubstringPattern(string s) {
        int size=s.size();
        int left=0,right=1,len=1;   // len表示子串长度
        while(right<size){
            if(len==0)len=right-left;   // 这个是right走到第三个子串以后开始有用
            if(s[left]==s[right]){
                left++;
                right++;
                len--;
            }
            else{
                right=right-left+1;      // 若left为0，right就是直接加1，若left不为0，说明在开始遍历比对，但是出错了，要退回 right开始比对的下一个 的位置。
                left=0;
                len=right;
            }
            if(len>size/2)return false;
        }
        if(len==0)return true;
        return false;
    }
};
```

　思路2：枚举，复杂度O(N2)  
　　直接假设子串长度为i，然后从头到尾遍历s，如果是子串，那么一定有s[j]=s[j-i]  

```
class Solution {
public:
    bool repeatedSubstringPattern(string s) {
        int size=s.size();
        for(int i=1;i*2<=size;i++){ //子串长度
            if(size % i !=0) continue;
            bool flag=true;
            for(int j=i;j<size;j++){
                if(s[j]!=s[j-i]){
                    flag = false;
                    break;
                }
            }
            if(flag)return true;
            
        }
        return false;
    }
};
```

　思路3：双倍字符串【厉害】  
　　如果将两个s连在一起，并移除第一个和最后一个字符，如果得到的长字符串包含s，那么s一定有子串。  

```cpp
class Solution {
public:
    bool repeatedSubstringPattern(string s) {
        // 注意find（s，1）就是从脚标1位置开始遍历，所以就相当于去掉了第一个字符。
        return (s + s).find(s, 1) != s.size();
    }
};
```

思路4：KMP算法
KMP的算法，[B站视频](https://www.bilibili.com/video/BV1jb411V78H?from=search&seid=9922549172647865522)有个动画讲的挺好的，方便理解。[知乎程序讲解](https://www.zhihu.com/question/21923021)。
KMP的优点主要是减少假设的子串与主串比对的次数。
注意：1.KMP 算法虽然有着良好的理论时间复杂度上限，但大部分语言自带的字符串查找函数并不是用 KMP 算法实现的。这是因为在实现 API 时，我们需要在平均时间复杂度和最坏时间复杂度二者之间权衡。普通的暴力匹配算法以及优化的 BM 算法拥有比 KMP 算法更为优秀的平均时间复杂度；
2.在KMP算法中，next数组的意义是，`next[i]=x` 表示 s[0:x]和s[i-x:i)是长度为x的完全相同的前缀和后缀。这也是 KMP 算法最重要的一部分。

```cpp
class Solution {
public:
    bool kmp(const string &pattern){
        int n=pattern.size();
        //求next数组
        vector<int> next(n+1,-1); //注意next数组取得是n+1长度
        int i=0,j=-1;
        while(i<n){
            if(j==-1 || pattern[i]==pattern[j]){
                i++;j++;
                next[i]=j;
            }
            else j=next[j];
        }
        //next[n]代表是整个pattern子串有多长的前后缀，循环节长度为n-next[n]，只要判断n%x==0；
        //例如"abcabcabc" next[n]=6,循环节长度为3
        int x = n - next[n];  
        if (n % x == 0 && x != n)
            return true;
        return false;
    }
    bool repeatedSubstringPattern(string s) {
        return kmp(s);
    }
};
```

下面是KMP的通用程序。不是上题的解题。

```
//KMP的通用程序
//求next数组的过程完全可以看成字符串匹配的过程，即以模式字符串为主字符串和模式字符串。

int nexts[n+1];//n为模式串的长度
void getNext(char * p, int * next)
{
	next[0] = -1;
	int i = 0, j = -1;

	while (i < strlen(p))
	{
		if (j == -1 || p[i] == p[j])  //等于-1时，就想象模式字符串要向右移动了。
		{
			++i;
			++j;
			next[i] = j;
		}	
		else
			j = next[j];
	}
}
int KMP(char * t, char * p) 
{
	int i = 0; 
	int j = 0;

	while (i < strlen(t) && j < strlen(p))
	{
		if (j == -1 || t[i] == p[j]) 
		{
			i++;
           	j++;
		}
	 	else 
           	j = next[j];
    }
    if (j == strlen(p))
       return i - j;
    else 
       return -1;
}
```

<div id="1392-最长快乐前缀"></div>  
**1392. 最长快乐前缀**(hard)  [题目](https://leetcode-cn.com/problems/longest-happy-prefix/)  
　题目：「快乐前缀」是在原字符串中既是 非空 前缀也是后缀（不包括原字符串自身）的字符串。
　　　给你一个字符串 s，请你返回它的 最长快乐前缀。
　　　如果不存在满足题意的前缀，则返回一个空字符串。
　示例：
　　　输入：s = "level"
　　　输出："l"
　　　解释：不包括 s 自己，一共有 4 个前缀（"l", "le", "lev", "leve"）和 4 个后缀（"l", "el", "vel", "evel"）。最长的既是前缀也是后缀的字符串是 "l" 。
  思路：KMP，无脑KMP （KMP算法，看459 思路4）

```
class Solution {
public:
    string longestPrefix(string s) {
    //无脑KMP,直接求next[]就行 
        int n=s.length();
        vector<int> next(n+1,-1);
        int i=0,j=-1;
        while(i<n){
            if(j==-1 || s[i]==s[j]){
                i++;j++;
                next[i]=j;
            }
            else j=next[j];
        }
        return s.substr(0,next[n]);
    }
};
```
<div id="214-最短回文串"></div>  
**214. 最短回文串**(hard)  [力扣](https://leetcode-cn.com/problems/shortest-palindrome/)  
题目：
　　给定一个字符串 s，你可以通过在字符串前面添加字符将其转换为回文串。找到并返回可以用这种方式转换的最短回文串。  
示例:  
　　　输入: "aacecaaa"  
　　　输出: "aaacecaaa"  
思路：
　　使用KMP中的next数组，首先创建一个s的翻转子串，和s进行拼接得到tmp字串，最好中间加一个‘#’，别的字符，因为不希望最长前后缀的长度超过s的长度。

　　　　tmp= s+'@'+ reverse;

　　对tmp子串求next数组。因为s串在前面，所以算s的最长前缀，和reverse的最长后缀，我们减去这个长度，就是我们要在s前面添加的长度。

```cpp
class Solution {
public:
    string shortestPalindrome(string s) {
        //KMP
        string reverse(s.rbegin(),s.rend());
        string tmp = s +'@'+reverse;
        vector<int> next(tmp.length()+1,-1);
        int i=0,j=-1;
        while(i<tmp.length()){
            if(j==-1 || tmp[i]==tmp[j]){
                i++;j++;
                next[i]=j;
            }
            else j=next[j];
        }
        int len=s.length()-next[tmp.length()];
        return reverse.substr(0,len)+s;
    }
};
```

