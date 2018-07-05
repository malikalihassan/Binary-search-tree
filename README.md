#include<iostream>
using namespace std;

struct binaryTreeNode
{
	int data;
	binaryTreeNode*llink;
	binaryTreeNode  *rlink;
};

class bSearchTree
{
	binaryTreeNode *root;
public:
	bSearchTree();//defult constructor
	~bSearchTree();
	binaryTreeNode* returnNode();
	void insertValue(const int & insertItem);
	bool Search(const int&searchItem);
	void inorderTraversal(binaryTreeNode *p);
	void preOrderTraversal(binaryTreeNode *p);
	void postOrderTraversal(binaryTreeNode *p);
	int Max(binaryTreeNode *p, int & max);
	int noOfLeaves(binaryTreeNode *p, int&y);
	void deletefuc(int data);
	binaryTreeNode* deleteNode(binaryTreeNode * &root, int iTem);
	int maxDepth(binaryTreeNode* node);//height finder
	binaryTreeNode*FindMin(binaryTreeNode* root);
	int SMax(binaryTreeNode *p, int & smax, int & max);
	void deleteAllleaves(binaryTreeNode * &p);
	int checkMaxNumber(binaryTreeNode *p, const int & maxi, const int &position);
	int nodesCounter();
	void nodeCount(binaryTreeNode*root, int &count);
	binaryTreeNode* rotate_right(binaryTreeNode* node);
	binaryTreeNode* rotate_left(binaryTreeNode* node);
	void createBone(binaryTreeNode*root);
	void createBalanceTree(binaryTreeNode*root);
	void makeRotation(double m);
};
void bSearchTree::createBalanceTree(binaryTreeNode*root)
{
	double n = nodesCounter();
	double m = floor(log(n + 1) / log(2.0));//first answer is 3
	m = pow(2, m) -  1;//here 8-1 answer is 7
	makeRotation(n - m);
	int k = (int)m;//only will take integer
	while (k>0)
	{
		k = k / 2;
		makeRotation(k);
	}
}
void bSearchTree::makeRotation(double m)
{
	binaryTreeNode*node = root;
	binaryTreeNode*temp = root;
	while (m>0)
	{
		if (temp == node)
		{
			node = rotate_left(node);
			root = node;
			temp = node;
			node = node->rlink;

		}
		else
		{
			temp->rlink = rotate_left(node);
			temp = temp->rlink;
			node = temp->rlink;
		}
		m--;
	}
}
void bSearchTree::createBone(binaryTreeNode*root)
{
	binaryTreeNode*temp = root;
	binaryTreeNode*grand = root;
	while (temp != NULL)
	{
		if (temp->llink != NULL)
		{
			if (temp != grand)
			{
				grand->rlink = rotate_right(root);
				temp = grand->rlink;
			}
			else
			{
				root = rotate_right(temp);
				temp = root;
				grand = temp;
			}
		}
		else
		{
			grand = temp;
			temp = temp->rlink;

		}
	}
}

binaryTreeNode* bSearchTree::rotate_right(binaryTreeNode* node)
{
	if (node->llink != NULL)
	{
		binaryTreeNode*temp;
		temp = node->llink;
		node = temp->rlink;
		temp->rlink = node;
		node = temp;
	}
	return node;
}
binaryTreeNode* bSearchTree::rotate_left(binaryTreeNode* node)
{
	if (node->rlink != NULL)
	{
		binaryTreeNode*temp;
		temp = node->rlink;
		node = temp->llink;
		temp->llink = node;
		node = temp;
	}
	return node;
}
int bSearchTree::nodesCounter()
{
	int count = 0;
	nodeCount(root, count);
	return count;
}
void bSearchTree::nodeCount(binaryTreeNode*p, int &count)
{
	if (p != NULL)
	{
		nodeCount(p->llink, count);
		{count++; }
		nodeCount(p->rlink, count);
	}
}

bSearchTree::bSearchTree()
{
	root = NULL;
}

bSearchTree ::~bSearchTree()
{
}

binaryTreeNode* bSearchTree::returnNode()
{
	return root;
}


