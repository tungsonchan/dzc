[toc]

## Leetcode

### [剑指offer]( https://leetcode-cn.com/problemset/lcof/ )

#### [面试题03. 数组中重复的数字](https://leetcode-cn.com/problems/shu-zu-zhong-zhong-fu-de-shu-zi-lcof/)：置换

~~~ c++
class Solution {
public:
    int findRepeatNumber(vector<int>& nums) {
        int i,n=nums.size(),j;
        for (int i=0;i<n;i++)
        if (nums[i]!=i)
        {
            while (nums[i]!=i)
            {
                if (nums[nums[i]]==nums[i]) return nums[i];
                j=nums[nums[i]];
                nums[nums[i]]=nums[i];
                nums[i]=j;
            }
        }
        return -1;
    }
};
~~~



#### [面试题04. 二维数组中的查找](https://leetcode-cn.com/problems/er-wei-shu-zu-zhong-de-cha-zhao-lcof/)：二分 + 双指针单调性

~~~ c++
class Solution {
public:
    bool findNumberIn2DArray(vector<vector<int>>& matrix, int target) {
        if (matrix.size()==0 || matrix[0].size()==0) return false;
        int d=0,h=matrix.size()-1,l=0,r=matrix[0].size()-1;
        return check(matrix, target, d,h,l,r);
    }

    bool check(vector<vector<int>>& matrix, int target,int d,int h,int l,int r)
    {
        //cout<<d<<" "<<h<<" "<<l<<" "<<r<<endl;
        if (d==h&&l==r)
        {
            if (target==matrix[d][l]) return true;
            else return false;
        }
        int mid1=(d+h)/2;
        int mid2=(l+r)/2;
        if (matrix[mid1][mid2]==target) return true;
        if (matrix[mid1][mid2]>target) return ((mid2-1>=0 && mid2-1>=l && check(matrix,target,d,h,l,mid2-1))|| 
        (mid1-1>=0 && mid1-1>=d && check(matrix,target,d,mid1-1,mid2,r)));
        else return((mid1+1<matrix.size() && mid1+1<=h && check(matrix,target,mid1+1,h,l,mid2)) || 
        (mid2+1<matrix[0].size() &&mid2+1<=r && check(matrix,target,d,h,mid2+1,r)));
    }
};
~~~

~~~ c++
class Solution {
public:
    bool findNumberIn2DArray(vector<vector<int>>& matrix, int target) {
        if (matrix.empty()) return false;
        int n=matrix.size(),m=matrix[0].size();
        int x=n-1,y=0;
        while (x>=0&&y<m)
        {
            //cout << matrix[x][y] <<" ";
            if (target==matrix[x][y]) return true;
            if (target<matrix[x][y]) x--;
            else y++;
        }
        return false;
    }
};
~~~

#### [面试题05. 替换空格](https://leetcode-cn.com/problems/ti-huan-kong-ge-lcof/)：模拟/字符串操作

~~~ c++
class Solution {
public:
    string replaceSpace(string s) {
        string ans;
        for (auto c:s)
        {
            if (c==' ') ans+="%20";
                   else ans+=c;
        }
        return ans;
    }
};
~~~

``` c++
class Solution {
public:
    string replaceSpace(string s)
    {
        string::size_type pos(0);
        while(true)
        {
            if((pos=s.find(" "))!=string::npos) s.replace(pos,1,"%20");
            else break;
        }
        return s; 
    }
};

```



#### [面试题06. 从尾到头打印链表](https://leetcode-cn.com/problems/cong-wei-dao-tou-da-yin-lian-biao-lcof/)

~~~ c++
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode(int x) : val(x), next(NULL) {}
 * };
 */
class Solution {
public:
    vector<int> reversePrint(ListNode* head) {
        vector<int> ans;
        ListNode* pre=nullptr;
        ListNode* now=head;
        ListNode* next=head;
        while (now)
        {
            next=now->next;
            now->next=pre;
            pre=now;
            now=next;
        }
        while (pre)
        {
            ans.push_back(pre->val);
            pre=pre->next;
        }
        return ans;
    }
};
~~~



#### [面试题07. 重建二叉树](https://leetcode-cn.com/problems/zhong-jian-er-cha-shu-lcof/)：递归/非递归 :star:

递归：

~~~ c++
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode(int x) : val(x), left(NULL), right(NULL) {}
 * };
 */
class Solution {
public:
    TreeNode* buildTree(vector<int>& preorder, vector<int>& inorder) {
        if (preorder.empty() || inorder.empty()) 
            return NULL;
        int mid=distance(begin(inorder),find(inorder.begin(),inorder.end(),preorder[0]));
        vector<int> preleft(preorder.begin()+1,preorder.begin()+mid+1);
        vector<int> preright(preorder.begin()+mid+1,preorder.end());
        vector<int> inleft(inorder.begin(),inorder.begin()+mid);
        vector<int> inright(inorder.begin()+mid+1,inorder.end());
        TreeNode *root=new TreeNode(preorder[0]);
        root->left=buildTree(preleft,inleft);
        root->right=buildTree(preright,inright);
        return root;
    }
};

~~~

非递归：

~~~ c++
class Solution {
public:
    TreeNode* buildTree(vector<int>& preorder, vector<int>& inorder) {
        if (preorder.empty() || inorder.empty()) 
            return NULL;
        TreeNode *root=new TreeNode(preorder[0]);
        int length = preorder.size();
        stack<TreeNode*> s;
        s.push(root);
        int inorderIndex = 0;
        for (int i = 1; i < length; i++) {
            int preorderVal = preorder[i];
            TreeNode *node = s.top();
            if (node->val != inorder[inorderIndex]) {
                node->left = new TreeNode(preorderVal);
                s.push(node->left);
            } else {
                while (!s.empty() && s.top()->val == inorder[inorderIndex]) {
                    node = s.top();
                    s.pop();
                    inorderIndex++;
                }
                node->right = new TreeNode(preorderVal);
                s.push(node->right);
            }
        }
        return root;
    }
};

~~~



#### [面试题09. 用两个栈实现队列](https://leetcode-cn.com/problems/yong-liang-ge-zhan-shi-xian-dui-lie-lcof/) ：栈

~~~ c++
class CQueue {
public:
    CQueue() {
    }
    
    void appendTail(int value) {
        s1.push(value);
    }
    
    int deleteHead() {
        if (s1.empty() && s2.empty()) return -1;
        int ans,h;
        if (s2.empty())
        {
            while (!s1.empty())
            {
                h=s1.top();
                s1.pop();
                s2.push(h);
            }
        }
        ans=s2.top();
        s2.pop();
        return ans;
    }
private:
    stack<int> s1;
    stack<int> s2;
};

/**
 * Your CQueue object will be instantiated and called as such:
 * CQueue* obj = new CQueue();
 * obj->appendTail(value);
 * int param_2 = obj->deleteHead();
 */
~~~



#### [面试题10- I. 斐波那契数列](https://leetcode-cn.com/problems/fei-bo-na-qi-shu-lie-lcof/)

~~~ c++
class Solution {
public:
    int fib(int n) {
        if (n==1) return 1;
        if (n==0) return 0;
        int n1=0,n2=1,ans=0;
        for (int i=2;i<=n;i++)
        {
            ans=(n1+n2)%1000000007;
            n1=n2;
            n2=ans;
        }
        return ans;
    }
};
~~~



#### [面试题10- II. 青蛙跳台阶问题](https://leetcode-cn.com/problems/qing-wa-tiao-tai-jie-wen-ti-lcof/)

~~~ c++
class Solution {
public:
    int numWays(int n) {
        const int mod = 1000000007;
        if (n==0) return 1;
        if (n==1) return 1;
        int n1=1,n2=1,ans=0;
        for (int i=2;i<=n;i++)
        {
            ans=(n1+n2)%1000000007;
            n1=n2;
            n2=ans;
        }
        return ans;
    }
};
~~~

#### [面试题11. 旋转数组的最小数字](https://leetcode-cn.com/problems/xuan-zhuan-shu-zu-de-zui-xiao-shu-zi-lcof/)：二分

~~~ c++
class Solution {
public:
    int minArray(vector<int>& numbers) {
        int n=numbers.size();
        if (n==0) return NULL;
        if (n==1) return numbers[0];
        int l=0,r=n-1,mid;
        while (l<r)
        {
            mid=(l+r)/2;
            if (numbers[mid]>numbers[r]) l=mid+1;
            else if (numbers[mid]<numbers[r]) r=mid;
            else r--;
        }
        return min(numbers[l],numbers[r]);
    }
};
~~~

#### [面试题12. 矩阵中的路径](https://leetcode-cn.com/problems/ju-zhen-zhong-de-lu-jing-lcof/)：DFS

~~~ c++
class Solution {
public:
    bool dfs(vector<vector<char>>& board, string word,vector<vector<bool>>& vis,int x,int y)
    {
        if (word.size()==0) return true;
        //cout << x << y << board[x][y]<<endl;
        int n=board.size();
        int m=board[0].size();
        int zx[4]={0,1,0,-1};
        int zy[4]={1,0,-1,0};
        for (int i=0;i<4;i++)
            if (x+zx[i]>=0&&x+zx[i]<n&&y+zy[i]>=0&&y+zy[i]<m)
                if (vis[x+zx[i]][y+zy[i]] && board[x+zx[i]][y+zy[i]]==word[0])
                {
                    vis[x+zx[i]][y+zy[i]]=false;
                    if (dfs(board,word.substr(1),vis,x+zx[i],y+zy[i])) return true;
                    vis[x+zx[i]][y+zy[i]]=true;
                }
        return false;
    }

    bool exist(vector<vector<char>>& board, string word) {
        if (word.size()==0) return true;
        int n=board.size();
        int m=board[0].size();
        vector<vector<bool>> vis(n,vector<bool>(m,true));
        for (int i=0;i<n;i++)
            for (int j=0;j<m;j++)
            if (board[i][j]==word[0])
            {
                vis[i][j]=false;
                if (dfs(board,word.substr(1),vis,i,j)) return true;
                vis[i][j]=true;
            }
        return false;
    }
    
};
~~~

#### [面试题13. 机器人的运动范围](https://leetcode-cn.com/problems/ji-qi-ren-de-yun-dong-fan-wei-lcof/):DFS

~~~ c++
class Solution {
public:
    int movingCount(int m, int n, int k) {
        vector<vector<bool>> vis(m,vector<bool>(n,false));
        if (calc(0,0)<=k) dfs(m,n,k,vis,0,0);
        int ans=0;
        for (int i=0;i<m;i++)
            for (int j=0;j<n;j++)
                if (vis[i][j]) ans++;
        return ans;
    }

    void dfs(int m,int n,int k,vector<vector<bool>> &vis,int x,int y)
    {
        vis[x][y]=true;
        int zx[4]={0,1,0,-1};
        int zy[4]={1,0,-1,0};
        for (int i=0;i<4;i++)
            if (x+zx[i]>=0&&x+zx[i]<m&&y+zy[i]>=0&&y+zy[i]<n && !vis[x+zx[i]][y+zy[i]] && calc(x+zx[i],y+zy[i])<=k)
                dfs(m,n,k,vis,x+zx[i],y+zy[i]);
        return;
    }

    int calc(int i,int j)
    {
        int ans=0;
        while (i>0)
        {
            ans+=i%10;
            i/=10;
        }
        while (j>0)
        {
            ans+=j%10;
            j/=10;
        }
        return ans;
    }
};
~~~

#### [面试题14- I. 剪绳子](https://leetcode-cn.com/problems/jian-sheng-zi-lcof/):DP

~~~ c++
class Solution {
public:
    int cuttingRope(int n) {
        vector<int> f(n+1,0);
        if (n==1) return 1;
        if (n==2) return 1;
        f[1]=1; f[2]=2;
        for (int i=3;i<=n;i++)
            for (int j=1;j<i;j++)
                f[i]=max(f[i],max(f[j],j)*(i-j));
        return f[n];
    }
};
~~~

#### [面试题14- II. 剪绳子 II](https://leetcode-cn.com/problems/jian-sheng-zi-ii-lcof/)：贪心+快速幂

~~~ c++
class Solution {
public:
    int cuttingRope(int n) {
        if (n<4) return n-1;
        int mod=1000000007;
        long ans=1,x=3;
        int a=n/3-1;
        while (a>0)
        {
            if (a%2==1) ans=(ans*x)%mod;
            x=(x*x)%mod;
            a/=2;
        }
        if (n%3==0) ans*=3;
        if (n%3==1) ans*=4;
        if (n%3==2) ans*=6;
        return ans%mod;
    }
};
~~~

#### [面试题15. 二进制中1的个数](https://leetcode-cn.com/problems/er-jin-zhi-zhong-1de-ge-shu-lcof/)：位运算

~~~ c++
class Solution {
public:
    int hammingWeight(uint32_t n) {
        int ans=0;
        while (n>0){
            ans+=n&1;
            n>>=1;
        }
        return ans;
    }
};
~~~

~~~ c++
class Solution {
public:
    int hammingWeight(uint32_t n) {
        int ans=0;
        while (n>0){
            ans++;
            n&=n-1;
        }
        return ans;
    }
};
~~~

#### [面试题16. 数值的整数次方](https://leetcode-cn.com/problems/shu-zhi-de-zheng-shu-ci-fang-lcof/)：快速幂

~~~ c++
class Solution {
public:
    double myPow(double x, int n) {
        long long m=n;
        if (m<0)
        {
            x=1/x;
            m=-m;
        }
        double ans=1,h=x;
        for (long long i=m;i;i/=2)
        {
            if (i%2==1) ans=ans*h;
            h=h*h;
        }
        return ans;
    }
};
~~~

#### [面试题18. 删除链表的节点](https://leetcode-cn.com/problems/shan-chu-lian-biao-de-jie-dian-lcof/)

~~~ c++
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode(int x) : val(x), next(NULL) {}
 * };
 */
class Solution {
public:
    ListNode* deleteNode(ListNode* head, int val) {
        if (head->val==val)
        {
            return head->next;
        }
        ListNode* h=head;
        ListNode* pre=head;
        while (h->val!=val)
        {
            pre=h;
            h=h->next;
        }
        pre->next=h->next;
        delete h;
        return head;
    }
};
~~~

#### [面试题19. 正则表达式匹配](https://leetcode-cn.com/problems/zheng-ze-biao-da-shi-pi-pei-lcof/):DP

~~~ c++
class Solution {
public:
    bool isMatch(string s, string p) {
        int n=s.size();
        int m=p.size();
        bool f[n+1][m+1];
        memset(f,false,sizeof(f));
        f[0][0]=true;
        for (int j=0;j<m;j++)
        	if (p[j]=='*'&&f[0][j-1]) f[0][j+1]=true;
        for (int i=0;i<n;i++)
            for (int j=0;j<m;j++)
            {
            	if (s[i]==p[j]||p[j]=='.') f[i+1][j+1]=f[i][j];
            	if (p[j]=='*')
            	{
            		if (s[i]!=p[j-1]&&p[j-1]!='.') f[i+1][j+1]=f[i+1][j-1];
                	else
						f[i+1][j+1]=f[i+1][j]||f[i][j+1]||f[i+1][j-1];
  				}
            	
			}
        return f[n][m];
    }
};
~~~

#### [面试题20. 表示数值的字符串](https://leetcode-cn.com/problems/biao-shi-shu-zhi-de-zi-fu-chuan-lcof/) ：模拟 :star:

~~~ c++
class Solution {
public:
    bool isNumber(string s) {
        if (s.empty()) return false;
        int l=0,r=s.size();
        while (s[l]==' ') l++;
        while (s[r-1]==' ') r--;
        s=s.substr(l,r-l);
        if (s.empty()) return false;
        int e=s.find('e');
        if (e==string::npos) return judgeP(s);
                    else return judgeP(s.substr(0,e))&&judgeS(s.substr(e+1));
    }

    bool judgeP(string s)
    {
        bool result=false,point=false;
        int n=s.size();
        for(int i=0;i<n;++i)
        {
            if (s[i]=='+'||s[i]=='-')
            {
                if (i!=0)return false;
            }
            else if (s[i]=='.')
            {
                if (point) return false;
                point=true;
            }
            else if(s[i]<'0'||s[i]>'9') return false;
            else result=true;
        }
        return result;
    }

    bool judgeS(string s)
    {   
        bool result=false;
        for(int i=0;i<s.size();++i)
        {
            if(s[i]=='+'||s[i]=='-')
            {
                if(i!=0)return false;
            }
            else if(s[i]<'0'||s[i]>'9') return false;
            else result=true;
        }
        return result;
    }
};
~~~

#### [面试题21. 调整数组顺序使奇数位于偶数前面](https://leetcode-cn.com/problems/diao-zheng-shu-zu-shun-xu-shi-qi-shu-wei-yu-ou-shu-qian-mian-lcof/)

~~~ c++
class Solution {
public:
    vector<int> exchange(vector<int>& nums) {
        if (nums.size()<2) return nums;
        int n=nums.size();
        int l=0,r=n-1;
        while (l<r)
        {
            while (l<n&&nums[l]%2==1) l++;
            while (r>=0&&nums[r]%2==0) r--;
            if (l<r)
            {
                int x=nums[l];
                nums[l]=nums[r];
                nums[r]=x;
            }
            l++;
            r--;
        }
        return nums;
    }
};
~~~

#### [面试题22. 链表中倒数第k个节点](https://leetcode-cn.com/problems/lian-biao-zhong-dao-shu-di-kge-jie-dian-lcof/)

~~~ c++
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode(int x) : val(x), next(NULL) {}
 * };
 */
class Solution {
public:
    ListNode* getKthFromEnd(ListNode* head, int k) {
        ListNode* h=head;
        int n=0;
        while (h!=NULL)
        {
            n++;
            h=h->next;
        }
        h=head;
        n=n-k;
        while (n--) h=h->next;
        return h;
    }
};
~~~

#### [面试题24. 反转链表](https://leetcode-cn.com/problems/fan-zhuan-lian-biao-lcof/)

~~~ c++
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode(int x) : val(x), next(NULL) {}
 * };
 */
