# 单链表

## 线性表的分类
> 依据存储形式：顺序表 和 链表
> 1. 顺序表
> - 存储原理：用一段连续的存储空间实现
> - 一般的话，我们实现顺序表就是用数组来实现
> 
> 1. 链表
> - 存储原理：链式存储
> - 分类
> > - 单链表 --> 很重要
> > - 双向链表
> > - 循环链表
> > - 静态链表

## 顺序表的优缺点
> 优点
> - 便于实现和理解，适合修改数据操作次数多的算法
> 
> 缺点
> - 插入和删除时间复杂度过高，每次操作都需要进行大量的移位操作

## 单链表的定义
> 定义：单向链表中包含数据域和指针域，其中数据域用于存放数据，指针域用来连接当前结点和下一节点。

单个结点的定义
```C++
struct Node{ // 链表中单个结点
    dataType data; // 数据域
    struct Node* next; // 指针域 
};
```
整体连接方式
```C++
node1->next = node2;
node2->next = node3;
node3->next = node4;
node4->next = NULL
```
```C++
node1 -> node2 -> node3 -> node4
```

>链表分为两类，含头结点和不含头结点

##  含头结点的单链表操作集(全):small_airplane:
```C++
#include<bits/stdc++.h>

// typedef int Name --> 将int重命名为Name，重命名完之后，Name等价于int，当然原int可以继续使用

// dataType 等价于 int，这说明我的数据类型是int类型的
typedef int dataType;
// Status 表示状态
typedef int Status;

// C里面没有TRUE和FALSE，只有0和非零来表示布尔值
const int TRUE = 1;
const int FALSE = 0;

// 定义单个结点
struct Node{
    dataType data;// 数据域
    struct Node* next; // 指针域
};
// malloc(sizeof):
// return: void*类型 --> 指向任何变量的地址
// param: sizeof --> 开辟的空间大小
// 作用：从内存池里划分出一片大小为sizeof的空间，并返回一个指针，该指针可以指向任何变量
//
// free(void*)
// 作用：它能够释放的空间必须是用malloc函数开辟出来的
// 如果用malloc函数开辟了空间，就必须要用free函数还回去，否则的话程序会非常吃内存。
//
// Java垃圾回收机制：如果一块空间没有被其它变量引用或者指向，则该片空间会自动释放。

// (struct Node)* node = (struct Node* )malloc(sizeof(struct Node));
// 如果这样定义的话，代码非常冗余
// 因此可以用 typedef struct Node LNode;
// LNode* node = (Lnode*)malloc(sizeof(LNode));
// 这就是重新命名的好处：可以压缩代码，使得代码整体变得简洁和直观

// 重命名
typedef struct Node LNode;

// 定义链表
typedef struct Node* linkList;
// 因为只要知道了链表的表头结点的地址(指针)，就相当于获得了整张链表
// 因此，将struct Node* 命名为linkList.linkList就是整张链表

// 以下操作都是带头结点的链表操作
// 0. 链表初始化 ---> 调用之后，返回一个链表（链表没有值，next==NULL），返回一个链表的头结点
linkList initList()
{
    linkList head; // 此时这个head处于一个野指针的状态，不指向任何地方
    head = (LNode* )malloc(sizeof(LNode)); // sizeof(LNode) = sizeof(data) + sizeof(next)
    // 注意：malloc有可能返回NULL，这个情况就是内存不足
    if(head == NULL){ // head==NULL 说明空间开辟失败
        return NULL;
    }
    // head!=NULL 就说明空间开辟成功
    // 头结点的数据域，是"不使用的"
    // 因为开始初始化是没有数据的，因此头结点的指针域next==NULL
    head->next = NULL;
    return head;
}
// 1. 链表创建 ---> 头插法
Status headCreate(linkList head,dataType data[],int length){
    if(head == NULL){
        return FALSE;
    }
    for(int i=0;i<length;i++){
        LNode* node = (LNode*) malloc(sizeof(LNode));
        if(node == NULL){
            return FALSE;
        }
        node->data = data[i];
        // 以下两步不能颠倒次序
        node->next = head->next;
        head->next = node;
    }
    return TRUE;
}

// 2. 链表创建 ---> 尾插法
Status tailCreate(linkList head,dataType data[],int length)
{
    if(head == NULL){
        return FALSE;
    }
    LNode* s = head;
    for(int i=0;i<length;i++){
        LNode* t = (LNode*)malloc(sizeof(LNode));
        if(t == NULL){
            return FALSE;
        }
        t->data = data[i];
        t->next = s->next;
        s->next = t;
        s=t;
    }
    s->next = NULL;
    return TRUE;
}

// 3. 链表是否为空
Status isEmpty(linkList head){
    // 若进行该不判断，则会出现空指针引用异常
    if(head == NULL){
        return FALSE;
    }
    if(head -> next == NULL){
        return TRUE;
    }
    return FALSE;
}
// 4. 销毁链表 ---> 空间从哪里来就回到哪里去 用free函数释放
Status deleteList(linkList head)
{
    if(head == NULL){
        return FALSE;
    }
    LNode* p = head;
    while(head->next != NULL){
        // p始终指向链表的第一个结点(非头结点)
        p = head->next;
        // 头结点的指针 指向 被释放的结点p的下一个结点
        head->next = p->next;
        free(p);
    }
    return TRUE;
}
// 5. 查找元素 --> 按位查找(返回结点)
LNode* searchNodeOfIndex(linkList head,int index){
    // 若index == 0 则返回头结点
    if(index < 0){
        return NULL;
    }
    int j=0;
    LNode* p = head;
    while(p!=NULL && j < index){
        p = p->next;
        j++;
    }
    return p;
}

// 6. 查找元素 --> 按元素查找(返回第一次出现该元素的所在结点)
LNode* searchNodeOfElem(linkList head,dataType data){
    if(head == NULL){
        return NULL;
    }
    LNode* p = head->next;
    while (p!=NULL && p->data != data){
        p = p->next;
    }
    // 循环结束后，p要么是NULL，要么是需要查找的结点
    return p;
}

// 7. 增加元素 --> 按结点进行后插操作(在链表中的某个结点的后方插入元素)
Status insertElemOfNodeP(LNode* node,dataType data){
    if(node == NULL){
        return FALSE;
    }
    // 创建一个新结点
    LNode* t = (LNode*)malloc(sizeof(LNode));
    if(t==NULL){
        return FALSE;
    }
    // 往新创建的结点中加入需要插入的数据
    t->data = data;
    t->next = node->next;
    node->next = t;
    return TRUE;
}

// 8. 增加元素 --> 按结点进行前插操作(在链表中的某个结点的前方插入元素)
/**
 * 因为单链表在遍历之后，是无法知晓之前的元素的，因此需要进行特殊处理。
 */
Status insertElemOfNodeQ(LNode* node,dataType data){
    /**
     * 往某个结点的前方插入数据：等价于待插入的数据与当前结点的数据进行交换，然后往当前结点的后方插入数据
     * 原结点：node1(3) --> node2(4) --> node3(1)
     * 数据排列： 3 4 1
     * 现需要向node3(1)结点的前面插入数据data = 0;
     * 策略：将data数据与node3(1) 的数据进行交换 得： data = 1,node3(0),再在node3后方插入数据data得到
     * 处理后的结点: node1(3) --> node2(4) --> node3(0) --> node4(1)
     * 数据排列： 3 4 0 1
     */
     if(node == NULL){
         return FALSE;
     }
     // 开辟新结点
     LNode* t = (LNode*)malloc(sizeof(LNode));
     if(!t){
         return FALSE;
     }
     // data 与 node->data进行交换
     dataType temp = data;
     data = node->data;
     node->data = temp;
     // 调用在后方插入元素的函数
    insertElemOfNodeP(node,data);
     return TRUE;
}

// 9. 增加元素 --> 按位序插入(1,2,...,n)
/**
 * 在第i个位置上插入元素，则需要获取第i-1个结点，并在该结点之后完成插入操作
 */
Status insertElemOfIndex(linkList head,int index,dataType data){
    // 获取第i-1个位置上的结点
    LNode* node = searchNodeOfIndex(head,index-1);
    if(node == NULL){
        return FALSE;
    }
    // 往node结点的后面插入元素
    Status status = insertElemOfNodeP(node,data);
    if(status){
        return TRUE;
    }
    return FALSE;
}

// 10. 删除元素 --> 按位序删除(1,2,...,n,并用变量e接收被删除的数据)
/**
 * 若要删除第i个元素，则需要知道第i-1个元素的结点，并将第i-1上的结点与第i+1上的结点相连
 *
 */
Status deleteElemOfIndex(linkList head,int index,dataType *e){
    // 获取i-1个元素的结点
    LNode* node = searchNodeOfIndex(head,index-1);
    if(node == NULL){
        return FALSE;
    }
    // node->next 是第i个元素的结点
    LNode* p = node->next;
    *e = p->data;
    // 将i-1个元素的结点的指针 指向 第 i+1 个结点
    node->next = p->next;
    return TRUE;
}

// 11. 修改元素 --> 按位序修改
Status updateElemOfIndex(linkList head,int index,dataType e){
    // 获取index上的结点
    LNode* node = searchNodeOfIndex(head,index);
    if(node == NULL){
        return FALSE;
    }
    // 直接对该结点的数据域进行操作
    node->data = e;
    return TRUE;
}

// 12. 求链表的长度 --> 返回链表长度
int getLength(linkList head)
{
    if(head == NULL){
        return -1;
    }
    int len = 0;
    LNode* p = head;
    // p = p->next // 在最后一个结点执行了一次p=p->next
    while(p->next != NULL){
        p = p->next;
        len++;
    }
    return len;
}

// 13. 遍历整张表 --> 输出链表的所有元素
void display(linkList head){
    if(head == NULL){
        return;
    }
    LNode* p = head->next;
    while(p!=NULL){
        printf("%d ",p->data);
        p = p->next;
    }
    printf("\n");
    printf("len: %d\n",getLength(head));
    printf("****************************\n");
}

// *************************************高级操作*************************************

// 1. 获取链表中的最小值的下标,并用min变量接收最小值
int getMinValue(linkList head,dataType *min){
    if(head == NULL){
        return -1;
    }
    int len = getLength(head);
    // 最小值初始为链表的第一个元素
    *min = (head->next)->data;
    int index = 1;
    for(int i=2;i<=len;i++){
        // 获取第i个结点，与min进行比较
        LNode* node = searchNodeOfIndex(head,i);
        // 如果比min小，更好min值和索引值
        if((node->data) < *min){
            *min = node->data;
            index = i;
        }
    }
    return index;
}
// 2. 获取链表中的最大值的下标，并用max变量保存
int getMaxValue(linkList head,dataType *max){
    if(head == NULL){
        return -1;
    }
    int len = getLength(head);
    *max = (head->next)->data;
    int index = 1;
    for(int i=2;i<=len;i++){
        LNode* node = searchNodeOfIndex(head,i);
        if(node->data > *max){
            *max = node->data;
            index = i;
        }
    }
    return index;
}

// 3. 交换两个结点间的数据
Status swapDataOfNode(LNode* node1,LNode* node2){
    if(node1 == NULL || node2 == NULL){
        return FALSE;
    }
    // 直接对两个结点的数据域进行交换
    dataType t = node1->data;
    node1->data = node2->data;
    node2->data = t;
    return TRUE;
}

// 4. 对链表进行排序 --> 冒泡
Status sortOfBubble(linkList head){
    if(head == NULL){
        return FALSE;
    }
    int len = getLength(head);
    int flag = 1;
    LNode* node1;
    LNode* node2;
    // 实现的是冒泡排序，flag是对每轮是否进行交换的一个标志，若有一轮没有进行交换，则说明了已经有序了，就不需要执行后续的交换操作了
    while(flag){
        flag = 0;
        for(int i=1;i<len;i++){
            // 获取相邻的两个结点
            node1 = searchNodeOfIndex(head,i);
            node2 = searchNodeOfIndex(head,i+1);
            // 若第i个结点上的数据比第i+1个数据大，则交换两个数据
            if(node1->data > node2->data){
                flag = true;
                swapDataOfNode(node1,node2);
            }
        }
    }
}

// 5. 合并两个链表 --> 表二添加至表一尾部
Status mergeDoubleList(linkList head1,linkList head2){
    if(head1 == NULL || head2 == NULL){
        return FALSE;
    }
    // 获取head1表尾结点
    LNode* node = searchNodeOfIndex(head1, getLength(head1));
    if(node == NULL){
        return FALSE;
    }
    // head1表尾与head2表头相连接
    node->next = head2->next;
    return TRUE;
}

// 3. 合并两个有序链表
/*
 * 方法一：连接两个表 再排序 --> 本函数实现
 * 方法二：双指针实现
 */
Status mergeDoubleSortList(linkList head1,linkList head2){
    if(mergeDoubleList(head1,head2)){
        sortOfBubble(head1);
        return TRUE;
    }
    return FALSE;
}

// 4. 翻转整个链表
Status flipList(linkList head){
    if(head == NULL){
        return FALSE;
    }
    int len = getLength(head);
    LNode* node1;
    LNode* node2;
    // 对表头和表尾的结点元素直接进行交换
    for(int i=1;i<=len/2;i++){
        node1 = searchNodeOfIndex(head,i);
        node2 = searchNodeOfIndex(head,len-i+1);
        swapDataOfNode(node1,node2);
    }
}

int main()
{
    // 打印窗口出现中文乱码的话可以加上下面的语句
    system("chcp 65001");

    linkList head1 = initList();
    linkList head2 = initList();
    dataType data[] = {3,4,5,6,7,8};
    headCreate(head1,data,6);
    // 头插法可以实现倒序存储
    display(head1);
    tailCreate(head2,data,6);
    display(head2);
    // 头插法和尾插法的存储顺序是相反的

    // 判断head1是否为空表
    if(!isEmpty(head1)){
        printf("NO\n");
    }
    // 销毁head1
    deleteList(head1);
    // 判断head1是否为空表
    if(isEmpty(head1)){
        printf("TRUE\n");
    }
    display(head1);

    // 将第一个位置上的元素修改为0
    updateElemOfIndex(head2,1,0);
    display(head2);

    // 删除第1个元素并用变量e保存被删除的元素
    dataType e;
    deleteElemOfIndex(head2,6,&e);
    display(head2);
    printf("e = %d\n",e);

    // 在第6个位置上插入元素10
    insertElemOfIndex(head2,6,10);
    display(head2);

    // 在第1个位置上插入元素6
    insertElemOfIndex(head2,1,6);
    display(head2);

    // 获取第一次出现元素5的结点
    LNode* node = searchNodeOfElem(head2,5);
    printf("%d \n",node->data);
    // 在该结点的后面插入元素100,前面插入99
    insertElemOfNodeP(node,10);
    insertElemOfNodeQ(node,99);
    display(head2);

    // 获取第九个结点并往该结点的前面和后面均插入0
    node = searchNodeOfIndex(head2,9);
    insertElemOfNodeP(node,0);
    insertElemOfNodeQ(node,0);
    display(head2);

    sortOfBubble(head2);
    display(head2);

    // 重新建个表
    dataType datas[] = {110,330,51,38,99,1,64,11};
    headCreate(head1,datas,8);
    // 输出最大最小值
    dataType max,min;
    int index_max,index_min;
    index_max = getMaxValue(head1,&max);
    index_min = getMinValue(head1,&min);
    printf("max's index = %d max = %d\n",index_max,max);
    printf("min's index = %d min = %d\n",index_min,min);
    display(head1);

    // 排序
    sortOfBubble(head1);
    display(head1);

    // 合并两个表
    mergeDoubleSortList(head1,head2);
    display(head1);
    // 排序
    sortOfBubble(head1);
    display(head1);

    // 翻转
    flipList(head1);
    display(head1);
}
```