void bSearchTree::inorderTraversal(binaryTreeNode *p)
{

	if (p != NULL)
	{
		inorderTraversal(p->llink);
		cout << p->data; cout << " " << endl;
		inorderTraversal(p->rlink);
	}
}
//Function to find minimum in a tree
binaryTreeNode*bSearchTree::FindMin(binaryTreeNode* root)
{
	while (root->llink != NULL)
		root = root->llink;
	return root;
}
int bSearchTree::Max(binaryTreeNode *p, int & max)//max number
{

	if (p != NULL)
	{
		Max(p->llink, max);
		if (p->data > max)
		{
			max = p->data;
		}
		Max(p->rlink, max);
	}
	return max;
}
int bSearchTree::SMax(binaryTreeNode *p, int & smax, int & max)//Smax number
{
	
	if (p != NULL)
	{
		SMax(p->llink, smax, max);
		
		if (p->data>smax&&p->data<max&&p->data!=max)
		{
			smax = p->data;
		
		}
		SMax(p->rlink, smax, max);
	}
	return smax;
}
int bSearchTree::checkMaxNumber(binaryTreeNode *p, const int & maxi, const int &position)//position max finder
{
	int max = maxi;
	int smax = 0;
	for (int i = 1; i < position; i++)
	{
		max = SMax(p, smax, max);
		smax = 0;
	}
	return max;
}
int bSearchTree::noOfLeaves(binaryTreeNode *p, int& y)//numberOfleaves
{

	if (p != NULL)
	{
		if (p->llink == NULL && p->rlink == NULL)
		{
			y++;
		}
		noOfLeaves(p->llink, y);
		p->data;
		noOfLeaves(p->rlink, y);
	}
	return y;
}


void bSearchTree::preOrderTraversal(binaryTreeNode *p)
{
	if (p != NULL)
	{
		cout << p->data; cout << " " << endl;
		preOrderTraversal(p->llink);
		preOrderTraversal(p->rlink);
	}
}

void bSearchTree::postOrderTraversal(binaryTreeNode *p)
{
	if (p != NULL)
	{
		postOrderTraversal(p->llink);
		postOrderTraversal(p->rlink);
		cout << p->data; cout << " " << endl;
	}
}
void bSearchTree::insertValue(const int & insertItem)
{
	binaryTreeNode * current = root;
	binaryTreeNode * trailCurrent = root;
	binaryTreeNode * newNode;
	newNode = new binaryTreeNode;
	newNode->data = insertItem;
	newNode->llink = NULL;
	newNode->rlink = NULL;
	if (root == NULL)
	{
		root = newNode;
	}
	else {
		while (current != NULL)
		{
			trailCurrent = current;
			if (current->data == insertItem)
			{
				cout << "duplicates are not allowed";
				return;
			}
			else if (current->data > insertItem)

			{
				current = current->llink;
			}
			else
			{
				current = current->rlink;
			}
		}
		if (trailCurrent->data > insertItem)
		{
			cout << trailCurrent->data; cout << "inseration left"; cout << newNode->data << endl;
			trailCurrent->llink = newNode;
		}
		else if (trailCurrent->data < insertItem)
		{
			cout << trailCurrent->data; cout << "inseration right"; cout << newNode->data << endl;
			trailCurrent->rlink = newNode;
		}
	}
}
bool bSearchTree::Search(const int&searchItem)
{
	bool found = false;
	binaryTreeNode * current = root;
	if (root == NULL)
	{
		return 0;
	}
	while (current != NULL & !found)
	{
		if (current->data == searchItem)
			found = true;
		else if (current->data>searchItem)
		{
			current = current->llink;
		}
		else
		{
			current = current->rlink;
		}
	}
	return found;
}
int bSearchTree::maxDepth(binaryTreeNode* node)
{
	if (node == NULL)
		return 0;
	else
	{
		/* compute the depth of each subtree */
		int lDepth = maxDepth(node->llink);
		int rDepth = maxDepth(node->rlink);

		/* use the larger one */
		if (lDepth > rDepth)
			return(lDepth + 1);
		else return(rDepth + 1);
	}
}
void bSearchTree::deletefuc(int data)
{
	deleteNode(root, data);

}
binaryTreeNode* bSearchTree::deleteNode(binaryTreeNode * &root, int data)//root must be update
{
	if (root == NULL)
		return root;
	else if (data < root->data)
		root->llink = deleteNode(root->llink, data);
	else if (data > root->data)
		root->rlink = deleteNode(root->rlink, data);
	// Wohoo... I found you, Get ready to be deleted	
	else {
		// Case 1:  No child
		if (root->llink == NULL && root->rlink == NULL) {
			delete root;
			root = NULL;
			cout << "hello case 1 no child";
		}
		//Case 2: One child 
		else if (root->llink == NULL) {
			cout << "hello case 2 left null";
			binaryTreeNode *temp = root;
			root = root->rlink;
			delete temp;
		}
		else if (root->rlink == NULL) {
			cout << "hello case 2 right null";
			binaryTreeNode *temp = root;
			root = root->llink;
			cout<<root->data;
			delete temp;

		}
		// case 3: 2 children
		else {
			cout << "hello last";
			binaryTreeNode *temp = FindMin(root->rlink);
			root->data = temp->data;
			root->rlink = deleteNode(root->rlink, temp->data);
		}
	}
	return root;
}