class Solution {
public:
    ListNode* reverseList(ListNode* head) {
        ListNode* pre=nullptr;
        ListNode* now=head;
        ListNode* next=head;
        while (now)
        {
            next=now->next;
            now->next=pre;
            pre=now;
            now=next;
        }
        return pre;
    }
};
~~~

#### [面试题25. 合并两个排序的链表](https://leetcode-cn.com/problems/he-bing-liang-ge-pai-xu-de-lian-biao-lcof/):star:

~~~ c++
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode(int x) : val(x), next(NULL) {}
 * };
 */
class Solution {
public:
    ListNode* mergeTwoLists(ListNode* l1, ListNode* l2) {
        ListNode* head=new ListNode(0);
        ListNode* now=head;
        while (l1 && l2)
        {
            if (l1->val<l2->val)
            {
                now->next=l1;
                l1=l1->next;
            }
            else
            {
                now->next=l2;
                l2=l2->next;
            }
            now=now->next;
        }
        if (l1) now->next=l1;
        else now->next=l2;
        return head->next;
    }
};
~~~

#### [面试题26. 树的子结构](https://leetcode-cn.com/problems/shu-de-zi-jie-gou-lcof/)：递归

~~~ c++
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode(int x) : val(x), left(NULL), right(NULL) {}
 * };
 */
class Solution {
public:
    bool isSubStructure(TreeNode* A, TreeNode* B) {
        if (!A||!B) return false;
        bool ret=false;
        if (A->val == B->val) ret=iswhole(A,B);
        if (A->left!=NULL) ret|=isSubStructure(A->left,B);
        if (A->right!=NULL) ret|=isSubStructure(A->right,B);
        return ret;
    }

    bool iswhole(TreeNode* A,TreeNode* B)
    {
        if (!B) return true;
        if (!A) return false;
        if (A->val == B->val) 
            return iswhole(A->left,B->left)&&iswhole(A->right,B->right);
        else return false;
    }
};
~~~

#### [面试题27. 二叉树的镜像](https://leetcode-cn.com/problems/er-cha-shu-de-jing-xiang-lcof/)

~~~ C++
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode(int x) : val(x), left(NULL), right(NULL) {}
 * };
 */
class Solution {
public:
    TreeNode* mirrorTree(TreeNode* root) {
        if (root==NULL) return root;
        TreeNode* l=mirrorTree(root->left);
        TreeNode* r=mirrorTree(root->right);
        root->left=r;
        root->right=l;
        return root;
    }
};
~~~

#### [面试题28. 对称的二叉树](https://leetcode-cn.com/problems/dui-cheng-de-er-cha-shu-lcof/)

~~~ c++
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode(int x) : val(x), left(NULL), right(NULL) {}
 * };
 */
class Solution {
public:
    bool isSymmetric(TreeNode* root) {
        if (!root) return true;
        return check(root->left,root->right);
    }
    bool check(TreeNode* left,TreeNode* right)
    {
        if (!left&&!right) return true;
        if (!left || !right) return false;
        if (left->val==right->val && check(left->left,right->right)&&check(left->right,right->left)) return true;
        return false;
    }
};
~~~

#### [面试题29. 顺时针打印矩阵](https://leetcode-cn.com/problems/shun-shi-zhen-da-yin-ju-zhen-lcof/)：模拟:star:

~~~ c++
class Solution {
public:
    vector<int> spiralOrder(vector<vector<int>>& matrix) {
        vector<int> ans;
        if (matrix.empty() || matrix[0].empty()) return ans;
        int n=matrix.size();
        int m=matrix[0].size();
        int total=n*m;
        vector<pair<int,int>> add={{0,1},{1,0},{0,-1},{-1,0}};
        int x=0,y=0;
        int up=0,down=n-1,left=0,right=m-1;
        int d=0;
        int change=0;
        ans.push_back(matrix[x][y]);
        for (int i=1;i<total;i++)
        {
            x+=add[d].first;
            y+=add[d].second;
            while (x<up||x>down||y<left||y>right)
            {
                x-=add[d].first;
                y-=add[d].second;
                d=(d+1)%4;
                x+=add[d].first;
                y+=add[d].second;
                change++;
                if (change>1) return ans;
            }
            if (change>0)
            {
                change=0;
                if (d==0) left++;
                if (d==1) up++;
                if (d==2) right--;
                if (d==3) down--;
            }
            ans.push_back(matrix[x][y]);
        }
        return ans;
    }
};
~~~

#### [面试题30. 包含min函数的栈](https://leetcode-cn.com/problems/bao-han-minhan-shu-de-zhan-lcof/) 

~~~ c++
class MinStack {
public:
    /** initialize your data structure here. */
    stack<int> s,smin;
    MinStack() {

    }
    
    void push(int x) {
        s.push(x);
        if (smin.empty()) smin.push(x);
        else 
        {
            if (x<smin.top()) smin.push(x);
            else smin.push(smin.top());
        }
    }
    
    void pop() {
        if (s.empty()) return;
        s.pop();
        smin.pop();
    }
    
    int top() {
        return s.top();
    }
    
    int min() {
        return smin.top();
    }
};

/**
 * Your MinStack object will be instantiated and called as such:
 * MinStack* obj = new MinStack();
 * obj->push(x);
 * obj->pop();
 * int param_3 = obj->top();
 * int param_4 = obj->min();
 */
~~~

#### [面试题31. 栈的压入、弹出序列](https://leetcode-cn.com/problems/zhan-de-ya-ru-dan-chu-xu-lie-lcof/)：栈

~~~ c++
class Solution {
public:
    bool validateStackSequences(vector<int>& pushed, vector<int>& popped) {
        if (pushed.empty()||popped.empty()) return true;
        stack<int> s;
        int l=0;
        for (int i=0;i<pushed.size();i++)
        {
            s.push(pushed[i]);
            while (!s.empty() && s.top()==popped[l])
            {
                l++;
                //cout<<s.top()<<endl;
                s.pop();
            }
        }
        if (l==popped.size()) return true;
        else return false;
    }
};
~~~

#### [面试题32 - I. 从上到下打印二叉树](https://leetcode-cn.com/problems/cong-shang-dao-xia-da-yin-er-cha-shu-lcof/)：队列:star:

~~~ c++
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode(int x) : val(x), left(NULL), right(NULL) {}
 * };
 */
class Solution {
public:
    vector<int> levelOrder(TreeNode* root) {
        vector<int> ans;
        if (!root) return ans;
        queue<TreeNode*> q;
        q.push(root);
        while (!q.empty())
        {
            TreeNode* now=q.front(); 
            q.pop();
            ans.push_back(now->val);
            if(now->left) q.push(now->left);
            if(now->right) q.push(now->right);
        }
        return ans;
    }
};
~~~

#### [面试题32 - II. 从上到下打印二叉树 II](https://leetcode-cn.com/problems/cong-shang-dao-xia-da-yin-er-cha-shu-ii-lcof/)

~~~ c++
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode(int x) : val(x), left(NULL), right(NULL) {}
 * };
 */
class Solution {
public:
    vector<vector<int>> levelOrder(TreeNode* root) {
        vector<vector<int>> ans;
        if (!root) return ans;
        queue<TreeNode*> q;
        q.push(root);
        while (!q.empty())
        {
            int len=q.size();
            vector<int> h;
            for (int i=0;i<len;i++)
            {
                TreeNode* now=q.front(); 
                q.pop();
                h.push_back(now->val);
                if(now->left) q.push(now->left);
                if(now->right) q.push(now->right);
            }
            ans.push_back(h);
        }
        return ans;
    }
};
~~~

#### [面试题32 - III. 从上到下打印二叉树 III](https://leetcode-cn.com/problems/cong-shang-dao-xia-da-yin-er-cha-shu-iii-lcof/)：BFS->Queue+Stack :star:

~~~ c++
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode(int x) : val(x), left(NULL), right(NULL) {}
 * };
 */
class Solution {
public:
    vector<vector<int>> levelOrder(TreeNode* root) {
        vector<vector<int>> ans;
        if (!root) return ans;
        queue<TreeNode*> q;
        stack<TreeNode*> sta;
        q.push(root);
        int change=0;
        while (!q.empty())
        {
            int len=q.size();
            vector<int> h;
            for (int i=0;i<len;i++)
            {
                TreeNode* now=q.front(); 
                q.pop();
                h.push_back(now->val);
                if (change==0)
                {
                    if(now->left) sta.push(now->left);
                    if(now->right) sta.push(now->right);
                }
                else
                {
                    if(now->right) sta.push(now->right);
                    if(now->left) sta.push(now->left);
                }
            }
            change=1-change;
            ans.push_back(h);
            while(!sta.empty()){
                TreeNode *temp=sta.top();
                q.push(temp);
                sta.pop();
            }
        }
        return ans;
    }
};
~~~

#### [面试题33. 二叉搜索树的后序遍历序列](https://leetcode-cn.com/problems/er-cha-sou-suo-shu-de-hou-xu-bian-li-xu-lie-lcof/):star:

~~~ c++
class Solution {
public:
    bool verifyPostorder(vector<int>& postorder) {
        if(postorder.size()<2) return true;
        return isPostorder(postorder,0,postorder.size()-1);
    }

    bool isPostorder(vector<int>& postorder, int l, int r)
    {
        if (l>=r) return true;
        int i=l;
        while (postorder[i]<postorder[r]) i++;
        int bound=i;
        for (;i<r;i++)
          if(postorder[i]<postorder[r]) return false;
        return isPostorder(postorder,l,bound-1) && isPostorder(postorder,bound,r-1);
    }
};
~~~

#### [面试题34. 二叉树中和为某一值的路径](https://leetcode-cn.com/problems/er-cha-shu-zhong-he-wei-mou-yi-zhi-de-lu-jing-lcof/)

~~~ c++
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode(int x) : val(x), left(NULL), right(NULL) {}
 * };
 */
class Solution {
public:
    vector<vector<int>> pathSum(TreeNode* root, int sum) {
        vector<vector<int>> ans;
        vector<int> path;
        dfs(root,sum,ans,path,0);
        return ans;
    }

    void dfs(TreeNode* root, int sum,vector<vector<int>>& ans,vector<int>& path,int now)
    {
        if (!root) return;
        if (now+root->val==sum&&root->left==NULL&&root->right==NULL)
        {
            path.push_back(root->val);
            ans.push_back(path);
            path.pop_back();
            return;
        }
        path.push_back(root->val);
        dfs(root->left,sum,ans,path,now+root->val);
        dfs(root->right,sum,ans,path,now+root->val);
        path.pop_back();
    }

};
~~~

#### [面试题35. 复杂链表的复制](https://leetcode-cn.com/problems/fu-za-lian-biao-de-fu-zhi-lcof/)：DFS/BFS :star:

~~~ c++
/*
// Definition for a Node.
class Node {
public:
    int val;
    Node* next;
    Node* random;
    
    Node(int _val) {
        val = _val;
        next = NULL;
        random = NULL;
    }
};
*/
class Solution {
public:
     Node* copyRandomList(Node* head) {
        unordered_map<Node*, Node*> umap;
        return copyRandomList_helper(umap, head);
    }

    Node* copyRandomList_helper(unordered_map<Node*, Node*> &umap, Node* node) {
        if (node == nullptr) return node;
        auto res = umap.find(node);
        if (res != umap.end()) return res->second;
        Node* newNode = new Node(node->val);
        umap[node] = newNode;
        newNode->next = copyRandomList_helper(umap, node->next);
        newNode->random = copyRandomList_helper(umap, node->random);
        return newNode;
    }
};
~~~

~~~ c++
/*
// Definition for a Node.
class Node {
public:
    int val;
    Node* next;
    Node* random;
    
    Node(int _val) {
        val = _val;
        next = NULL;
        random = NULL;
    }
};
*/
class Solution {
public:
    Node* copyRandomList(Node* head) {
        if (head == nullptr) return head;
        queue<Node *> quk;
        quk.push(head);
        unordered_map<Node *, Node *> umap;
        Node *newHead = new Node(head->val);
        umap[head] = newHead;
        while (!quk.empty()) {
            Node *cur = quk.front();
            quk.pop();
            if (cur->next != nullptr) {
                auto res = umap.find(cur->next);
                if (res == umap.end()) {
                    quk.push(cur->next);
                    umap[cur->next] = new Node(cur->next->val);
                }
                umap[cur]->next = umap[cur->next];
            }
            if (cur->random != nullptr) {
                auto res = umap.find(cur->random);
                if (res == umap.end()) {
                    quk.push(cur->random);
                    umap[cur->random] = new Node(cur->random->val);
                }
                umap[cur]->random = umap[cur->random];
            }
        }
        return newHead;
    }
};

~~~

#### [面试题36. 二叉搜索树与双向链表](https://leetcode-cn.com/problems/er-cha-sou-suo-shu-yu-shuang-xiang-lian-biao-lcof/)：中序遍历:star:

~~~ c++
/*
// Definition for a Node.
class Node {
public:
    int val;
    Node* left;
    Node* right;

    Node() {}

    Node(int _val) {
        val = _val;
        left = NULL;
        right = NULL;
    }

    Node(int _val, Node* _left, Node* _right) {
        val = _val;
        left = _left;
        right = _right;
    }
};
*/
class Solution {
public:
    Node* pre=NULL;
    Node* head=NULL;
    Node* tail=NULL;

    void inorder(Node* root) {
        if (root == NULL) return;
        inorder(root->left);
        if (pre == NULL) head = root;
        else pre->right = root;
        root->left=pre;
        pre=root;
        tail=root;
        inorder(root->right);
        return;
    }
    
    Node* treeToDoublyList(Node* root)
    {
        if (root == NULL) return NULL;
        inorder(root);
        head->left=tail;
        tail->right=head;
        return head;
    }
};
~~~

#### [面试题37. 序列化二叉树](https://leetcode-cn.com/problems/xu-lie-hua-er-cha-shu-lcof/):star:

~~~ c++
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode(int x) : val(x), left(NULL), right(NULL) {}
 * };
 */
class Codec {
public:

    // Encodes a tree to a single string.
    string serialize(TreeNode* root) 
    {
        string ans;
        stack <TreeNode*> q;
        q.push(root);
        while (!q.empty())
        {
            TreeNode* p=q.top();
            q.pop();
            if (p)
            {
                q.push(p->right);
                q.push(p->left);
                ans+=to_string(p->val)+",";
            }
            else ans+="null,";
        }
        ans.erase(ans.end()-1);
        return ans;
    }

    // Decodes your encoded data to tree.
    TreeNode* deserialize(string data) 
    {
        TreeNode* ans;
        stringstream ss;
        ss << data;
        string temp;
        vector<string> instr;
        while (getline(ss, temp, ','))
        {
            instr.push_back(temp);
        }
        int start=0;
        ans=createTree(instr, start);
        return ans;
    }

    TreeNode* createTree(const vector<string> &instr, int &start)
    {
        if(instr[start] == "null")
        {
            start++;
            return nullptr;
        }
        TreeNode* root = new TreeNode(stoi(instr[start]));
        start++;
        root->left = createTree(instr, start);
        root->right = createTree(instr, start);
        return root;
    }
};

// Your Codec object will be instantiated and called as such:
// Codec codec;
// codec.deserialize(codec.serialize(root));
~~~

#### [面试题38. 字符串的排列](https://leetcode-cn.com/problems/zi-fu-chuan-de-pai-lie-lcof/)：dfs+set/swap去重 :star:

~~~ c++
class Solution {
public:
    vector<string> permutation(string s) {
        vector<string> ans;
        dfs(s, 0, ans);
        return ans;
    }
    void dfs(string& s, int pos, vector<string>& ans) {
        if (pos >= s.size()) {
            ans.push_back(s);
            return;
        }
        for (int i = pos; i < s.size(); i++) {
            if (judge(s, pos, i)) continue;
            swap(s[pos], s[i]);
            dfs(s, pos+1, ans);
            swap(s[pos], s[i]);
        }
    }

    bool judge(string& s, int start, int end) {
        for (int i = start; i < end; i++) {
            if (s[i] == s[end]) return true;
        }
        return false;
    }
};
~~~

set

~~~ c++
class Solution {
public:
    vector<string> permutation(string s) {
        vector<string> ans;
        set<string> result;
        dfs(s, 0, result);
        for (auto i:result) ans.push_back(i);
        return ans;
    }
    void dfs(string& s, int pos, set<string>& result) {
        if (pos >= s.size()) {
            result.insert(s);
            return;
        }
        for (int i = pos; i < s.size(); i++) {
            swap(s[pos], s[i]);
            dfs(s, pos+1, result);
            swap(s[pos], s[i]);
        }
    }
};
~~~

#### [面试题39. 数组中出现次数超过一半的数字](https://leetcode-cn.com/problems/shu-zu-zhong-chu-xian-ci-shu-chao-guo-yi-ban-de-shu-zi-lcof/)：抵消:star:

~~~ c++
class Solution {
public:
    int majorityElement(vector<int>& nums) {
        if (nums.size()==1) return nums[0];
        while (nums.size()>2)
        {
            int l=0;
            int r=1;
            while (r<nums.size()&&nums[r]==nums[l]) r++;
            if (r==nums.size()) break;
            //cout<<nums[l]<<" "<<nums[r]<<endl;
            swap(nums[r],nums[nums.size()-1]);
            nums.pop_back();
            swap(nums[l],nums[nums.size()-1]);
            nums.pop_back();
        }
        //cout<<nums.size()<<endl;
        return nums[0];
    }
};
~~~

#### [面试题40. 最小的k个数](https://leetcode-cn.com/problems/zui-xiao-de-kge-shu-lcof/)：堆 multiset/priority_queue:star:

~~~ c++
class Solution {
public:
    vector<int> getLeastNumbers(vector<int>& arr, int k) {
        if (arr.empty()||k==0||k>arr.size()) return {};
        multiset<int> res;
        vector<int> ans;
        for (auto i:arr)
        {
            if (res.size()<k) res.insert(i);
            else
            {
                auto tmp=res.end();
                tmp--;
                if (i<*tmp)
                {
                    res.erase(tmp);
                    res.insert(i);
                }
            }
        }
        for (auto i:res) ans.push_back(i);
        return ans;
    }
};
~~~

