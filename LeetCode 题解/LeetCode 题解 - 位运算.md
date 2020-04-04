# 位运算
<!-- GFM-TOC -->
[面试题65. 不用加减乘除做加法](#面试题65-不用加减乘除做加法 )
<!-- GFM-TOC -->

[ ](# )
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


