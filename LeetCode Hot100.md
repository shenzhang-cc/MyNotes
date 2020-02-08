# LeetCode Hot 100

## [1. 两数之和](https://leetcode-cn.com/problems/two-sum/)
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



## [2. 两数相加](https://leetcode-cn.com/problems/add-two-numbers/)
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



## [3. 无重复字符的最长子串](https://leetcode-cn.com/problems/longest-substring-without-repeating-characters/)

<img src="LeetCode%20Hot100.assets/image-20200201094422861.png" alt="image-20200201094422861" style="zoom:100%;" align="left"/>

**利用向量实现的滑动窗口：**

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

**双指针法**

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



## [4. 寻找两个有序数组的中位数](https://leetcode-cn.com/problems/median-of-two-sorted-arrays/)

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

## 31. 下一个排列

## 32. 最长有效括号

## 33. 搜索旋转排序数组

## 34. 在排序数组中查找元素的第一个和最后一个位置

## 39. 组合总和

## 42. 接雨水

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

## 85. 最大矩形

## 94. 二叉树的中序遍历

## 96. 不同的二叉搜索树

## 98. 验证二叉搜索树

## 101. 对称二叉树

## 102. 二叉树的层次遍历

## 104. 二叉树的最大深度

## 105. 从前序与中序遍历序列构造二叉树

## 114. 二叉树展开为链表

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

## 394 字符串解码

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




























## 

