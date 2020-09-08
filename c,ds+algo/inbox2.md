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

```c
字符串  
printf("%s\n",p==NULL?"NULL":p);           //当字符串为NULL时，直接输出会报错

char s[]="Hello kitty!";
printf("%s\n",s);
printf("%d\n",strlen(s));                  //字符串长度为12
printf("%d\n",sizeof(s)/sizeof(s[0]));     //字符数组长度为13，最后一个用于存储字符串结束符'\0'，但不计入字符串的长度
s[0]='X';                                  //字符数组可以修改
printf("%s\n",s);
	
char *p="Hello kitty!";                    //字符串常量
printf("%s\n",p);
printf("%d\n",strlen(p));
p[0]='X';                                  //报错！字符串常量不能修改
printf("%s\n",p);
```

动态内存申请  
int *p=(int*)malloc(sizeof(int)*0);  
char *pChr=(char*)malloc(sizeof(char)*0);  
当**长度=0**时，申请成功且非NULL，但内容为垃圾。  
动态内存申请和释放必须对应（否则会内存泄露），而且**只能释放(free)一次**，否则会报错。  

```c
malloc：void *malloc(size_t size)，size -- 内存块的大小，以字节为单位；未初始化
calloc：void *calloc(size_t nitems, size_t size)，nitems -- 要被分配的元素个数，size -- 元素的大小；**初始化为0**
realloc:void *realloc(void *ptr, size_t size)，ptr指向此前malloc、calloc 或 realloc 分配内存块，如果为NULL，此时等同malloc、calloc；size -- 内存块的新的大小，以字节为单位。如果大小为 0，且 ptr 指向一个已存在的内存块，此时等同free
memset：void *memset(void *str, int c, size_t n)，c -- 要被设置的值。该值以 int 形式传递，但是函数在填充内存块时是使用该值的无符号字符形式。
以下均为内存拷贝，当区域重叠时，第一个结果可能不正确，第二个正确；但，目前，很多编译器做了优化，两者结果相同均正确。
memcpy：void *memcpy(void *dst, const void *src, size_t count);
memmove：void *memmove(void *dst, const void *src, size_t count);
strcpy：char *strcpy(char *dest,const char *src) 连接字符串
strcat：char *strcat(char *dest,char *src)       复制字符串
```