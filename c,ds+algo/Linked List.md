<details>
<summary>目录</summary>
	
- 结点定义  
- 哑结点  
- 结点插入  
- 结点删除  
- 结点移动  
- 结点查找
- 链尾/链表长度（结点个数）  
- **递归回溯模拟向后指针**  
- 链表逆序（翻转）  
- 链表两两翻转  
</details>

***
结点定义
```c
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     struct ListNode *next;
 * };
 */
struct ListNode {
    int val;
    struct ListNode *next;
};
```
***
哑结点  
1.方便对非空链表和空链表统一处理）；  
2.方便对涉及到第一个结点的操作等特殊情况进行处理，如删除第一个结点等。
```c
struct ListNode *dummyHead=(struct ListNode *)malloc(sizeof(struct ListNode));dummyHead->next=head;
...
head=dummyHead->next;            //head可能不是第一个结点了，所以要重新赋值
free(dummyHead);dummyHead=NULL;  //释放
return head;
```
***
结点插入
![](https://github.com/anneszcn/pkb/blob/master/data%20structure/pic/insert.png)  
``` c
//将q插到p之后
q->next=p->next;
p->next=q;
```
***
结点删除
``` c
//将p之后结点删除
q=p->next;
p->next=q->next;
free(q);
```
***
结点移动  
- 前移  
pre_p p...pre_q q，将q移到p之前（实际是移到p前驱之后，也是后移），当p、q重合时也适用  
```c
pre_q->next=q->next;
q->next=pre_p->next;
pre_p->next=q;
```
- 后移  
pre_q q... p，将q移动p之后  
    1. solution 1  
  先删除q，再插入到p后，当p,q重合时，结点丢失，内存泄露；通过加判断条件（if (p != q) ）来解决  
     ```c
     pre_q->next=q->next;
     q->next=p->next;
     p->next=q;
     当p、q重合时，该结点丢失
     ```
    2. solution 2  
  当p,q重合时也能正确处理
```c  
tmp=p->next;        /* 保存 */
p->next=q;          /* 先连 */
pre->next=q->next;  /* 删除 */
q->next=tmp;        /* 恢复 */
```
***
结点查找
```c
/*
    4种情况：
                k        返回
    1.空链表              NULL(=p=head)
    2.非空链表  <1        NULL
    3.非空链表  >n         p(=NULL)
    4.非空链表 1<=k<=n     p
*/
struct ListNode *FindKth(struct ListNode *head,int k)
{   
    /* 
       改进点  
     1.减少k判断次数；
     2.省去变量i
    */
    struct ListNode *p=(k<1)?NULL:head;

    while (p && k>1) {
        p=p->next;
        k--;
    }
    
    return p;
    
    /*
    struct ListNode *p=head;
   
    int i=1;
    while (p && i<k) {
        p=p->next;
        i++;
    }
    
    #if 0
    int i=k;
    while (p && i>1) {
        p=p->next;
        i--;
    }
    #endif
    
    return (k<1)?NULL:p; 
    */
}
```
***
链尾/链表长度（结点个数）
```c
struct ListNode *dummyHead=(struct ListNode *)malloc(sizeof(struct ListNode)),head,tail;
head=dummyHead->next;
tail=head;
int len=0;

while (tail->next) {
    len++;
    tail=tail->next;
}
递归
struct ListNode* GetTail(struct ListNode *head,int *len)
{
    if (head == NULL) {
	*len=0;
	return NULL;
    }
    if (head->next == NULL) {
	*len=1;
	return head;
    }
	
    int tmp;
    struct ListNode *res=GetTail(head->next,&tmp);
    *len=tmp+1;
	
    return res;
}
```
<details>
<summary>递归回溯模拟向后指针</summary>
	
```c
int len=0,count=0,k;
struct ListNode *tail=NULL,Kth=NULL,Kth2=NULL; //Kth2:Kth from the end

void linkedlist(struct ListNode *head)
{
    if (head == NULL) {
        len=0;
	tail=NULL;
	
	Kth=NULL;
	Kth2=NULL;
    }
    
    count++;
    if (count == k) Kth=head; //当前结点为第k个
    
    linkedlist(head->next);
    
    static int flag;
    if (!flag++) {  //此时，当前结点为尾结点；仅执行一次，将链表参数保存起来
        len=count;
	tail=head;
    }
    /*
        (1 <= k && k <= len)：k值有效；
	(count-- == len+1-k)：当前结点为倒数第k个；自减--退后一结点
    */
    if ((1 <= k && k <= len) && (count-- == len+1-k)) Kth2=head;
}
```
</details>

***
链表逆序（翻转）
```c
struct ListNode* reverse(struct ListNode* head)
{
    struct ListNode *pre=NULL;
    struct ListNode *curr=head;
	
    while (curr) {
	struct ListNode *next=curr->next;
	curr->next=pre;
	pre=curr;
	curr=next;
    }
	
    return pre;
}

递归
struct ListNode* reverse_recurse(struct ListNode *head) {
    if (head == NULL || head->next == NULL) return head;
    
    struct ListNode *last = reverse(head->next);
    head->next->next = head;
    head->next = NULL;
    return last;
}
仅翻转前n个结点（当n>链表长度时会出错）
struct ListNode* reverseN2(struct ListNode *head, int n,struct ListNode **ppsuccessor) {
    if (head == NULL) return head;
    if (n < 1) return NULL;
    //if (n > 链表长度) return NULL; 链表长度未知，所以，遗漏了这种情况
	
    if (n == 1) { 
        *ppsuccessor = head->next;
        return head;
    }
    
    struct ListNode *temp=NULL,*last = reverseN2(head->next, n - 1,&temp);
    head->next->next=head;
    head->next=temp;
    *ppsuccessor=temp;
    return last;
}
```
***
两两翻转
```c
struct ListNode* swapPairs(struct ListNode* head) {
    struct ListNode **pp = &head, *a, *b;
	
    while ( (a = *pp) && (b = a->next)) {
        a->next = b->next;
        b->next = a;
        *pp = b;
        pp = &(a->next);
    }
    return head;
}
```
***

