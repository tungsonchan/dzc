[toc]

## [Leetcode Record]( https://leetcode-cn.com/problemset/all/ )

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
            printf("%d\n",ans);
        }
        return ans;
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

