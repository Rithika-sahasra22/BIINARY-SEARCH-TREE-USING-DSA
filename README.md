# BIINARY-SEARCH-TREE-USING-DSA 

 #include<stdio.h>
#include<stdlib.h>
typedef struct bst{
	int data;
	struct bst * left,*right;
}node;
node *root,*curr,*parent;
void insert(int);
void inorder(node *);
void search(int);
void del(int);
void main(){
	int ch,ele;
	root=NULL;
	while(1){
	    printf("****************MENU********************\n");
		printf("\n1.Add\n2.Search\n3.Delete\n4.Display\n5.Exit\n");
		printf("Enter your choice: ");
		scanf("%d",&ch);
		switch(ch){
			case 1: printf("Enter element: ");
				scanf("%d",&ele);
				insert(ele);
				break;
			case 2: printf("Enter element to search: ");
				scanf("%d",&ele);
				search(ele);
				break;
			case 3: printf("Enter element to delete: ");
				scanf("%d",&ele);
				del(ele);
				break;
			case 4:	if(root==NULL)
					printf("Tree is empty\n");
				else{
					printf("Tree is: ");
					inorder(root);
				}
				break;
			case 5: exit(0);
			default:printf("Wrong input\n");
		}
	}
}
void insert(int ele){
	node *nn;
	curr=root;
	while(curr!=NULL){
		parent=curr;
		if(ele<curr->data)
			curr=curr->left;
		else if(ele>curr->data)
			curr=curr->right;
		else{
			printf("Duplicate keyes are not allowed\n");
			return;
		}
	}
	nn=(node*)malloc(sizeof(node));
	nn->data=ele;
	nn->left=nn->right=NULL;
	if(root!=NULL){
		if(ele<parent->data)
			parent->left=nn;
		else
			parent->right=nn;
	}
	else
		root=nn;
}
void search(int ele){
	int f=0;
	curr=root;
	while(curr!=NULL){
		if(curr->data==ele){
			printf("Element is present\n");
			f=1;
			break;
		}
		parent=curr;
		if(curr->data>ele)
			curr=curr->left;
		else
			curr=curr->right;
	}
	if(f==0)
		printf("Element nor present in tree\n");
}
void del(int ele){
	node *in_succ=NULL;
	if(root==NULL){
		printf("Tree is empty\n");
		return;
	}
	curr=root;
	parent=NULL;
	while(curr!=NULL){
		if(curr->data==ele)
			break;
		parent=curr;
		if(ele>curr->data)
			curr=curr->right;
		else
			curr=curr->left;
	}
	if(curr!=NULL){
		if(curr->left!=NULL && curr->right!=NULL){
			in_succ=curr->right;
			if(in_succ->left==NULL && in_succ->right==NULL){
				curr->data=in_succ->data;
				curr->right=NULL;
				free(in_succ);
			}
			else{
				if(in_succ->right!=NULL && in_succ->left==NULL){
					curr->data=in_succ->data;
					curr->right=in_succ->right;
					free(in_succ);
				}
				else{
					while(in_succ->left!=NULL){
						parent=in_succ;
						in_succ=in_succ->left;
					}
					curr->data=in_succ->data;
					if(in_succ->right!=NULL)
						parent->left=in_succ->right;
					else
						parent->left=NULL;
					free(in_succ);
				}
				printf("Node is deleted\n");
				return;
			}
		}
		if(curr->left!=NULL && curr->right==NULL){
			if(parent!=NULL){
				if(parent->left=curr)
					parent->left=curr->left;
				else
					parent->right=curr->left;
			}
			else
				root=curr->left;
			printf("Node is deleted\n");
			free(curr);
			return;
		}
		if(curr->left==NULL && curr->right!=NULL){
			if(parent!=NULL){
				if(parent->left==curr)
					parent->left=curr->right;
				else
					parent->right=curr->right;
			}
			else
				root=curr->right;
			printf("Node is deleted\n");
			free(curr);
			return;
		}
		if(curr->left==NULL && curr->right==NULL){
			if(parent!=NULL){
				if(parent->left==curr)
					parent->left=NULL;
				else
					parent->right=NULL;
			}
			else
				root=NULL;
			printf("Node is deleted\n");
			free(curr);
			return;
		}
	}
	else
		printf("Element not present in tree\n");
}
void inorder(node *root){
	if(root!=NULL){
		inorder(root->left);
		printf("%d ",root->data);
		inorder(root->right);
	}
}