~~~ c++
class Solution {
public:
    vector<int> getLeastNumbers(vector<int>& arr, int k) {
        if (arr.empty()||k==0||k>arr.size()) return {};
        vector<int> ans;
        priority_queue<int> pq;
        for (int i = 0; i < arr.size(); i++) 
        {
            pq.push(arr[i]);
            if (i >= k) {
                pq.pop();
            }
        }
        while (!pq.empty()) {
            ans.push_back(pq.top());
            pq.pop();
        }
        return ans;
    }
};
~~~

#### [面试题41. 数据流中的中位数](https://leetcode-cn.com/problems/shu-ju-liu-zhong-de-zhong-wei-shu-lcof/)：双堆:star:

~~~ c++
class MedianFinder {
    priority_queue<int> lo;                            
    priority_queue<int, vector<int>, greater<int>> hi;  
public:
    /** initialize your data structure here. */
    MedianFinder() {

    }
    void addNum(int num)
    {
        lo.push(num); 
        hi.push(lo.top());
        lo.pop();
        if (lo.size() < hi.size()) {         
            lo.push(hi.top());
            hi.pop();
        }
    }

    // Returns the median of current data stream
    double findMedian()
    {
        return lo.size() > hi.size() ? (double) lo.top() : (lo.top() + hi.top()) * 0.5;
    }

};

/**
 * Your MedianFinder object will be instantiated and called as such:
 * MedianFinder* obj = new MedianFinder();
 * obj->addNum(num);
 * double param_2 = obj->findMedian();
 */
~~~

#### [面试题42. 连续子数组的最大和](https://leetcode-cn.com/problems/lian-xu-zi-shu-zu-de-zui-da-he-lcof/)

~~~ c++
class Solution {
public:
    int maxSubArray(vector<int>& nums) {
        int n=nums.size();
        int h=nums[0];
        int ans=nums[0];
        for (int i=1;i<n;i++)
        {
            h=max(nums[i],h+nums[i]);
            //cout<<i<<":"<<h<<endl;
            ans=max(ans,h);
        }
        return ans;
    }
};
~~~

#### [面试题43. 1～n整数中1出现的次数](https://leetcode-cn.com/problems/1nzheng-shu-zhong-1chu-xian-de-ci-shu-lcof/)：数学规律

~~~ c++
class Solution {
public:
    int countDigitOne(int n) {
        int ans=0;
        for (long long i=1; i<=n; i*=10)
        {
            long long divider=i*10;
            ans+=(n/divider)*i+min(max(n%divider-i+1,(long long)0),i);
        }
        return ans;
    }
};
~~~

#### [面试题44. 数字序列中某一位的数字](https://leetcode-cn.com/problems/shu-zi-xu-lie-zhong-mou-yi-wei-de-shu-zi-lcof/)：找规律:star:

~~~ c++
class Solution {
public:
    int findNthDigit(int n) {
        long base = 9,digits = 1;
        while (n - base * digits > 0){
            n -= base * digits;
            base *= 10;
            digits ++;
        }
        //cout << "base "<<base<<" digits "<<digits<<endl;
        int idx = n % digits;
        if (idx == 0) idx = digits;
        long number = 1;
        for (int i = 1;i < digits;i++) number *= 10;
        number+=n/digits;
        if (idx == digits) number--;
        // cout<<"number "<<number<<endl;
        // cout<<"idx "<<idx<<endl;
        for (int i=idx;i<digits;i++) number /= 10;
        return number % 10;
    }
};
~~~

~~~ c++
class Solution {
public:
    int findNthDigit(int n) {
        n -= 1;
        for (long digits=1;digits < 11;++digits ){
            int first_num = pow(10,digits-1);
            if (n < 9 * first_num * digits){
                return int(to_string(first_num + n/digits)[n%digits])-'0';
            }
            n -= 9 * first_num * digits;
        }
        return 0;
    }
};
~~~

#### [面试题45. 把数组排成最小的数](https://leetcode-cn.com/problems/ba-shu-zu-pai-cheng-zui-xiao-de-shu-lcof/)：sort重置

~~~ c++
class Solution {
public:
    static bool cmp(string a,string b)
    {
        string l=a;
        while (l.size()%b.size()!=0) l+=a;
        string r=b;
        while (r.size()<l.size()) r+=b;
        return l<r;
    }
    string minNumber(vector<int>& nums) {
        vector<string> ans;
        for (int i=0;i<nums.size();i++)
            ans.push_back(to_string(nums[i]));
        sort(ans.begin(),ans.end(),cmp);
        string outs;
        for (int i=0;i<nums.size();i++) outs+=ans[i];
        return outs;
    }
};
~~~

#### [面试题46. 把数字翻译成字符串](https://leetcode-cn.com/problems/ba-shu-zi-fan-yi-cheng-zi-fu-chuan-lcof/)：递推

~~~ c++
class Solution {
public:
    bool inrange(int a) {
        if (a<26 && a>9) return true;
        return false;
    }

    int translateNum(int num) {
        if (num<10) return 1;
        if (num<100)
        {
            if (inrange(num)) return 2;
            else return 1;
        }
        vector<int> a;
        int f1,f2,f3;
        while (num>0)
        {
            a.push_back(num%10);
            num/=10;
        }
        int n=a.size()-1;
        f3=1;
        if (inrange(a[n]*10+a[n-1])) f2=2;
         else f2=1;
        for (int i=n-2;i>=0;i--)
        {
            if (inrange(a[i+1]*10+a[i])) f1=f2+f3;
                else f1=f2;
            f3=f2; f2=f1;
        }
        return f1;
    }
};
~~~

#### [面试题47. 礼物的最大价值](https://leetcode-cn.com/problems/li-wu-de-zui-da-jie-zhi-lcof/)：DP

~~~ c++
class Solution {
public:
    int maxValue(vector<vector<int>>& grid) {
        int n=grid.size();
        int m=grid[0].size();
        for (int i=1;i<n;i++) grid[i][0]+=grid[i-1][0];
        for (int i=1;i<m;i++) grid[0][i]+=grid[0][i-1];
        for (int i=1;i<n;i++)
            for (int j=1;j<m;j++)
                grid[i][j]+=max(grid[i-1][j],grid[i][j-1]);
        return grid[n-1][m-1];
    }
};
~~~

#### [面试题48. 最长不含重复字符的子字符串](https://leetcode-cn.com/problems/zui-chang-bu-han-zhong-fu-zi-fu-de-zi-zi-fu-chuan-lcof/)：双指针 map

~~~ c++
class Solution {
public:
    int lengthOfLongestSubstring(string s) {
        map<char, int> m;
        int l=0,r=0,ans=0;
        while (r<s.size())
        {
            if (m.find(s[r])!=m.end()) l=max(l,m[s[r]]+1);
            m[s[r]]=r;
            ans=max(ans,r-l+1);
            r++;
        }
        return ans;
    }
};
~~~

#### [面试题49. 丑数](https://leetcode-cn.com/problems/chou-shu-lcof/)：三指针

~~~ c++
class Solution {
public:
    int nthUglyNumber(int n) {
        vector<int> ans;
        ans.push_back(1);
        int i2=0,i3=0,i5=0,i=1;
        while (i++<n)
        {
            int tmp=min(2*ans[i2],min(3*ans[i3],5*ans[i5]));
            ans.push_back(tmp);
            if(tmp==2*ans[i2]) i2++;
            if(tmp==3*ans[i3]) i3++;
            if(tmp==5*ans[i5]) i5++;
        }
        return ans.back();
    }
};
~~~

#### [面试题50. 第一个只出现一次的字符](https://leetcode-cn.com/problems/di-yi-ge-zhi-chu-xian-yi-ci-de-zi-fu-lcof/)：map

~~~ c++
class Solution {
public:
    char firstUniqChar(string s) {
        if (s=="") return ' ';
        unordered_map<char,int> num;
        for (int i=0;i<s.size();i++) num[s[i]]++;
        for (int i=0;i<s.size();i++)
        if (num[s[i]]==1) return s[i];
        return ' ';
    }
};

~~~

#### [面试题51. 数组中的逆序对](https://leetcode-cn.com/problems/shu-zu-zhong-de-ni-xu-dui-lcof/)：归并排序/树状数组 :star:

~~~ c++
class Solution {
public:
    int ans=0;
    int reversePairs(vector<int>& nums) 
    {    
        vector<int> copynums(nums.size(), 0);
        merge_sort(nums, copynums, 0, nums.size()-1);
        return ans;
    }

    void merge_sort(vector<int>& nums, vector<int>& copynums, int left, int right) {
        if (left >= right) return;
        int mid = (left+right) / 2;
        merge_sort(nums, copynums, left, mid);
        merge_sort(nums, copynums, mid+1, right);
        int i = left, j = mid+1, k = left;
        while (i <= mid && j <= right) {
            if (nums[j] < nums[i]) {
                copynums[k++] = nums[j++];
                ans += (mid-i+1);
            } else {
                copynums[k++] = nums[i++];
            }
        }
        if (i <= mid) copy(nums.begin()+i, nums.begin()+mid+1, copynums.begin()+k);
        if (j <= right) copy(nums.begin()+j, nums.begin()+right+1, copynums.begin()+k);
        copy(copynums.begin()+left, copynums.begin()+right+1, nums.begin()+left);
    }
};
~~~

~~~ c++
class Solution {
public:
    int c[50001];
    int s[50001];
    int n;
    int lowbit(int x){
        return x&(-x);
    }
    void update(int i,int ans){
        while(i <= n){
            c[i] += ans;
            i += lowbit(i);
        }
    } 
    int sum(int i){ 
        int s = 0;
        while(i > 0){
            s += c[i];
            i -= lowbit(i);
        }
        return s;
    } 
    int reversePairs(vector<int>& nums) {
        n=nums.size();
        for (int i=0;i<n;i++) s[i]=nums[i];
        sort(s,s+n);
        int len=unique(s,s+n)-s;
        int ans=0;
        memset(c,0,sizeof(c));
        for(int i=0;i<n;i++){
            int l=lower_bound(s,s+len,nums[i])-s+1;
            update(l,1);
            ans += i+1 - sum(l);
        }
        return ans;
    }
};
~~~

#### [面试题52. 两个链表的第一个公共节点](https://leetcode-cn.com/problems/liang-ge-lian-biao-de-di-yi-ge-gong-gong-jie-dian-lcof/)：双指针

~~~ c++
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode(int x) : val(x), next(NULL) {}
 * };
 */
class Solution {
public:
    ListNode *getIntersectionNode(ListNode *headA, ListNode *headB) {
        ListNode* l1=headA;
        ListNode* l2=headB;
        while (l1!=l2)
        {
            l1=(l1==NULL)?headB:l1->next;
            l2=(l2==NULL)?headA:l2->next;
        }
        return l1;
    }
};
~~~

#### [面试题53 - I. 在排序数组中查找数字 I](https://leetcode-cn.com/problems/zai-pai-xu-shu-zu-zhong-cha-zhao-shu-zi-lcof/)：二分

~~~ c++
class Solution {
public:
    int search(vector<int>& nums, int target) {
        if (nums.empty()) return 0;
        int n=nums.size();
        int l=0,r=n-1;
        while (l<r &&nums[l]!=target)
        {
            int mid=(l+r)/2;
            if (nums[mid]==target)
            {
                l=mid;
                r=mid;
            }
            else
            if (nums[mid]<target) l=mid+1;
            else r=mid-1;
        }
        if (nums[l]!=target) return 0;
        while (l-1>=0&&nums[l-1]==target) l--;
        r=l;
        while (r+1<n&&nums[r+1]==target) r++;
        return r-l+1;
    }
};
~~~

#### [面试题53 - II. 0～n-1中缺失的数字](https://leetcode-cn.com/problems/que-shi-de-shu-zi-lcof/)：二分

~~~ c++
class Solution {
public:
    int missingNumber(vector<int>& nums) {
        int n=nums.size();
        int l=0,r=n-1;
        while (l<=r)
        {
            int mid=(l+r)/2;
            if (nums[mid]==mid) l=mid+1;
            else r=mid-1;
        }
        return l;
    }
};
~~~

#### [面试题54. 二叉搜索树的第k大节点](https://leetcode-cn.com/problems/er-cha-sou-suo-shu-de-di-kda-jie-dian-lcof/)：dfs

~~~ c++
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode(int x) : val(x), left(NULL), right(NULL) {}
 * };
 */
class Solution {
public:
    int ans;
    int kthLargest(TreeNode* root, int k) {
        dfs(root,k);
        return ans;
    }
    
    void dfs(TreeNode* root,int &k)
    {
        if (!root) return;
        dfs(root->right,k);
        k--;
        if (k==0) ans=root->val;
        if (k>0) dfs(root->left,k);
        return;
    }
};
~~~

#### [面试题55 - II. 平衡二叉树](https://leetcode-cn.com/problems/ping-heng-er-cha-shu-lcof/)：dfs

~~~ c++
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode(int x) : val(x), left(NULL), right(NULL) {}
 * };
 */
class Solution {
public:
    bool isBalanced(TreeNode* root) {
        return depth(root)!=-1;
    }

    int depth(TreeNode* root)
    {
        if (!root) return 0;
        int l=depth(root->left);
        int r=depth(root->right);
        if (l==-1 || r==-1) return -1;
        return abs(l-r)<2?max(l,r)+1:-1;
    }
};
~~~

#### [面试题56 - I. 数组中数字出现的次数](https://leetcode-cn.com/problems/shu-zu-zhong-shu-zi-chu-xian-de-ci-shu-lcof/)：位运算:star:

~~~ c++
class Solution {
public:
    vector<int> singleNumbers(vector<int>& nums) {
        vector<int> ans(2,0);
        int h=0;
        for (int num:nums) h^=num;
        int pos=h&(-h);
        for (int num:nums)
            if ((num&pos)==pos) ans[1]^=num;
            else ans[0]^=num;
        return ans;
    }
};
~~~

#### [面试题56 - II. 数组中数字出现的次数 II](https://leetcode-cn.com/problems/shu-zu-zhong-shu-zi-chu-xian-de-ci-shu-ii-lcof/)：位运算:star:

~~~ c++
class Solution {
public:
    int singleNumber(vector<int>& nums) {
        int res = 0;
        for (int i=0; i<32; i++) {
            int sum = 0;
            for(int num:nums) {
                num >>= i;
                sum += (num & 1);
            }
            res |= ((sum % 3) << i);
        }
        return res;
    }
};
~~~

#### [面试题57. 和为s的两个数字](https://leetcode-cn.com/problems/he-wei-sde-liang-ge-shu-zi-lcof/):双指针

~~~ c++
class Solution {
public:
    vector<int> twoSum(vector<int>& nums, int target) {
        vector<int> ans;
        int l=0,r=nums.size()-1;
        while (l<r)
        {
            if (nums[l]+nums[r]==target)
            {
                ans.push_back(nums[l]);
                ans.push_back(nums[r]);
                break;
            }
            if (nums[l]+nums[r]<target) l++;
            else r--;
        }
        return ans;
    }
};
~~~

#### [面试题57 - II. 和为s的连续正数序列](https://leetcode-cn.com/problems/he-wei-sde-lian-xu-zheng-shu-xu-lie-lcof/)：双指针

~~~ c++
class Solution {
public:
    vector<vector<int>> findContinuousSequence(int target) {
        vector<vector<int>> ans;
        vector<int> res;
        int l=1,r=2;
        while (l<r)
        {
            int sum=(l+r)*(r-l+1);
            if (sum>2*target) l++;
            else
            if (sum==2*target)
            {
                res.clear();
                for (int i=l;i<=r;i++) res.push_back(i);
                ans.push_back(res);
                l++;
            }
            else r++;
        }
        return ans;
    }
};
~~~

#### [面试题58 - I. 翻转单词顺序](https://leetcode-cn.com/problems/fan-zhuan-dan-ci-shun-xu-lcof/):字符串模拟

~~~ c++
class Solution {
public:
    string reverseWords(string s) {
        string ans;
        stack<string> sta;
        int l=0,r;
        int n=s.size();
        while (s[n-1]==' ') n--;
        while (l<n)
        {
            while (l<n && s[l]==' ') l++;
            r=l;
            while (r<n && s[r]!=' ') r++;
            sta.push(s.substr(l,r-l));
            l=r;
        }
        while (!sta.empty())
        {
            ans+=sta.top()+' ';
            sta.pop();
        }
        return ans.substr(0,ans.size()-1);
    }
};
~~~

#### [面试题58 - II. 左旋转字符串](https://leetcode-cn.com/problems/zuo-xuan-zhuan-zi-fu-chuan-lcof/):字符串模拟

~~~ c++
class Solution {
public:
    string reverseLeftWords(string s, int n) {
        string ans;
        ans=s.substr(n,s.size()-n);
        ans+=s.substr(0,n);
        return ans;
    }
};
~~~

#### [面试题59 - I. 滑动窗口的最大值](https://leetcode-cn.com/problems/hua-dong-chuang-kou-de-zui-da-zhi-lcof/)：双端队列:star:

~~~ c++
class Solution {
public:
    vector<int> maxSlidingWindow(vector<int>& nums, int k) {
        vector<int> ans;
        deque<int> deq;
        for (int i=0;i<nums.size();i++)
        {
            while (!deq.empty()&&nums[i]>nums[deq.back()]) deq.pop_back();
            if (!deq.empty()&&deq.front()<i-k+1) deq.pop_front();
            deq.push_back(i);
            if (i>=k-1) ans.push_back(nums[deq.front()]);
        }
        return ans;
    }
};

~~~

#### [面试题59 - II. 队列的最大值](https://leetcode-cn.com/problems/dui-lie-de-zui-da-zhi-lcof/):双端队列

~~~ c++
class MaxQueue {
public:
    queue<int> q;
    deque<int> d;
    MaxQueue() {

    }
    
    int max_value() {
        if (q.empty()) return -1;
        return d.front();
    }
    
    void push_back(int value) {
        while (!d.empty()&&d.back()<value) d.pop_back();
        q.push(value);
        d.push_back(value);
    }
    
    int pop_front() {
        if (q.empty()) return -1;
        int ans=q.front();
        if (d.front()==ans) d.pop_front();
        q.pop();
        return ans;
    }
};

