# LeetCode Hot 100

## tag

数组：

栈：[20. 有效的括号](# 20. 有效的括号) [42. 接雨水](# 42. 接雨水)  [84. 柱状图中最大的矩形](# 84. 柱状图中最大的矩形)  [85. 最大矩形](# 85. 最大矩形)   [94. 二叉树的中序遍历](# 94. 二叉树的中序遍历)  [394. 字符串解码](# 394. 字符串解码) [739. 每日温度](# 739. 每日温度) 

队列：

树：

堆：[23. 合并k个排序链表](# 23. 合并k个排序链表) [215. 数组中的第k个最大元素](# 215. 数组中的第k个最大元素) 

图：[56. 合并区间]()

双指针：[11. 盛水最多的容器]()

排序：[23. 合并k个排序链表](# 23. 合并k个排序链表)  [75. 颜色分类]() [215. 数组中的第k个最大元素](# 215. 数组中的第k个最大元素)

动态规划：[42. 接雨水](# 42. 接雨水) [198. 打家劫舍](# 198. 打家劫舍) [239. 滑动窗口最大值](# 239. 滑动窗口最大值) [337. 打家劫舍III](# 337. 打家劫舍III)

## 1. 两数之和
<img src="LeetCode%20Hot100.assets/image-20200131210844744.png" alt="image-20200131210844744"  align="left" style="zoom:80%;" />

**法一：暴力法**

遍历数组，对每个元素，分别检查余下元素中是否存在满足target - cur 的元素。

```c++
class Solution {
public:
    vector<int> twoSum(vector<int>& nums, int target) {
        vector<int> result = {2, 0};
        int n = nums.size();
        for(int i = 0; i < n - 1; i++) {
            for(int j = i+1; j < n; j++) {
                if (nums[i] + nums[j] == target) {
                    result[0] = i;
                    result[1] = j;
                    break;
                }
            }
        }
        return result; 
    }
};
```

时间复杂度：$O(n^2)$
对于每个元素，我们试图通过遍历数组的其余部分来寻找它所对应的目标元素，这将耗费 $ O(n)$ 的时间。因此时间复杂度为 $O(n^2)$。


空间复杂度：$O(1)$。

**法二：哈希表法**

> ​		为了对运行时间复杂度进行优化，我们需要一种更有效的方法来检查数组中是否存在目标元素。如果存在，我们需要找出它的索引。保持数组中的每个元素与其索引相互对应的最好方法是什么？哈希表。
​		通过以空间换取速度的方式，我们可以将查找时间从 O(n) 降低到 O(1)。哈希表正是为此目的而构建的，它支持以 近似 恒定的时间进行快速查找。我用“近似”来描述，是因为一旦出现冲突，查找用时可能会退化到 O(n)。但只要你仔细地挑选哈希函数，在哈希表中进行查找的用时应当被摊销为 O(1)。

***两遍哈希表：***将数组中每个元素的“值—索引”对儿存入哈希表，再次遍历数组，利用哈希表查找目标元素是否存在，**注意不能是该元素本身**。利用哈希表查找时要选择合适的哈希函数。

c++中的map哈希表常用函数：

> c.insert(v)  v是一个{key，value} 形式的键—值对儿；
>
> c.count(k)  返回关键字等于k的元素的数量，对于map，因为不允许关键字重复，返回值永远是1或 0； 
>
> c.find(k)  返回一个迭代器，指向第一个关键字为k的元素，若k不在容器中，则返回尾后迭代器。

```c++
class Solution {
public:
    vector<int> twoSum(vector<int>& nums, int target) {
        map<int, int> m;
        vector<int> result (2, 0);
        for (int i = 0; i < nums.size(); i++) 
        {
            m.insert({nums[i], i});
        }
        for (int i = 0; i < nums.size(); i++) 
        {
            if (m.count(target - nums[i]) && m[target - nums[i]] != i)
            {
                result[0] = i;
                result[1] = m[target - nums[i]];
            }
        }
        return result;
    }
};
```

时间复杂度：$O(n)$
我们把包含有 n个元素的列表遍历两次。由于哈希表将查找时间缩短到 O(1) ，所以时间复杂度为 O(n)。

空间复杂度：$O(n)$
所需的额外空间取决于哈希表中存储的元素数量，该表中存储了 n 个元素。

***一遍哈希表 :*** 每个元素在“登记之前”，先检查哈希表中是否已经有要找的元素了。

```c++
class Solution {
public:
    vector<int> twoSum(vector<int>& nums, int target) {
        map<int, int> m;
        vector<int> result (2, 0);
        for (int i = 0; i < nums.size(); i++) {
            if (m.count(target - nums[i]) && m[target - nums[i]] != i) {
                result[0] = i;
                result[1] = m[target - nums[i]];
                break;
            }
            else 
                m.insert({nums[i], i});
        } 
        return result;
    }
};
```

时间复杂度：$O(n)$
我们只遍历了包含有 n个元素的列表一次。在表中进行的每次查找只花费 O(1)的时间。

空间复杂度：$O(n)$
所需的额外空间取决于哈希表中存储的元素数量，该表最多需要存储 n 个元素。



## 2. 两数相加
<img src="LeetCode%20Hot100.assets/image-20200131221610304.png" alt="image-20200131221610304"  align="left"/>

无需单独设置进位位的方法，另外新建立了一个存放结果的链表，有优化空间。

```c++
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
        ListNode* result = new ListNode(0);
        ListNode* l3 = result;
        int sum = 0;
        while(l1 != NULL || l2 != NULL)
        {
            if (l1 != NULL) 
            {
                sum += l1->val;
                l1 = l1->next;    
            }
            if (l2 != NULL)
            {
                sum += l2->val;
                l2 = l2->next;
            }
            l3->next = new ListNode(sum % 10);
            l3 = l3->next;
            sum /= 10;
        }
        if (sum != 0)
        {
            l3->next = new ListNode(sum);
        }
        return result->next;       
    }
};
```



## 3. 无重复字符的最长子串

<img src="LeetCode%20Hot100.assets/image-20200201094422861.png" alt="image-20200201094422861" style="zoom:100%;" align="left"/>

**法一：利用向量实现的滑动窗口：**

对每一个传入字符串的字符与vector中的所有内容进行比较，无重复则将字符存入vector末尾，有重复则先更新不重复字符串最大值mlen，再将vector中的重复字符即其之前的内容删掉，返回mlen。

删除vector中的内容有多种方法，这里调用的erase方法会自动将后面的元素向前移动。

> erase(v)  v是一个迭代器
>
> erase(a, b)  a,b都是迭代，指示一个范围，最终删除的内容是[a, b)

```c++
class Solution {
public:
    int lengthOfLongestSubstring(string s) {
        int n = s.length();
        vector<char> str;
        int len_max = 0;
        for(int i = 0; i < n; i++)  // 对每个元素
        {
            char c = s[i];
            // 在当前子串中检查是否存在重复元素
            for(int j = str.size() - 1; j >= 0; j--)
            {
                if(c == str[j])
                {
                    if(len_max < str.size()) len_max = str.size();
                    str.erase(str.begin(), str.begin() + j + 1);
                }
            }
            str.push_back(c);
        }
        if (len_max < str.size()) len_max = str.size();
        return len_max;
    }
};
```

**法二：双指针法**

再s的字符数量大于等于2个的时候，用头指针、尾指针指针分别指向s的开头和其开头+1，扫描两指针之间的字符并与尾指针进行对比，根据对比情况移动指针。
这期间只有两种情况，1：范围[头,尾)之间有跟尾字符重复的字符；2：没有。
遇到情况1时，更新最大长度mlen，头指针指向重复字符的后一位置，尾指针后移一位，继续扫描对比。
遇到情况2时，头指针不动，尾指针后移，继续。								

```c++
class Solution {
public:
    int lengthOfLongestSubstring(string s) {
        int len = s.length();
        // 长度小于2的字符串，双指针没法指，直接返回长度
        if (len == 0) return 0;
        if (len == 1) return 1;
        // beg指向s的第一个字符，end由第二个字符向后逐渐扫描
        // mlen最小为1。
        int beg = 0, end = 1, mlen = 1;
        for (; end < len; end++)
        {
            // 遍历[beg, end)，检查是否有重复字符
            for (int i = beg; i < end; i++)
            {
                if (s[i] == s[end])
                {
                    if (end - beg > mlen)
                        mlen = end - beg;
                    // beg更新为重复元素的下一个位置
                    beg = i + 1; 
                    break;
                }
            }  
        }
        // 如果字符串全部不重复时
        if (end - beg > mlen)
            mlen = end - beg;  
        return mlen;
    }
};
```



## 4. 寻找两个有序数组的中位数

<img src="LeetCode%20Hot100.assets/image-20200201162400220.png" alt="image-20200201162400220" style="zoom:100%;" align="left"/>

**[求第K小法](https://leetcode-cn.com/problems/median-of-two-sorted-arrays/solution/xiang-xi-tong-su-de-si-lu-fen-xi-duo-jie-fa-by-w-2/)**

将求n个有序数中的中位数理解为[求第k小](https://leetcode-cn.com/problems/median-of-two-sorted-arrays/solution/xiang-xi-tong-su-de-si-lu-fen-xi-duo-jie-fa-by-w-2/)的数

n为奇数，中位数——第（n+1）/2小的数；

n为偶数，中位数——第（n+1）/2小、第（n+2）小的数之和的平均值。

求两个有序数组中第k小的数：

![k  没 刂 4 一 刂 小 竹  一 刂 k-l 小  k 小 3 ,  丶 〔 一 刂 ](LeetCode%20Hot100.assets/clip_image001.png)

<img src="LeetCode%20Hot100.assets/clip_image002.png" alt="img" style="zoom: 25%;" />

```c++
class Solution {
public:
    double findMedianSortedArrays(vector<int>& nums1, vector<int>& nums2) {
        int n = nums1.size();
        int m = nums2.size();
        // 将奇偶两种情况合并，若两数组中元素总数为奇数个，则求两次同样的k
        int left = (n + m + 1) / 2; 
        int right = (n + m + 2) / 2; 
        return (getKth(nums1, 0, n-1, nums2, 0, m-1, left) + getKth(nums1, 0, n-1, nums2, 0, m-1, right)) * 0.5;  // 这里 * 0.5 换成 (……)/ 2 会出错
    }
private:
    // 求第k小
    int getKth(vector<int>& nums1, int start1, int end1, vector<int>& nums2, int start2, int end2, int k) {
        int len1 = end1 - start1 +1;
        int len2 = end2 - start2 +1;
        // 保证len1 < len2,这样就能保证当有数组空了，一定是len1;
        if (len1 > len2) return getKth(nums2, start2, end2, nums1, start1, end1, k);
        // 递归出口
        if (len1 == 0) return (nums2[start2 + k -1]);
        if (k == 1) 
            return (nums1[start1] > nums2[start2] ? nums2[start2] : nums1[start1]);
        int i = start1 + (len1 > k/2 ? k/2 : len1) - 1;
        int j = start2 + (len2 > k/2 ? k/2 : len2) - 1;
        if(nums1[i] > nums2[j])
            return getKth(nums1, start1, end1, nums2, j + 1, end2, k-(j + 1 - start2));
        else
            return getKth(nums1, i + 1, end1, nums2, start2, end2, k-(i + 1 - start1));
    }
};
```



## 5. 最长回文子串

<img src="LeetCode%20Hot100.assets/image-20200201165207592.png" alt="image-20200201165207592"  align="left"/>

**中心扩展法：**

一个中心、两个中心两种情况（aaabaaa、 aaabbaaa）；两个指针分别向两侧扩展；

<img src="LeetCode%20Hot100.assets/image-20200201165938764.png" alt="image-20200201165938764" style="zoom:60%;" />

```c++
class Solution {
public:
    string longestPalindrome(string s) {
        // 边界条件处理
        if (s.length() < 2) return s; 
        
        int maxLen = 0;
        int lo = 0; //回文串的起始位置
        for (int i = 0; i < s.length()- 1; i++) {
            expandaroundCenter(s, i, i, lo, maxLen);
            expandaroundCenter(s, i, i+1, lo, maxLen);
        }
        return s.substr(lo, maxLen);
    }
private:
    void expandaroundCenter(string& s, int left, int right, int& lo, int& maxLen) {
        while(left >= 0 && right < s.length() && s[left] == s[right]) {
            left--;
            right++;
        } 
        if ((right - left -1) > maxLen) {
            maxLen = (right - left -1);
            lo = left + 1;
        }
    }
};
```

![image-20200201170654171](LeetCode%20Hot100.assets/image-20200201170654171.png)

## 10. 正则表达式匹配

## 11. 盛水最多的容器

<img src="LeetCode%20Hot100.assets/image-20200221185543027.png" alt="image-20200221185543027" style="zoom:80%;" align="left"/>

**法一：暴力法** 略

**法二：双指针法**

左、右两个指针分别指向数组的首元素、末元素，计算当前范围内的水量。之后将左右两个端点中高度==较矮==的那个向前移动一步，再次计算水量。如此迭代直到遍历整个数组。设置变量保存出现过的最大值，每步迭代时进行更新。

选择移动较矮的是因为当前两端点的距离已经是最大值了，后续步骤中距离只会变得更小。而当前的主要制约因素是高度较矮的一端，如果保持高度矮的不变，移动较高的一端，那么无论下个值得高度是更大或是更小，水量都只会变小。

```c++
class Solution {
public:
    int maxArea(vector<int>& height) {
        int l = 0, r = height.size() - 1;
        int maxArea = 0;
        while (l < r)
        {
            int curArea = min(height[l], height[r]) * (r - l);
            maxArea = max(maxArea, curArea);
            if (height[l] < height[r]) l++;
            else r--;
        }
        return maxArea;
    }
};
```



## 15. 三数之和

<img src="LeetCode%20Hot100.assets/image-20200201170920851.png" alt="image-20200201170920851"  align="left"/>

**双指针法**

先将数组排序，对数组中的每个数，在其右侧区间用两个指针一首一尾指示。根据判断条件两边夹击，逐渐缩小范围找到结果。***注意重复元素的处理***。

```c++
class Solution {
public:
    vector<vector<int>> threeSum(vector<int>& nums) {
        vector<vector<int>> result;
        int i, j, k;
        sort(nums.begin(), nums.end());
        if(nums.size() < 3 || nums.front() > 0 || nums.back() < 0) return result; //边界条件
        for(i = 0; i < nums.size() - 2; i++)
        {   
            if(nums[i] > 0) break;
            if((i > 0) && (nums[i] == nums[i - 1])) continue;  //i的重复项处理
            j = i + 1;
            k = nums.size() - 1;
            while(j < k)
            {
                if(nums[i] + nums[j] + nums[k] < 0)  j++;
                else if(nums[i] + nums[j] + nums[k] > 0) k--;
                else 
                {
                    result.push_back({nums[i], nums[j], nums[k]});
                    while((j < k) && (nums[j] == nums[j+1])) j++; //j的重复项处理
                    while((j < k) && (nums[k] == nums[k-1])) k--; //k的重复项处理
                    j++;
                    k--;  
                } 
            }
        }
        return result;
    }
};
```

一个**错误**版本：

```c++
class Solution {
public:
    vector<vector<int>> threeSum(vector<int> & nums) {
        vector<vector<int>> result;
        if (nums.size() < 3) return result;
        sort(nums.begin(), nums.end());
        if (nums.front() > 0) return result;
        if (nums.back() < 0) return result;
        int i, j, k;
        for (int k  = 0; k  < nums.size() - 2; k++)
        {
            if (nums[k] > 0) break;
            if ((k  > 0) && (nums[k] == nums[k  - 1])) continue;
            i  = k  + 1;
            j  = nums.size() - 1;
            while (i  < j)
            {
                错误部分
                //if (nums[i] + nums[j] + nums[k] < 0)
                //{
                //    i++;
                //}
                //if (nums[i] + nums[j] + nums[k] > 0)
                //{
                //    j--;
                //}
                //if (nums[i] + nums[j] + nums[k] == 0)
                //{
                //    result.push_back({nums[i], nums[j], nums[k]});
                //    while (i  < j  && nums[i] == nums[i  + 1])
                //    {
                //        i++;
                //    }
                //    while (i  < j  && nums[j] == nums[j  - 1])
                //    {
                //        j--;
                //    }
                //    i++;
                //    j--;
                //}
            }
        }
        return result;
    }
};
```



## 17. 电话号码的字母组合

<img src="LeetCode%20Hot100.assets/image-20200201172737389.png" alt="image-20200201172737389"  align="left"/>

**法一 ：利用队列**

1.建立一个map哈希表；map的映射关系：key—value，将一个数字对应的多个字母看作一个字符串作为一个“value”。eg：2 — “abc”

2.新建一个队列；

3.将第一个字符串所对应的码表逐步进入到队列中；

4.将每一个字符与后一个字符串所对应的码表中每一个值相加并逐步进入到队列中；而后弹出这个字符；

5.重复4步骤直到加完每一个字符串；

6.最终队列中存储的即为所有情况的string

```c++
class Solution {
public:
    vector<string> letterCombinations(string digits) {
        vector<string> res;//用于输出向量
        map<char, string> m = {{'2',"abc" }, {'3',"def"}, {'4',"ghi"},
                                {'5',"jkl"}, {'6',"mno"}, {'7',"pqrs"},
                                {'8',"tuv"},{'9',"wxyz"}};//映射map哈希表
        int size = digits.size();//输入字符串产长度
        queue<string> que;//新建队列
        
        //先将第一个元素对应的码表入队
        for (int j = 0; j < m[digits[0]].size(); j++)
        {
            string str;
            str.push_back(m[digits[0]][j]);//char转string
            que.push(str);//string入队
        }
        string s;//用于存储队头元素
        for (int i = 1; i < size; i++)
        {
            int length = que.size();//当前队列长度
            while (length--)
            {
                for (int j = 0; j < m[digits[i]].size(); j++)
                {
                    s = que.front();
                    s = s + m[digits[i]][j];//队头元素加上新元素
                    que.push(s);//入队
                }
                que.pop();//队头出队
            }
        }
        while (!que.empty())
        {
            res.push_back(que.front());//队头元素存储至res
            que.pop();//队头出队
        }
        return res;//返回
    }
};
```



## 19. 删除链表的倒数第N个节点

<img src="LeetCode%20Hot100.assets/image-20200201173707324.png" alt="image-20200201173707324"  align="left"/>

**两次遍历**

1. 将问题转化为删除**正数**第（L - n + 1）个节点

2. 遍历列表，获得长度L

3. 找到第（L - n + 1）个节点，删除之

```c++
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode(int x) : val(x), next(NULL) {}
 * };
 */
class Solution {
public:
    ListNode* removeNthFromEnd(ListNode* head, int n) {
        int len = 0;
        ListNode* result = new ListNode(0);
        result->next = head;
        ListNode* l = result;
        while (l != NULL)
        {
            len++;
            l = l->next;
        }
        //重置指针l
        l = result;
        for (int i = 0; i < (len - n - 1); i++)
        {
            l = l->next;
        }
        l->next = l->next->next; 
        return result->next;
    }
};
```

   **一次遍历：双指针法**

![image-20200201174113864](LeetCode%20Hot100.assets/image-20200201174113864.png)

```c++
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode(int x) : val(x), next(NULL) {}
 * };
 */
class Solution {
public:
    ListNode* removeNthFromEnd(ListNode* head, int n) {
        ListNode* result = new ListNode(0);
        result->next = head;
        ListNode* first = result;
        ListNode* second = result;
        for (int i = 0; i < n + 1; i++)
        {
            first = first->next;
        }
        while (first != NULL)
        {
            first = first->next;
            second = second->next; 
        }
        second->next = second->next->next;
        return result->next;
    }
};
```



## 20. 有效的括号 

[回到tag](# tag)

<img src="LeetCode%20Hot100.assets/image-20200201174255381.png" alt="image-20200201174255381"  align="left"/>

**法一：利用哈希表和栈**

```c++
class Solution {
public:
    bool isValid(string s) {
        stack<char> st;
        map<char, char> mp = {{')', '('}, {']', '['}, {'}', '{'}};
        for (int i = 0; i < s.size(); i++) {
            if (st.empty() && (s[i] == ')' || s[i] == '}' || s[i] == ']'))
                return false;
            if (s[i] == ')' || s[i] == '}' || s[i] == ']') {
                if (mp[s[i]] == st.top())
                    st.pop();
                else 
                    return false;
            }
            else 
                st.push(s[i]);
        }
        return st.empty();      
    }
};
```

**法二：比较ASCII码**

```c++
class Solution {
public:
    bool isValid(string s) {
        stack<char> st;
        for (int i = 0; i < s.size(); i++) {
            if (st.empty() && (s[i] == ')' || s[i] == '}' || s[i] == ']'))
                return false;
            if (s[i] == ')' || s[i] == '}' || s[i] == ']') {
                if (s[i] - st.top() == 1 || s[i] - st.top() == 2) 
                    st.pop();
                else 
                    return false;
            }
            else 
                st.push(s[i]);
        }
        return st.empty();      
    }
};
```



## 21. 合并两个有序链表

<img src="LeetCode%20Hot100.assets/image-20200201174653043.png" alt="image-20200201174653043"  align="left" style="zoom:80%;" />![image-20200201174914365](LeetCode%20Hot100.assets/image-20200201174914365.png)

<img src="LeetCode%20Hot100.assets/image-20200201174949749.png" alt="image-20200201174949749" style="zoom:80%;" />

```c++
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode(int x) : val(x), next(NULL) {}
 * };
 */
class Solution {
public:
    ListNode* mergeTwoLists(ListNode* l1, ListNode* l2) {
        ListNode* result = new ListNode(0);
        ListNode* l3 = result;
        if (l1 == NULL) return l2;
        if (l2 == NULL) return l1;
        while (l1 != NULL || l2 != NULL)
        {
            if (l1 != NULL && l2 != NULL)
            {
                if (l1->val < l2->val) 
                {
                    l3->next = new ListNode(l1->val);
                    l3 = l3->next;
                    l1 = l1->next;
                }
                else
                {
                    l3->next = new ListNode(l2->val);
                    l3 = l3->next;
                    l2 = l2->next;
                }
                continue;
            }
            if (l1 == NULL)
            {
                l3->next = new ListNode(l2->val);
                l3 = l3->next;
                l2 = l2->next;
                continue;
            }
            if (l2 == NULL)
            {
                l3->next = new ListNode(l1->val);
                l3 = l3->next;
                l1 = l1->next;
                continue;
            }
        }
        l3 = NULL;
        return result->next;
    }    
};
```

*错误记录：*

E1: 一次比较添加了两个节点

```c++
             …………
             if (l1 != NULL && l2 != NULL) {
                if (l1->val > l2->val) {
                    l3->next = new ListNode(l2->val);
                    l3 = l3->next;
                    l3->next = new ListNode(l1->val);
                    l3 = l3->next;
                }
                else {
                    l3->next = new ListNode(l1->val);
                    l3 = l3->next;
                    l3->next = new ListNode(l2->val);
                    l3 = l3->next;
                }
                l1 = l1->next;
                l2 = l2->next;
            }
            …………
```

E2：不加continue时：

![image-20200201175942987](LeetCode%20Hot100.assets/image-20200201175942987.png)



另一种写法：

```c++
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode(int x) : val(x), next(NULL) {}
 * };
 */
class Solution {
public:
    ListNode* mergeTwoLists(ListNode* l1, ListNode* l2) {
        ListNode* result = new ListNode(0);
        ListNode* l3 = result;
        if (l1 == NULL) return l2;
        if (l2 == NULL) return l1;
        while (l1 != NULL && l2 != NULL)
        {
            if (l1->val < l2->val) 
            {
                l3->next = new ListNode(l1->val);
                l3 = l3->next;
                l1 = l1->next;
            }
            else
            {
                l3->next = new ListNode(l2->val);
                l3 = l3->next;
                l2 = l2->next;
            }
        }
        while (l1 != NULL)
        {
            l3->next = new ListNode(l1->val);
            l3 = l3->next;
            l1 = l1->next;
        }
        while (l2 != NULL)
        {
            l3->next = new ListNode(l2->val);
            l3 = l3->next;
            l2 = l2->next;
        }
        l3 = NULL;
        return result->next;
    }    
};
```



## 22. 括号生成

## 23. 合并K个排序链表

[回到tag](# tag)

<img src="LeetCode%20Hot100.assets/image-20200214190109146.png" alt="image-20200214190109146" style="zoom:80%;" align="left"/>

**法一：最小堆**

时间：O(nlogk)  空间：O(k)

- 把所有的不为空的头结点都放到一个最小堆中。
- 弹出堆顶元素，接到新链表的尾部，若该节点还有next，把其放进堆中（堆中永远维持着最小的k个节点）。直到堆空为止。
**注意：针对自定义的数据类型构造堆的方法**

版本一：使用lambda表达式的方式构造堆。
```c++
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
    typedef function<bool(const ListNode*, const ListNode*)> Compare;
    
    ListNode* mergeKLists(vector<ListNode*>& lists) {
        if (lists.size() == 0) return NULL;
        // lambda表达式
        Compare cmp = [](const ListNode *a, const ListNode *b) {
            return a->val > b->val;
        };
        priority_queue<ListNode*, vector<ListNode*>, Compare> q(cmp);
        
        
        for (auto& p: lists) {
            if (p) q.push(p);
        }
        
        ListNode *dummy = new ListNode(-1);
        auto tail = dummy;
        
        while (!q.empty()) {
            auto cur = q.top(); q.pop();
            if (cur->next) q.push(cur->next);
            tail->next = cur;
            tail = cur;
        }
        return dummy->next;
    }
};
```

版本二：另一种构造最小堆的写法

```c++
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
    struct cmp {
        bool operator () (const ListNode* l1, const ListNode* l2) 
        {return l1->val > l2->val;}
    };

    ListNode* mergeKLists(vector<ListNode*>& lists) {
        if (lists.size() == 0) return NULL;
        priority_queue<ListNode*, vector<ListNode*>, cmp> q;
        for (int i = 0; i < lists.size(); i++)
        {
            if (lists[i]) q.push(lists[i]); 
        }
        ListNode* res = new ListNode(-1);
        ListNode* tail = res;
        while (!q.empty())
        {
            ListNode* cur = q.top(); q.pop();
            if (cur->next != NULL) q.push(cur->next);
            tail->next = cur;
            tail = cur;
        }
        return res->next;
    }
};
```





**法二：转化为合并两个有序链表**

**法三：排序**



## 31. 下一个排列

<img src="LeetCode%20Hot100.assets/image-20200222120741816.png" alt="image-20200222120741816" style="zoom:80%;" align="left"/>

**算法：**

- 设置两个变量$i，j$，从后向前查找第一对满足$n[i] < n[j]$的元素。
- 则$[j, end]$一定是一个降序，从其中找到第一个满足$n[k] > n[i]$的元素
- 交换n[k]、n[i]，此时的[j,  end]一定还是降序序列
- 将[j, end]重新排序为升序

[参考题解](https://leetcode-cn.com/problems/next-permutation/solution/xia-yi-ge-pai-lie-suan-fa-xiang-jie-si-lu-tui-dao-/)

```c++
class Solution {
public:
    void nextPermutation(vector<int>& nums) {
        int i = nums.size() - 2, j = nums.size() - 1;
        while (i >= 0)
        {
            if (nums[i] < nums[j]) break;
            i--;
            j--;
        }
        if (i < 0)
        {
            reverse(nums.begin(), nums.end());
            return;
        }
        int k = nums.size() - 1;
        while (nums[k] <= nums[i])
        {
            k--;
        }
        int temp = nums[k];
        nums[k] = nums[i];
        nums[i] = temp;
        sort(nums.begin() + j, nums.end());
        return;
    }
};
```



## 32. 最长有效括号

## 33. 搜索旋转排序数

## 34. 在排序数组中查找元素的第一个和最后一个位置

## 39. 组合总和

## 42. 接雨水

[回到tag](# tag)

<img src="LeetCode%20Hot100.assets/image-20200212205627917.png" alt="image-20200212205627917" style="zoom:80%;" align="left"/>

**法一：递减栈**

维护一个递减栈：

- 当前墙的高度小于栈顶墙的高度时，就入栈；
- 当前墙的高度大于栈顶墙的高度时，则说明之前的积水到这里停下了。出栈，然后计算以这个出栈的墙的高度为“洼地”，当前墙与新栈顶的墙为边界所构成的一个“坑”中的水量。

**注意：**此题的栈是递减栈，与[第84题](# 84. 柱状图中最大的矩形)的“递增栈”对比，不能再通过给输入数组首部添“0”的方式来处理。

```c++
class Solution {
public:
    int trap(vector<int>& height) {
        if (height.empty()) return 0;
        // 与“递增栈”对比，添0的方式不能用喽
        // height.insert(height.begin(), 0);
        stack<int> stk;
        int sum = 0;
        int left = 0, right = 0, prevH = 0;
        for (int i = 0; i < height.size(); i++)
        {
            while (!stk.empty() && height[i] > height[stk.top()])
            {
                prevH = height[stk.top()];
                stk.pop();
                // 栈为空时，跳出循环
                if (stk.empty()) continue;
                right = i;
                left = stk.top();
                int temp = min(height[left], height[right]) - prevH;
                sum = sum + temp * (right - left - 1);
            }
            stk.push(i);
        }
        return sum;   
    }
};
```



## 46. 全排列

## 48. 旋转图像

## 49. 字母异位词分组

## 53. 最大子序和

<img src="LeetCode%20Hot100.assets/image-20200203182921853.png" alt="image-20200203182921853"  align="left"/>

**暴力法**略。

**法一：在线处理**

每读取一个新数据，都能给出当前数据下的最大子序和

```c++
class Solution {
public:
    int maxSubArray(vector<int>& nums) {
        int maxSum = nums[0], tempSum = 0;
        for (int i = 0; i < nums.size(); i++)
        {
            tempSum += nums[i];
            if (tempSum > maxSum) maxSum = tempSum;
            if (tempSum < 0) tempSum = 0;
        }
        return maxSum;
    }
};
```

*注意：*maxSum的初值

**法二：动态规划**

$dp[i]$表示以nums[i]为结尾的序列中的最大和，状态转移方程：
$$
dp[i] = max(dp[i - 1] + nums[i], nums[i])
$$
[力扣题解：[详细解读动态规划的实现, 易理解]](https://leetcode-cn.com/problems/maximum-subarray/solution/xiang-xi-jie-du-dong-tai-gui-hua-de-shi-xian-yi-li/)

```c++
class Solution {
public:
    int maxSubArray(vector<int>& nums) {
        int max_endHere = nums[0];
        int max_soFar = nums[0];
        for (int i = 1; i < nums.size(); i++)
        {
            max_endHere = max(max_endHere + nums[i], nums[i]);
            if (max_endHere > max_soFar) 
                max_soFar = max_endHere;
        }
        return max_soFar;
    }
};
```

**法三：分治算法**

```c++
class Solution {
public:
    int maxSubArray(vector<int>& nums) {
        return divideAndConquer(nums, 0, nums.size() - 1);
    }

    int divideAndConquer(vector<int>& nums, int left, int right) {
        if (left == right) 
        {
            return nums[left];
        }
        int center = (left + right) / 2;
    
        int maxLeft, maxRight, maxCross;
        // 注意maxCenLeft和maxCenRight的初值
        int maxCenLeft = nums[center], maxCenRight = nums[center + 1];
        int tempCenLeft = 0, tempCenRight = 0;
    
        maxLeft = divideAndConquer(nums, left, center);
        maxRight = divideAndConquer(nums, center + 1, right);
        for (int i = center; i >= left; i--)
        {
            tempCenLeft += nums[i];
            if (tempCenLeft > maxCenLeft) 
                maxCenLeft = tempCenLeft;
        }
        for (int i = center + 1; i <= right; i++)
        {
            tempCenRight += nums[i];
            if (tempCenRight > maxCenRight) 
                maxCenRight = tempCenRight;
        }
        maxCross = maxCenLeft + maxCenRight;
        return maxLeft > maxRight ?
               (maxLeft > maxCross ? maxLeft : maxCross)
               : (maxRight > maxCross ? maxRight : maxCross);
    }

};
```

**法四：贪心算法**

对当前元素，每次都选择局部最优解，则最终得到的就是全局最优解。

```c++
class Solution {
public:
    int maxSubArray(vector<int>& nums) {
        int maxSum = nums[0]; // 全局最优解
        int maxCur = nums[0]; // 当前元素对应的局部最优解
        for (int i = 1; i < nums.size(); i++)
        {
            int maxTemp = 0;
            for (int j = i; j >= 0; j--)
            {
                maxTemp += nums[j];
                if (maxTemp > maxCur) maxCur = maxTemp;
            }
            if (maxCur > maxSum) maxSum = maxCur;
        }
        return maxSum;
    }
};
```



## 55. 跳跃游戏

## 56. 合并区间

## 62. 不同路径

## 64. 最小路径和

## 70. 爬楼梯

<img src="LeetCode%20Hot100.assets/image-20200204212653962.png" alt="image-20200204212653962"  align="left" style="zoom:80%;" />

**动态规划**

$dp[i]$表示 $i$ 级台阶的方法数，则有两种方式到达第 $i$ 级台阶：

- 第$(i - 1)$级台阶向上走一步；

- 第$(i - 2)$级台阶向上走两步；

所以递推关系式为：
$$
dp[i] = dp[i - 1] + dp[i - 2]
$$

**真正的动态规划**

```c++
class Solution {
public:
    int climbStairs(int n) {
        if (n == 1) return 1;
        vector<int> dp(n + 1);
        dp[1] = 1;
        dp[2] = 2;
        for (int i = 3; i <= n; i++)
        {
            dp[i] = dp[i - 1] + dp[i - 2];
        }
        return dp[n];
    }
};
```

**错误版本两则：虚假的动态规划……**

```c++
有了循环，为什么还要递归哦，我这脑子在想啥……
// class Solution {
// public:
//     int climbStairs(int n) {
//         int res;
//         if (n == 0) return 1;
//         if (n == 1) return 1;
//         for (int i = 2; i <= n; i++)
//         {
//             res = climbStairs(i - 1) + climbStairs(i - 2);
//         }
//         return res;
//     }
// };

这样就只用了1和2的解，动态规划是要把利用递推关系得到的结果每次存起来，到n的时候就直接用n - 1和n - 2的值了。总不能每次都慢慢从1和2开始慢慢递推计算
// class Solution {
// public:
//     int climbStairs(int n) {
//         int res;
//         if (n == 2) return 2;
//         if (n == 1) return 1;
//         res = climbStairs(n - 1) + climbStairs(n - 2);
//         return res;
//     }
// };
```







## 72. 编辑距离

## 75. 颜色分类

## 76. 最小覆盖子串

## 78. 子集

## 79. 单词搜索

## 84. 柱状图中最大的矩形

[回到tag](# tag)

<img src="LeetCode%20Hot100.assets/image-20200210173049054.png" alt="image-20200210173049054" style="zoom:80%;" align="left"/>

<img src="LeetCode%20Hot100.assets/image-20200210173216816.png" alt="image-20200210173216816" style="zoom:80%;" align="left"/>

**分析**

![image-20200210173445680](LeetCode%20Hot100.assets/image-20200210173445680.png)

**法一：递增栈**

利用一个栈存储元素的下标，构造一个递增栈（栈中所存的下标对应的柱状图的高度递增），这样对栈中的每个元素，当其作为“最矮”的柱状图，计算矩形面积时， left _i 就是栈中紧挨着它的下一个元素的值。而 right_i 就是遍历过程中发现的比栈顶元素代表的柱状图的高度小的柱状图。好像怎么都描述不清……看图：

<img src="LeetCode%20Hot100.assets/20200210_093925822_iOS.png" style="zoom:50%;" />

​```c++
class Solution {
public:
    int largestRectangleArea(vector<int>& heights) {
        if (heights.empty()) return 0;
        // 先将输入的高度的数组，前后各添加一个0。
        // 前面添的0相当于给所有元素添加一个初始的left_i
        // 末尾添的0，便于将栈中剩余的元素全部弹出并计算其面积
        heights.insert(heights.begin(), 0);
        heights.insert(heights.end(), 0);
        int res = 0;
        stack<int> stk;
        for (int i = 0; i < heights.size(); i++)
        {
            while (!stk.empty() && heights[i] < heights[stk.top()])
            {
                int temp = stk.top();
                stk.pop();
                res = max(res, heights[temp] * (i - stk.top() - 1));
            }
            stk.push(i);
        }
        return res;
    }
};
```

优化版：将0先直接添加到栈中，就可以使得循环中少一个判空的判断。经测试，肉眼可见的提升了运算速度。

```c++
class Solution {
public:
    int largestRectangleArea(vector<int>& heights) {
        if (heights.empty()) return 0;
        heights.insert(heights.begin(), 0);
        heights.insert(heights.end(), 0);
        int res = 0;
        stack<int> stk;
        stk.push(0);
        for (int i = 1; i < heights.size(); i++)
        {
            while (heights[i] < heights[stk.top()])
            {
                int temp = stk.top();
                stk.pop();
                res = max(res, heights[temp] * (i - stk.top() - 1));
            }
            stk.push(i);
        }
        return res;
    }
};
```



## 85. 最大矩形

[回到tag](# tag)

<img src="LeetCode%20Hot100.assets/image-20200210200110018.png" alt="image-20200210200110018" style="zoom:80%;" align="left"/>

**法一：套用LeetCode 84的算法**

观察下图橙色部分，可发现与84题一样。则可以调用84题的函数，分别求最大面积，而后比较得出全局最大值即可。其中的height数组可通过向下遍历行，逐步更新。

<img src="LeetCode%20Hot100.assets/image-20200210200247715.png" alt="image-20200210200247715" style="zoom:80%;" />

```python
class Solution:
    def maximalRectangle(self, matrix: List[List[str]]) -> int:
        if not matrix: return 0
        dp = [0] * len(matrix[0])
        maxarea = 0
        for i in range(len(matrix)):
            for j in range(len(matrix[0])):
                # 更新height数组的关键语句
                dp[j] = dp[j] + 1 if matrix[i][j] == "1" else 0
            maxarea = max(maxarea, self.leetcode84(dp))
        return maxarea

    def leetcode84(self, height: List[int]) -> int:
        if not height: return 0
        height = [0] + height + [0]
        maxarea = 0
        stk = []
        stk.append(0)
        for i in range(1, len(height)):
            while height[i] < height[stk[-1]]:
                temp = stk.pop()
                maxarea = max(maxarea, height[temp] * (i - stk[-1] - 1))
            stk.append(i)
        return maxarea
```



## 94. 二叉树的中序遍历

[回到tag](# tag)



## 96. 不同的二叉搜索树

<img src="LeetCode%20Hot100.assets/image-20200219203745115.png" alt="image-20200219203745115" style="zoom:80%;" align="left"/>

**法一：动态规划**

动态规划其实就是数学里的那种递推关系。不过难点在于找出这个递推关系吧，其实编码实现多写几次也挺简单的。不过就是熟练之后，可以写的更漂亮、优雅，也就是只维持求下一个f（i）所需的必需的值，而不是维持整个数组。

对于每一个根i他都是由左边子树（1, 2, ..., i - 1)和右边子树（i + 1, i + 2, ..., n)组成的。因此他的个数肯定是两个子树情况的积。而且，这种根一共有n个，再将这些加起来就可以了。

此题是“明安图数”，也叫“卡特兰数”，有现成的递推公式可用：

<img src="LeetCode%20Hot100.assets/image-20200219204545996.png" alt="image-20200219204545996" style="zoom:80%;" />

```c++
class Solution {
public:
    int numTrees(int n) {
        long count = 1;
        for (int i = 0; i < n; i++)
        {
            count = count * 2 * (2 * i + 1) / (i + 2);
        }
        return count;
    }
};
```



## 98. 验证二叉搜索树

<img src="LeetCode%20Hot100.assets/image-20200219204943184.png" alt="image-20200219204943184" style="zoom:80%;" align="left"/>

利用二叉树搜索树的性质，中序遍历：左-根-右，因为是二叉搜索树，则所得到的的序列一定是递增序列。要注意的是int类型的变量上下限为：INT_MAX = 2147483647, INT_MIN = -INT_MAX - 1

> C++中int类型是32位的，范围是-2147483648到2147483647 。 
 （1）最轻微的上溢是INT_MAX + 1 :结果是 INT_MIN; 
 （2）最严重的上溢是INT_MAX + INT_MAX :结果是-2; 
 （3）最轻微的下溢是INT_MIN - 1:结果是是INT_MAX; 
 （4）最严重的下溢是INT_MIN +  INT_MIN:结果是0 。

所以此时的preValue的初值要使用LONG_MIN。确保这个边界初值小于所有int变量。

**法一：迭代**

```c++
class Solution {
public:
    bool isValidBST(TreeNode* root) {
        if (root == NULL ) return true;
        long preValue = LONG_MIN;
        stack<TreeNode*> stk;
        TreeNode* cur = root;
        while (cur || !stk.empty())
        {
            while (cur)
            {
            stk.push(cur);
            cur = cur->left;
            }
            if (!stk.empty())
            {
                cur = stk.top();
                stk.pop();
                if (cur->val > preValue) 
                    preValue = cur->val;
                else return false;
                cur = cur->right;
            }
        }
        return true;
    }
};
```



## 101. 对称二叉树

<img src="LeetCode%20Hot100.assets/image-20200219200021830.png" alt="image-20200219200021830" style="zoom:80%;" align="left"/>

**思路：**检查二叉树是否对称，满足：

- p->val == q->val
- p的左子树 = q的右子树， p的右子树 = q的左子树

**法一：递归**

```c++
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
        if (root == NULL) return true;
        return isMirror(root->left, root->right);
    }

    bool isMirror(TreeNode* p, TreeNode* q) {
        if (p==NULL && q==NULL) return true;
        if (p==NULL || q==NULL) return false;
        if ((p->val == q->val) 
            && isMirror(p->left, q->right) 
            && isMirror(p->right, q->left))
            return true;
        return false;
    }
};
```



## 102. 二叉树的层次遍历

## 104. 二叉树的最大深度

## 105. 从前序与中序遍历序列构造二叉树

<img src="LeetCode%20Hot100.assets/image-20200219200624937.png" alt="image-20200219200624937" style="zoom:80%;" align="left"/>

**法一：递归构建**

[参考的题解](https://leetcode-cn.com/problems/construct-binary-tree-from-preorder-and-inorder-traversal/solution/di-gui-gou-jian-by-yanyufang/)

与求第k小时的编码策略类似。

```c++
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
        int p_len = preorder.size(), i_len = inorder.size();
        if (p_len == 0 || i_len == 0) return NULL;
        return construct(preorder, 0, p_len - 1, inorder, 0, i_len - 1);
    }
    TreeNode* construct(vector<int>& preorder, int p_start, int p_end, vector<int>& inorder, int i_start, int i_end) {
        TreeNode* root = new TreeNode(preorder[p_start]);
        int root_pos = i_start;
        while (root_pos <= i_end && inorder[root_pos] != preorder[p_start])
            root_pos++;
        int lenL = root_pos - i_start,
            lenR = i_end - root_pos;
        if (lenL > 0)
        root->left = construct(preorder, p_start + 1, p_start + lenL, inorder, i_start, i_start + lenL - 1);
        if (lenR > 0)
        root->right = construct(preorder, p_start + lenL + 1, p_end, inorder, i_start + lenL + 1, i_end);
        return root;
    }
};
```



## 114. 二叉树展开为链表

<img src="LeetCode%20Hot100.assets/image-20200219202123486.png" alt="image-20200219202123486" style="zoom:80%;" align="left"/>

**法一（可能不是原地）**

可以看到题目的链表节点顺序是中序遍历，故可以先中序遍历树，将节点存储在数组中，之后再一个个修改节点指针的指向即可。不过，==这种算法到底算不算原地展开呢。==

```c++
class Solution {
    vector<TreeNode*> m_nodes;
public:
	void preOrder(TreeNode* root){
		if(!root) return;
		m_nodes.push_back(root);
		preOrder(root->left);
		preOrder(root->right);
	}
    void flatten(TreeNode* root)
    {
    	if(!root) return;
    	preOrder(root); // push the sorted nodes into vector<TreeNode*> m_nodes
    	for(int i = 0; i < m_nodes.size(); ++i) {
            m_nodes[i]->right = i + 1 < m_nodes.size() ? m_nodes[i + 1] : NULL;
            m_nodes[i]->left = NULL;
    	}
        return;
    }
};
```



## 121. 买卖股票的最佳时机

<img src="LeetCode%20Hot100.assets/image-20200205153558132.png" alt="image-20200205153558132" style="zoom:80%;" align="left"/>

**法一：动态规划**

$dp[i]$表示第 $i$ 天的==最大收益==（或许这里不应该称为“最大收益”，总之就是填网格时，第 i 个格子应该填的数值），$minprice$表示前$i - 1$天的最低价格， $maxprofit$表示第$i - 1$天的最大利润。则$dp[i]$有两种可能情况：

- 第 i 天卖出：$dp[i] = price[i] - minprice$
- 第 i 天不卖出：$dp[i] = dp[i - 1]=maxprofit$

状态转移方程：$dp[i] = max(第i天卖出, 第i天不卖出)$

第一版

```c++
class Solution {
public:
    int maxProfit(vector<int>& prices) {
        if (prices.empty() || prices.size() == 1) return 0;
        int maxprofitSoFar = prices[1] - prices[0], 
            minpriceSoFar = prices[0],
            maxprofitCur, // 相当于dp[i]
            maxprofit = maxprofitSoFar;
            for (int i = 1; i < prices.size(); i++)
            {
                if (prices[i - 1] < minpriceSoFar)
                    minpriceSoFar = prices[i - 1];
                maxprofitCur = max(prices[i] - minpriceSoFar, maxprofitSoFar);
                if (maxprofitCur > maxprofit)
                    maxprofit = maxprofitCur;
            }
        return maxprofit > 0 ? maxprofit : 0;
    }
};
```

优化版：其实只要两个变量就可以了，不需要那么多中间变量。虽然但是，对性能提升没啥卵用，看起来更优雅，嗯。

```c++
class Solution {
public:
    int maxProfit(vector<int>& prices) {
        int maxprofit = 0;
        if (!prices.empty())
        {
            // max_element() 返回指向vector中最大元素的迭代器
            int minprice = *max_element(prices.begin(), prices.end());  
            for (int i = 0; i < prices.size(); i++)
            {
                minprice = min(prices[i], minprice);
                maxprofit = max(maxprofit, prices[i] - minprice);
            }
        }        
        return maxprofit;
    }
};
```

第三版：维护一个dp数组，每个结果都保留下来，这样编码会更简单，不过因为动态规划每次只会使用临近的前一个值，这样写存在空间浪费。

```c++
class Solution {
public:
    int maxProfit(vector<int>& prices) {
        if (prices.size() <= 1) return 0;
        vector<int> res(prices.size());
        res[0] = 0;
        res[1] = max(prices[1] - prices[0], res[0]);
        int minPrice = min(prices[0], prices[1]);
        for (int i = 2; i < prices.size(); i++)
        {
            if (prices[i] < minPrice)
                minPrice = prices[i];
            res[i] = max(res[i - 1], prices[i] - minPrice);
        }
        int maxValue = *max_element(res.begin(), res.end());
        return maxValue > 0 ? maxValue : 0;
    }
};
```



**法二：更合理的解释，代码其实差不多**

[点我看那个更合理的解释](https://leetcode-cn.com/problems/best-time-to-buy-and-sell-stock/solution/dui-zhe-ge-ti-de-yuan-li-lai-ge-jian-dan-de-zheng-/)

```c++

```



## 124. 二叉树中的最大路径和

## 128. 最长连续序列

## 136. 只出现一次的数字

## 139. 单词拆分

## 141. 环形链表

## 198. 打家劫舍

[回到tag](# tag) 

<img src="LeetCode%20Hot100.assets/image-20200205182201396.png" alt="image-20200205182201396" style="zoom:80%;" align="left"/>

**动态规划**

$f(x)$表示抢劫完前$x$间房屋所得到的总的最大金额, Ax表示第$x$间房的金额。$f(x)$的值有两种情况：

- 抢第$x - 1$间房：则第无法抢第$x$间房，$f(x)=f(x - 1)$
- 不抢第$x - 1$间房：则$f(x) = f(x - 2) + Ax$

则递推公式为：$f(x) = max(f(x - 1), f(x - 2) + Ax)$

边界：$f(1) = A1$

​			$f(2) = max(A1, A2)$

第一版：维护一整个$ dp$数组，这样编码简单，不过浪费了一定空间（因为对后面的元素，前面的记录已经用不到了）

```c++
class Solution {
public:
    int rob(vector<int>& nums) {
        if (nums.empty()) return 0;
        if (nums.size() == 1) return nums[0];
        vector<int> maxSum(nums.size());
        maxSum[0] = nums[0];
        maxSum[1] = max(nums[0], nums[1]);
        for (int i = 2; i < nums.size(); i++)
        {
            maxSum[i] = max(maxSum[i - 1], maxSum[i - 2] + nums[i]);
        }
        int res = *max_element(maxSum.begin(), maxSum.end());
        return res;
    }
};
```

第二版：对每个元素来说，只需要紧挨着它的前两个位置的最大值，所以只需要两个变量就可以了（虽然看结果，对性能的提升没啥大卵用，不过优雅多了hhah）。

```c++
class Solution {
public:
    int rob(vector<int>& nums) {
        int preMax = 0, curMax = 0;
        for (int i = 0; i < nums.size(); i++)
        {
            int temp = curMax;
            curMax = max(curMax, preMax + nums[i]);
            preMax = temp;
        }
        return curMax;
    }
};
```

## 215. 数组中的第k个最大元素

[回到tag](# tag) 

<img src="LeetCode%20Hot100.assets/image-20200213163239649.png" alt="image-20200213163239649" style="zoom:80%;" align="left"/>

**法一：最小堆**

维持一个包含k个元素的最小堆，每新加入一个元素，就pop一次，则最终遍历完数组后，留在最小堆中的k个元素中，堆顶是最小值，其他值均比栈顶大。则栈顶元素是原数组中的第k个最大值。

<img src="LeetCode%20Hot100.assets/20200213_080343074_iOS.png" style="zoom: 40%;" />

```c++
class Solution {
public:
    int findKthLargest(vector<int>& nums, int k) {
        priority_queue<int, vector<int>, greater<int>> q;
        for (auto it : nums)
        {
            q.push(it);
            if (q.size() > k) q.pop();
        }
        return q.top();
    }
};
```

## 226. 翻转二叉树

<img src="LeetCode%20Hot100.assets/image-20200219201837656.png" alt="image-20200219201837656" style="zoom:80%;" align="left"/>

**法一：递归**

```c++
class Solution {
public:
    TreeNode* invertTree(TreeNode* root) {
        if (root == NULL) return NULL;
        TreeNode* L = invertTree(root->left);
        TreeNode* R = invertTree(root->right);
        root->left = R;
        root->right = L;
        return root;
    }
};
```



## 238. 除自身以外数组的乘积

<img src="LeetCode%20Hot100.assets/image-20200226121627810.png" alt="image-20200226121627810" style="zoom:80%;" align="left"/>

**法一：左右乘积列表**

L[i]代表nums[i]左边元素的乘积，注意L[0]=1，因为nums[0]左侧没有元素了。$L[i] = L[i - 1] * nums[i - 1]$

R[i]同理，注意R[n - 1] = 1;

```c++
class Solution {
public:
    vector<int> productExceptSelf(vector<int>& nums) {
        int n = nums.size();
        vector<int> L(n), R(n), output(n);
        // vector<int> L, R, output;
        // 这样写就会报错，因为未指定vector的大小，则默认初始化成容量为0了。
        // 后续如果是用push_back()这类方法当然不会出错
        // 但如果直接使用下标运算符[]，当然就会数组越界啦
        L[0] = 1; R[n - 1] = 1;
        for (int i = 1; i < n; i++)
        {
            L[i] = L[i - 1] * nums[i - 1];
        }
        for (int i = n - 2; i >= 0; i--)
        {
            R[i] = R[i + 1] * nums[i + 1];
        }
        for (int i = 0; i < n; i++)
        {
            output[i] = L[i] * R[i];
        }
        return output;
    }
};
```



## 239. 滑动窗口最大值

[回到tag](# tag) 

<img src="LeetCode%20Hot100.assets/image-20200213211252734.png" alt="image-20200213211252734" style="zoom:90%;" align="left"/>

**法一：暴力法**

遍历出所有的滑动窗口，分别求出最大值。时间复杂度为为$O(Nk)$.

```c++
class Solution {
public:
    vector<int> maxSlidingWindow(vector<int>& nums, int k) {
        vector<int> res;
        int i = 0, j = i + k - 1;
        while (j < nums.size())
        {
            int temp = nums[i];
            for (int m = i; m <= j; m++)
            {
                if (nums[m] > temp)
                    temp = nums[m];
            }
            res.push_back(temp);
            i++;
            j++;
        }
        return res;
    }
};
```

**法二：索引堆**

**法三：动态规划**

**法四：双向队列**

## 337. 打家劫舍III

<img src="LeetCode%20Hot100.assets/image-20200219215447370.png" alt="image-20200219215447370" style="zoom:80%;" align='left'/>

**法一：动态规划、DFS、递归**

对每个节点，都有选和不选两种情况：

- 情况一：选择当前节点。则此节点的左右儿子节点都不能选。最大值就是当前节点加上其左右儿子为当前节点时情况二的值。
- 情况二：不选择当前节点。则此时其左右儿子节点可选可不选。最大值应该是左右儿子为当前节点时，情况一和情况二中最大值的值相加。即：maxSum = max( 左儿子情况一， 左儿子情况二) + max(右儿子情况一， 右儿子情况二)。

[参考题解：用pair的写法](https://leetcode-cn.com/problems/house-robber-iii/)

```c++
class Solution {
public:
    int rob(TreeNode* root) {
        auto res = dfs(root);
        return max(res.first, res.second);
    }
    pair<int, int> dfs(TreeNode* root) {
        if (root == NULL) return {0, 0};
        auto left = dfs(root->left);
        auto right = dfs(root->right);
        return {root->val + left.second + right.second, 
                max(left.first, left.second) + max(right.first, right.second)};
    }

};
```



## 347. 前k个高频元素

<img src="LeetCode%20Hot100.assets/image-20200214202749879.png" alt="image-20200214202749879" style="zoom:80%;" align="left"/>

**法一：map+minHeap**

先用map建立数组中元素与出现次数之间的映射关系，然后维护一个包含k个元素的小顶堆。**注意针对哈希表中的元素建立小顶堆的方式**

```c++
class Solution {
public:
    struct cmp {
        bool operator () (const pair<int, int>& a, const pair<int, int>& b)
        {return a.second > b.second;}
    };

    vector<int> topKFrequent(vector<int>& nums, int k) {
        map<int, int> m;
        vector<int> res;
        for (auto a : nums)
        {
            m[a]++;
        }
        priority_queue<pair<int, int>, vector<pair<int, int>>, cmp> q;
        for (auto b : m)
        {
            q.push(b);
            if (q.size() > k) q.pop();
        }
        while (!q.empty())
        {
            res.push_back(q.top().first);
            q.pop();
        }
        reverse(res.begin(), res.end());
        return res;
    }
};
```



## 394. 字符串解码

[回到tag](# tag)

<img src="LeetCode%20Hot100.assets/image-20200208115759616.png" alt="image-20200208115759616" style="zoom:80%;" align="left"/>

**法一：利用辅助栈**

python支持字符串直接与整数相乘达到复制整数倍个字符串，而c++不支持。

```python
s = 'abc'
a = 3
s = a * s
// 则s = 'abcabcabc'
```



<img src="LeetCode%20Hot100.assets/image-20200208120208901.png" alt="image-20200208120208901" style="zoom:80%;" />

![](LeetCode%20Hot100.assets/20200208_062216667_iOS-1581143187481.png)

```python
class Solution:
    def decodeString(self, s: str) -> str:
        stack, res, multi = [], '', 0
        for c in s:
            if c == '[':
                stack.append([res, multi])
                res = ''
                multi = 0
            elif c == ']':
                last_res, cur_multi = stack.pop() 
                res = last_res + cur_multi * res
            elif '0' <= c <= '9':
                multi = multi * 10 + int(c);
            # c为字母时
            else: 
                res = res + c
        return res
```

## 538. 把二叉搜索树转化为累加树

<img src="LeetCode%20Hot100.assets/image-20200219203445648.png" alt="image-20200219203445648" style="zoom:80%;" align="left"/>

**法一：反中序遍历**

右-根-左的顺序遍历树。不断累加右子树节点的值。

```c++
class Solution {
public:
    TreeNode* convertBST(TreeNode* root) {
        int d = 0;
        antiInorder(root, d);
        return root;
    }
    void antiInorder(TreeNode* root, int& d)
    {
        if (root == NULL) return;
        antiInorder(root->right, d);
        d += root->val;
        root->val = d;
        antiInorder(root->left, d);
    }
};
```



## 560. 和为K的子数组

<img src="LeetCode%20Hot100.assets/image-20200222163624768.png" alt="image-20200222163624768" style="zoom:80%;" align="left"/>

**法一：暴力法**  找出所有可能的数组，分别计算和。==超时==

**法二：优化的暴力法**

没必要每次重新计算子数组的和，对于同一个start，迭代end，每一个新的end对应的sum为上一个sum加上当前新增的元素。

相比最开始的暴力法，省去了每次单独计算子数组和的$O(n)$时间

```c++
class Solution {
public:
    int subarraySum(vector<int>& nums, int k) {
        int count = 0;
        for (int i = 0; i < nums.size(); i++)
        {
            int sum = 0;
            for (int j = i; j < nums.size(); j++)
            {
                sum += nums[j];
                if (sum == k) count++;
            }
        }
        return count;
    }
};
```

**法三：哈希表**

若$sum[j] - k = sum[i]$，则 [i, j]间的元素和为k。利用哈希表存储{ ${sum_i, times}$}。其中times为每个累加和出现的次数。对每一个新的累加和，检验其减k之后的值是否存在哈希表中，如存在，则将对应的次数加到计数变量上。再将当前的sum值记录入表。==注意要先检验再添加记录。==

```c++
class Solution {
public:
    int subarraySum(vector<int>& nums, int k) {
        map<int, int> mp;
        int count = 0;
        int sum = 0;
        mp[0] = 1;
        for (int i = 0; i < nums.size(); i++)
        {
            sum += nums[i];
            // find()返回指向给定元素的迭代器，若元素不在表中，则返回尾后迭代器
            if (mp.find(sum - k) != mp.end()) count += mp[sum - k];
            mp[sum]++;
        }
        return count;
    }
};
```



## 581. 最短无序连续子数组

<img src="LeetCode%20Hot100.assets/image-20200222180627430.png" alt="image-20200222180627430" style="zoom:80%;" align="left"/>

**法一：排序**

最简单的一个方法：额外复制一个数组，将其排序后与原数组对比，从两边向内寻找，分别找到第一个与原数组不同的元素，进而确定左右边界。

```c++
class Solution {
public:
    int findUnsortedSubarray(vector<int>& nums) {
        vector<int> sorted_nums = nums;
        sort(sorted_nums.begin(), sorted_nums.end());
        int i = 0, j = nums.size() - 1;
        while (i <= j && nums[i] == sorted_nums[i])
            i++;
        if (i > j) return 0;
        while (nums[j] == sorted_nums[j])
            j--;
        return j - i + 1;
    }
};
```



## 617. 合并二叉树

<img src="LeetCode%20Hot100.assets/image-20200219202720388.png" alt="image-20200219202720388" style="zoom:80%;" align="left"/>

**法一：递归**

- p、q都不为空时，返回p->val + q->val;
- 其中一个节点不空时，谁不空返回谁
- 都空时随便返回一个

```c++
class Solution {
public:
    TreeNode* mergeTrees(TreeNode* t1, TreeNode* t2) {
        // 以t1为基础，将t2合并到t1上
        if (t1 == NULL) return t2;
        if (t2 == NULL) return t1;
        t1->val = t1->val + t2->val;
        t1->left = mergeTrees(t1->left, t2->left);
        t1->right = mergeTrees(t1->right, t2->right);
        return t1;
    }
};
```



## 739. 每日温度

[回到tag](# tag)

<img src="LeetCode%20Hot100.assets/image-20200208161657443.png" alt="image-20200208161657443" style="zoom:80%;" align="left"/>

**法一：递减栈**

设置一个单调递减的栈，栈中存的元素是给定列表中元素的下标，这里的递减是指栈中所存的下标值对应于原列表中元素的大小单调递减。对列表中的每个值，若其大于栈顶元素，就将栈顶弹出，循环操作直至该元素不再大于栈顶。也就是说只有当前元素小于栈顶元素时才能入栈，则构建出来的一定是递减栈。

<img src="LeetCode%20Hot100.assets/20200208_082333141_iOS.png" style="zoom: 35%;" />

```c++
class Solution {
public:
    vector<int> dailyTemperatures(vector<int>& T) {
        vector<int> res(T.size(), 0);
        if (T.empty()) return res;
        T.push_back(INT_MIN); // 方便处理最后一个元素
        stack<int> stk;
        for (int i = 0; i < T.size(); i++)
        {
            while (!stk.empty() && T[i] > T[stk.top()])
            {
                res[stk.top()] = i - stk.top();
                stk.pop();
            }
            stk.push(i);
        }
        return res;
    }
};
```

> INT_MIN在标准头文件limits.h中定义
>
> 1.#define INT_MAX 2147483647
>
> 2.#define INT_MIN (-INT_MAX - 1)


























## 

