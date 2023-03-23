#include<stdio.h>
typedef struct node
{
	ElemType data;               //数据域 
	struct node * prior, * next; //分别指向前驱结点和后继结点的指针 
	
}DLinkNode;                      //双链表结点类型 
//初始化线性表运算算法
void InitList(DLinkNode * &L) 
{
	L=(DLinkNode * )malloc(sizeof(DLinkNode));  //创建头结点L
	L->prior=L->next=NULL; 
}//算法的时间复杂度为(0)
//销毁线性表运算算法
void DestroyList(DLinkNode * &L)
{
	DLinkNode *pre=L,* p=pre->next;
	while(p!=NULL)
	{
		free(pre);
		pre=p;p=p->next;           //pre、p同步后移 
	}
	free(pre); 
}//算法的时间复杂度 为O(0),其中n为双链表L中数据结点的个数 
//求线性表长度运算算法
int GetLength(DLinkNode * L)
{
	int i=0;
	DLinkNode *p=L->next;      //p指向首结点
	while(p!=NULL)
	{
		i++;                   //i累加数据结点个数
		p=p->next; 
	} 
	return 0;
} //O(n)
//求线性表中第i个元素运算算法
int GetElem(DLinkNode *L,int i,ElemType &e)
{
	int j=0;
	DLinkNode *p=L;           //p指向头结点，计数器j置为0 
	if(i<=0)return 0;         //参数i错误 返回0 
	 while(p!=NULL&&j<i)
	 {
 		j++;
 		p=p->next;
 	}
	 if(p==NULL)return 0;  
}