/**
 * Your MaxQueue object will be instantiated and called as such:
 * MaxQueue* obj = new MaxQueue();
 * int param_1 = obj->max_value();
 * obj->push_back(value);
 * int param_3 = obj->pop_front();
 */
~~~

#### [面试题60. n个骰子的点数](https://leetcode-cn.com/problems/nge-tou-zi-de-dian-shu-lcof/):DP

~~~ c++
class Solution {
public:
    vector<double> twoSum(int n) {
        vector<double> ans;
        int total=pow(6,n);
        int f[n+1][6*n+1];
        memset(f,0,sizeof(f));
        for (int i=1;i<=6;i++) f[1][i]=1;
        for (int i=2;i<=n;i++)
            for (int j=i;j<=i*6;j++)
                for (int k=1;k<=6;k++)
                if (j>=k)
                    f[i][j]+=f[i-1][j-k];
        for (int i=n;i<=6*n;i++) ans.push_back(double(f[n][i])/double(total));
        return ans;
    }
};
~~~

#### [面试题62. 圆圈中最后剩下的数字](https://leetcode-cn.com/problems/yuan-quan-zhong-zui-hou-sheng-xia-de-shu-zi-lcof/)：约瑟夫 :star:

~~~ c++
class Solution {
public:
    int lastRemaining(int n, int m) {
        int ans=0;
        for (int i=2;i<=n;i++)
            ans=(ans+m)%i;
        return ans;
    }
};
~~~

#### [面试题64. 求1+2+…+n](https://leetcode-cn.com/problems/qiu-12n-lcof/) 虚函数:star:

~~~ c++
class Solution {
public:
    int sumNums(int n) {
        n && (n += sumNums(n-1));
        return n;
    }
};
~~~

~~~ c++
class A
{
public:
	virtual int sum(int n)
	{
		return 0;
	}
};
 
A* a[2];
 
class B :public A
{
public:
	virtual int sum(int n)
	{
		return n + a[!!n]->sum(n - 1);
	}
};

class Solution {
public:
    int sumNums(int n) {
        A a1;
	    B b1;
	    a[0] = &a1;
	    a[1] = &b1;
	    int ret = a[1]->sum(n);
	    return ret;
    }
};
 
~~~

#### [面试题65. 不用加减乘除做加法](https://leetcode-cn.com/problems/bu-yong-jia-jian-cheng-chu-zuo-jia-fa-lcof/) ：位运算:star:

~~~ c++
class Solution {
public:
    int add(int a, int b) {
        int carry;
        while (b)
        {
            carry=(unsigned int)(a&b)<<1;
            a^=b;
            b=carry;
        }
        return a;
    }
};
~~~

#### [面试题66. 构建乘积数组](https://leetcode-cn.com/problems/gou-jian-cheng-ji-shu-zu-lcof/)

~~~ c++
class Solution {
public:
    vector<int> constructArr(vector<int>& a) {
        int n=a.size();
        vector<int> ans(n);
        int l=1;
        for (int i=0;i<n;i++)
        {
            ans[i]=l;
            l*=a[i];
        }
        int r=1;
        for (int i=n-1;i>=0;i--)
        {
            ans[i]*=r;
            r*=a[i];
        }
        return ans;
    }
};
~~~

#### [面试题67. 把字符串转换成整数](https://leetcode-cn.com/problems/ba-zi-fu-chuan-zhuan-huan-cheng-zheng-shu-lcof/)：字符串处理＋int边界处理:star:

~~~ c++
class Solution {
public:
    int strToInt(string str) {
        int num=str.size();
        if (num<1) return 0;
        int i=0,flag=0;
        while (i<num&&str[i]==' ') i++;
        if (i==num) return 0;
        if (str[i]=='-') flag=1,i++;
        else
        if (str[i]=='+') i++;
        if (str[i]<'0'||str[i]>'9') return 0;
        long long ans=0;
        long long bord=1u<<31;
        while (i<num&&str[i]>='0'&&str[i]<='9')
        {
            ans=ans*10+int(str[i]-'0');
            if (ans>bord) break;
            i++;
        }
        if (flag==1) ans=-ans;
        if (ans<-bord) return -bord;
        if (ans>bord-1) return bord-1;
        return ans;
    }
};
~~~

#### [面试题68 - I. 二叉搜索树的最近公共祖先](https://leetcode-cn.com/problems/er-cha-sou-suo-shu-de-zui-jin-gong-gong-zu-xian-lcof/) ：二叉搜索树性质:star:

~~~ c++
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode(int x) : val(x), left(NULL), right(NULL) {}
 * };
 */
class Solution {
public:
    TreeNode* lowestCommonAncestor(TreeNode* root, TreeNode* p, TreeNode* q) {
        if (root == NULL) return NULL;
        TreeNode* h = root;
        int m=p->val,n= q->val;
        if (m>n) swap(m,n);
        while (!(m<h->val&&h->val<n))
        {
            if (m<h->val&&n<h->val) h=h->left;
            else if (m>h->val&&n>h->val) h=h->right;
            else break;
        }
        return h;
    }
};
~~~

#### [面试题68 - II. 二叉树的最近公共祖先](https://leetcode-cn.com/problems/er-cha-shu-de-zui-jin-gong-gong-zu-xian-lcof/) ：LCA :star:

~~~ c++
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode(int x) : val(x), left(NULL), right(NULL) {}
 * };
 */
class Solution {
public:
    TreeNode* lowestCommonAncestor(TreeNode* root, TreeNode* p, TreeNode* q) {
        if (!root || root == p || root == q) return root;
        TreeNode *left = lowestCommonAncestor(root->left, p, q);
        TreeNode *right = lowestCommonAncestor(root->right, p, q);
        if (left && right) return root;
        return left ? left : right;
    }
};
~~~



## [程序员面试金典]( https://leetcode-cn.com/problemset/lcci/ )

#### [面试题 01.01. 判定字符是否唯一](https://leetcode-cn.com/problems/is-unique-lcci/):位运算

~~~ c++
class Solution {
public:
    bool isUnique(string astr) {
        int ans=0;
        for (char c:astr)
        {
            int i=c-'a';
            int x=1<<i;
            if ((x&ans)==x) return false;
            ans|=x;
        }
        return true;
    }
};
~~~



#### [面试题 01.06. 字符串压缩](https://leetcode-cn.com/problems/compress-string-lcci/):双指针

~~~ c++
class Solution {
public:
    string compressString(string S) {
        string news;
        int i=0,j;
        while (i<S.size())
        {
            news+=S[i];
            j=i;
            while (j+1<S.size()&&S[j+1]==S[i]) j++;
            news+=to_string(j-i+1);
            i=j+1;
        }
        if (news.size()>=S.size()) return S;
        else return news;
    }
};
~~~

#### [面试题 01.07. 旋转矩阵](https://leetcode-cn.com/problems/rotate-matrix-lcci/)：规律:star:

~~~ c++
class Solution {
public:
    void rotate(vector<vector<int>>& matrix) {
        int n = matrix.size();
        if(n == 0) return;
        int r = (n>>1)-1; //左上角区域的最大行下标，
        int c = (n-1)>>1; //左上角区域的最大列下标，行列下标从 0 开始。
        for(int i = r; i >= 0; --i) {
            for(int j = c; j >= 0; --j) {
                swap(matrix[i][j], matrix[j][n-i-1]);
                swap(matrix[i][j], matrix[n-i-1][n-j-1]);
                swap(matrix[i][j], matrix[n-j-1][i]);
            }
        }
    }
};

~~~



#### [面试题 16.03. 交点](https://leetcode-cn.com/problems/intersection-lcci/)：数学:star:

~~~ c++
class Solution {
public:
    // 判断 (xk, yk) 是否在「线段」(x1, y1)~(x2, y2) 上
    // 这里的前提是 (xk, yk) 一定在「直线」(x1, y1)~(x2, y2) 上
    bool inside(int x1, int y1, int x2, int y2, int xk, int yk) {
        // 若与 x 轴平行，只需要判断 x 的部分
        // 若与 y 轴平行，只需要判断 y 的部分
        // 若为普通线段，则都要判断
        return (x1 == x2 || (min(x1, x2) <= xk && xk <= max(x1, x2))) && (y1 == y2 || (min(y1, y2) <= yk && yk <= max(y1, y2)));
    }

    void update(vector<double>& ans, double xk, double yk) {
        // 将一个交点与当前 ans 中的结果进行比较
        // 若更优则替换
        if (!ans.size() || xk < ans[0] || (xk == ans[0] && yk < ans[1])) {
            ans = {xk, yk};
        }
    }

    vector<double> intersection(vector<int>& start1, vector<int>& end1, vector<int>& start2, vector<int>& end2) {
        int x1 = start1[0], y1 = start1[1];
        int x2 = end1[0], y2 = end1[1];
        int x3 = start2[0], y3 = start2[1];
        int x4 = end2[0], y4 = end2[1];

        vector<double> ans;
        // 判断 (x1, y1)~(x2, y2) 和 (x3, y3)~(x4, y3) 是否平行
        if ((y4 - y3) * (x2 - x1) == (y2 - y1) * (x4 - x3)) {
            // 若平行，则判断 (x3, y3) 是否在「直线」(x1, y1)~(x2, y2) 上
            if ((y2 - y1) * (x3 - x1) == (y3 - y1) * (x2 - x1)) {
                // 判断 (x3, y3) 是否在「线段」(x1, y1)~(x2, y2) 上
                if (inside(x1, y1, x2, y2, x3, y3)) {
                    update(ans, (double)x3, (double)y3);
                }
                // 判断 (x4, y4) 是否在「线段」(x1, y1)~(x2, y2) 上
                if (inside(x1, y1, x2, y2, x4, y4)) {
                    update(ans, (double)x4, (double)y4);
                }
                // 判断 (x1, y1) 是否在「线段」(x3, y3)~(x4, y4) 上
                if (inside(x3, y3, x4, y4, x1, y1)) {
                    update(ans, (double)x1, (double)y1);
                }
                // 判断 (x2, y2) 是否在「线段」(x3, y3)~(x4, y4) 上
                if (inside(x3, y3, x4, y4, x2, y2)) {
                    update(ans, (double)x2, (double)y2);
                }
            }
            // 在平行时，其余的所有情况都不会有交点
        }
        else {
            // 联立方程得到 t1 和 t2 的值
            double t1 = (double)(x3 * (y4 - y3) + y1 * (x4 - x3) - y3 * (x4 - x3) - x1 * (y4 - y3)) / ((x2 - x1) * (y4 - y3) - (x4 - x3) * (y2 - y1));
            double t2 = (double)(x1 * (y2 - y1) + y3 * (x2 - x1) - y1 * (x2 - x1) - x3 * (y2 - y1)) / ((x4 - x3) * (y2 - y1) - (x2 - x1) * (y4 - y3));
            // 判断 t1 和 t2 是否均在 [0, 1] 之间
            if (t1 >= 0.0 && t1 <= 1.0 && t2 >= 0.0 && t2 <= 1.0) {
                ans = {x1 + t1 * (x2 - x1), y1 + t1 * (y2 - y1)};
            }
        }
        return ans;
    }
};

~~~



#### [面试题 17.16. 按摩师](https://leetcode-cn.com/problems/the-masseuse-lcci/) :DP

~~~ c++
class Solution {
public:
    int massage(vector<int>& nums) {
        int n=nums.size();
        if (n==0) return 0;
        if (n==1) return nums[0];
        vector<int> f(n,0);
        f[0]=nums[0]; f[1]=nums[1];
        int ans=max(f[0],f[1]);
        for (int i=2;i<n;i++)
        {
            int h=0;
            for (int j=0;j<i-1;j++)
                if (f[j]>h) h=f[j];
            f[i]=nums[i]+h;
            ans=max(ans,f[i]);
        }
        return ans;
    }
};
~~~



### [problemset](https://leetcode-cn.com/problemset/all/)

#### [1. 两数之和](https://leetcode-cn.com/problems/two-sum/):map一遍hash:star:

~~~c++
class Solution {
public:
    vector<int> twoSum(vector<int>& nums, int target) {
        map <int,int> m;
        vector<int> ret;
        for (int i=0; i<nums.size(); i++)
        {
            int a=target-nums[i];
            if(m.count(a))
            {
                ret.push_back(m[a]);
                ret.push_back(i);
                break;
            }
            m[nums[i]]=i;
        }
        return ret;      
    }
};
~~~



#### [2. 两数相加](https://leetcode-cn.com/problems/add-two-numbers/)：链表处理

~~~c++
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode(int x) : val(x), next(NULL) {}
 * };
 */
class Solution {
public:
    ListNode* addTwoNumbers(ListNode* l1, ListNode* l2) {
        ListNode *ret= new ListNode(0);
        ListNode *head=ret;
        int tot,carry=0;
        while (l1!=NULL || l2!=NULL)
        {
            tot=0;
            if (l1!=NULL)
            {
                tot+=l1->val;
                l1=l1->next;
            }
            if (l2!=NULL)
            {
                tot+=l2->val;
                l2=l2->next;
            }
            if (carry!=0) tot+=carry;
            head->next=new ListNode(tot%10);
            head=head->next;
                
            if (tot>=10) carry=1;
            else carry=0;
        }
        if (carry) head->next=new ListNode(1);
        return ret->next;
    }
};
~~~



#### [3. 无重复字符的最长子串](https://leetcode-cn.com/problems/longest-substring-without-repeating-characters/):统计字符出现次数+双指针

~~~c++
class Solution {
public:
    int lengthOfLongestSubstring(string s) {
        int num[300];
        if (s.size()==0) return 0;
        memset(num,0,sizeof(num));
        int ans=0;
        int l=0,r=0;
        while (l<s.size())
        {
            while (r<s.size()&&num[int(s[r])]==0)
            {
                num[int(s[r])]=1;
                r++;
            }
            if (r-l>ans) ans=r-l;
            //printf("%d %d\n",l,r);
            if (r==s.size()) break;
            int i=l;
            while (i<s.size()&&s[i]!=s[r])
            {
                num[int(s[i])]=0;
                i++;
            }
            num[int(s[r])]=0;
            l=i+1;
        }
        return ans;
    }
};
~~~



#### [4. 寻找两个有序数组的中位数](https://leetcode-cn.com/problems/median-of-two-sorted-arrays/):双指针+分治:star:

~~~c++
class Solution {
public:
    double findMedianSortedArrays(vector<int>& nums1, vector<int>& nums2) {
        int n=nums1.size();
        int m=nums2.size();
        if (n>m) return findMedianSortedArrays(nums2,nums1);
        int L1,R1,L2,R2,c1,c2;
        int ma=1000000001;
        int le=0,ri=2*n;
        while (le<=ri)
        {
            c1=(le+ri)/2;
            c2=n+m-c1;
            L1=(c1==0)?-1:nums1[(c1-1)/2];
            R1=(c1==2*n)?ma:nums1[c1/2];
            L2=(c2==0)?-1:nums2[(c2-1)/2];
            R2=(c2==2*m)?ma:nums2[c2/2];
            if (L1>R2) ri=c1-1;
            else
                if (L2>R1) le=c1+1;
            else break;
        }
        return (max(L1,L2)+min(R1,R2))/2.0;
    }
};
~~~



#### [5. 最长回文子串](https://leetcode-cn.com/problems/longest-palindromic-substring/):DP

~~~C++
class Solution {
public:
    string longestPalindrome(string s) {
        int n=s.size();
        int f[n+1][n+1];
        int ma=1,le=0,ri=0;
        memset(f,0,sizeof(f));
        for (int i=0;i<n;i++)
        {
            f[i][i]=1;
            if (i<n&&s[i]==s[i+1])
            {
                f[i][i+1]=1;
                ma=2;
                le=i;
                ri=i+1;
            }
        }
        
        for (int l=2;l<n;l++)
            for (int i=0;i<n;i++)
            {
                int j=i+l;
                if (j<n&&s[i]==s[j]&&f[i+1][j-1]==1)
                {
                    f[i][j]=1;
                    if (j-i+1>ma)
                    {
                        ma=j-i+1;
                        le=i;
                        ri=j;
                    }
                }
            }
        return s.substr(le,ma);
    }
};
~~~



#### [6. Z 字形变换](https://leetcode-cn.com/problems/zigzag-conversion/):模拟

```c++
class Solution {
public:
    string convert(string s, int numRows) {
        int nums=s.size();
        if (nums==1||numRows==1) return s;
        int gap=2*numRows-2;
        string ans;
        for (int i=1;i<=numRows;i++)
        {
            if (i==1||i==numRows)
            {
                int j=i;
                while (j<=nums) ans=ans+s[j-1],j+=gap;
            }
            else
            {
                int j=i,tot=numRows;
                while(j<=nums)
                {
                    ans=ans+s[j-1];
                    if (2*tot-j<=nums) ans=ans+s[2*tot-j-1];
                    j+=gap;
                    tot+=gap;
                }
            }
        }
        ans=ans+'\0';
        return ans;
    }
};
```



#### [7. 整数反转](https://leetcode-cn.com/problems/reverse-integer/)：翻转+溢出处理

~~~c++
class Solution {
public:
    int reverse(int x) {
        long long h=x,ans=0;
        while (h!=0)
        {
            ans=ans*10+h%10;
            h/=10;
        }
        long long bord=1u<<31;
        if (ans<-bord||ans>bord-1) return 0;
        else return ans;
    }
};
~~~



#### [8. 字符串转换整数 (atoi)](https://leetcode-cn.com/problems/string-to-integer-atoi/)：模拟+边界处理

~~~c++
class Solution {
public:
    int myAtoi(string str) {
        int num=str.size();
        if (num<1) return 0;
        int i=0,flag=0;
        while (i<num&&str[i]==' ') i++;
        if (i==num) return 0;
        if (str[i]=='-') flag=1,i++;
        else
        if (str[i]=='+') i++;
        if (str[i]<'0'||str[i]>'9') return 0;
        long long ans=0;
        long long bord=1u<<31;
        while (i<num&&str[i]>='0'&&str[i]<='9')
        {
            ans=ans*10+int(str[i]-'0');
            if (ans>bord) break;
            i++;
        }
        if (flag==1) ans=-ans;
        if (ans<-bord) return -bord;
        if (ans>bord-1) return bord-1;
        return ans;
    }
};
~~~



