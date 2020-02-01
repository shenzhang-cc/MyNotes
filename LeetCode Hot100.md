# LeetCode Hot 100

## [1. 两数之和](https://leetcode-cn.com/problems/two-sum/)
<img src="LeetCode%20Hot100.assets/image-20200131210844744.png" alt="image-20200131210844744"  align="left"/>

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

## 55. 跳跃游戏

## 56. 合并区间

## 62. 不同路径

## 64. 最小路径和

## 70. 爬楼梯

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

## 124. 二叉树中的最大路径和

## 128. 最长连续序列

## 136. 只出现一次的数字

## 139. 单词拆分

## 141. 环形链表

























## 

