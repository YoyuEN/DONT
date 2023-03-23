#include<stdio.h>
typedef struct node
{
	ElemType data;               //������ 
	struct node * prior, * next; //�ֱ�ָ��ǰ�����ͺ�̽���ָ�� 
	
}DLinkNode;                      //˫���������� 
//��ʼ�����Ա������㷨
void InitList(DLinkNode * &L) 
{
	L=(DLinkNode * )malloc(sizeof(DLinkNode));  //����ͷ���L
	L->prior=L->next=NULL; 
}//�㷨��ʱ�临�Ӷ�Ϊ(0)
//�������Ա������㷨
void DestroyList(DLinkNode * &L)
{
	DLinkNode *pre=L,* p=pre->next;
	while(p!=NULL)
	{
		free(pre);
		pre=p;p=p->next;           //pre��pͬ������ 
	}
	free(pre); 
}//�㷨��ʱ�临�Ӷ� ΪO(0),����nΪ˫����L�����ݽ��ĸ��� 
//�����Ա��������㷨
int GetLength(DLinkNode * L)
{
	int i=0;
	DLinkNode *p=L->next;      //pָ���׽��
	while(p!=NULL)
	{
		i++;                   //i�ۼ����ݽ�����
		p=p->next; 
	} 
	return 0;
} //O(n)
//�����Ա��е�i��Ԫ�������㷨
int GetElem(DLinkNode *L,int i,ElemType &e)
{
	int j=0;
	DLinkNode *p=L;           //pָ��ͷ��㣬������j��Ϊ0 
	if(i<=0)return 0;         //����i���� ����0 
	 while(p!=NULL&&j<i)
	 {
 		j++;
 		p=p->next;
 	}
	 if(p==NULL)return 0;  
}