#### [9. 回文数](https://leetcode-cn.com/problems/palindrome-number/)：翻转

~~~c++
class Solution {
public:
    bool isPalindrome(int x) {
        if (x<0) return false;
        if (x<10) return true;
        if (x%10==0) return false;
        int h=0;
        while (h<x)
        {
            h=h*10+x%10;
            x/=10;
        }
        printf("%d %d\n",h,x);
        return h==x||h/10==x;
    }
};
~~~



#### [10. 正则表达式匹配](https://leetcode-cn.com/problems/regular-expression-matching/)：DP

~~~c++
class Solution {
public:
    bool isMatch(string s, string p) {
        int n=s.size();
        int m=p.size();
        bool f[n+1][m+1];
        memset(f,false,sizeof(f));
        f[0][0]=true;
        for (int j=0;j<m;j++)
        	if (p[j]=='*'&&f[0][j-1]) f[0][j+1]=true;
        for (int i=0;i<n;i++)
            for (int j=0;j<m;j++)
            {
            	if (s[i]==p[j]||p[j]=='.') f[i+1][j+1]=f[i][j];
            	if (p[j]=='*')
            	{
            		if (s[i]!=p[j-1]&&p[j-1]!='.') f[i+1][j+1]=f[i+1][j-1];
                	else
						f[i+1][j+1]=f[i+1][j]||f[i][j+1]||f[i+1][j-1];
  				}
            	
			}
        return f[n][m];
    }
};
~~~



#### [11. 盛最多水的容器](https://leetcode-cn.com/problems/container-with-most-water/)：双指针

~~~c++
class Solution {
public:
    int maxArea(vector<int>& height) {
        int l=0,r=height.size()-1;
        int ans=0;
        while (l<r)
        {
            ans=max(ans,min(height[l],height[r])*(r-l));
            if (height[l]<height[r]) l++;
            else r--;
        }
        return ans;
    }
};
~~~



#### [12. 整数转罗马数字](https://leetcode-cn.com/problems/integer-to-roman/)：贪心

~~~c++
class Solution {
public:
    string intToRoman(int num) {
        string result;
        vector<int> store = {1000, 900, 500, 400, 100, 90, 50, 40, 10, 9, 5, 4, 1};
        vector<string> strs = {"M", "CM", "D", "CD", "C", "XC", "L", "XL", "X", "IX", "V", "IV", "I"};
        for (int i=0;i<store.size();i++)
        {
            while (num>=store[i])
            {
                result.append(strs[i]);
                num-=store[i];
            }
        }
        return result;
    }
};
~~~



#### [13. 罗马数字转整数](https://leetcode-cn.com/problems/roman-to-integer/)：模拟

~~~c++
class Solution {
public:
    int romanToInt(string s) {
        if (s.size()==0) return 0;
        vector<int> store = {1000, 500, 100, 50, 10, 5, 1};
        vector<char> strs = {'M', 'D', 'C', 'L', 'X', 'V', 'I'};
        int ans=0,last=0,now;
        for (int i=0;i<s.size();i++)
        {
            for (int j=0;j<7;j++)
            if (strs[j]==s[i])
            {
                now=store[j];
                break;
            }
            if (i!=0&&last<now) ans+=now-last*2;
            else ans+=now;
            last=now;
        }
        return ans;
    }
};
~~~



#### [14. 最长公共前缀](https://leetcode-cn.com/problems/longest-common-prefix/)：二分

~~~c++
class Solution {
public:
    string longestCommonPrefix(vector<string>& strs) {
        if (strs.size()==0) return "";
        int n=strs[0].size();
        string ans;
        int l=1,r=n,mid;
        bool check;
        while (l<=r)
        {
            mid=(l+r)/2;
            check=true;
            for (int i=1;i<strs.size();i++)
            {
                if (strs[i].substr(0,mid)!=strs[0].substr(0,mid))
                {
                    check=false;
                    break;
                }
            }
            if (check) l=mid+1;
            else r=mid-1;
        }
        return strs[0].substr(0,(l+r)/2);
    }
};
~~~



#### [15. 三数之和](https://leetcode-cn.com/problems/3sum/)：双指针+去重

~~~c++
class Solution {
public:
    vector<vector<int>> threeSum(vector<int>& nums) {
        vector<vector<int>> ans;
        if (nums.size()<3) return ans;
        sort(nums.begin(),nums.end());
        for (int i=0; i<nums.size()-2; i++)
        {
            if (nums[i]>0) continue;
            if (i>0&&nums[i]==nums[i-1]) continue;
            int l=i+1,r=nums.size()-1;
            while (l<r)
            {
                if (nums[l]+nums[r]==-nums[i])
                {
                    ans.push_back({nums[i],nums[l],nums[r]});
                    while (l<r&&nums[l]==nums[l+1]) l++;
                    while (l<r&&nums[r]==nums[r-1]) r--;
                    l++,r--;
                }
                else
                if (nums[l]+nums[r]>-nums[i]) r--;
                else l++;
            }
        }
        return ans;
    }
};
~~~



#### [16. 最接近的三数之和](https://leetcode-cn.com/problems/3sum-closest/)：双指针

~~~c++
class Solution {
public:
    int threeSumClosest(vector<int>& nums, int target) {
        int ans=100000;
        sort(nums.begin(),nums.end());
        for (int i=0;i<nums.size()-2;i++)
        {
            int l=i+1,r=nums.size()-1;
            while (l<r)
            {
                if (nums[i]+nums[l]+nums[r]==target)
                {
                    ans=target;
                    break;
                }
                else
                {
                    int h=nums[i]+nums[l]+nums[r];
                    if (abs(h-target)<abs(ans-target)) ans=h;
                    if (h>target) r--;
                        else l++;
                }
            }
            if (ans==target) break;
        }
        return ans;
    }
};
~~~



#### [17. 电话号码的字母组合](https://leetcode-cn.com/problems/letter-combinations-of-a-phone-number/)：队列（宽搜）/深搜

队列：

~~~c++
class Solution {
public:
    vector<string> letterCombinations(string digits) {
        vector<string> ans;
        map<char,string> dial={
            {'2',"abc"},{'3',"def"},
            {'4',"ghi"},{'5',"jkl"},{'6',"mno"},
            {'7',"pqrs"},{'8',"tuv"},{'9',"wxyz"}
        };
        int n=digits.size();
        string str;
        queue<string> q;
        if (n==0) return ans;
        q.push("");
        for (int i=0;i<n;i++)
        {
            int len=q.size();
            while (len--)
            {
                for (int j=0;j<dial[digits[i]].size();j++)
                {
                    str=q.front();
                    str=str+dial[digits[i]][j];
                    q.push(str);
                }
                q.pop();
            }
        }
        while (!q.empty())
        {
            ans.push_back(q.front());
            q.pop();
        }
        return ans;
    }
};
~~~

深搜：

~~~c++
class Solution {
public:
    int n;
    vector<string> ans;
    string digits;
    map<char,string> dial={
            {'2',"abc"},{'3',"def"},
            {'4',"ghi"},{'5',"jkl"},{'6',"mno"},
            {'7',"pqrs"},{'8',"tuv"},{'9',"wxyz"}
        };
    vector<string> letterCombinations(string digits) {
        n=digits.size();
        if (n==0) return ans;
        this->digits = digits;
        dfs("",0);
        return ans;
    }
    void dfs(string str,int k)
    {
        if (k==n)
        {
            ans.push_back(str);
            return;
        }
        char c=this->digits[k];
        for (int i=0;i<dial[c].size();i++)
        {
            dfs(str+dial[digits[k]][i],k+1);
        }
    }
};
~~~



#### [18. 四数之和](https://leetcode-cn.com/problems/4sum/)：双指针+剪枝去重

~~~c++
class Solution {
public:
    vector<vector<int>> fourSum(vector<int>& nums, int target) {
        vector<vector<int>> ans;
        if (nums.size()<4) return ans;
        sort(nums.begin(),nums.end());
        for (int i=0; i<nums.size()-3; i++)
        {
            if (nums[i]>=0&&target<0) break;
            if (nums[i]+nums[i+1]+nums[i+2]+nums[i+3]>target) break;
            if (nums[i]+nums[nums.size()-3]+nums[nums.size()-2]+nums[nums.size()-1]<target) continue;
            if (i>0&&nums[i]==nums[i-1]) continue;
            for (int j=i+1; j<nums.size()-2;j++)
            {
                if (nums[i]+nums[j]+nums[j+1]+nums[j+2]>target) break;
                if (nums[i]+nums[j]+nums[nums.size()-2]+nums[nums.size()-1]<target) continue;
                if (j>i+1&&nums[j]==nums[j-1]) continue;
                int l=j+1,r=nums.size()-1;
                while (l<r)
                {
                    if (nums[l]+nums[r]+nums[i]+nums[j]==target)
                    {
                        ans.push_back({nums[i],nums[j],nums[l],nums[r]});
                        while (l<r&&nums[l]==nums[l+1]) l++;
                        while (l<r&&nums[r]==nums[r-1]) r--;
                        l++,r--;
                    }
                    else
                    if (nums[l]+nums[r]>target-nums[i]-nums[j]) r--;
                    else l++;
                }
            }
            
        }
        return ans;
    }
};
~~~



#### [19. 删除链表的倒数第N个节点](https://leetcode-cn.com/problems/remove-nth-node-from-end-of-list/)：遍历两遍/双指针

遍历两遍：

~~~c++
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode(int x) : val(x), next(NULL) {}
 * };
 */
class Solution {
public:
    ListNode* removeNthFromEnd(ListNode* head, int n) {
        int m=1;
        ListNode *h=head;
        while (h->next!=NULL)
        {
            m++;
            h=h->next;
        }
        m=m-n-1;
        h=head;
        if (m<0) return h->next;
        while (m--) h=h->next;
        h->next=h->next->next;
        return head;
    }
};
~~~

双指针：

~~~c++
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode(int x) : val(x), next(NULL) {}
 * };
 */
class Solution {
public:
    ListNode* removeNthFromEnd(ListNode* head, int n) {
        int m=n;
        ListNode *l=head,*r=head;
        if (head->next==NULL) return NULL;
        while (m--) r=r->next;
        if (r!=NULL)
        {
            while (r->next!=NULL)
            {
                l=l->next;
                r=r->next;
            }
            l->next=l->next->next;
        }
        else return l->next;
        return head;
    }
};
~~~



#### [20. 有效的括号](https://leetcode-cn.com/problems/valid-parentheses/)：栈

~~~c++
class Solution {
public:
    bool isValid(string s) {
        if (s.size()==0) return true;
        if (s.size()%2==1) return false;
        stack<char> st;
        st.push(s[0]);
        for (int i=1;i<s.size();i++)
        {
            if ((s[i]==')'&&st.top()=='(')||(s[i]=='}'&&st.top()=='{')||(s[i]==']'&&st.top()=='[')) st.pop();
            else st.push(s[i]);
        }
        return st.empty();
    }
};
~~~



#### [21. 合并两个有序链表](https://leetcode-cn.com/problems/merge-two-sorted-lists/)：链表操作

~~~c++
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode(int x) : val(x), next(NULL) {}
 * };
 */
class Solution {
public:
    ListNode* mergeTwoLists(ListNode* l1, ListNode* l2) {
        if (l1==NULL) return l2;
        if (l2==NULL) return l1;
        struct ListNode* now= new ListNode(-1);
        struct ListNode* head=now;
        while (l1!=NULL&&l2!=NULL)
        {
            if (l1->val<=l2->val)
            {
                now->next=l1;
                l1=l1->next;
            }
            else
            {
                now->next=l2;
                l2=l2->next;
            }
            now=now->next;
        }
        now->next=l1==NULL?l2:l1;
        return head->next;
    }
};
~~~



#### [22. 括号生成](https://leetcode-cn.com/problems/generate-parentheses/)：回溯

~~~c++
class Solution {
public:
    vector<string> generateParenthesis(int n) {
        if (n==0) return {};
        vector<string> ans;
        dfs(ans,"",n,0);
        return ans;
    }

    void dfs(vector<string>& ans,string str,int l,int r)
    {
        if (l==0&&r==0)
        {
            ans.push_back(str);
            return;
        }
        if (l>0) dfs(ans,str+'(',l-1,r+1);
        if (r>0) dfs(ans,str+')',l,r-1);
    }
};
~~~



#### [23. 合并K个排序链表](https://leetcode-cn.com/problems/merge-k-sorted-lists/)：两两合并+双端队列优化:star:

无优化：

~~~c++
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode(int x) : val(x), next(NULL) {}
 * };
 */
class Solution {
public:
    ListNode* mergeKLists(vector<ListNode*>& lists) {
        int size = lists.size();
        if (size == 0) {
            return nullptr;
        }
        if (size == 1) {
            return lists[0];
        }
        queue<ListNode*> waiting(deque<ListNode*>(lists.begin(), lists.end())); //将vector转为队列
        //如果队列元素大于1，则取出两个进行合并，合并后的链表继续添加到链表尾部
        while (waiting.size() > 1) {
            ListNode *l1 = waiting.front();
            waiting.pop();
            ListNode *l2 = waiting.front();
            waiting.pop();
            waiting.push(mergeTwoLists(l1, l2));
        } 
        return waiting.front();
    }
    ListNode* mergeTwoLists(ListNode *l1, ListNode *l2) {
        ListNode *head = new ListNode(0);
        ListNode *p = head;
        while (l1 && l2) {
            if (l1->val < l2->val) {
                p->next = l1;
                l1 = l1->next;
            } else {
                p->next = l2;
                l2 = l2->next;
            }
            p = p->next;
        }
        p->next = l1 ? l1 : l2;
        return head->next;
    }
};
~~~



~~~c++
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode(int x) : val(x), next(NULL) {}
 * };
 */
class Solution {
public:
    ListNode* mergeKLists(vector<ListNode*>& lists) {
        int size = lists.size();
        if (size == 0) {
            return nullptr;
        }
        if (size == 1) {
            return lists[0];
        }
        queue<ListNode*> waiting(deque<ListNode*>(lists.begin(), lists.end())); //将vector转为队列
        //如果队列元素大于1，则取出两个进行合并，合并后的链表继续添加到链表尾部
        while (waiting.size() > 1) {
            ListNode *l1 = waiting.front();
            waiting.pop();
            ListNode *l2 = waiting.front();
            waiting.pop();
            waiting.push(mergeTwoLists(l1, l2));
        } 
        return waiting.front();
    }
    ListNode* mergeTwoLists(ListNode *l1, ListNode *l2) {
        ListNode *head = new ListNode(0);
        ListNode *p = head;
        while (l1 && l2) {
            if (l1->val < l2->val) {
                p->next = l1;
                l1 = l1->next;
            } else {
                p->next = l2;
                l2 = l2->next;
            }
            p = p->next;
        }
        p->next = l1 ? l1 : l2;
        return head->next;
    }
};
~~~



#### [31. 下一个排列](https://leetcode-cn.com/problems/next-permutation/)：扫描:star:

~~~c++
class Solution {
public:
    void nextPermutation(vector<int>& nums) {
        if (nums.size()>1)
        {
            int i;
            for (i=nums.size()-1;i>0&&nums[i - 1]>=nums[i];i--);
            reverse(nums.begin()+i,nums.end());
            if (i--!=0)                                                          
            {
                int left=i+1;
                int right=nums.size()-1;
                while (left<=right)                   
                {
                    int mid=(left+right)/2;
                    if (nums[mid]>nums[i]) right=mid-1;   
                                      else left=mid+1;
                }
                swap(nums[i], nums[left]);
            } 
        }
    }
};
~~~



#### [32. 最长有效括号](https://leetcode-cn.com/problems/longest-valid-parentheses/)：DP，双向计数

DP:

~~~c++
class Solution {
public:
    int longestValidParentheses(string s) {
        if (s.size()==0) return 0;
        int l=0,r=0,ans=0;
        for (int i=0;i<s.size();i++)
        {
            if (s[i]=='(') l++;
            else r++;
            if (l==r)
            {
                if (l+r>ans) ans=l+r;
            }
            else
            if (r>l)
            {
                l=0;
                r=0;
            }
        }
        l=0,r=0;
        for (int i=s.size()-1;i>=0;i--)
        {
            if (s[i]=='(') l++;
            else r++;
            if (l==r)
            {
                if (l+r>ans) ans=l+r;
            }
            else
            if (l>r)
            {
                l=0;
                r=0;
            }
        }
        return ans;
    }
};
~~~

双向计数：

~~~C++
class Solution {
public:
    int longestValidParentheses(string s) {
        if (s.size()==0) return 0;
        int l=0,r=0,ans=0;
        for (int i=0;i<s.size();i++)
        {
            if (s[i]=='(') l++;
            else r++;
            if (l==r)
            {
                if (l+r>ans) ans=l+r;
            }
            else
            if (r>l)
            {
                l=0;
                r=0;
            }
        }
        l=0,r=0;
        for (int i=s.size()-1;i>=0;i--)
        {
            if (s[i]=='(') l++;
            else r++;
            if (l==r)
            {
                if (l+r>ans) ans=l+r;
            }
            else
            if (l>r)
            {
                l=0;
                r=0;
            }
        }
        return ans;
    }
};
~~~



#### [33. 搜索旋转排序数组](https://leetcode-cn.com/problems/search-in-rotated-sorted-array/)：二分/异或精简:star:

~~~c++
class Solution {
public:
    int search(vector<int>& nums, int target) {
        if (nums.size()==0) return -1;
        int l=0,r=nums.size()-1,mid;
        if (nums.size()>2)
        while (l<r)
        {
            mid=(l+r)/2;
            if (nums[mid]>nums[l])
            {
                if (nums[l]<=target&&target<=nums[mid]) r=mid;
                else l=mid;
            }
            else
            {
                if (nums[mid]<=target&&target<=nums[r]) l=mid;
                else r=mid;
            }
            if (l==r-1) break;
        }
        if (nums[l]==target) return l;
        if (nums[r]==target) return r;
        return -1;
    }
};
~~~

