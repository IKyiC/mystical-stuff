# 数据结构

## 线性表

### 线性表的定义和基本操作

定义：具有**相同数据类型**的n个数据元素的**有限**序列

### 线性表的顺序表示

c语言动态分配语句：
~~~c
L.data=(Elemtype*)malloc(sizeof(Elemtype)*InitSize);
.........
free(L.data);
~~~

####  基本操作

#####  插入

1.存储空间是否足够
2.第i个元素及之后后移
3.在i处插入
4.length++

#####   删除

1.第i个位置后的元素前移
2.length--

#####  按值查找

###  线性表的链式表示

####  单链表

#####  定义

~~~c
typedef struct LNode{
    ElemType data;
    struct LNode *next;
}LNode;
~~~

性质：***非随机存取***的存储结构
#####  建立

~~~c
L=(LinkList)malloc(sizeof(LNode));//建立头结点
L->next=NULL;//初始为空链表
scanf("%d",&x);//输入结点值
LNode *s;int x;
while(x!=??){  //输入？表示结束
    s=(LNode*)malloc(sizeof(LNode));//建立新结点
    s->next=L->next;
    s->data=x;
    L->next=s;
    scanf("%d",&x);
}
~~~

#####  查找

~~~c
if(i<1) return NULL; //若i无效则返回NULL
int j=1;
LNode *p=L->next; //将第一个结点赋给指针p
while(p!=NULL&&j<i){
    p=p->next;
    j++;
}
~~~

#####  插入

~~~c
p=GetElem(L,i-1);//查找添加位置的前驱结点
s=(LNode*)malloc(sizeof(LNode));//建立新结点
s->data=x;
s->next=p->next;
p->next=s;
~~~

#####  删除

~~~c
p=GetElem(L,i-1);//查找删除位置的前驱结点
q=p->next;
p->next=q->next;
free(q);//释放内存空间
~~~

#####  合并（有序）

~~~c
pa=La->next,pb=Lb->next;
Lc=pc=La;
while(pa&&pb){
    if(pa->data<=pb->data){
        pc->next=pa;
        pc=pa;
        pa=pa->next;
    }
    else{
        pc->next=pb;
        pc=pb;
        pb=pb->next;
    }
    pc->next=pa?pa:pb;
    free(Lb);
}
~~~

####  双链表

#####  定义

~~~c
typedef struct DNode{
    ElemType data;
    struct DNode *prior,*next;
}DNode;
~~~

#####  插入

~~~c
s->next=p->next;
p->next->prior=s;
s->prior=p;
p->next=s;
~~~

#####  删除

~~~c
p->next=q->next;
q->next->prior=p;
free(q);
~~~

####  循环链表

最后一个结点不是指向NULL，改为指向头结点

####  静态链表

#####  定义

~~~c
#define MaxSize 50//定义最大长度
typedef struct{
    ElemType data;
    int next;	//下一组元素下标
}SLinkList[MaxSize];
~~~

##  栈

###  栈的定义和基本操作

定义：只允许在一端**（栈顶）**进行插入和删除操作的线性表
###  栈的顺序存储结构

####  定义

~~~c
#define MaxSize 50	//定义栈中元素的最大数量
typedef struct{
    ElemType data[MaxSize];	//存放栈中元素
    int top;	//栈顶指针
}SqStack;
~~~

####  初始化

~~~c
void InitStack(SqStack &S){
    S.top=-1;
}
~~~

####  进栈

~~~c
if(S.top==MaxSize-1) return false;//栈满，报错
S.data[++S.top]=x;	//先移动指针，再赋值
~~~

####  出栈

~~~c
if(S.top==-1) return false;	//栈空，报错
x=S.data[S.top--];	//先出站，再移动指针
~~~

###  栈的链式存储结构

优点：便于多个栈共享存储空间，提高效率
ps：个人觉得和单链表唯一的区别就是入栈和出栈都在表头进行.（注意有无头结点）

###  例题

1.某栈的输入序列为a，b，c，d
问：d，c，a，b是否为其可能的输出序列？
答：不可能。首先输出的是d，说明abc一定是按cba输出（若数据多，中间可能夹杂其他数，**但abc的顺序不变**）

##  队列

###  队列基本概念

定义：只允许在表的一端进行插入，而在表的另一端进行删除

###  队列的顺序存储结构

####  定义

~~~~c
#define MaxSize 50;//定义最大元素个数
typedef struct{
    ElemType data[MaxSize];
    int front,rear;//队头指针和队尾指针
}SqQueue;
~~~~

####  循环队列

构造一个环状空间，通过除法取余实现操作
初始时：Q.front=Q.rear
队首指针进1：Q.front=(Q.front+1)%MaxSize
队尾指针进1：Q.rear=(Q.rear+1)%MaxSize
队列长度(数组序号的最大值)：(Q.rear+MaxSize-Q.front)

#####  区分队头队尾的处理方法

1. 牺牲一个存储单元，即以队头指针在队尾的下一位置作为队满标志
   队满条件：(Q.rear+1)%MaxSize=Q.front
   队空条件：Q.rear=Q.front
2. 增设表示元素个数的成员
3. 设置判断变量tag

#####  循环队列的操作(使用处理方法1)

######  初始化

~~~c
Q.rear=Q.front;
~~~

######  入队

~~~~c
if((Q.rear+1)%MazSize==Q.front) return false;
Q.data[Q.rear]=x;
Q.rear=(Q.rear+1)%MaxSize;
~~~~



###  队列的链式存储结构

（适用于数据元素变化比较大的情形，且不会发生溢出的情况）

####  定义

~~~c
typedef struct LinkNode{
    ElemType data;
    struct LinkNode *next;
}LinkNode;
typedef struct{
    LinkNode *front,*rear;
}LinkQueue;
~~~

####  基本操作(带有头节点)

#####  初始化

~~~c
Q.front=Q.rear=(LinkNode*)malloc(sizeof(LinkNOde));
Q.front->next=NULL;//初始化为空
~~~

