# 链表  

链表是空节点，或者有一个值和一个指向下一个链表的指针，因此很多链表问题可以用递归来处理。  
链表有时候在头起位置创建一个哑结点更为有效。例：删除倒数第N个节点。  

```cpp 
        //Definition for singly-linked list.
        struct ListNode {
        　　int val;
        　　ListNode *next;
        　　ListNode(int x) : val(x), next(NULL) {}
        };
```

<!-- GFM-TOC -->
[19. 删除链表的倒数第N个节点(easy)](#19-删除链表的倒数第N个节点)   
[21. 合并两个有序链表(easy)](#21-合并两个有序链表)  
[24. 两两交换链表中的节点(medium)](#24-两两交换链表中的节点)  
[83-删除有序链表重复节点(easy)](#83-删除有序链表重复节点)  
[160-相交链表(easy)](#160-相交链表)  
[206-链表翻转(easy)](#206-链表翻转)  
[234-回文链表(easy)](#234-回文链表)  
[445-两数相加 II(medium)](#445-两数相加 II)  
<!-- GFM-TOC -->

<div id="19-删除链表的倒数第N个节点"></div>  
**19-删除链表的倒数第N个节点**(easy)  

　　题目：  
　　　给定一个链表，删除链表的倒数第 n 个节点，并且返回链表的头结点。  
　　示例：  
　　　给定一个链表: 1->2->3->4->5, 和 n = 2.  
　　　当删除了倒数第二个节点后，链表变为 1->2->3->5.  
　　注意：  
　　　注意两种测试用例，  
　　　1.n=0；  
　　　2.删除头结点的例子，双指针法需要判断P不要越界。  
	
[way1](#19-way1): 递归，创建一个全局变量记录递归次数，等于n的时候，直接返回head->next  
[way2](#19-way2)：双指针法，创建一个p和q都是head的副本，然后让q指向head的下n+1个，然后循环p加一个  q加一个，直到q为NULL，那么此时p的下一个就是需要删除的点。  

<div id="19-way1"></div>

```cpp
/********************************* way1 ********************************************/
//执行用时 :4 ms, 在所有 C++ 提交中击败了91.42% 的用户
//内存消耗 :12.8 MB, 在所有 C++ 提交中击败了5.28%的用户
class Solution {
public:
    int i=0;
    ListNode* removeNthFromEnd(ListNode* head, int n) {
        if(!head ) {
            return NULL;
        }
        head->next = removeNthFromEnd(head->next,n) ;
        i++;
        if(i==n) return head->next;			//删除操作
        return head;
    }
};
```

<div id="-way2"></div>

```cpp
/********************************* way2 ********************************************/
//执行用时 :4 ms, 在所有 C++ 提交中击败了91.42% 的用户
//内存消耗 :12.8 MB, 在所有 C++ 提交中击败了5.28%的用户
class Solution {
public:
    int i=0;
    ListNode* removeNthFromEnd(ListNode* head, int n) {
        if(!head )  return NULL;
        ListNode* p  =head;
        ListNode* q  =head;

        for( int i = 0 ; i < n+1  ; i ++ ){
            q = q->next;
            if(!q) return head->next;		//双指针法需要判断P不要越界。
        }
        while(q)
        {
            p=p->next;
            q=q->next;
        }
        p->next=p->next->next;				//删除操作
        return head;
    }
};
```

<div id="21-合并两个有序链表"></div>  
**21-合并两个有序链表**(easy)  

　　题目：  
　　　将两个有序链表合并为一个新的有序链表并返回。新链表是通过拼接给定的两个链表的所有节点组成的。   
　　示例：  
　　　输入：1->2->4, 1->3->4  
　　　输出：1->1->2->3->4->4  
　　注意：  
　　　1.考虑简单的情况，就是两个链表一开始就分别为空的情况，可以直接输出。  
　　　2.链表的处理，大多数都可以用递归处理。  

[way1](#21-way1): 递归  
[way2](#21-way2)：普通的循环while(l1!=nullptr && l2!=nullptr)，直到有一个链表的next位置为空  
<div id="-way1"></div>

```cpp
/********************************* way1 ********************************************/
//执行用时 :8 ms, 在所有 C++ 提交中击败了81.02% 的用户
//内存消耗 :17 MB, 在所有 C++ 提交中击败了5.22%的用户

class Solution {
public:
    ListNode* mergeTwoLists(ListNode* l1, ListNode* l2) {
		if(l1==nullptr) return l2;
        if(l2==nullptr) return l1;
        
        if(l1->val < l2->val) {
            l1->next=mergeTwoLists(l1->next,l2);	//将当前l1的值入队，并用l1->next递归
            return l1;
        }
        else 
            l2->next=mergeTwoLists(l1,l2->next);
            return l2;
    }
};
```
<div id="-way2"></div>

```cpp
/********************************* way2 *********************************************/
//执行用时 :12 ms, 在所有 C++ 提交中击败了40.86% 的用户
//内存消耗 :16.8 MB, 在所有 C++ 提交中击败了5.22%的用户

class Solution {
public:
    ListNode* mergeTwoLists(ListNode* l1, ListNode* l2) {
		
		ListNode* res = new ListNode(-1);	//创建一个链表存结果
        ListNode* res_start = res;			//将这个结果链表的表头存下，
											//因为上面那个链表的指针一直在变。
        if(l1==nullptr)return l2;
        if(l2==nullptr)return l1;
		
		//注意循环条件是&&，必须要l1与l2都不为空时，当有一个为空，就不需要判断了。
		while(l1!=nullptr && l2!=nullptr){ 

			if(l1->val <l2->val){			
				res->next =l1 ;
                l1=l1->next;
			}
			else {
				res->next =l2 ;
                l2=l2->next;
			}
            res = res->next;		
		}
        res->next = l1 == nullptr ? l2 : l1;//如果有一个为空，那么直接指向另一个不为空的链

        return res_start->next ;
    }
};
```

<div id="24-两两交换链表中的节点"></div>  
**24-两两交换链表中的节点(medium)**  

　　题目：  
　　　给定一个链表，两两交换其中相邻的节点，并返回交换后的链表。  
　　　你不能只是单纯的改变节点内部的值，而是需要实际的进行节点交换。  
　　示例:  
　　　给定 1->2->3->4, 你应该返回 2->1->4->3.  

[way1](#24-way1): 正常思维，3个指针(p1:head，p2:head->next,p3：head->next->next),做一个死循环，循环内三种情况（处理完了所有节点，只剩一个，还剩2个以上），分别处理这三种情况。  
[way2](#24-way2)：递归，注意因为是两两交换，所以递归就要隔一个递归一次，返回值就是这两个节点的next节点。  

<div id="24-way1"></div>

```cpp
/********************************* way1 ********************************************/
//执行用时 :8 ms, 在所有 C++ 提交中击败了26.34% 的用户
//内存消耗 :9.9 MB, 在所有 C++ 提交中击败了5.60%的用户

class Solution {
public:
    ListNode* swapPairs(ListNode* head) {
        if(!head || !head->next) return head;
        ListNode *p1 =head;
        ListNode *p2 =head->next;
        ListNode *p3 =head->next->next;

        ListNode *start = p2;  			//链表的头结点
        while(1)
        {
            p2->next =p1;
            p1->next =p3;
            if(!p3 || !p3->next){		//没有节点了和 只剩最后一个
                p2->next =p1;
                p1->next =p3;
                return start;
            }
            else{                       //还有两个及以上
                p2->next =p1;
                p1->next =p3->next;
                p1=p3;
                p2=p3->next;
                p3=p3->next->next;
            }
        }
        return start;
    }
};
```
<div id="24-way2"></div>

```cpp
/********************************* way2 *********************************************/
class Solution {
public:
    ListNode* swapPairs(ListNode* head) {
        if(!head || !head->next) return head;

        ListNode *next=head->next;              //先存起来，不能先交换
        head->next= swapPairs(next->next);      //递归是隔一个递归一次
        next->next = head;
        return next;
    }
};
```
<div id="83-删除有序链表重复节点"></div>  
**83-删除有序链表重复节点**(easy)  

　　示例：  
　　　Given 1->1->2, return 1->2.  
　　　Given 1->1->2->3->3, return 1->2->3.  

way1: 递归  

```cpp
//执行用时 :12 ms, 在所有 C++ 提交中击败了75.18% 的用户
//内存消耗 :13.4 MB, 在所有 C++ 提交中击败了5.04%的用户
class Solution{
public:
    ListNode* deleteDuplicates(ListNode* head) {
        if (!head || !head->next) return head;
        head->next = deleteDuplicates(head->next);
        return head->val == head->next->val ? head->next : head;
    	}
};
```

<div id="160-相交链表"></div>  
**160-相交链表**(easy)  

　　题目：  
　　　链表是空节点，或者有一个值和一个指向下一个链表的指针，因此很多链表问题可以用递归来处理。  
　　　编写一个程序，找到两个单链表相交的起始节点。  
　　示例 1：  
　　　输入： listA = [4,1,8,4,5], listB = [5,0,1,8,4,5]  
　　　输出： 8	  
　　注意：  
　　　如果两个链表没有交点，返回 null.  
　　　在返回结果后，两个链表仍须保持原有的结构。   
　　　可假定整个链表结构中没有循环。  
　　　程序尽量满足 O(n) 时间复杂度，且仅用 o(1) 内存。  

[way1](#160-way1): 暴力法，每遍历链A中的一个节点，就遍历一遍链B，复杂度O(mn)，空间O(1)  
[way2](#160-way2)：双指针法，  
	设 A 的长度为 a + c，B 的长度为 b + c，其中 c 为尾部公共部分长度，可知 a + c + b = b + c + a。也就是A走完直接从B开头走，B走完从A开头走，如果相交就会同时走到那个点，停止循环。如果不相交，就会同时走到NULL，也相等，退出循环。
[way3](#160-way3): 哈希表法  
	使用一个hash set 遍历一个链表，set中存放其所有指针， 遍历另一个链表，去set中找相同指针  
	时间复杂度O(m + n) 空间复杂度O(m) 或 O(n)  
<div id="160-way1"></div>

```cpp
/********************************* way1 ********************************************/
//执行用时 :752 ms, 在所有 C++ 提交中击败了5.54% 的用户
//内存消耗 :16.9 MB, 在所有 C++ 提交中击败了18.84%的用户

class Solution {
public:
    ListNode *getIntersectionNode(ListNode *headA, ListNode *headB) {
		ListNode *headB_start = headB;
        if(!headA || !headB) return NULL;
        while(headA!=NULL){
			headB = headB_start;
			while(headB!=NULL)
			{
				if(headA==headB) return headA;
				else {
					headB=headB->next;
				}	
			}
            headA=headA->next;
		}
        return NULL;
    }	
};
```
<div id="160-way2"></div>

```cpp
/********************************* way2 ********************************************/
//执行用时 :92 ms, 在所有 C++ 提交中击败了22.21% 的用户
//内存消耗 :17 MB, 在所有 C++ 提交中击败了18.44%的用户

class Solution {
public:
    
    ListNode *getIntersectionNode(ListNode *headA, ListNode *headB) {

        if(headA == nullptr || headB == nullptr) 
            return nullptr;

        ListNode* cur_a = headA;
        ListNode* cur_b = headB;

        while(cur_a != cur_b)
        {
            cur_a = (cur_a == nullptr ? headB : cur_a->next);
            cur_b = (cur_b == nullptr ? headA : cur_b->next);
        }

        return cur_a;
    }
};
```

<div id="206-链表翻转"></div>  
**206-链表翻转**(easy)  

　　题目：  
　　　反转一个单链表。  
　　示例:  
　　　输入: 1->2->3->4->5->NULL  
　　　输出: 5->4->3->2->1->NULL  
　　注意：  
　　　1.要想着链表是空的情况。  
　　　2.递归有点难想，OneNote上有图解释。  
　　　3.递归的思路注意：  
　　　　想要用递归把1->2->3->null  变成3->2->1->null；  
　　　　　进入递归->  
　　　　　　　　　　　->　　入下一层递归  
　　　　　　　　　　　　　->　　下一个节点是null，可以返回了  
　　　　　　　　　　　　　<-　　递归的回程，  
　　　　　　　　　　　<-  
　　　　　返回程序<-  
　　　　　递归的回程，递归的返回值必须是3这个节点。因为3是新链表的头。  
　　　　　如果返回值变了的话，递归完成后将找不到头，没有办法输出链表。	
　　　4.正常的迭代思路，可以增加一个头结点更加便于翻转。  
	
[way1](#206-way1): 递归
[way2](#206-way2)：普通循环，

<div id="-way1"></div>

```cpp
/********************************* way1 ********************************************/
//执行用时 :8 ms, 在所有 C++ 提交中击败了77.90% 的用户
//内存消耗 :11 MB, 在所有 C++ 提交中击败了5.08%的用户

class Solution {
public:
    ListNode* reverseList(ListNode* head) {
		ListNode* end = new ListNode(0);			//用end存储最后一个节点，也是反转后的第一个节点
		if(head==NULL || head->next == NULL)		
			return head;							//找到head的最后一个节点
		end = reverseList(head->next);				//end存的一直都是最后一个节点。
		
		head->next->next =head;
		head->next=NULL;
		
		return end;	
	}
};
```
<div id="-way2"></div>

```cpp
/********************************* way2 *********************************************/
//执行用时 :4 ms, 在所有 C++ 提交中击败了97.38% 的用户
//内存消耗 :10.1 MB, 在所有 C++ 提交中击败了5.08%的用户

class Solution {
public:
    ListNode* reverseList(ListNode* head) {
		ListNode* temp =new ListNode(0);
		if(head==NULL ||head->next==NULL) return head;
		ListNode* new_list=head;
		ListNode* old_list=head->next;
        head->next=NULL;
     	while(old_list!=NULL)
		{
			temp= old_list->next;	
			old_list->next =new_list;
			new_list=old_list;
			old_list=temp;	
		}	
		return new_list;
	}
};
```

<div id="234-回文链表"></div>  
**234-回文链表**(easy)  

　　题目：  
　　　请判断一个链表是否为回文链表。  
　　示例 1:  
　　　输入: 1->2  
　　　输出: false  
　　示例 2:  
　　　输入: 1->2->2->1  
　　　输出: true  
　　注意：  
　　　用 O(n) 时间复杂度和 O(1) 空间复杂度解决此题  
	
[way1](#234-way1): 用一个Vector存，双指针 时间复杂度O(N+N/2)=O(N),空间复杂度O(N)。  
[way2](#234-way2)：用一个双指针，快慢指针+ 后半部分翻转，就是快指针一次走两个，慢指针一次走一个，快指针到头，慢指针正好在中点，然后将后半部分翻转，然后再对比 
[way3](#234-way3)：快慢指针+前半部分翻转，慢指针一边走一变翻转，快指针到头，直接比较前后相不相同。  
<div id="-way1"></div>

```cpp
/********************************* way1 ********************************************/
//执行用时 :28 ms, 在所有 C++ 提交中击败了43.00% 的用户
//内存消耗 :17.6 MB, 在所有 C++ 提交中击败了5.03%的用户

class Solution {
public:
    bool isPalindrome(ListNode* head) {
        if(!head) return true;
        vector<ListNode*> vec;
        while(head){
            vec.push_back(head);
            head= head->next;
        }
        vector<ListNode*>::iterator left=vec.begin();
        vector<ListNode*>::iterator right=vec.end()-1;
        while(left!=right && left<right){
            if((*left)->val != (*right)->val)return false;
            left++;
            right--;
        }
        return true;
    }
};
```
<div id="234-way2"></div>

```cpp
/********************************* way2 *********************************************/
//执行用时 :28 ms, 在所有 C++ 提交中击败了43.16% 的用户
//内存消耗 :18.4 MB, 在所有 C++ 提交中击败了5.02%的用户
class Solution {
public:
	ListNode* reverseList(ListNode* head) {
		ListNode* end = new ListNode(0);			//用end存储最后一个节点，也是反转后的第一个节点
		if(head==NULL || head->next == NULL)		
			return head;							//找到head的最后一个节点
		end = reverseList(head->next);				//end存的一直都是最后一个节点。
		
		head->next->next =head;
		head->next=NULL;
		
		return end;	
	}
    bool isPalindrome(ListNode* head) {
        if(!head) return true;
        ListNode* slow=head;
		ListNode* fast=head;
		while(fast!=NULL && fast->next!=NULL){		
			//fast为NULL，说明链表为偶数，fast下一个为NULL，链表为奇数
			fast=fast->next->next;					//快的走一步，慢的走两步
			slow=slow->next;
		}
		if(fast)slow =slow->next;						//奇数个，跳过中间的节点 
		ListNode* l2=reverseList(slow);
		while(l2){
			if(l2->val != head->val) return false;
			head = head->next;
			l2 = l2->next;
		}
        return true;
    }
};
```
<div id="234-way3"></div>

```cpp
/********************************* way3 *********************************************/
//执行用时 :36 ms, 在所有 C++ 提交中击败了20.88% 的用户
//内存消耗 :15.9 MB, 在所有 C++ 提交中击败了5.02%的用户
class Solution {
public:
    bool isPalindrome(ListNode* head) {
        if(!head || !head->next)
            return true;
        ListNode *fast = head, *slow = head;
        ListNode *p, *pre = NULL;
        while(fast && fast->next){
            p = slow;
            slow = slow->next;    //快慢遍历
            fast = fast->next->next;

            p->next = pre;  //翻转
            pre = p;
        }
        if(fast)  //奇数个节点时跳过中间节点
            slow = slow->next;

        while(p){       //前半部分和后半部分比较
            if(p->val != slow->val)
                return false;
            p = p->next;
            slow = slow->next;
        }
        return true;
    }
};
```

<div id="445-两数相加 II"></div>  
**445-两数相加 II**(medium)  

　　题目：  
　　　给定两个非空链表来代表两个非负整数。数字最高位位于链表开始位置。它们的每个节点只存储单个数字。将这两数相加会返回一个新的链表。  
　　　你可以假设除了数字 0 之外，这两个数字都不会以零开头。  
　　进阶:  
　　　如果输入链表不能修改该如何处理？换句话说，你不能对列表中的节点进行翻转。  
　　示例:  
　　　输入: (7 -> 2 -> 4 -> 3) + (5 -> 6 -> 4)  
　　　输出: 7 -> 8 -> 0 -> 7   

```cpp
class Solution {
public:
    ListNode* addTwoNumbers(ListNode* l1, ListNode* l2) 
    {
        //因为对于链表来说：先遍历的节点是后计算的，所以要想到栈的用法，先进后出。
        stack<ListNode*> s1;
        stack<ListNode*> s2;
        while(l1!=NULL)
        {
            s1.push(l1);
            l1=l1->next;
        }
        while(l2!=NULL)
        {
            s2.push(l2);
            l2=l2->next;
        }
        int flag=0;//初始化进位符
        stack<int> s3;//存储每一位的加和结果
        while(!s1.empty()||!s2.empty()||flag==1)//这里的flag为1其实是防止遇到[5]和[5]这种情况
        {
            int num=0;//用num来记录每个位置相加的结果
            if(!s1.empty())
            {
                num+=s1.top()->val;
                s1.pop();
            }
            if(!s2.empty())
            {
                num+=s2.top()->val;
                s2.pop();
            }
            num+=flag;
            //判断sum与10的关系，进而o推出有没有进位符号
            if(num>=10)
            {
                num=num%10;
                flag=1;
            }
            else flag=0;
            
            //将最终的加和结果存到栈s3中
            s3.push(num);
        }
        //从s3栈输出结果
        ListNode* pHead=new ListNode(s3.top());
        s3.pop();
        ListNode* work=pHead;
        while(!s3.empty())
        {
            ListNode* temp=new ListNode(s3.top());
            s3.pop();
            work->next=temp;
            work=work->next;       
        }
        return pHead;      
    }
};
```