异或：

~~~c++
class Solution {
public:
    int search(vector<int>& nums, int target) {
        int l = 0, r = nums.size() - 1;
        while (l < r) {
            int mid = (l + r) / 2;
            if ((nums[0] > target) ^ (nums[0] > nums[mid]) ^ (target > nums[mid]))
                l = mid + 1;
            else
                r = mid;
        }
        return l == r && nums[l] == target ? l : -1;
    }
};
~~~



#### [39. 组合总和](https://leetcode-cn.com/problems/combination-sum/)：DP/DFS :star:

DP:

~~~c++
class Solution {
public:
    vector<vector<int>> combinationSum(vector<int>& candidates, int target) {
        map<int, set<vector<int>>> dict;
        for (int i=1;i<=target;i++)
            for (auto it:candidates)
                if (i==it) dict[i].insert(vector<int>{it});
                else
                    if (i>it)
                        for (auto vec:dict[i-it])
                        {
                            vec.push_back(it);
                            sort(vec.begin(),vec.end());
                            if (dict[i].count(vec)==0) dict[i].insert(vec);
                        }
        vector <vector<int>> ans;
        for (auto it:dict[target]) ans.push_back(it);
        return ans;
    }
};
~~~

DFS:

~~~c++
class Solution {
public:
    vector<vector<int>> out;
    vector<int> tmp;
    void result(int idx, vector<int>& candidates, int target)
    {
        if (target <= 0)
        {
            if (target == 0)
            {
                out.push_back(tmp);
            }
            return;
        }
        for (int i = idx; i < candidates.size(); i++)
        {
            tmp.push_back(candidates[i]);
            result(i, candidates, target - candidates[i]);
            tmp.pop_back();
        }
    }
    vector<vector<int>>combinationSum(vector<int>& candidates, int target) {
        result(0, candidates, target);
        return out;
    }
};
~~~



#### [41. 缺失的第一个正数](https://leetcode-cn.com/problems/first-missing-positive/)：抽屉原理+异或:star:

~~~c++
class Solution {
public:
    int firstMissingPositive(vector<int>& nums) {
        for (int i=0;i<nums.size();i++)
        {
            while (nums[i]!=i+1)
            {
                if (nums[i]<=0||nums[i]>nums.size()||nums[i]==nums[nums[i]-1]) break;
                int now=nums[i]-1;
                nums[now]=nums[now]^nums[i];
                nums[i]=nums[now]^nums[i];
                nums[now]=nums[now]^nums[i];
                //nums[i]=nums[now];
                //nums[now]=now+1;
            }
        }
        for (int i=0;i<nums.size();i++)
        if (nums[i]!=i+1) return i+1;
        return nums.size()+1;
    }
};
~~~



#### [42. 接雨水](https://leetcode-cn.com/problems/trapping-rain-water/)：双指针

~~~c++
class Solution {
public:
    int trap(vector<int>& height) {
        int left=0,right=height.size()-1;
        int ans=0,lmax=0,rmax=0;
        while (left<right)
        {
            if (height[left]<height[right])
            {
                if (height[left]>=lmax) lmax=height[left];
                else ans+=lmax-height[left];
                left++;
            }
            else
            {
                if (height[right]>=rmax) rmax=height[right];
                else ans+=rmax-height[right];
                right--;
            }
        }
        return ans;
    }
};
~~~

#### [46. 全排列](https://leetcode-cn.com/problems/permutations/)：DFS

~~~ c++
class Solution {
public:
    vector<vector<int>> permute(vector<int>& nums) {
        vector<vector<int>> res;
        vector<int> path;
        vector<bool> used(nums.size(), false);
        dfs(nums, res, used, path);
        return res; 
    }
    void dfs(const vector<int>& nums, vector<vector<int> > & res, vector<bool> & used, vector<int> & path) {
        if (path.size() == nums.size()) {
            res.push_back(path);
            return;
        }
        for (int i = 0; i < nums.size(); ++ i) {
            if (!used[i]){
                used[i] = true;
                path.push_back(nums[i]);
                dfs(nums, res, used, path);
                path.pop_back();
                used[i] = false;
            }
        }
    }
};

~~~



#### [50. Pow(x, n)](https://leetcode-cn.com/problems/powx-n/)：分治/快速幂:star:

分治：

~~~c++
class Solution {
public:
    double myPow(double x, int n) {
        long long m=n;
        if (m==0) return 1;
        if (m<0)
        {
            x=1/x;
            m=-m;
        }
        if (m==1) return x;
        double hal=myPow(x,int(m/2));
        if (m%2==0) return hal*hal;
        else return x*hal*hal;
    }
};
~~~

快速幂：

~~~c++
class Solution {
public:
    double myPow(double x, int n) {
        long long m=n;
        if (m<0)
        {
            x=1/x;
            m=-m;
        }
        double ans=1,h=x;
        for (long long i=m;i;i/=2)
        {
            if (i%2==1) ans=ans*h;
            h=h*h;
        }
        return ans;
    }
};
~~~



#### [53. 最大子序和](https://leetcode-cn.com/problems/maximum-subarray/)：DP/分治

DP:

~~~c++
class Solution {
public:
    int maxSubArray(vector<int>& nums) {
        int n=nums.size();
        int ans=nums[0];
        int last=nums[0],now;
        for (int i=1;i<n;i++)
        {
            if (last>0) now=last+nums[i];
            else now=nums[i];
            if (now>ans) ans=now;
            last=now;
        }
        return ans;
    }
};
~~~



分治：

~~~c++
class Solution
{
public:
    int maxSubArray(vector<int> &nums)
    {
        //类似寻找最大最小值的题目，初始值一定要定义成理论上的最小最大值
        int result = INT_MIN;
        int numsSize = int(nums.size());
        result = maxSubArrayHelper(nums, 0, numsSize - 1);
        return result;
    }

    int maxSubArrayHelper(vector<int> &nums, int left, int right)
    {
        if (left == right)
        {
            return nums[left];
        }
        int mid = (left + right) / 2;
        int leftSum = maxSubArrayHelper(nums, left, mid);
        //注意这里应是mid + 1，否则left + 1 = right时，会无线循环
        int rightSum = maxSubArrayHelper(nums, mid + 1, right);
        int midSum = findMaxCrossingSubarray(nums, left, mid, right);
        int result = max(leftSum, rightSum);
        result = max(result, midSum);
        return result;
    }

    int findMaxCrossingSubarray(vector<int> &nums, int left, int mid, int right)
    {
        int leftSum = INT_MIN;
        int sum = 0;
        for (int i = mid; i >= left; i--)
        {
            sum += nums[i];
            leftSum = max(leftSum, sum);
        }

        int rightSum = INT_MIN;
        sum = 0;
        //注意这里i = mid + 1，避免重复用到nums[i]
        for (int i = mid + 1; i <= right; i++)
        {
            sum += nums[i];
            rightSum = max(rightSum, sum);
        }
        return (leftSum + rightSum);
    }
};

~~~



#### [55. 跳跃游戏](https://leetcode-cn.com/problems/jump-game/)：dp<贪心:star:

~~~c++
class Solution {
public:
    bool canJump(vector<int>& nums) {
        int ans=0;
        for (int i=0;i<nums.size();i++)
        {
            if (ans<i) return false;
            ans=max(ans,i+nums[i]);
            if (ans>=nums.size()) break;
        }
        return true;
    }
};
~~~



#### [56. 合并区间](https://leetcode-cn.com/problems/merge-intervals/)：sort+双指针

~~~ c++
class Solution {
public:
    vector<vector<int>> merge(vector<vector<int>>& intervals) {
        int n=intervals.size();
        sort(intervals.begin(),intervals.end());
        vector<vector<int>> ans;
        for (int i=0;i<n;i++)
        {
            int h=i+1;
            while (h<n&&intervals[i][0]<=intervals[h][0]&&intervals[h][0]<=intervals[i][1])
            {
                intervals[i][1]=max(intervals[i][1],intervals[h][1]);
                h++;
            }
            ans.push_back(intervals[i]);
            i=h-1;
        }
        return ans;
    }
};
~~~



#### [70. 爬楼梯](https://leetcode-cn.com/problems/climbing-stairs/)：DP/Fibonacci

DP:

~~~c++
class Solution {
public:
    int climbStairs(int n) {
        if (n==1) return 1;
        if (n==2) return 2;
        int n1=1,n2=2,ans=0;
        for (int i=3;i<=n;i++)
        {
            ans=n1+n2;
            n1=n2;
            n2=ans;
        }
        return ans;
    }
};
~~~

Fibonacci:

![image-20191223102046767](C:\Users\Dell\AppData\Roaming\Typora\typora-user-images\image-20191223102046767.png)

~~~c++
class Solution {
public:
    int climbStairs(int n) {
        double sqrt5=sqrt(5);
        double fibn=pow((1+sqrt5)/2,n+1)-pow((1-sqrt5)/2,n+1);
        return (int)(fibn/sqrt5);
    }
};
~~~



#### [72. 编辑距离](https://leetcode-cn.com/problems/edit-distance/)：DP

~~~c++
class Solution {
public:
    int minDistance(string word1, string word2) {
        int n=word1.size(),m=word2.size();
        int f[n+1][m+1];
        for (int i=0;i<=n;i++) f[i][0]=i;
        for (int j=0;j<=m;j++) f[0][j]=j;
        for (int i=1;i<=n;i++)
        for (int j=1;j<=m;j++)
        {
            if (word1[i-1]==word2[j-1]) f[i][j]=min(f[i-1][j-1],min(f[i-1][j],f[i][j-1])+1);
                               else f[i][j]=min(f[i-1][j-1]+1,min(f[i-1][j],f[i][j-1])+1);
            //printf("%d %d %d\n",i,j,f[i][j]);
        }
        return f[n][m];
    }
};
~~~



#### [76. 最小覆盖子串](https://leetcode-cn.com/problems/minimum-window-substring/)：双指针:star:

~~~c++
class Solution {
public:
    string minWindow(string s, string t) {
        int count[256]={0};
        for (auto c:t) count[c]++;
        int len=0,minLength=s.length();
        string ans;
        for (int l=0,r=0;r<s.length();r++) 
        {
            if (--count[s[r]]>=0) len++;
            while (len==t.length()) 
            {
                if (r-l+1<= minLength) 
                {
                    minLength=r-l+1;
                    ans=s.substr(l,r-l+1);
                }
                if (++count[s[l++]]>0) len--;
            }
        }
        return ans;
    }
};
~~~



#### [84. 柱状图中最大的矩形](https://leetcode-cn.com/problems/largest-rectangle-in-histogram/)：单调栈:star:

~~~c++
class Solution {
public:
    int largestRectangleArea(vector<int>& heights) {
        stack<int> s;
        heights.push_back(0);
        int n=heights.size();
        int ans=0,now;
        for (int i=0;i<n;i++)
        {
            while (!s.empty()&&heights[s.top()]>=heights[i])
            {
                now=s.top();
                s.pop();
                ans=max(ans,heights[now]*(s.empty()?i:i-s.top()-1));
            }
            s.push(i);
        }        
        return ans;
    }
};
~~~



#### [94. 二叉树的中序遍历](https://leetcode-cn.com/problems/binary-tree-inorder-traversal/)：二叉树基本操作

递归：

~~~c++
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode(int x) : val(x), left(NULL), right(NULL) {}
 * };
 */
class Solution {
public:
    vector<int> inorderTraversal(TreeNode* root) {
        if (root==NULL) return {};
        vector<int> ans,r;
        ans=inorderTraversal(root->left);
        ans.push_back(root->val);
        r=inorderTraversal(root->right);
        ans.insert(ans.end(),r.begin(),r.end());
        //for (int i=0;i<r.size();i++) ans.push_back(r[i]);
        return ans;
    }
};
~~~

非递归：

~~~c++
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode(int x) : val(x), left(NULL), right(NULL) {}
 * };
 */
class Solution {
public:
    vector<int> inorderTraversal(TreeNode* root) {
        stack<TreeNode*> S;
        vector<int> v;
        TreeNode* rt = root;
        while(rt || S.size())
        {
            while(rt)
            {
                S.push(rt);
                rt=rt->left;
            }
            rt=S.top();
            S.pop();
            v.push_back(rt->val);
            rt=rt->right;
        }
        return v;
    }
};
~~~

#### [99. 恢复二叉搜索树](https://leetcode-cn.com/problems/recover-binary-search-tree/) ：Morris遍历:star:

~~~ c++
class Solution {
public:
    void recoverTree(TreeNode* root) {
        TreeNode *x = NULL, *y = NULL, *pre = NULL, *rightmost = NULL;
        while (root) {
            // 如果有左子树，就递归遍历左子树。
            if (root->left) {
                rightmost = root->left;
                // 找出左子树的最右边一个叶子结点
                while (rightmost->right && rightmost->right != root) {
                    rightmost = rightmost->right;
                }
                // 如果左子树最右边的叶子结点的右儿子是空的，
                // 那就说明根结点是第一次访问，那么就把它的右儿子指向根结点。
                // 然后递归遍历左子树。
                if (rightmost->right != root) {
                    rightmost->right = root;
                    root = root->left;
                // 否则的话说明根结点是第二次访问了，
                // 那就说明左子树已经递归完毕了，
                // 那么就判断一下是否存在逆序对。
                // 记得把左子树最右叶子结点的右儿子改回空指针。
                // 然后递归遍历右子树了。
                } else {
                    if (pre && pre->val > root->val) {
                        if (x == NULL) x = pre;
                        y = root;
                    }
                    rightmost->right = NULL;
                    pre = root;
                    root = root->right;
                }
            // 如果没有左子树，那就直接遍历右子树，同时判断是否存在逆序对。
            } else {
                if (pre && pre->val > root->val) {
                    if (x == NULL) x = pre;
                    y = root;
                }
                pre = root;
                root = root->right;
            }
        }
        swap(x->val, y->val);
    }
};
~~~



#### [121. 买卖股票的最佳时机](https://leetcode-cn.com/problems/best-time-to-buy-and-sell-stock/)

~~~ c++
class Solution {
public:
    int maxProfit(vector<int>& prices) {
        int n=prices.size();
        if (n<2) return 0;
        int ans=0,minprice=1e9;
        for (int i=0;i<n;i++)
        {
            ans=max(ans,prices[i]-minprice);
            minprice=min(minprice,prices[i]);
        }
        return ans;
    }
};
~~~



#### [124. 二叉树中的最大路径和](https://leetcode-cn.com/problems/binary-tree-maximum-path-sum/)：DP

~~~c++
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode(int x) : val(x), left(NULL), right(NULL) {}
 * };
 */
class Solution {
public:
    int maxPathSum(TreeNode* root) {
        int ans=INT_MIN;
        treedp(root,ans);
        return ans;
    }

    int treedp(TreeNode* k,int &ans)
    {
        if (k==NULL) return 0;
        int lmax,rmax;
        lmax=max(treedp(k->left,ans),0);
        rmax=max(treedp(k->right,ans),0);
        ans=max(ans,lmax+rmax+k->val);
        return max(lmax,rmax)+k->val;
    }
};
~~~



#### [128. 最长连续序列](https://leetcode-cn.com/problems/longest-consecutive-sequence/)：hash:star:

~~~c++
class Solution {
public:
    int longestConsecutive(vector<int>& nums) {
        if (nums.empty()) return 0;
        unordered_set<int> hash(nums.begin(),nums.end());
        int ans=0;
        for (int i=0;i<nums.size();i++)
        {
            if (hash.count(nums[i]-1)==0)
            {
                int j=nums[i]+1;
                while (hash.count(j)) j++;
                if (j-nums[i]>ans) ans=j-nums[i];
            }
        }
        return ans;
    }
};
~~~

#### [133. 克隆图](https://leetcode-cn.com/problems/clone-graph/):深拷贝:star:

~~~ c++
/*
// Definition for a Node.
class Node {
public:
    int val;
    vector<Node*> neighbors;
    
    Node() {
        val = 0;
        neighbors = vector<Node*>();
    }
    
    Node(int _val) {
        val = _val;
        neighbors = vector<Node*>();
    }
    
    Node(int _val, vector<Node*> _neighbors) {
        val = _val;
        neighbors = _neighbors;
    }
};
*/

class Solution {
public:
    Node* cloneGraph(Node* node) {
        if (!node) return nullptr;
		unordered_map<Node*, Node*> mp;
		queue<Node*> que({ node });
		const auto new_node = new Node(node->val);
		mp[node] = new_node;
		while (!que.empty()) {
			auto temp = que.front();
			que.pop();
			for (const auto&e : temp->neighbors) {
				if (!mp.count(e)) {
					que.push(e);
					mp[e] = new Node(e->val);
				}
				mp[temp]->neighbors.push_back(mp[e]);
			}
		}
		return mp[node];
    }
};
~~~



#### [135. 分发糖果](https://leetcode-cn.com/problems/candy/):贪心，记录波峰

![image-20191210170129730](C:\Users\Dell\AppData\Roaming\Typora\typora-user-images\image-20191210170129730.png)

~~~c++
class Solution {
public:
    int candy(vector<int>& ratings) {
        int size=ratings.size();
        if (size==0) return 0;
        int ans=1,candynow=1,maxn=0,downindex=0;
        for (int i=1;i<size;i++)
        {
            if (ratings[i]>ratings[i-1])
            {
                candynow++;
                downindex=0;
            }
            else
            if (ratings[i]==ratings[i-1])
            {
                candynow=1;
                downindex=0;
            }
            else
            {
                if (downindex==0)
                {
                    maxn=candynow;
                    downindex=i;
                }
                candynow=1;
                ans+=i-downindex;
                if (i-downindex+1>=maxn) ans++;
            }
            ans+=candynow;
        }
            
        return ans;
    }
};
~~~



#### [136. 只出现一次的数字](https://leetcode-cn.com/problems/single-number/)：异或:star:

~~~c++
class Solution {
public:
    int singleNumber(vector<int>& nums) {
        int ans=nums[0];
        for (int i=1;i<nums.size();i++) ans=ans^nums[i];
        return ans;
    }
};
~~~

#### [141. 环形链表](https://leetcode-cn.com/problems/linked-list-cycle/):快慢指针:star:

