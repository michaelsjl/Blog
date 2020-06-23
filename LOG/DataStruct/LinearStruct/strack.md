# 2.2 堆栈

### 2.2.1 堆栈的抽象数据类型描述

#### 堆栈（Stack）：具有一定操作约束的线性表，只在一端(栈顶，Top)做插入，删除。

插入数据：入栈（Push)

删除数据：出栈（Pop)

后入先出：Last In First Out (LIFO)


#### 类型名称：堆栈（Stack)

数据对象集: 一个有0个或多个元素的有穷线性表

操作集：长度为MaxSize的堆栈S [公式] Stack, 堆栈元素item [公式] ElementType

1.Stack CreateStack(int MaxSize):生成空堆栈，其最大长度为MaxSize;

2.int IsFull(Stack S, int MaxSize):判断堆栈S是否已满；

3.void Push(Stack S, ElementType item):将元素item压入堆栈；

4.int IsEmpty(Stack S):判断堆栈S是否为空；

5.ElementType Pop(Stack S):删除并返回栈顶元素；

![](/images/2.2.1-strack.jpg)

push 和 pop 可以穿插交替进行；

按照操作系列

（1）Push(S, A), Push(S, B), Push(S, C), Pop(S), Pop(S), Pop(S)堆栈输出是？CBA

（2）而Push(S, A), Pop(S), Push(S,B), Push(S,C), Pop(S), Pop(S)堆栈输出是？ACB

### 栈的顺序存储实现

栈的顺序存储结构通常由一个一维数组和一个记录栈顶元素位置的变量组成。

```markdown
#define MaxSize <存储数据元素的最大个数>
typedef struct SNode *Stack;
struct SNode{
    ElementType Data[MaxSize];
    int Top;
};
```

#### (1)入栈
```
void Push(Stack PtrS, ElementType item){
    if (PtrS->Top == MaxSize - 1){
        printf("堆栈满");
        return;
    }else{
        PtrS->Data[++(PtrS->Top)] = item;  // item应该放top上面的一个位置
        return;
    }
}
```

![](/images/2.2.2-stack1.png)

![](/images/2.2.2-stack2.png)

#### (2)出栈
```
ElementType Pop(Stack PtrS){
    if (PtrS -> Top == -1){
        printf("堆栈空");
        return ERROR;  /*ERROR是ElementType的特殊值，标志错误*/
    }else
        return (PtrS->Data[(PtrS->Top)--]);
}
```
![](/images/2.2.2-stack3.jpg)

#### 例题
【例】请用一个数组实现两个堆栈，要求最大地利用数组空间，使数组只要有空间入栈操作就可以成功。

【分析】一种比较聪明的方法是使这两个栈分别从数组的两头开始向中间生长；当两个栈的栈顶指针相遇时，表示两个栈都满了。

```
#define MaxSize<存储数据元素的最大个数>
struct DStack{
    ElementType Data[MaxSize];
    int Top1;  /*堆栈1的栈顶指针*/
    int Top2;  /*堆栈2的栈顶指针*/
}S;
S.Top1 = -1;
S.Top2 = MaxSize;
```

```
void Push(struct DStack *PtrS, ElementType item, int Tag){
    /*Tag作为区分两个堆栈的标志，取值为1和2*/
    if (PtrS->Top2 - PtrS->Top1 == 1){  /*两个堆栈挨在一起了，说明堆栈满了*/
        printf("堆栈满"); return;
    }
    if (Tag == 1)  /*根据flag区分，对第一个堆栈操作，如果是入第一个堆栈，就把item放到Top1的后面一个位置*/
        PtrS->Data[++(PtrS->Top1)] = item;
    else  /*对第二个堆栈操作，如果是入第二个堆栈，就把item放到Top2前面一个位置*/
        PtrS->Data[--(PtrS->Top2)] = item;
}

ElementType Pop(struct DStack *PtrS, int Tag){
    /*Tag作为区分两个堆栈的标志，取值为1和2*/
    if (Tag == 1){  /*对第一个堆栈操作*/
        if(PtrS -> Tag == -1){  /*堆栈1空*/
            printf("堆栈1空"); return NULL;
        }else return PtrS->Data[(PtrS->Top1)--];
    }else{   /*对第二个堆栈操作*/
        if (Ptr -> Tag == MaxSize){  /*堆栈2空*/
            printf("堆栈2空");return NULL;
        }else return PtrS->Data[(PtrS->Top2)++];
    }
}
```