#####  判断为空

~~~~c
Q.front=Q.rear;
~~~~

#####  入队

~~~~c
LinkNode *p=(LinkNode*)malloc(sizeof(LinkNode));
p->data=x;
p->next=NULL;//插入新节点到链尾
Q.rear->next=p;
p=Q.rear;
~~~~

##### 出队

~~~c
if(Q.rear==Q.front) return false;
LinkNode *p=Q.front->next;
Q.front->next=p->next;
if(p==Q.rear) Q.rear=Q.front;//只有一个元素，设置为空表
free(p);
~~~

##  数组和特殊矩阵

##  树与二叉树

###  二叉树

####  概念

__定义__：每个顶点至多有两棵子树，且有左右之分，不能随意颠倒
__性质__：1.非空二叉树第k层至多有2^k-1^个结点
			2.高度为h的二叉树至多有2^h^-1个结点
			3.叶结点度数为n~0~，度为2结点数为n~2~，则有n~0~=n~2~+1

**满二叉树**：高度为h，含有2^n^-1个结点的二叉树

**完全二叉树**：高度为h，当且仅当其每个结点都与高度为h的满二叉树编号为1~n的结点一一对应
**性质**：1.具有n个结点的完全二叉树的高度为**[log~2~n]-1**
			2.对于结点i有：i>1时，双亲编号为[i/2];2i$\leqslant$n时，左孩子编号为2i，否则无左孩子；当2i+1$\leqslant$n时，右孩子			   编号为2i+1，否则无右孩子。
			3.满二叉树一定是完全二叉树，完全二叉树不一定是满二叉树

####  表示

顺序表示（适用于完全二叉树和满二叉树）:就是一个数组，下标值表示位置，0表示不存在此结点

链式表示
~~~c
typedef struct BiTNode{
    ElemType data;
    struct BiTNode *lchild,*rchild;
}BiTNode,*BiTree;
~~~

####  遍历

**原则**：先遍历左子树，再遍历右子树

#####  先序遍历

递归：

~~~c
void PreOrder(BiTree T){
    if(T!=NULL){
        visit(T);
        PreOrder(T->lchild);//递归遍历左子树
        PreOrder(T->rchild);//递归遍历右子树
    }
}
~~~

非递归：

~~~c
void PreOrder(BiTree T){
    InitStack(S);
    BiTree p=T;
    while(p||!IsEmpty(S)){
        if(p){
            visit(p);
            Push(S,p);
            p=p->lchild;
        }
        else{
            Pop(S,p);		/**把栈数组元素赋值给p，top--**//
            p=p->rchild;
        }
    }
}
~~~



#####  中序遍历

递归：

~~~~c
void PreOrder(BiTree T){
    if(T!=NULL){
        PreOrder(T->lchild);
        visit(T);
        PreOrder(T->rchild);
    }
}
~~~~

非递归：

~~~c
void InOrder2(BiTree T){
    InitStack(S);	//初始化栈S，该栈的数组类型应为BiTree
    BiTree p=T;		//p是遍历指针
    while(p||!IsEmpty(S)){	//一路向左
        if(p){
            Push(S,p);		//当前结点入栈
            p=p->lchild;		//左孩子不空，一直向左走
        }
        else{
            Pop(S,p);		//出栈
            visit(p);		//访问出栈结点
            p=p->rchild;	//转向栈结点的右子树
        }
    }
}
~~~



#####  后序遍历

递归：

~~~c
void PreOrder(BiTree T){
    if(T!=NULL){
        PreOrder(T->lchild);
        PreOrder(T->rchild);
        visit(T);
    }
}
~~~

非递归：

~~~c
void PreOrder(BiTree T){
    InitStack(T);
    BiTree p=T;
    BiTree r=NULL;		//用于记录最近访问过的结点
    while(p||!IsEmpty(S)){
        if(p){		//一直向左
            push(S,p);
            p=p->lchild;
        }
        else{
            GetTop(S,p);		//读取栈顶元素(把栈顶元素赋给p)
            if(p->rchild&&p->rchild!=r){	//若右子树存在且未被访问
                p=p->rchild;
            }
            else{				//否则弹出结点并访问
                pop(S,p);		//将该结点弹出
                visit(p);	
                r=p;			//记录最近访问过的结点
                p=NULL;			//访问完后重置p指针
            }
        }
    }
}
~~~

#####  层次遍历

~~~c
void LevelOrder(BiTree T){
    InitQueue(Q);		//初始化辅助队列
    BiTree p;
    EnQueue(Q,T);
    while(!IsEmpty(Q)){
        DeQueue(Q,p);
        visit(p);
        if(p->lchild!=NULL){
            EnQueue(Q,p->lchild);
        }
        if(p->rchild!=NULL){
            EnQueue(Q,p->rchild);
        }
    }
}
~~~

####  二叉树的构造

#####  按先序序列递归建立

~~~c
BiTree CreateTree1(){
    BiTree *p;
    char ch;
    fflush(stdin);	//清空缓存区
    scanf("%c",&ch);
    if(ch=='#') p=NULL; //表示该结点后为空，无子树
    else{
        p=New BNODE;
        if(!p) exit(0);
        p->data=ch;
        p->lchild=CreateTree1;
        p->rchild=CreateTree1;
    }
}
~~~

#####  交互问答式建立

~~~c
BiTree CreateTree2(BiTree p,int n){
    char ch;
    if(n==0) printf("根结点：");
    fflush(stdin);
    scanf("%c",&ch);
    fflush(stdin);
    if(ch!='#'){
        n=1;
        p=New BNODE;
        p->data=ch;
        p->lchild=NULL;
        p->rchild=NULL;
        printf("%c的左孩子是：",ch);
        p->lchild=CreateTree2(p->lchild,n);
        printf("%c的右孩子是：",ch);
        p->rchild=CreateTree2(p->rchild,n);
    }
    return p;
}
~~~

####  求二叉树的宽度