~~~ c++
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode(int x) : val(x), next(NULL) {}
 * };
 */
class Solution {
public:
    bool hasCycle(ListNode *head) {
        if (head==NULL || head->next==NULL) return false;
        ListNode* fast=head;
        ListNode* slow=head;
        while (fast!=NULL && fast->next!=NULL)
        {
            slow=slow->next;
            fast=fast->next->next;
            if (slow==fast) return true;
        }
        return false;
    }
};
~~~



#### [142. 环形链表 II](https://leetcode-cn.com/problems/linked-list-cycle-ii/) ：快慢指针:star:

~~~ c++
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode(int x) : val(x), next(NULL) {}
 * };
 */
class Solution {
public:
    ListNode *detectCycle(ListNode *head) {
        if (head==NULL || head->next==NULL) return NULL;
        ListNode* fast=head;
        ListNode* slow=head;
        while (fast!=NULL && fast->next!=NULL)
        {
            slow=slow->next;
            fast=fast->next->next;
            if (slow==fast)
            {
                fast=head;
                while (fast!=slow)
                {
                    fast=fast->next;
                    slow=slow->next;
                }
                return fast;
            }
        }
        return NULL;
    }
};
~~~



#### [198. 打家劫舍](https://leetcode-cn.com/problems/house-robber/)：DP

~~~c++
class Solution {
public:
    int rob(vector<int>& nums) {
        int n=nums.size();
        if (n==0) return 0;
        int f0,f1,l0,l1;
        l0=0;
        l1=nums[0];
        for (int i=1;i<n;i++)
        {
            f0=max(l0,l1);
            f1=l0+nums[i];
            l0=f0;
            l1=f1;
        }
        return l0>l1?l0:l1;
    }
};
~~~



#### [199. 二叉树的右视图](https://leetcode-cn.com/problems/binary-tree-right-side-view/)：递归

~~~ c++
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode(int x) : val(x), left(NULL), right(NULL) {}
 * };
 */
class Solution {
public:
    vector<int> rightSideView(TreeNode* root) {
        vector<int> ans;
        if (root==NULL) return ans;
        dfs(root, 1, ans);
        return ans;
    }

    void dfs(TreeNode* root, int k, vector<int>& ans)
    {
        if (k>ans.size())
        {
            ans.push_back(root->val);
        }
        if (root->right!=NULL) dfs(root->right, k+1, ans);
        if (root->left!=NULL) dfs(root->left, k+1, ans);
    }
};
~~~



#### [200. 岛屿数量](https://leetcode-cn.com/problems/number-of-islands/)：递归

~~~c++
class Solution {
public:
    int numIslands(vector<vector<char>>& grid) {
        int ans=0;
        for (int i=0;i<grid.size();i++)
         for (int j=0;j<grid[0].size();j++)
            if (grid[i][j]=='1')
            {
                calc(grid,i,j);
                ans++;
            }
        return ans;
    }
    void calc(vector<vector<char>>& grid,int p,int q)
    {
        grid[p][q]='0';
        if (p>0&&grid[p-1][q]=='1') calc(grid,p-1,q);
        if (q>0&&grid[p][q-1]=='1') calc(grid,p,q-1);
        if (p+1<grid.size()&&grid[p+1][q]=='1') calc(grid,p+1,q);
        if (q+1<grid[0].size()&&grid[p][q+1]=='1') calc(grid,p,q+1);
    }
};
~~~



#### [207. 课程表](https://leetcode-cn.com/problems/course-schedule/)：拓扑排序:star:

~~~ c++
class Solution {
public:
    bool canFinish(int numCourses, vector<vector<int>>& prerequisites) {
        vector<int> indegree(numCourses);
        vector<vector<int>> graph(numCourses);
        vector<int> v;
        for (int i=0;i<numCourses;i++)
        {
            indegree[i]=0;
            graph.push_back(v);
        }
        for (int i=0;i<prerequisites.size();i++)
        {
            indegree[prerequisites[i][0]]++;
            graph[prerequisites[i][1]].push_back(prerequisites[i][0]);
        }
        queue<int> q;
        for (int i=0;i<numCourses;i++)
            if (indegree[i]==0) q.push(i);
        int ans=0;
        while (!q.empty())
        {
            int tmp=q.front();
            ans++;
            q.pop();
            for (int i=0;i<graph[tmp].size();i++)
            {
                indegree[graph[tmp][i]]--;
                if (indegree[graph[tmp][i]]==0) q.push(graph[tmp][i]);
            }
        }
        if (ans==numCourses) return true;
        return false;
    }
};
~~~



#### [210. 课程表 II](https://leetcode-cn.com/problems/course-schedule-ii/)

~~~ c++
class Solution {
public:
    vector<int> findOrder(int numCourses, vector<vector<int>>& prerequisites) {
        vector<int> indegree(numCourses);
        vector<vector<int>> graph(numCourses);
        vector<int> v;
        for (int i=0;i<numCourses;i++)
        {
            indegree[i]=0;
            graph.push_back(v);
        }
        for (int i=0;i<prerequisites.size();i++)
        {
            indegree[prerequisites[i][0]]++;
            graph[prerequisites[i][1]].push_back(prerequisites[i][0]);
        }
        queue<int> q;
        for (int i=0;i<numCourses;i++)
            if (indegree[i]==0) q.push(i);
        vector<int> ans;
        while (!q.empty())
        {
            int tmp=q.front();
            ans.push_back(tmp);
            q.pop();
            for (int i=0;i<graph[tmp].size();i++)
            {
                indegree[graph[tmp][i]]--;
                if (indegree[graph[tmp][i]]==0) q.push(graph[tmp][i]);
            }
        }
        if (ans.size()!=numCourses) ans.clear();
        return ans;
    }
};
~~~






#### [213. 打家劫舍 II](https://leetcode-cn.com/problems/house-robber-ii/)：DP

~~~c++
class Solution {
public:
    int rob(vector<int>& nums) {
        int n=nums.size();
        if (n==0) return 0;
        if (n==1) return nums[0];
        int f0,f1,l0,l1,r0,r1;
        l0=0;
        l1=nums[0];
        r0=0;
        r1=nums[1];
        for (int i=1;i<n-1;i++)
        {
            f0=max(l0,l1);
            f1=l0+nums[i];
            l0=f0;
            l1=f1;
            f0=max(r0,r1);
            f1=r0+nums[min(i+1,n-1)];
            r0=f0;
            r1=f1;
        }
        return max(max(l0,l1),max(r0,r1));
    }
};
~~~



#### [226. 翻转二叉树](https://leetcode-cn.com/problems/invert-binary-tree/)：二叉树基本操作

~~~c++
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode(int x) : val(x), left(NULL), right(NULL) {}
 * };
 */
class Solution {
public:
    TreeNode* invertTree(TreeNode* root) {
        if (root==NULL) return NULL;
        struct TreeNode* l=invertTree(root->left);
        struct TreeNode* r=invertTree(root->right);
        root->left=r;
        root->right=l;
        return root;
    }
};
~~~



#### [233. 数字 1 的个数](https://leetcode-cn.com/problems/number-of-digit-one/)：数学:star:

![image-20191216112122068](C:\Users\Dell\AppData\Roaming\Typora\typora-user-images\image-20191216112122068.png)

~~~c++
class Solution {
public:
    int countDigitOne(int n) {
        int ans=0;
        for (long long i=1; i<=n; i*=10)
        {
            long long divider=i*10;
            ans+=(n/divider)*i+min(max(n%divider-i+1,(long long)0),i);
        }
        return ans;
    }
};
~~~

#### [235. 二叉搜索树的最近公共祖先](https://leetcode-cn.com/problems/lowest-common-ancestor-of-a-binary-search-tree/) ：迭代

~~~ c++
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode(int x) : val(x), left(NULL), right(NULL) {}
 * };
 */
class Solution {
public:
    TreeNode* lowestCommonAncestor(TreeNode* root, TreeNode* p, TreeNode* q) {
        if (root == NULL) return NULL;
        TreeNode* h = root;
        int m=p->val,n= q->val;
        if (m>n) swap(m,n);
        while (!(m<h->val&&h->val<n))
        {
            if (m<h->val&&n<h->val) h=h->left;
            else if (m>h->val&&n>h->val) h=h->right;
            else break;
        }
        return h;
    }
};
~~~

#### [236. 二叉树的最近公共祖先](https://leetcode-cn.com/problems/lowest-common-ancestor-of-a-binary-tree/) :LCA

~~~ c++
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode(int x) : val(x), left(NULL), right(NULL) {}
 * };
 */
class Solution {
public:
    TreeNode* lowestCommonAncestor(TreeNode* root, TreeNode* p, TreeNode* q) {
        if (!root || root == p || root == q) return root;
        TreeNode *left = lowestCommonAncestor(root->left, p, q);
        TreeNode *right = lowestCommonAncestor(root->right, p, q);
        if (left && right) return root;
        return left ? left : right;
    }
};
~~~



#### [238. 除自身以外数组的乘积](https://leetcode-cn.com/problems/product-of-array-except-self/)：模拟

~~~c++
class Solution {
public:
    vector<int> productExceptSelf(vector<int>& nums) {
        if (nums.size()==0) return {};
        vector<int> ans;
        int h=1;
        for (int i=0;i<nums.size();i++)
        {
            ans.push_back(h);
            h*=nums[i];
        }
        h=1;
        for (int i=nums.size()-1;i>=0;i--)
        {
            ans[i]*=h;
            h*=nums[i];
        }
        return ans;
    }
};
~~~



#### [310. 最小高度树](https://leetcode-cn.com/problems/minimum-height-trees/) ：拓扑排序变形:star:

~~~ c++
class Solution {
public:
    vector<int> findMinHeightTrees(int n, vector<vector<int>>& edges) {
        if (n == 1) return { 0 };
	        else if (n == 2) return{ 0,1 };
        vector<int> degree(n);
        vector<vector<int>> graph(n);
        vector<int> v;
        for (int i = 0; i < n; i++)
        {
            degree[i] = 0;
            graph.push_back(v);
        }
        for (int i=0;i<edges.size();i++)
        {
            degree[edges[i][0]]++;
            degree[edges[i][1]]++;
            graph[edges[i][0]].push_back(edges[i][1]);
            graph[edges[i][1]].push_back(edges[i][0]);
        }
        queue<int> que;
        int cnt=0;
        for (int i=0;i<n;i++)
            if (degree[i]==1)
            {
                cnt++;
                que.push(i);
            }
        while (n>2)
        {
            n-=cnt;
            while (cnt--)
            {
                int tmp=que.front();
                que.pop();
                degree[tmp]=0;
                for (int i=0;i<graph[tmp].size();i++)
                {
                    degree[graph[tmp][i]]--;
                    if (degree[graph[tmp][i]]==1) que.push(graph[tmp][i]);
                }
            }
            cnt=que.size();
        }
        vector<int> ans;
        while (!que.empty())
        {
            ans.push_back(que.front());
            que.pop();
        }
        return ans;
    }
};
~~~



#### [289. 生命游戏](https://leetcode-cn.com/problems/game-of-life/)：位运算

~~~ c++
class Solution {
public:
    void gameOfLife(vector<vector<int>>& board) {
        int dx[] = {-1,  0,  1, -1, 1, -1, 0, 1};
        int dy[] = {-1, -1, -1,  0, 0,  1, 1, 1};
        int n=board.size();
        int m=board[0].size();
        for(int i = 0; i < n; i++) {
            for(int j = 0 ; j < m; j++) {
                int sum = 0;
                for(int k = 0; k < 8; k++) {
                    int nx = i + dx[k];
                    int ny = j + dy[k];
                    if(nx >= 0 && nx < n && ny >= 0 && ny < m) {
                        sum += (board[nx][ny]&1);
                    }
                }
                if(board[i][j] == 1) {
                    if(sum == 2 || sum == 3) {
                        board[i][j] |= 2;
                    }
                } else {
                    if(sum == 3) {
                        board[i][j] |= 2;
                    }
                }
            }
        }
        for(int i = 0; i < n; i++) {
            for(int j = 0; j < m; j++) {
                board[i][j] >>= 1; 
            }
        }
    }
};
~~~



#### [365. 水壶问题](https://leetcode-cn.com/problems/water-and-jug-problem/)：贝祖定理/DFS :star:

~~~ c++
class Solution {
public:
    bool canMeasureWater(int x, int y, int z) {
        if (x + y < z) return false;
        if (x == 0 || y == 0) return z == 0 || x + y == z;
        return z % gcd(x, y) == 0;
    }
};
~~~

~~~ c++
using PII = pair<int, int>;

class Solution {
public:
    bool canMeasureWater(int x, int y, int z) {
        stack<PII> stk;
        stk.emplace(0, 0);
        auto hash_function = [](const PII& o) {return hash<int>()(o.first) ^ hash<int>()(o.second);};
        unordered_set<PII, decltype(hash_function)> seen(0, hash_function);
        while (!stk.empty()) {
            if (seen.count(stk.top())) {
                stk.pop();
                continue;
            }
            seen.emplace(stk.top());
            
            auto [remain_x, remain_y] = stk.top();
            stk.pop();
            if (remain_x == z || remain_y == z || remain_x + remain_y == z) {
                return true;
            }
            // 把 X 壶灌满。
            stk.emplace(x, remain_y);
            // 把 Y 壶灌满。
            stk.emplace(remain_x, y);
            // 把 X 壶倒空。
            stk.emplace(0, remain_y);
            // 把 Y 壶倒空。
            stk.emplace(remain_x, 0);
            // 把 X 壶的水灌进 Y 壶，直至灌满或倒空。
            stk.emplace(remain_x - min(remain_x, y - remain_y), remain_y + min(remain_x, y - remain_y));
            // 把 Y 壶的水灌进 X 壶，直至灌满或倒空。
            stk.emplace(remain_x + min(remain_y, x - remain_x), remain_y - min(remain_y, x - remain_x));
        }
        return false;
    }
};
~~~



#### [300. 最长上升子序列](https://leetcode-cn.com/problems/longest-increasing-subsequence/)：n^2/nlogn:star:

n^2:

~~~ c++
class Solution {
public:
    int lengthOfLIS(vector<int>& nums) {
        if (nums.empty()) return 0;
        int n=nums.size();
        int f[n];
        int ans=1;
        for (int i=0;i<n;i++) f[i]=1;
        for (int i=1;i<n;i++)
        {
            for (int j=0;j<i;j++)
                if (nums[j]<nums[i]&&f[j]+1>f[i]) f[i]=f[j]+1;
            if (ans<f[i]) ans=f[i];
        }
        return ans;
    }
};
~~~

nlogn二分：

~~~ c++
class Solution {
public:
    int lengthOfLIS(vector<int>& nums) {
        int len = 1, n = (int)nums.size();
        if (n == 0) return 0;
        vector<int> d(n + 1, 0);
        d[len] = nums[0];
        for (int i = 1; i < n; i++) 
        {
            if (nums[i] > d[len]) d[++len] = nums[i];
            else{
                int l = 1, r = len, pos = 0;
                while (l <= r) 
                {
                    int mid = (l + r) >> 1;
                    if (d[mid] < nums[i]) 
                    {
                        pos = mid;
                        l = mid + 1;
                    }
                    else r = mid - 1;
                }
                d[pos + 1] = nums[i];
            }
        }
        return len;
    }
};
~~~

树状数组：

~~~ C++
vector<int> nms;
int n;

int id[10005];
bool cmp(int a, int b) {
    if (nms[a] == nms[b]) return a > b;
    return nms[a] < nms[b];
}

int mx[10005];
int lowbit(int x) {return x & -x;}
void add(int p, int v) {for (int i = p; i <= 10000; i += lowbit(i)) mx[i] = max(mx[i], v);}
int get(int p) {
    int ret = 0;
    for (int i = p; i >= 1; i -= lowbit(i)) ret = max(ret, mx[i]);
    return ret;
}

class Solution {
public:
    int lengthOfLIS(vector<int>& nums) {
        nms = nums;
        n = nms.size();
        nms.insert(nms.begin(), 0); // [1, n]
        for (int i = 1; i <= n; i++) id[i] = i;
        sort(id + 1, id + n + 1, cmp);
        memset(mx, 0, sizeof(mx));
        for (int i = 1; i <= n; i++) add(id[i], get(id[i] - 1) + 1);
        return get(n);
    }
};

~~~



#### [312. 戳气球](https://leetcode-cn.com/problems/burst-balloons/)：DP:star:

~~~c++
class Solution {
public:
    int maxCoins(vector<int>& nums) {
        nums.insert(nums.begin(),1);
        nums.push_back(1);
        int n=nums.size();
        int f[n][n];
        memset(f,0,sizeof(f));
        for (int r=2;r<n;r++)
        {
            for (int i=0;i<n-r;i++)
            {
                int j=i+r;
                for (int k=i+1;k<j;k++)
                f[i][j]=max(f[i][j],f[i][k]+f[k][j]+nums[i]*nums[k]*nums[j]);
            }
        }
        return f[0][n-1];
    }
};
~~~



#### [337. 打家劫舍 III](https://leetcode-cn.com/problems/house-robber-iii/)：DP:star:

~~~c++
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode(int x) : val(x), left(NULL), right(NULL) {}
 * };
 */
class Solution {
public:
    int rob(TreeNode* root) {
        pair<int,int> ans=treedp(root);
        return max(ans.first,ans.second);
    }

    pair<int,int> treedp(TreeNode *k)
    {
        if (k==NULL) return {0,0};
        if (k->left==NULL&&k->right==NULL) return {0,k->val};
        pair<int,int> f,l,r;
        l=treedp(k->left);
        r=treedp(k->right);
        f.first=max(l.first,l.second)+max(r.first,r.second);
        f.second=k->val+l.first+r.first;
        return f;
    }
};
~~~



#### [355. 设计推特](https://leetcode-cn.com/problems/design-twitter/)：哈希表+优先队列:star:

~~~ c++
bool operator<(const pair<int, int>& a, const pair<int, int>& b) {
    return a.first < b.first;
}

class Twitter {
public:
    int timeStamp;
    unordered_map<int, unordered_set<int>> subMap;
    unordered_map<int, vector<pair<int, int>>> postMap;