void bSearchTree::deleteAllleaves(binaryTreeNode * &p)//all leaves delete
{

	if (p != NULL)
	{
		if (p->llink == NULL && p->rlink == NULL)
		{
			binaryTreeNode* temp = p;
			cout << endl;
			cout << temp->data;
			cout << endl;
			delete temp;
			p = NULL;
		}
		else if (p->llink != NULL && p->rlink != NULL)
		{
			deleteAllleaves(p->llink);
			deleteAllleaves(p->rlink);
		}
		else if (p->llink == NULL && p->rlink != NULL)
		{
			deleteAllleaves(p->rlink);
		}
		else if (p->llink != NULL && p->rlink == NULL)
		{
			deleteAllleaves(p->llink);
		}
	}

}
int main()
{
	bSearchTree tree;
	int choice, value;
	do
	{
		cout << "please press 1 for insert" << endl;
		cout << "please press 2 for inorder print" << endl;
		cout << "please press 3 for preOrder print" << endl;
		cout << "please press 4 for postOrder print" << endl;
		cout << "please press 5 for search" << endl;
		cout << "please press 6 for Max" << endl;
		cout << "please press 7 for secondmaxNumber" << endl;
		cout << "please press 8 for number of leaves" << endl;
		cout << "please press 9 for hight of tree" << endl;
		//wr0ongcout << "please press 10 for number of nodes" << endl;
		cout << "please press 11 for delete" << endl;
		cout << "please press 12 for delete all leaves" << endl;
		cout << "please press 13 for max values at specific number" << endl;
		cout << "please press 14 for NodesCount" << endl;
		cout << "please press 15 for dsw balanced tree" << endl;
		cin >> choice;
		switch (choice)
		{

		case 1:
		{
			int myarray[14] = { 60,50,30 , 35, 32, 40, 48, 45, 53, 57, 70, 80, 75, 77 };
			/*cout << "please enter value";
			cin >> value;*/
			for (int i = 0; i < 14; i++)
			{
				tree.insertValue(myarray[i]);
			
			}
			break;
		}
		case 2:
		{binaryTreeNode*node=tree.returnNode();
		cout << node->data;
			tree.inorderTraversal(tree.returnNode());
			break;
		}case 3:
		{
			tree.preOrderTraversal(tree.returnNode());
			break;
		}case 4:
		{
			tree.postOrderTraversal(tree.returnNode());
			break;
		}
		case 5:
		{
			cout << "plz enter value for seacrh" << endl;
			cin >> value;
			tree.Search(value);
			break;
		}
		case 6:
		{   cout << endl; value = 0;
		cout << tree.Max(tree.returnNode(), value);
		cout << endl;
		break;
		}
		case 7:
		{   cout << endl; int value11 = 0; int value1 = tree.Max(tree.returnNode(), value);
		cout << tree.SMax(tree.returnNode(), value11, value1);
		cout << endl;
		break;
		}
		case 8:
		{
			value = 0;
			cout << endl;
			cout << tree.noOfLeaves(tree.returnNode(), value);
			cout << endl;
			break;
		}case 9:
		{
			cout << endl;
			cout << tree.maxDepth(tree.returnNode());
			cout << endl;
			break;
		}
		case 10:
		{
			cout << endl;
			break;
		}
		case 11:
		{
			cout << "plz enter item for delete" << endl;
			cin >> value;
			tree.deletefuc(value);
			break;
		}
		case 12:
		{
			binaryTreeNode*node = tree.returnNode();
			tree.deleteAllleaves(node);
			break;
		}
		case 13:
		{

			int value1 = tree.Max(tree.returnNode(), value);
			cout << "plz enter position" << endl;
			cin >> value;
			value1 = tree.checkMaxNumber(tree.returnNode(), value1, value);
			cout << value1;
			break;
		}
		case 14:
		{cout << " number  of nodes " << endl;
		cout << tree.nodesCounter();
		break;
		}
		default:
			break;
		}

	} while (true);
	system("pause");
	return 0;
}
