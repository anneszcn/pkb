宏
```c
#define CLEARBUFFER { \
char ch; \
while ((ch = getchar()) != '\n' && ch != EOF); \
} \

#define ADDBINARYSUM(a,b,c) ((a)^(b)^(c))
#define ADDBINARYCARRY(a,b,c) ((a)&(c) | (b)&(c) | (a)&(b))
```

异或  
printf("%d\n",1);      //1  
printf("%d\n",1^2);    //3  
printf("%d\n",1^2^3);  //0  
a^0=a  
a^a=0  

位运算  
x & (x-1)：去掉最右边的1  
x & (-x)或x&(x^(x-1))：仅保留最右边的1，其余置0：x为奇数事结果为1；x为偶数时结果为能整除x的最大2次幂[ref](https://www.cnblogs.com/circlegg/p/7189676.html)  

二进制加法  
加数1：a  
加数2：b  
当前进位：c(carry)  
则：和sum=a^b^c  
    进位c=a&c | b&c | a&b；或a&(b^c) | (b&c)  

字符串输出  
printf("%s\n",p==NULL?"NULL":p);  

动态内存申请  
int *p=(int*)malloc(sizeof(int)*0);  
char *pChr=(char*)malloc(sizeof(char)*0);  
当**长度=0**时，申请成功且非NULL，但内容为垃圾。  

```c
malloc：void *malloc(size_t size)，size -- 内存块的大小，以字节为单位；未初始化
calloc：void *calloc(size_t nitems, size_t size)，nitems -- 要被分配的元素个数，size -- 元素的大小；初始化为0
memset：void *memset(void *str, int c, size_t n)，c -- 要被设置的值。该值以 int 形式传递，但是函数在填充内存块时是使用该值的无符号字符形式。
memcpy：
memmove：
strcpy：
strcat：
```