~~~c
int Width(BiTree *T){
    int i,n=0,front=0,rear=0,max=0,lev=1,maxlev[10]={0};
    
    /**定义一个队列**/
    struct W{
        BiTree *Node;
        int Nodelev;
    }Q[50];
    
    Q[front].Node=T;
    Q[front].Nodelev=1;
    while(front<=rear){
        if(Q[front].Node->lchild){
            Q[++rear].Node=Q[front].Node->lchild;
            Q[rear].Nodelev=Q[front].Nodelev+1;
        }
        if(Q[front].Node->rchild){
			Q[++rear].Node=Q[front].Node->rchild;
            Q[rear].Nodelev=Q[front].Nodelev+1;
        }
        front++;
    }
    for(i=0;i<=rear;i++){
		if(max<maxlev[i]) max=maxlev[i];
    }
    return max;
}
~~~

**设计思路：**相当于用一个队列，对二叉树进行层序遍历，需要注意的是，引入了一个Nodelev来记录长度的变化

####  求二叉树的深度

~~~c
int Depth(BiTree *bt){
    int ldepth,rdepth;
    if(bt==NULL) return 0;
    else{
        ldepth=Depth(bt->lchild);//此处的函数完全可以不看成函数，他就是左/右子树的深度
        rdepth=Depth(bt->rchild);
        if(ldepth>rdepth) return ldepth+1;
        else return rdepth+1;
    }
}
~~~

**设计思路**：“递归”：“递”，如何“递”？对于二叉树递归类问题，一开始先不要考虑细节，先考虑根和其左右子树的关系，观察得到depth=max(左子树depth，右子树depth)+1，故而能够确定递归的“递”，如何“归”？，即分别递归左右子树并返回两者中深度较大者+1，易知对于空结点返回NULL，即可得到代码

**对于上面的设计思路有感而发，现考虑下面一个问题:**
**如何用递归求二叉树结点个数？**

~~~c
int count=0;
int Count(BiTree *bt,int count){
    if(bt==NULL) return 0;
    count+=Count(bt->lchild,count);//算出左子树的结点个数
    count+=Count(bt->rchild,count);//算出右子树的结点个数
    count++;					   //自身也算一个结点数
    return count;
}
~~~

###  线索二叉树

####  表示

~~~c
typedef struct ThreadNode{
    ElemType data;
    struct ThreadNode *lchild,*rchild;
    int ltag,rtag;
}
~~~

####  中序遍历线索二叉树

ps:前驱，后继指的是将遍历写出后结点的前一个和后一个
例如：中序遍历为1,2,5,6.1的前驱就是NULL，后继就是2

~~~c
/**说明**/
//p->ltag=1,p->lchild指向左孩子；p->ltag=0,p->lchild指向前驱
//p->rtag=1,p->rchild指向右孩子；p->rtag=0,p->rchild指向后继
void InOrderTraverse_Thr(ThTree *T){
    p=T->lchild;
    while(p!=T){
        while(p->ltag==1) p=p->lchild;
        visit(p->data);
        while(p->rtag==0&&p->rchild!=T){
            p=p->rchild;
            visit(p->data);
        }
        p=p->rchild;
    }
}
~~~

###  二叉树的复制

####  二叉树的相似和等价

相似二叉树定义：具有相同结构的二叉树
等价二叉树定义：相似且相应结点包含信息相同

**判断二叉树等价：**

