# Data-Structures-Algorithms-

前序遍历递归：
void preOrder(TreeNode* root) {
    if (!root) {
        return;
    }
    cout << root->val;
    preOrder(root->left);
    preOrder(root->right);
}



中序遍历递归：
void inOrder(TreeNode* root) {
    if (!root) {
        return;
    }
    inOrder(root->left);
    cout << root->val;
    inOrder(root->right);
}



后序遍历递归：
void postOrder(TreeNode* root) {
    if (!root) {
        return;
    }
    postOrder(root->left);
    postOrder(root->right);
    cout << root->val;
}



非递归版本：
可见递归版本实现起来非常简单，面试的时候，往往面试官会强制你写出非递归的版本，网上关于非递归版本的介绍也有很多，这里我分享一个自己认为是比较好记的版本。

显然，我们需要用一个stack来模拟递归时的函数调用。对于三种遍历，我们都使用push当前节点->push左子树->pop左子树->push右子树->pop右子树的方式。但是cout时机会有所不同。

对于前序遍历来说，每次访问到一个节点就cout；

对于中序遍历来说，每次将右子节点进栈时，把当前节点cout；

对于后序遍历来说，每次pop的时候cout。

另外我们还需要一个last_pop指针来存放上一个pop出去的节点。

如果当前节点的左右节点都不是上一个pop的节点，那么我们将左子节点入栈；

如果当前节点的左节点是上一个pop的节点，但右节点不是，那么就把右子节点入栈；

否则的话，就需要让当前节点出栈。

大致思路就是这样，俗话说Talk is cheap, let's coding. 直接上代码，注意三种遍历的代码总体结构都是完全一样的，只是cout的时机有所不同。

前序遍历非递归：
void preorder_traversal_iteratively(TreeNode* root)
{
    if (root == 0)
        return;
    stack<TreeNode*> s;
    s.push(root);
    cout << root->val << ' '; // visit root
    TreeNode* last_pop = root;
    while (!s.empty())
    {
        TreeNode* top = s.top();
        if (top->left != 0 && top->left != last_pop && top->right != last_pop) // push_left
        {
            s.push(top->left);
            cout << top->left->val << ' '; // visit top->left
        }
        else if (top->right != 0 && top->right != last_pop && (top->left == 0 || top->left == last_pop)) // push_right
        {
            s.push(top->right);
            cout << top->right->val << ' '; // visit top->right
        }
        else // pop
        {
            s.pop();
            last_pop = top;
        }
    }
}




中序遍历非递归：
void inorder_traversal_iteratively(TreeNode* root)
{
    if (root == 0)
        return;
    stack<TreeNode*> s;
    s.push(root);
    TreeNode* last_pop = root;
    while (!s.empty())
    {
        TreeNode* top = s.top();
        if (top->left != 0 && top->left != last_pop && top->right != last_pop) // push_left
        {
            s.push(top->left);
        }
        else if (top->right != 0 && top->right != last_pop && (top->left == 0 || top->left == last_pop)) // push_right
        {
            s.push(top->right);
            cout << top->val << ' '; // visit top
        }
        else // pop
        {
            s.pop();
            last_pop = top;
            if (top->right == 0)
                cout << top->val << ' '; // visit top
        }
    }
}




后序遍历非递归：
void postorder_traversal_iteratively(TreeNode* root)
{
    if (root == 0)
        return;
    stack<TreeNode*> s;
    s.push(root);   
    TreeNode* last_pop = root;
    while (!s.empty())
    {       
        TreeNode* top = s.top();
        if (top->left != 0 && top->left != last_pop && top->right != last_pop) // push_left
        {
            s.push(top->left);
        }
        else if (top->right != 0 && top->right != last_pop && (top->left == 0 || top->left == last_pop)) // push_right
        {
            s.push(top->right);
        }
        else // pop
        {
            s.pop();
            last_pop = top;
            cout << top->val << ' '; // visit top
        }
    }
}
