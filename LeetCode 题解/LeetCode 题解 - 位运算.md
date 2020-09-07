# 位运算  
<!-- GFM-TOC -->
[201. 数字范围按位与（medium）](#201-数字范围按位与)   
[461. 汉明距离(easy)](#461-汉明距离)   
[面试题65. 不用加减乘除做加法（easy）](#面试题65-不用加减乘除做加法 )    

<!-- GFM-TOC -->
其中重要方法：Brian Kernighan算法(布赖恩·克尼根算法)，201题有介绍。


<div id="面试题65-不用加减乘除做加法"></div>  
**面试题65-不用加减乘除做加法**（easy）  
　　　写一个函数，求两个整数之和，要求在函数体内不得使用   “+”、“-”、“*”、“/” 四则运算符号。  
　思路：  
　　　因为异或(^)可以进行无进位的加运算，进位可以用与运算(&)来然后左移一位操作，注意左移要用无符号类型转换。  

```cpp
class Solution {
public:
    int add(int a, int b) {
        int carry=a&b;
        int sum = a^b;
        while(carry!=0){
            int tmp =sum;
            sum = sum^((unsigned int)carry<<1);
            carry = tmp & ((unsigned int)carry<<1);
        }
        return sum;
    }
};
```
<div id="201-数字范围按位与"></div>   
**201-数字范围按位与**（medium）[题目](https://leetcode-cn.com/problems/bitwise-and-of-numbers-range/)   
　　　给定范围 [m, n]，其中 0 <= m <= n <= 2147483647，返回此范围内所有数字的按位与（包含 m, n 两端点）。  
　示例1:   
　　输入: [5,7]  
　　输出: 4  
　思路1：移位  
　　　　直接算的话超时了。从左向右考虑，只考虑m和n这两个数的公共前缀即可，也就是  
　　　　　m为“00001001”  
　　　　　n为"00001100"  
　　　　那他们的公共前缀为“00001”，结果也为这个，只需将m和n一起向右左移，直到m==n时，得到结果，再将m值向左移回原来的位置得到最终结果。
```
class Solution {
public:
    int rangeBitwiseAnd(int m, int n) {
        int shift=0;
        if(n==m) return m;
        while(n>m){
            n=n>>1;
            m=m>>1;
            shift++;
        }
        return m<<shift;
    }
};
``` 
　思路2:Brian Kernighan 算法  
　　Brian Kernighan 算法的关键在于我们每次对 n 和 n−1之间进行按位与运算后，n中最右边的1会被抹去变成 0。  
　　　例如：　　n=12    "00001100"  
　　　　　　　　n-1=11  "00001011"  
　　　　　　　　n & n-1 "00001000"  
　　基于上述技巧，我们可以用它来计算两个二进制字符串的公共前缀。
　　其思想是，n一直做（n&n-1）的操作，直到n<=m时停止，此时非公共的前缀已经消失，最后的n就是最后结果。  
```
class Solution {
public:
    int rangeBitwiseAnd(int m, int n) {
        while(n>m) n=n&(n-1);
        return n;
    }
};
```
　思路3：从右向左，因为m与n的差大于1，那么最左边的那个1，一定被与掉了；同理，m与n的差大于3，那么最左边的两个1一定被与掉了。基于此，我们计算m与n的差值，因为要进行log运算，所以加1更方便。求出shift，再算出(m&n)的结果，先左移再右移得到最终结果。  
```
class Solution {
public:
    int rangeBitwiseAnd(int m, int n) {
        if(m==0 && n==2147483647) return 0; //因为n是INT_MAX 不能在下一步加1，所以拎出来
        int shift = static_cast<int> (ceil( log(n-m+1)/log(2) ));
        m = (m&n)>>shift<<shift;
        return  m;
    }
};
```
<div id=" 461-汉明距离"></div>
 **461. 汉明距离**(easy)  [题目](https://leetcode-cn.com/problems/hamming-distance/)  
　题目：两个整数之间的汉明距离指的是这两个数字对应二进制位不同的位置的数目。给出两个整数 x 和 y，计算它们之间的汉明距离。  
　注意：  
　　　0 ≤ x, y < 2＾31.  
　示例:  
　　输入: x = 1, y = 4  
　　输出: 2  
　　　1   (0 0 0 1)  
　　　4   (0 1 0 0)  
　思路1：直接异或，没坑  
```
class Solution {
public:
    int hammingDistance(int x, int y) {
        int n = x^y;
        int res=0;
        while(n!=0){
            res+=(n&1);
            n=n>>1;
        }
        return res;
    }
};
```
　思路2：还是异或，但是利用布赖恩·克尼根算法（上面201题提过）可以快速求出异或的结果的1的数量。  
```
class Solution {
public:
    int hammingDistance(int x, int y) {
        int n = x^y;
        int res=0;
        while(n){
            n=n&(n-1);
            res++;
        }
        return res;
    }
};
```