~~~c
bool IsEqual(BNode *root1, BNode *root2){
    if(root1==NULL && root2==NULL)
        return true;
    else if(root1==NULL || root2==NULL）
        return false;
    else if(root1->data != root2->data)
        return false;
    else if(IsEqual(root1->lchild, root2->lchild) && IsEqual(root1->rchild, root2->rchild))
        return true;
    else
        return false;
}
~~~



####  二叉树的复制

~~~c
BiTree Copy(BiTree oldtree){
    BiTree temp;
    if(oldtree!=NULL){
        temp=New Node;
        temp->lchild=Copy(oldtree->lchild);
        temp->rchild=Copy(oldtree->rchild);
        temp->data=oldtree->data;
        return temp;
    }
    return NULL;
}
~~~

###  堆

堆可以看作完全二叉树的数组表示形式，其中父子与子的关系位[i/2]

####  相关定义

**最大堆：**一颗完全二叉树的任意一个非终端结点的元素都不小于其左儿子结点和右儿子结点的元素
**最小堆：**一颗完全二叉树的任意一个非终端结点的元素都不大于其左儿子结点和右儿子结点的元素
**相关特点：**1.最大堆中根节点的元素最大 		 2.最小堆中根结点的元素最小

###  选择树

定义：一棵选择树是一棵二叉树，其中每一个结点都代表该结点两个儿子的较小者，根结点是最小者

###  树

####  存储结构

#####  双亲表示法

~~~c
#define MaxNodes 100//定义最大结点个数
typedef struct{
    ElemType data;
    int parent;		//双亲位置域（双亲数组的下标）
}PTNode;
typedef struct{
    PTNode nodes[MaxNodes];
    int n;
}PTree;
~~~

#####  孩子表示法

~~~c
typedef struct CTNode{
    int child;
    struct CTNode *next;
}*ChildPtr;
typedef struct{
    Telementtype data;
    ChildPtr firstchild;
}CTBox;
typedef struct{
    CTBox Nodes[MAX_TREE_SIZE];
    int n,r;
}CTree;
~~~

#####  孩子兄弟表示法（二叉树表示法）

~~~c
typedef struct CSNode{
    Elemtype data;
    struct CSNode *firstchild,*nextsibling;//第一个孩子和右兄弟指针
}CSNode,*CSTree;
~~~

###  森林与二叉树

####  森林与二叉树的转化（王道167页）

**森林–>二叉树：**

1. 每棵树转换为对应的二叉树（左孩子，右兄弟）
2. 每棵树的根也可视为兄弟关系，依次连接
3. 拉展开即可

**二叉树–>森林**

1. 将根的右链断开，又可得到一棵树，将其右链断开，一直断到不能再断
2. 根据上述森林转化为二叉树反推

###  树的应用

####  哈夫曼树和哈夫曼编码

#####  相关定义

**带权路径长度：**根到任意结点的路径长度与该结点上权值的乘积
**哈夫曼树：**树的带权路径长度最小的二叉树

#####  哈夫曼树的构造

1. 将每一个w~i~作为一个外结点，并从小到大顺序排序
2. 选取最小的两个外结点，增加一个内结点，形成一个增长树。外结点权之和写入内结点，与其他外结点一起再次排序
3. 重复步骤2
4. 直到形成最后一棵增长树，为哈夫曼树

#####  哈夫曼编码

**前缀编码：**没有一个编码是另一个编码的前缀
**0和1：**0表示转向左孩子，1表示转向右孩子

####  二叉排序树

**定义：**若左子树非空，则左子树上的所有结点的值均小于它的根节点的值；若右子树非空，则右子树上的所有结点的值均大于或等于它的根结点的值

####  判定树

特点：每个内部结点对应一个部分解，每个叶子对应一个解。求解的过程对应于从根到叶的一条路

##  图

###  基本术语

**图：**由顶点集V（有限非空）和边集E组成
**有向图：**E是有向边的有限集合，弧记为<v,w>，称为v邻接到w
**无向图：**E是无向边的有限集合，弧记为（v，w），称v和w相关联
**简单图：**不存在平行边和圈（数据结构中仅讨论简单图）
**完全图：**有n(n-1)/2条边的无向图，即任意两个顶点之间都有边，有向图则有n(n-1)
**子图：**两个图，G=(V,E),G’=(V’,E’)，若V‘是V的子集，E’是E的子集，则G‘是G的子图；若满足V(G)=V(G’)，则G’是G的生成子图
**连通：**从顶点v到顶点w有路径存在，则称v和w是连通的
**连通图：**图G中任意两个顶点都是连通的
**强连通图：**有向图中，任意两个顶点都相互可达
**生成树：**包含图中所有顶点的一个极小连通子图
**顶点的度：**依附于顶点v的边数，记为TD(v)，**无向图的顶点度数之和等于边数的两倍**
**稀疏图(|E|<|V|log|V|):**边数很好的图，反之称为稠密图
**简单路径：**顶点不重复出现的路径；除第一个顶点和最后一个顶点外，不重复出现其他点的回路称为**简单回路**

###  图的表示

####  邻接矩阵法

ps：适用于稠密图
对于非带权图：

$$ A[i][j]=\left\{
\begin{array}{rcl}
1       &      & {若(v_i,v_j)或<v_i,v_j>是E(G)中的边}\\
0       &      & {若(v_i,v_j)或<v_i,v_j>不是E(G)中的边}
\end{array} \right. $$

对于带权图：

$$ A[i][j]=\left\{
\begin{array}{rcl}
w(ij)       &      & {若(v_i,v_j)或<v_i,v_j>是E(G)中的边}\\
0或无穷       &      & {若(v_i,v_j)或<v_i,v_j>不是E(G)中的边}
\end{array} \right. $$

~~~c
#define MaxVertexNum 100
typedef char VertexType;	//顶点的数据类型
typedef int EdgeType;		//带权图边上权值的数据类型
typedef struct{
    VertexType Vex[MaxVertexNum];//顶点表
    EdgeType** Edge;//邻接矩阵，边表
    int vexnum,arcnum;//图的当前顶点数和弧数
}MGraph;
~~~

####  邻接表法

ps：这块记得看一下书或者ppt，可能要求会画

对图G中的每个顶点v~i~建立一个单链表(边表)。

~~~c
#define MaxVertexNum 100
typedef struct ArcNode{	//边表结点
    int adjvex;	//该弧指向的顶点位置
    struct ArcNode *next;	//指向下一条弧的指针
}ArcNode;
typedef struct VNode{	//顶点表结点
    VertexType data;
    ArcNode *first;		//指向第一条依附该顶点的弧的指针
}VNode,AdjList[MaxVertexNum];
typedef struct{
    AdjList vertices;	//邻接表
    int vexnum,arcnum;	//图的顶点数和弧数
}ALGraph;
~~~

####  十字链表（有向图）

ps：图的十字链表不是唯一的，但十字链表能确定唯一的图

相比于邻接表法，就是有向图的每个弧有一个结点

[十字链表画法](https://www.bilibili.com/video/BV1Nb411d7tV/?spm_id_from=333.1007.top_right_bar_window_history.content.click)(ctrl+点击)

~~~c
#define MaxVertexNum 100
typedef struct ArcBox{
    int tailvex,headvex;	//该弧头节点和尾结点的位置
    struct ArcBox *hlink,*tlink;	//弧头相同和弧尾相同的弧的链域
    InfoType info;	//该弧存储的相关信息
}ArcBox;
typedef struct VexNode{
    VertexType data;	//顶点数据
    ArcBox *firstin,*firstout;	//指向该顶点的第一条入弧和出弧
}VexNode;
typedef struct{
    VexNode xlist[MaxVertexNum];	//表头向量
    int vexnum,arcnum;
}
~~~

####  邻接多重表（无向图）

[邻接多重表画法](https://www.bilibili.com/video/BV1Ae41137Jd/?spm_id_from=333.337.search-card.all.click&vd_source=307be9573968ac54e6581924fd6b6a67)(CTRL+点击)

~~~c
#define MaxVertexNum 100
typedef enum {unvisited,visited} VisitIf;
typedef struct EBox{
    VisitIf mark;	//边访问标记
    int ivex,jvex;	//该边依附的两个顶点的位置
    struct EBox *ilink,*jlink;	//指向依附这两个顶点的下一条边
    InfoType *info;
}EBox;
typedef struct VexBox{
    VertexType data;
    EBox *firstedge;	//指向第一条依附于该顶点的边
}VexBox;
typedef struct{
    VexBox adjmulist[MaxVertexNum];
    int vexnum,edgenum;
}AMLGraph;
~~~

###  图的搜索算法

**遍历图注意问题：**确定遍历起点；为保证非连通图每个顶点能被访问到，应该轮换起点；为避免顶点的重复访问，应做标记

####  广度优先搜索BFS

**基本思想：**首先访问起点，**然后依次访问与其关联的每一个顶点**，并以关联顶点为新的起点，重复上述过程。若还有未被访问的顶点，则换一个起点，继续上述过程直至所有顶点被访问。

~~~c
bool visited[MAX_VERTEX_NUM];
void BFSTraverse(Graph G){	//对图G进行广度优先遍历
    for(i=0;i<G.vexnum;++i) visited[i]=FALSE;	//标记数组初始化
    InitQueue(Q);	//初始化队列Q
    for(i=0;i<G.vexnum;++i){	//从0号顶点开始遍历
        if(!visited[i]) BFS(G,i);
    }
}
void BFS(Graph G,int v){	//从顶点v出发，广度优先遍历图G
    visit(v);
    visited[v]=TRUE;
    Enqueue(Q,v);
    while(!isEmpty(Q)){
        DeQueue(Q,v);
 		for(w=FirstNeighbor(G,v);w>=0;w=NextNeighbor(G,v,w)){	//检测v的所有邻接点并访问
          	if(!visited[w]){
           		visit(w);
      			visited[w]=TRUE;
       			EnQueue(Q,w);
         	}//if
       	}//for
    }//while
}//BFS
~~~

**时间复杂度分析：**采用邻接表方式存储时，每条边和每个点至少访问依次，故为O(|V|)；采用邻接矩阵方式存储时，时间复杂度为O(|V|^2^)。
**空间复杂度分析：**由于需要借助一个队列，空间复杂度为O(|V|)。

####  深度优先搜索DFS

**基本思想：**首先访问起点，然后依次访问与该起点关联的任意一个顶点，再对访问的那个顶点进行上述操作。若图中还有未被访问的起点，则换一个起点，继续上述操作，直到所有顶点被访问。

~~~c
bool visited[MAX_VERTEX_NUM];
void DFSTraverse(Graph G){	//对G进行深度优先遍历
    for(v=0;v<G.vexnum;++v) visited[v]=FALSE;
    for(v=0;v<G.vexnum;++v){
        if(!visited[v]) DFS(G,v);
    }
}
void DFS(Graph G,int v){	//从顶点v出发，深度优先遍历图G
    visit(v);
    visited[v]=TRUE;
    for(w=FirstNeighbor(G,v);w>=0;w=NextNeighbor(G,v,w)){
        if(!visited[w]) DFS(G,w);
    }
}
~~~

####  搜索算法的应用

#####  判断是否存在u到v的路径

~~~c
int ExistPathDfs1(ALGraph G,int *visited,int u,int v){
    ArcNode *p;
    int w;
    if(u==v) return 1;
    else{
        visited[u]=1;
        for(p=G.vertices[u].firstarc;p;p=p->nextarc){	//从以u为顶点的边表进行遍历
            w=p->adjvex;	//指向下一个结点
            if(!visited[w]&&ExistPathDfs1(G,visited,w,v)) return 1;
        }
    }
    return 0;
}
~~~

#####  判断u到v是否有通路

~~~c
int ExistPathDfs2(ALGraph G,int *visited,int u,int v){
    ArcNode *p;
    int w;
    int static flag=0;
    visited[u]=1;
    p=G.vertices[u].firstarc;	//p指向u作为头结点的第一条弧
    while(p!=NULL){
        w=p->adjvex;		//赋值w为p指向的结点
        if(v==w){
            flag=1;
            return 1;
        }
        if(!visited[w]) p=p->nextarc;	//p指向下一条弧
    }
    if(!flag) return 0;
}
~~~

#####  一个小小的重点：求u到v所有简单路径（王道216）

~~~c
void FindPath(AGraph *G,int u,int v,int path[],int d){	//d表示路径长度
    int w;
    ArcNode *p;
    d++;
    path[d]=u;
    visited[u]=1;
    if(u==v) print(path[]);
    p=G->adjlist[u].firstarc;
    while(p!=NULL){
        w=p->adjvex;
        if(visited[w]==0) FindPath(G,w,v,path,d);
        p=p->nextarc;
    }
    visited[u]=0;
}
~~~

#####  判断图是否有回路存在

~~~c
int Cycle(ALGraph G,int *visited,int u){
    ArcNode *p;
    int w;
    int flag=0;
    visited[u]=1;
    p=G.vertices[u].firstarc;
    while(p&&!flag){
        w=p->adjvex;
        if(visited[w]!=1){
            if(!visited[w]){
                flag=Cycle(G,visited,w);
            }
        }
        else flag=1;
        p=p->nextarc
    }
    visited[u]=2;
    return flag;
}
~~~

###  图的应用

####  最小生成树

#####  Prim算法

算法步骤：

~~~c
void Prim(MGraph G,VertexType u){	//从第u个顶点出发
/**用于记录顶点集U到V-U的代价最小的边的辅助数组**/
    struct{
        VertexType adjvex;
        VRType lowcost;
    }closedge[MAX_VERTEX_NUM];
    
    k=LocateVex(G,u);	//记录G中u的位置
    for(j=0;j<G.vexnum;++j){	//辅助数组初始化,讲与u有关联的边全部纳入
        if(j!=k) closedge[j]={u,G.arcs[k][j].adj};	//{adjvex,lowcost}
    }
    closedge[k].lowcost=0;	//初始化U集
    for(i=1;i<G.vexnum;++i){
        k=minimum(closedge);	//求出T的下一个结点，此时closedge[k].lowcost为···中最小
        printf(closedge[k].adjvex,G.vexs[k]);	//输出生成树的边
        closedge[k].lowcost=0;	//将k并入U集
        for(j=0;j<G.vexnum;++j)
            if(G.arcs[k][j].adj<closedge[j].lowcost)//新顶点并入后，重新选择最小边
                closedge[j]={G.vexs[k],G.arcs[k][j].adj};
    }
}
~~~

#####  Kruskal算法

算法步骤：

~~~c

~~~

####  最短路径问题

#####  Dijkstra算法（单源最短路径）

ps:边上带有负权值时，Dijkstra算法不适用

本算法需要三个数组：
dist[]:记录源点v~0~到其他顶点当前的最短路径长度
path[]:path[i]记录源点到顶点i之间的最短路径的前驱结点
visit[]:visit[i]=1,表示当前i顶点已经访问过，visit[i]=0则表示没有访问过

在本算法中，若i到j没有弧，则G【i】【j】用无穷表示，而不是0

基本思想：初始化—找最小—更新—循环至包括n个点

~~~c
#define MaxSize 100
int path[MaxSize];
void Dijkstra(int n,Mgraph G){
    int dist[MaxSize];
    int visit[MaxSize];
    visit[0]=1;
    int i;
    /**初始化**/
    for(i=1;i<n;i++){
        dist[i]=G[0][i];
        path[i]=0;
    }
    int count=1;	//表示访问的结点数量
    while(count<n){
        int min=INF,index;	//此处min表示无穷，index表示索引
        for(i=0;i<n;i++){	//找出路径最小且未访问过的结点的索引
            if(visit[i]==0&&min>dist[i]){
                min=dist[i];
                index=i;
            }
        }
        visit[index]=1;
        count++;
        for(i=1;i<n;i++){
            if(visit[i]==0&&dist[index]+G[index][i]<dist[i]){	//更新路径（比较直接路径和间接路径大小）
                dist[i]=dist[index]+G[index][i];	//在这一步时，已经把部分距离无穷更新
                path[i]=index;
            }
        }
    }
}
~~~

#####  Flord算法（求各顶点间距离）

[Flord算法讲解](https://blog.csdn.net/m15253053181/article/details/128039852?ops_request_misc=%7B%22request%5Fid%22%3A%22168370778916800213051629%22%2C%22scm%22%3A%2220140713.130102334.pc%5Fall.%22%7D&request_id=168370778916800213051629&biz_id=0&utm_medium=distribute.pc_search_result.none-task-blog-2~all~first_rank_ecpm_v1~rank_v31_ecpm-2-128039852-null-null.142^v86^insert_down28v1,239^v2^insert_chatgpt&utm_term=flord算法c语言实现&spm=1018.2226.3001.4187)

核心代码：

~~~c
for (k = 0; k < VertexNum; k++) // 将每个顶点都作为中转尝试
{
    // 遍历整个矩阵 i-行 j-列
    for (i = 0; i < VertexNum; i++)
    {
        for (j = 0; j < VertexNum; j++)
        {
            // 若经过k顶点中转，路径更短，则更新矩阵
            if (dis[i][k] + dis[k][j] < dis[i][j])
            {
                dis[i][j] = dis[i][k] + dis[k][j]; // 更新dis矩阵
                path[i][j] = k;                    // 更新path矩阵
            }
        }
    }
}

~~~

#####　　拓扑排序

基本思路：

1. 从AOV网中选择一个没有前驱的顶点输出
2. 从网中删除该顶点和所有以它为起点的有向边
3. 重复1和2直至不存在无前驱的顶点

~~~c
bool TopologicalSort(MGraph){
    
    /**初始化栈，存储入度为0的顶点**/
    InitStack(S);
    int i;
    for(i=0;i<G.vexnum;i++){
        if(indegree[i]==0){
            Push(S,i);
        }
    }
    
    int count=0;//记录当前已经输出的顶点数
    while(!IsEmpty(S)){
        Pop(S,i);
        print[count++]=i;	//输出顶点i
        /**将所有i指向的顶点的入度-1，并且将入度为0的顶点压入栈**/
        for(p=G.vertices[i].firstarc;p;p=p->nextarc){
            v=p->adjvex;
            if(!(--indegree[v])) Push(S,v);
        }
    }
    if(count<G.vexnum) return false;	//排序失败，有向图中有回路
    else return true;		//排序成功
}
~~~

#####  逆拓扑排序

基本思路：

1. 从AOV网中选择一个没有后继的顶点并进行输出
2. 从网中删除该顶点和所有以它为终点的有向边
3. 重复1，2直到当前的AOV网为空

####  关键路径

#####  相关概念

路径长度：从源点到汇点路径上所有活动的持续时间之和
关键路径：从源点到汇点具有最大长度的路径
**事件V~j~的最早可能发生时间VE(j)：**源点V~1~到顶点V~j~的最长路径长度
**事件V~k~的最迟发生时间VL(k)：**汇点的最早发生时间VE(n)减去从V~k~到V~n~的最大路径长度
**活动a~i~的最迟允许开始时间L(i)：**不引起工期延误的前提下，活动（边）a~i~允许的最迟开始时间，为a~i~的最迟完成时间VL(k)减去a~i~的持续时间
**活动a~i~的最早可能开始时间：**若边<v~k~,v~j~>表示活动a~i~，则e(i)=ve(k)
**时间余量：**L(i)-E(i)表示活动a~k~的最早可能开始时间和最迟允许开始时间的时间余量，在关键路径上满足L(i)=E(i)

[关键路径讲解](https://www.bilibili.com/video/BV1s14y1T75U/?spm_id_from=333.880.my_history.page.click)

##  查找

###  顺序查找和折半查找

####  顺序查找

#####  顺序表的查找

引入”哨兵“从尾部开始查找，能避免一些复杂的判断语句
ASL~成功~=(n+1)/2

#####  链表的查找

从头节点开始遍历到最后
ASL~成功~=(n+1)/2

####  折半查找

（仅适用于有序的顺序表）
#####  基本思想

将给定值key与表的中间位置的元素比较，相等则返回，不相等比较大小，然后与前半或者后半部分比较

#####  计算平均查找长度

[平均查找长度](https://www.bilibili.com/video/BV1WL411e7M4/?spm_id_from=333.880.my_history.page.click)

查找特定值的时间复杂度为log~2~n

####  分块查找（线性查找+折半查找）

#####  基本思想

将查找表分为若干子块，块内元素可以无序，块间元素必须有序。首先折半查找确定块，然后在块内顺序查找

#####  平均查找长度

$$ ASL=\frac{s^{2}+2s+n}{2s}$$(n为长度，s为每块记录的个数)

$$ ASL=\frac{s+1}{2}$$+$$\frac{b+1}{2}$$(b为块的个数)

（上面两个公式是一样的）

###  树型查找

####  二叉排序树(BST)

#####  定义

左结点小于根结点小于右结点，左右子树也是二叉排序树

#####  查找

从根节点开始，逐步向下比较

#####  插入

从根节点开始，大于就看右节点，小于就看左节点，一直重复下去最后插到某个叶结点的左孩子或者右孩子

#####  删除

叶节点：直接删除
只有一棵子树的结点：让子树成为父节点的子树
有两颗子树的结点：令z的直接后继(比z大的第一个值)或直接前驱代替z，然后删特，就能转化为前两种情况

#####  构造

从空树出发，依次输入元素，插到合适位置

####  平衡二叉树（AVL）

#####  定义

平衡二叉树：左右子树的高度差不超过1
结点的平衡因子：结点左右子树高度之差

#####  插入

**基本思想：**首先检查是否因为插入导致不平衡，如果导致不平衡，就找离插入结点最近的平衡因子绝对值大于1的结点A，队以A为根的子树进行调整

**调整规律：**
LL:新结点被插到A的左子树的左子树上
LR:新结点被插到A的左子树的右子树上
RR:新结点被插到A的右子树的右子树上
RL:新结点被插到A的右子树的左子树上

[平衡二叉树插入调整](https://www.bilibili.com/video/BV11M411w7cL/?spm_id_from=333.880.my_history.page.click&vd_source=307be9573968ac54e6581924fd6b6a67)

###  B树和B+树

####  B树

#####  定义

[B树定义讲解](https://www.bilibili.com/video/BV13o4y1t7Ti/?spm_id_from=333.880.my_history.page.click)

#####  高度(磁盘读取次数)

n个关键字、高度为h、阶数为n
**log~m~(n+1)$$\leqslant$$h$$\leqslant$$log~[m/2]~((n+1)/2)+1**

#####  存储结构

~~~c
#define m 3	//B-树的阶，暂设为3
typedef struct Node{
    int keynum;	//该结点的关键字个数
    KeyType key[m];	//关键字向量，key[0]不可用
    struct node *parent;	//指向双亲
    struct Node *son[m];	//孩子指针为son[0..keynum]
}BTreeNode,*BTree;
~~~



#####  查找

[B树查找：2min45s](https://www.bilibili.com/video/BV1zZ4y1F7YV/?spm_id_from=333.880.my_history.page.click)

#####  插入

[B树的创建](https://www.bilibili.com/video/BV1Jh411q7xP/?spm_id_from=333.880.my_history.page.click)
基本思想：
第一步”定位“：利用查找，找到插入该关键字的**最底层**的某个**非叶结点**
第二步”插入“：结点的关键字个数应保持在[[m/2]-1,m-1]范围内，插入后如果超过m-1，就要进行分裂
(分裂的具体操作:从[m/2]的索引处分为左右两个部分，左部分放在原来的结点里面，右部分放在新的结点里面，把[m/2]处的关键字放到双亲结点中，如果导致双亲结点超过上限，就对双亲结点进行分裂，以此往复)

#####  删除

如果关键字k不在终端结点，用k的直接前驱或者后继来代替k，一直替代到终端，然后转化为讨论删除终端结点的关键字的情形

关键字在终端时，基本思想：

1. 直接删除关键字：如果关键字个数$$\geq$$[m/2]，直接删除即可
2. 兄弟够借：结点关键字个数为[m/2]-1，调整左右兄弟及其双亲，以此达到新的平衡
3. 兄弟不够借：该结点和他所有兄弟关键字个数都为[m/2]-1，则将关键字删除后与左右兄弟结点和双亲结点1进行合并
   (合并具体操作：双亲的关键字个数会-1，如果双亲是根节点，则直接把根结点删了(根节点的关键字个数为1)，新合并得到的结点当根节点，如果不是，关键字个数-1后的双亲如果不满足定义就对双亲进行合并，以此往复)

####  B+树

#####  定义

在B-树的基础上，满足两个条件：有k个子结点的结点必然有k个关键码，非叶子结点仅有索引作用，信息放在叶结点中，非叶结点仅含有对应子树的最大关键字和指向该子树的指针

#####  查找

和B树的查找类似，只是非结点上的关键字值等于给定值时不终止，必须往下找到叶子结点

#####  插入和删除

与B树相似

###  散列表—哈希查找

####  构造方法

#####  直接定址法

取关键字的某个线性函数值作为地址
Hash(key)=key或Hash(key)=a*key+b

#####  除留余数法

假定散列表表长为m，取一个不大于m但接近m的质数p，按公式（Hash(key)=key%p）转化为地址

#####  数字分析法

#####  平方取中法

取关键字平方的中间的几位数作为散列地址

####  解决冲突的方法

#####  开放定址法

H~i~=(H(key)+d~i~)%m，m为列表场，d~i~为增量序列，下面为d~i~的取法

######  线性探测法

d~i~=1，2，3…….,m-1
容易造成大量元素在相邻的散列地址上堆积，导致查找效率降低

######  平方探测法

d~i~=1^2^,-1^2^,2^2^,-2^2^,…….,$\pm$k(k$\leq$m/2)
列表长度k必须是一个可以表示为4k+3的素数

######  双散列法

H~i~=(H(key)+i*Hash~2~(key))%m，i是冲突次数初始为0

######  伪随机序列法

d~i~为伪随机数序列

#####  链接法

对于H(key)=key MOD k，所有同义词放在一个线性链表中

####  性能分析

装填因子$\alpha$=$\frac{表中记录数n}{散列表长度m}$
$\alpha$越大表示记录越满，发生冲突的可能性越大
$\alpha$越小表示记录越空，发生冲突的可能性越小

ASL~成功~=查找到散列表中已存在结点的平均比较次数

ASL~失败~=查找失败，但找到插入位置1的平均比较次数

例题分析：PPT.94页
[一种例题讲解](https://www.bilibili.com/video/BV1i24y1Z7YJ/?spm_id_from=333.880.my_history.page.click&vd_source=307be9573968ac54e6581924fd6b6a67)

##  排序

###  插入排序

####  直接插入排序

基本思想：从后往前逐个插入

时间复杂度：O(N^2^)

空间复杂度：O(1)

稳定性：稳定

####  折半插入排序

基本思想：在直接插入排序的基础上，把查找插入位置换成折半查找

时间复杂度：O(N^2^)

空间复杂度：O(1)

稳定性：稳定

####  希尔排序

基本思想：表中距离为d~i~的多个元素进行直接插入排序，缩小d~i~至1（此处缩小为增量变化缩小，不是确定的，一般为除2取整）

时间复杂度：O(Nlog~2~N)与O(N^2^)之间，某个范围内为O(N^1.3^)

空间复杂度：O(1)

稳定性：不稳定

[希尔排序](https://www.bilibili.com/video/BV1Dv4y147ai/?spm_id_from=333.880.my_history.page.click)

注：希尔排序仅适用于线性表为顺序存储的情况

简略代码：

~~~c
void ShellSort(int a[],int n)
{
    int i,j,temp,d;
    for(d=n/2;d>=1;d=n/2){
        for(i=d;i<n;i++){
            temp=a[i];
            j=i-d;
            while(j>=0&&temp.key<a[j]){
                a[j+d]=a[j];
                j-=d;
            }
            a[j+d]=temp;
        }
    }
}
~~~



###  交换排序

####  冒泡排序

基本思想：从后往前，两两比较相邻数决定是否交换顺序，第一次冒泡后得到的最小元素不再参与下次比较，如果某次冒泡没有交换，则直接结束

时间复杂度：O(N^2^)

空间复杂度：O(1)

稳定性：稳定

####  快速排序

基本思想：选定一个基准(通常取首元素)，用首尾两个指针比较基准排序，得到左边的序列小于基准，右边的序列大于基准，再分别对左右序列进行快速排序

时间复杂度：O(Nlog~2~N)与O(N^2^)之间，常常认为是O(Nlog~2~N),越无序越快

空间复杂度：O(log~2~N)

稳定性：不稳定

简略代码：

~~~c
int Partition (int a[],int low,int high)	//一次划分
{
    int pivot=a[low];
    while(low<high){
        while(low<high&&a[high]>=pivot) high--;
        a[low]=a[high];
        while(low<high&&a[low]<=pivot) low++;
        a[high]=a[low];
    }
    a[low]=pivot;
    return low;
}

void Qsort(int a[],int low,int high)	//递归排序
{
    if(low<high){
        int temp=Partition(a,low,high);
    	Qsort(a,low,temp-1);
        Qsort(a,temp+1,high);
    }
}
~~~



###  选择排序

####  简单选择排序

基本思想：简单

时间复杂度：O(N^2^)

空间复杂度：O(1)

稳定性：不稳定

####  堆排序

**堆的定义**

>大顶堆：L(i)>=L(2i)且L(i)>=L(2i+1)	(双亲比左右子树结点都大)
>
>小顶堆：L(i)<=L(2i)且L(i)<=L(2i+1)	(双亲比左右子树结点都小)

基本思想：构造一个大顶堆或者小顶堆，输出堆顶元素，然后把堆底元素送到堆顶，再更新为大顶堆或者小顶堆进行上述操作，直至只剩一个根结点

构造顶堆思路：对于[n/2]向下取整再减一的结点与其子树进行比较交换，然后逐次向前取结点，直至根结点。

空间复杂度：O(1)

时间复杂度：O(Nlog~2~N)

稳定性：不稳定

###  归并排序和基数排序

####  归并排序

基本思想：如果说前面两种排序的思想是在一个整表内对各个元素进行排序，那么归并的思想就是将各个元素逐个的组成有序表，再由有序表一个一个组合新的有序表，最终组成整表。

说明：虽然基本思想是这么说，但基本上还是用2路归并，也就是先一分为二，两个有序表排序，对于这两个有序表再进行2路归并，由此可以很简单地想到递归思想，轻松写出代码

空间复杂度：O(N)

时间复杂度：O(Nlog~2~N)

稳定性：稳定

合并有序表(基于2路排序)：

~~~c
void Merge(int a[],int low,int mid,int high)
{
    int i,j,k;
    for(k=low;k<=high;k++)	b[k]=a[k];	//拷贝一个a数组
    for(i=low,j=mid+1,k=i;i<=mid&&j<=high;k++){	//i指向前一半序列，j指向后一半序列
        if(b[i]<b[j])	a[k]=b[i++];
        else	a[k]=b[j++];
    }
    while(i<=mid)	a[k++]=b[i++];
    while(j<=high)	a[k++]=b[j++];
}
~~~

####  基数排序

[基数排序最低位优先基本思想](https://www.bilibili.com/video/BV1YM4y1A7wi/?spm_id_from=333.880.my_history.page.click&vd_source=307be9573968ac54e6581924fd6b6a67)

排序过程
>分配：把Q~0~….Q~r-1~置为空队列，考察元素的位数值(个位，十位，百位….)，依次放入
>
>收集：从Q~0~到Q~r-1~中结点依次弹出得到新队列
>
>继续进行上述过程，执行次数为最高位数

空间复杂度：O(r)	[r是队列个数]

时间复杂度：O(d(n+r))	[d是分配和收集次数]

稳定性：稳定



























