# 2.1 顺序链表
### 2.1.1 数组实现顺序表
```markdown
#define ElementType int
#define MAXSIZE 100
struct LNode{
    ElementType Data[MAXSIZE];
    int Last;
};
typedef struct LNode *List;
//    struct LNode L;
//    List PtrL;


//初始化(建立空的顺序表)
List MakeEmpty()
{
    List PtrL;
    PtrL = (List)malloc(sizeof(struct LNode));
    PtrL->Last = -1;
    return PtrL;
}

//查找
int Find(ElementType X, List PtrL)
{
    int i=0;
    while(i<=PtrL->Last && PtrL->Data[i]!=X)
        i++;
    if(i>PtrL->Last)
        return -1;
    else
        return i;
}

//插入（第i (1<=i<=n+1)个位置上插入一个值为X的新元素）
void Insert(ElementType X, int i, List PtrL)
{
    if (PtrL->Last>=MAXSIZE-1){
        std::cout<<"表满"<<std::endl;
        return;
    }
    if (i<1 || i>PtrL->Last+2){
        std::cout<<"位置不合法"<<std::endl;
        return;
    }
    for(int j=PtrL->Last; j>=i-1; j--)
    {
        PtrL->Data[j+1] = PtrL->Data[j];
    }
    PtrL->Data[i] = X;
    PtrL->Last++;
}

//删除（删除表的第i(1<=i<=n)个位置上的元素）
void Delete(int i, List PtrL)
{
    if (i<1 || i>PtrL->Last+2){
        std::cout<<"位置不合法"<<std::endl;
        return;
    }
    for(int j=i; j<=PtrL->Last; j++){
        PtrL->Data[j-1] = PtrL->Data[j];
    }
    PtrL->Last--;
}

```

### 2.1.2 链表实现顺序表

```markdown

#define ElementType int
#define MAXSIZE 100

typedef struct LNode *List;
struct LNode{
    ElementType Data;
    List Next;
};
//    struct LNode L;
//    List PtrL;

//求表长,时间性能为O(n)
int Length(List PtrL)
{
    List p = PtrL;
    int j=0;
    while(p)
    {
        p=p->Next;
        j++;
    }
    return j;
}

//按序号查找,平均时间性能为O(n)
List FindKth(int K, List PtrL)
{
    List p=PtrL;
    int j=0;
    while(p!=NULL && j<K)
    {
        p=p->Next;
        j++;
    };
    if (j==K) return p;
    else return NULL;
}

//按值查找,平均时间性能为O(n)
List Find(ElementType X, List PtrL)
{
    List p=PtrL;
    while(p!=NULL && p->Data!=X)
        p=p->Next;
    return p;
}

//插入
List Insert(ElementType X, int i, List PtrL)
{
    List s;
    if(i==1)
    {
        s = (List)malloc(sizeof(struct LNode));
        s->Data=X;
        s->Next = PtrL;
        return s;
    }
    List p = FindKth(i-1, PtrL);
    if (p==NULL)
    {
        printf("参数i出错");
        return NULL;
    }
    else
    {
        s=(List)malloc(sizeof(struct LNode));
        s->Data=X;
        s->Next=p->Next;
        p->Next=s;//注意这两句的顺序关系
        return PtrL;
    }
}

//删除
List Delete(int i, List PtrL)
{
    List s;
    if(i==1) //若要删除的是表的第一个结点，删除头结点有两种可能
    {
        s=PtrL;
        if(PtrL!=NULL){
            PtrL=PtrL->Next;
        }
        else{
            return NULL;
        }
        free(s);
        return PtrL;
    }
    
    List p=FindKth(i-1, PtrL);
    if(p==NULL){
        std::cout<<"第"<<i-1<<"个节点为空"<<std::endl;
        return NULL;
    }
    else if(p->Next==NULL){
        std::cout<<"第"<<i<<"个节点为空"<<std::endl;
        return NULL;
    }
    else{
        s=p->Next;
        p->Next=s->Next;
        free(s);
        return PtrL;
    }
}
```