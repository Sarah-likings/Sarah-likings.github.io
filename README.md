# Daily
the analysis of code  


analyse some code secrets  witch is difficult for me.

#include<iostream>
#include <string>
#include <stack>
#include <queue> 
using namespace std;
typedef struct node *BTree;
typedef struct node{
    char data;
    BTree lchild;
    BTree rchild;
}TNode;//树的结构
void CreateBTree(BTree &bt,string str);//创建二叉树 
void  DispBTree(BTree bt);//括号法输出二叉树
void PreOrder( BTree bt );//先序遍历二叉树
void InOrder(BTree bt);//中序遍历二叉树
void PostOrder(BTree bt);//后序遍历二叉树 
void LevelOrder(BTree bt);//层次遍历二叉树 
int flag=0;
int main()
{
    string str;
    BTree bt,p;
    str="A(B(D,F(E)),C(G(,H),I))";
    CreateBTree(bt,str);
    cout<<"btree:";
    DispBTree(bt);
    cout<<endl<<"preorder:";
    PreOrder(bt);
    flag=0;
    cout<<endl<<"inorder:";
    InOrder(bt);
    flag=0;
    cout<<endl<<"postorder:";
    PostOrder(bt);
    flag=0;
    cout<<endl<<"levelorder:";
     LevelOrder(bt);  
 } 
 void CreateBTree(BTree &bt, string str) {
    char ch;//当前读取的字符
    BTree stack[100], p = NULL;
    int top = -1;
    int k = 1; // 初始设置为左孩子
    int j = 0;
    bt = NULL; // 初始化根节点为空

    while ((ch = str[j++]) != '\0') {
        switch (ch) {
            case '(':
                top++;
                stack[top] = p; // 当前节点入栈
                k = 1;          // 下一个节点是左孩子
                break;
                
            case ',':
                k = 2;          // 下一个节点是右孩子
                break;
                
            case ')':
                top--;          // 当前子树处理完毕，弹出父节点
                break;
                
            default:
                p = (BTree)malloc(sizeof(TNode));
                p->data = ch;
                p->lchild = p->rchild = NULL;
                
                if (bt == NULL) {
                    bt = p;     // 第一个节点是根节点，只检查这一遍
                } else {
                    switch (k) {
                        case 1:
                            stack[top]->lchild = p; // 作为左孩子
                            break;
                        case 2:
                            stack[top]->rchild = p; // 作为右孩子
                            break;
                    }
                }
        }
    }
}

void DispBTree(BTree bt) {
    if (bt != NULL) {
        cout << bt->data;
        if (bt->lchild != NULL || bt->rchild != NULL) {
            cout << "(";
            DispBTree(bt->lchild);
            if (bt->rchild != NULL) {
                cout << ",";
                DispBTree(bt->rchild);
            }
            cout << ")";
        }
    }
}

void PreOrder(BTree bt) {
    if (bt != NULL) {
        if (flag++ > 0) cout << " ";
        cout << bt->data;
        PreOrder(bt->lchild);
        PreOrder(bt->rchild);
    }
}

void InOrder(BTree bt) {
    if (bt != NULL) {
        InOrder(bt->lchild);
        if (flag++ > 0) cout << " ";
        cout << bt->data;
        InOrder(bt->rchild);
    }
}

void PostOrder(BTree bt) {
    if (bt != NULL) {
        PostOrder(bt->lchild);
        PostOrder(bt->rchild);
        if (flag++ > 0) cout << " ";
        cout << bt->data;
    }
}

void LevelOrder(BTree bt) {
    if (bt == NULL) return;
    
    queue<BTree> q;
    q.push(bt);
    flag = 0;
    
    while (!q.empty()) {
        BTree p = q.front();
        q.pop();
        if (flag++ > 0) cout << " ";
        cout << p->data;
        
        if (p->lchild != NULL)
            q.push(p->lchild);
        if (p->rchild != NULL)
            q.push(p->rchild);
    }
}