    /** Initialize your data structure here. */
    Twitter() {
        timeStamp = 0;
    }

    /** Compose a new tweet. */
    void postTweet(int userId, int tweetId) {
        postMap[userId].push_back({ ++timeStamp , tweetId });
    }

    /** Retrieve the 10 most recent tweet ids in the user's news feed. Each item in the news feed must be posted by users who the user followed or by the user herself. Tweets must be ordered from most recent to least recent. */
    vector<int> getNewsFeed(int userId) {
        vector<int> result;
        priority_queue<pair<int, int>> myQueue;
        subMap[userId].insert(userId);
        for (int i : subMap[userId])
            for (pair<int, int>& p : postMap[i])
                myQueue.push(p);
        while (!myQueue.empty() && result.size() < 10){
            result.push_back(myQueue.top().second);
            myQueue.pop();
        }
        return result;
    }

    /** Follower follows a followee. If the operation is invalid, it should be a no-op. */
    void follow(int followerId, int followeeId) {
        subMap[followerId].insert(followeeId);
    }

    /** Follower unfollows a followee. If the operation is invalid, it should be a no-op. */
    void unfollow(int followerId, int followeeId) {
        subMap[followerId].erase(followeeId);
    }
};

~~~



#### [399. 除法求值](https://leetcode-cn.com/problems/evaluate-division/):带权并查集 :star:

~~~ c++
class Solution {
public:
    unordered_map<int,pair<int,double>>father;
    int Find(int x){
        if(x != father[x].first){
            int t = father[x].first;
            father[x].first = Find(father[x].first);
            father[x].second *= father[t].second;
            return father[x].first;
        }
        return x;
    }
    void Union(int e1,int e2,double result){
        int f1 = Find(e1);
        int f2 = Find(e2);
        if(f1 != f2){
            father[f2].first = f1;
            father[f2].second = father[e1].second * result / father[e2].second;
        }
    }
    vector<double> calcEquation(vector<vector<string>>& equations, vector<double>& values, vector<vector<string>>& queries) {
        vector<double> ans;
        unordered_map<string,int> now;
        int num=0;
        for (int i=0;i<equations.size();i++)
        {
            if (now[equations[i][0]]==0)
            {
                now[equations[i][0]]=++num;
                father[num].first=num;
                father[num].second=1;
            }
            if (now[equations[i][1]]==0)
            {
                now[equations[i][1]]=++num;
                father[num].first=num;
                father[num].second=1;
            }
            Union(now[equations[i][0]],now[equations[i][1]],values[i]);
        }
        for (int i=0;i<queries.size();i++)
        {
            if (now[queries[i][0]]==0 || now[queries[i][1]]==0)
            {
                ans.push_back(-1.0);
                continue;
            }
            int e1=now[queries[i][0]];
            int e2=now[queries[i][1]];
            int f1=Find(e1);
            int f2=Find(e2);
            if (f1!=f2)
            {
                ans.push_back(-1.0);
                continue;
            }
            double t1=father[e1].second;
            double t2=father[e2].second;
            ans.push_back(t2/t1);
        }
        return ans;
    }
};
~~~



#### [445. 两数相加 II](https://leetcode-cn.com/problems/add-two-numbers-ii/)：链表原地操作:star:

~~~ c++
class Solution {
public:
    ListNode* addTwoNumbers(ListNode* l1, ListNode* l2) {
        int count = 0, temp;
        ListNode *head, *last;
        for(head = l1; head; head = head->next)
            count++;
        for(head = l2; head; head = head->next)
            count--;
        if(count < 0) swap(l1,l2);            
        last = head = new ListNode(0);
        head->next = l1;
        for(int i = abs(count); i != 0; i--){
            if(l1->val != 9)
                last = l1;
            l1 = l1->next;
        }
        while(l1){
            temp = l1->val + l2->val;
            if(temp > 9){
                temp -= 10;
                last->val += 1;
                last = last->next;
                while(last != l1){
                    last->val = 0;
                    last = last->next;
                }
            }
            else if(temp != 9)             
                last = l1;
            l1->val = temp;
            l1 = l1->next;
            l2 = l2->next;
        }
        return head->val == 1 ? head : head->next;
    }
};

~~~



#### [542. 01 矩阵](https://leetcode-cn.com/problems/01-matrix/)：DP/BFS

DP:

~~~ c++
class Solution {
public:
    vector<vector<int>> updateMatrix(vector<vector<int>>& matrix) {
        int n=matrix.size(),m=matrix[0].size();
        vector<vector<int>> dp(n, vector<int>(m, 0));
        for (int i = 0; i < n; i++) {
            for (int j = 0; j < m; j++) {
                if (!matrix[i][j]) continue;
                int top = i == 0 ? 10001 : dp[i-1][j] + 1;
                int left = j == 0 ? 10001 : dp[i][j-1] + 1;
                dp[i][j] = min(top, left);
            }
        }
        for (int i = n-1; i >= 0; i--) {
            for (int j = m-1; j >= 0; j--) {
                if (!matrix[i][j]) continue;
                int bottom = i == n-1 ? 10001 : dp[i+1][j] + 1;
                int right = j == m-1 ? 10001 : dp[i][j+1] +1;
                dp[i][j] = min(dp[i][j], min(bottom, right));
            }
        }
        return dp;
    }
};

~~~

BFS:

~~~ c++
class Solution {
public:
    vector<vector<int>> updateMatrix(vector<vector<int>>& matrix) {
        int numr = matrix.size();
        int numc = matrix[0].size();
        vector<pair<int,int>> around = {{0,1},{0,-1},{-1,0},{1,0}};
        vector<vector<int>> result(numr, vector<int>(numc, INT_MAX));
        queue<pair<int,int>> que;
        for(int i = 0; i < numr; i++){
            for(int j = 0; j < numc; j++){
                if(matrix[i][j] == 0){
                    result[i][j] = 0;
                    que.push({i, j});
                }
            }
        }
        while(!que.empty()){
            auto temp = que.front();
            que.pop();
            for(int i = 0; i < 4; i++){
                int x = temp.first + around[i].first;
                int y = temp.second + around[i].second;
                if(x >= 0 && x < numr && y >= 0 && y < numc){
                    if(result[x][y] > result[temp.first][temp.second] + 1){
                        result[x][y] = result[temp.first][temp.second] + 1;
                        que.push({x, y});
                    }
                }

            }
        }
        return result;
    }
};

~~~



#### [543. 二叉树的直径](https://leetcode-cn.com/problems/diameter-of-binary-tree/)

~~~ c++
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode(int x) : val(x), left(NULL), right(NULL) {}
 * };
 */
class Solution {
public:
    int diameterOfBinaryTree(TreeNode* root) {
        int ans=0;
        depth(root,ans);
        return ans;
    }
    int depth(TreeNode* root,int& ans)
    {
        if (!root) return 0;
        int l=depth(root->left,ans);
        int r=depth(root->right,ans);
        ans=max(ans,l+r);
        return max(l,r)+1;
    }
};
~~~



#### [560. 和为K的子数组](https://leetcode-cn.com/problems/subarray-sum-equals-k/)：hash:star:

~~~c++
class Solution {
public:
    int subarraySum(vector<int>& nums, int k) {
        int sum=0,ans=0;
        map<int,int> hash;
        hash[0]=1;
        for (int i=0;i<nums.size();i++)
        {
            sum+=nums[i];
            if (hash.count(sum-k)) ans+=hash[sum-k];
            ++hash[sum];
        }
        return ans;
    }
};
~~~



#### [647. 回文子串](https://leetcode-cn.com/problems/palindromic-substrings/)：DP/manacher:star:

DP:

~~~c++
class Solution {
public:
    int countSubstrings(string s) {
        int n=s.size();
        if (n<2) return n;
        int f[n][n],ans=0;
        memset(f,0,sizeof(f));
        for (int i=0;i<n;i++)
        {
            f[i][i]=1;
            ans++;
            if (i!=0&&s[i]==s[i-1]) f[i-1][i]=1,ans++;
        }
        for (int l=2;l<n;l++)
        {
            for (int i=0;i<n-l;i++)
            {
                int j=i+l;
                if (s[i]==s[j]&&f[i+1][j-1]==1) f[i][j]=1,ans++;
            }
        }
        return ans;
    }
};
~~~

Manacher:

~~~c++
class Solution {
public:
    int countSubstrings(string s) {
        int n=s.size();
        string str;
        str.append(1,'@');
        for (int i=0;i<n;i++)
        {
            str.append(1,'#');
            str.append(1,s[i]);
        }
        str.append("#$");
        n=2*n+1;
        int len[n+2];
        memset(len,0,sizeof(len));
        int mx=0,p=0;
        for (int i=1;i<=n;i++)
        {
            if (mx>i) len[i]=min(mx-i,len[2*p-i]);
                else len[i]=1;
            while (str[i-len[i]]==str[i+len[i]]) len[i]++;
            if (len[i]+i>mx) mx=len[i]+i,p=i;
        }
        int ans=0;
        for (int i=1;i<=n;i++) ans+=len[i]/2;
        return ans;
    }
};
~~~



#### [739. 每日温度](https://leetcode-cn.com/problems/daily-temperatures/)：栈

~~~c++
class Solution {
public:
    vector<int> dailyTemperatures(vector<int>& T) {
        vector<int> ans;
        if (T.size()<1) return {};
        for (int i=0;i<T.size();i++) ans.push_back(0);
        if (T.size()==1) return ans;
        stack<int> s;
        s.push(T.size()-1);
        for (int i=T.size()-2;i>=0;i--)
        {
            if (T[i]<T[s.top()])
            {
                ans[i]=s.top()-i;
                s.push(i);
            } 
            else
            {
                while (!s.empty()&&T[i]>=T[s.top()]) s.pop();
                if (!s.empty()) ans[i]=s.top()-i;
                s.push(i);
            }
        }
        return ans;
    }
};
~~~

#### [820. 单词的压缩编码](https://leetcode-cn.com/problems/short-encoding-of-words/)：后缀去重/字典树:star:

~~~ c++
class Solution {
public:
    int minimumLengthEncoding(vector<string>& words) {
        unordered_set<string> good(words.begin(), words.end());
        for (string word: words) {
            for (int k = 1; k < word.size(); ++k) {
                good.erase(word.substr(k));
            }
        }
        int ans = 0;
        for (string word: good) {
            ans += word.size() + 1;
        }
        return ans;
    }
};

~~~



#### [836. 矩形重叠](https://leetcode-cn.com/problems/rectangle-overlap/)：反向思维/投影

~~~ c++
class Solution {
public:
    bool isRectangleOverlap(vector<int>& rec1, vector<int>& rec2) {
        if(rec1[1]>=rec2[3] || rec1[3]<=rec2[1] || rec1[2]<=rec2[0] || rec1[0]>=rec2[2])
            return false;
        return true;
    }
};
~~~

~~~ c++
class Solution {
public:
    bool isRectangleOverlap(vector<int>& rec1, vector<int>& rec2) {
        bool x_overlap = !(rec1[2] <= rec2[0] || rec2[2] <= rec1[0]);
        bool y_overlap = !(rec1[3] <= rec2[1] || rec2[3] <= rec1[1]);
        return x_overlap && y_overlap;
    }
};
~~~

#### [876. 链表的中间结点](https://leetcode-cn.com/problems/middle-of-the-linked-list/) ：快慢指针

~~~ c++
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode(int x) : val(x), next(NULL) {}
 * };
 */
class Solution {
public:
    ListNode* middleNode(ListNode* head) {
        ListNode* fast=head;
        ListNode* slow=head;
        while (fast!=NULL && fast->next!=NULL)
        {
            fast=fast->next->next;
            slow=slow->next;
        }
        return slow;
    }
};
~~~

#### [887. 鸡蛋掉落](https://leetcode-cn.com/problems/super-egg-drop/)：DP+二分/单调性优化

DP+二分

~~~ c++
class Solution {
    unordered_map<int, int> memo;
    int dp(int K, int N) {
        if (memo.find(N * 100 + K) == memo.end()) {
            int ans;
            if (N == 0) ans = 0;
            else if (K == 1) ans = N;
            else {
                int lo = 1, hi = N;
                while (lo + 1 < hi) {
                    int x = (lo + hi) / 2;
                    int t1 = dp(K-1, x-1);
                    int t2 = dp(K, N-x);

                    if (t1 < t2) lo = x;
                    else if (t1 > t2) hi = x;
                    else lo = hi = x;
                }

                ans = 1 + min(max(dp(K-1, lo-1), dp(K, N-lo)),
                                   max(dp(K-1, hi-1), dp(K, N-hi)));
            }

            memo[N * 100 + K] = ans;
        }

        return memo[N * 100 + K];
    }
public:
    int superEggDrop(int K, int N) {
        return dp(K, N);
    }
};
~~~



单调性优化

~~~ c++
class Solution {
public:
    int superEggDrop(int K, int N) {
        int dp[N + 1];
        for (int i = 0; i <= N; ++i) dp[i] = i;
        for (int k = 2; k <= K; ++k) {
            int dp2[N + 1];
            int x = 1; 
            dp2[0] = 0;
            for (int n = 1; n <= N; ++n) {
                while (x < n && max(dp[x-1], dp2[n-x]) >= max(dp[x], dp2[n-x-1])) x++;
                dp2[n] = 1 + max(dp[x-1], dp2[n-x]);
            }
            for (int n = 1; n <= N; ++n) dp[n] = dp2[n];
        }
        return dp[N];
    }
};

~~~

DP

~~~ c++
class Solution {
public:
    
    int superEggDrop(int n, int m) {
        int f[110][10010];//f[i][j]表示当前有i个鸡蛋且最多可测j次时能测量最多多高的楼层
        for(int j = 1; j <= m; j++){//注意要先枚举测量次数 因为目标是f[n][j] 这样可以更快达到 且防止溢出
            for(int i = 1; i <= n; i++){//拥有i个鸡蛋 最多可测量j次时
                f[i][j] = f[i-1][j-1] + f[i][j-1] + 1;//高度=下半部分高度+上半部分高度+1
            }
            if(f[n][j]>=m) return j;
        }
        return 0;//保证函数有返回值
    }
};

~~~





#### [892. 三维形体的表面积](https://leetcode-cn.com/problems/surface-area-of-3d-shapes/)

~~~ c++
class Solution {
public:
    int surfaceArea(vector<vector<int>>& grid) {
        int dr[]{0, 1, 0, -1};
        int dc[]{1, 0, -1, 0};
        int N = grid.size();
        int ans = 0;
        for (int r = 0; r < N; ++r)
            for (int c = 0; c < N; ++c)
                if (grid[r][c] > 0) {
                    ans += 2;
                    for (int k = 0; k < 4; ++k) {
                        int nr = r + dr[k];
                        int nc = c + dc[k];
                        int nv = 0;
                        if (0 <= nr && nr < N && 0 <= nc && nc < N)
                            nv = grid[nr][nc];
                        ans += max(grid[r][c] - nv, 0);
                    }
                }
        return ans;
    }
};
~~~

#### [914. 卡牌分组](https://leetcode-cn.com/problems/x-of-a-kind-in-a-deck-of-cards/)：素数筛:star:

~~~ c++
class Solution {
public:
    bool hasGroupsSizeX(vector<int>& deck) {
        vector<int> primes;
        bool mark[10000] = {0};
        for(int i = 2; i < 10000; i++) {
            if(mark[i]) continue;
            primes.push_back(i);
            for(int j = i + i; j < 10000; j += i) mark[j] = true;
        }
        unordered_map<int, int> cnt;
        for(auto v : deck) cnt[v]++;
        auto minIter = cnt.begin();
        for(auto it = cnt.begin(); it != cnt.end(); it++) {
            if(it->second < minIter->second) minIter = it;
        }
        if(minIter->second <= 1) return false;
        for(auto v : primes) {
            if(deck.size() % v) continue;
            if(v > minIter->second) break;
            bool flag = true;
            for(auto it = cnt.cbegin(); flag && it != cnt.cend(); ++it) 
                if(it->second % v) flag = false;
            if(flag) return true;
        }
        return false;
    }
};

~~~



#### [945. 使数组唯一的最小增量](https://leetcode-cn.com/problems/minimum-increment-to-make-array-unique/)

~~~ c++
class Solution {
public:
    int minIncrementForUnique(vector<int>& A) {
        if (A.size()==0) return 0;
        int n=A.size();
        sort(A.begin(),A.end());
        int ans=0;
        for (int i=1;i<n;i++)
        {
            if (A[i]<=A[i-1])
            {
                ans+=A[i-1]+1-A[i];
                A[i]=A[i-1]+1;
            }
        }
        return ans;
    }
};
~~~



#### [1071. 字符串的最大公因子](https://leetcode-cn.com/problems/greatest-common-divisor-of-strings/)：GCD:star:

~~~ c++
class Solution {
public:
    string gcdOfStrings(string str1, string str2) {
        return (str1+str2==str2+str1)?str1.substr(0, gcd(str1.size(), str2.size())): "";
    }
};
~~~



#### [1111. 有效括号的嵌套深度](https://leetcode-cn.com/problems/maximum-nesting-depth-of-two-valid-parentheses-strings/)

~~~ c++
class Solution {
public:
    vector<int> maxDepthAfterSplit(string seq) {
        vector<int> ans;
        int depth=0;
        for (int i=0;i<seq.size();i++)
        {
            if (seq[i]=='(')
            {
                depth++;
                ans.push_back(depth%2);
            }
            else 
            {
                ans.push_back(depth%2);
                depth--;
            }
        }
        return ans;
    }
};
~~~



#### [1248. 统计「优美子数组」](https://leetcode-cn.com/problems/count-number-of-nice-subarrays/)：双指针

~~~ c++
class Solution {
public:
    int numberOfSubarrays(vector<int>& nums, int k) {
        int ans=0,now=0,tot=0;
        int l=0,r=0;
        int n=nums.size();
        while (r<n)
        {
            if (now<k&&(nums[r++]&1)) now++;
            if (now==k)
            {
                tot=1;
                while (!(nums[l++]&1)) tot++;
                now--;
            }
            ans+=tot;
        }
        return ans;
    }
};
~~~

