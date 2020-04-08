# LeetCode 周赛

## 179

### T1.生成每种字符都是奇数个的字符串

<img src="LeetCode%E5%91%A8%E8%B5%9B.assets/image-20200308152527330.png" alt="image-20200308152527330" style="zoom: 80%;" />

未规定字符,则n为偶数,就n-1个a,1个b; n为奇数,就n个a.

```c++
class Solution {
public:
    string generateTheString(int n) {
        if (n == 1) return "a";
        string s;
        if (n % 2 == 0)
        {     
            string s1(n - 1, 'a');
            s = s1 + 'b';
        }
        else 
        {
            string s1(n, 'a');
            s = s1;
        }
        return s;
    }
};
```

### T2. 灯泡开关III

<img src="LeetCode%E5%91%A8%E8%B5%9B.assets/image-20200308145953176.png" alt="image-20200308145953176" style="zoom:80%;" />

**法一: 优化的暴力法**

- 暴力法: 每次点亮一个灯,将记录数组中对应位置处标记为true; 之后遍历从0遍历数组到当前已点亮的最大位置,若每个位置都已经为true,则计数器加一.

缺点显而易见,每次都要从头遍历一次.最终超时了,无法通过提交.

```c++
class Solution {
public:
    int numTimesAllBlue(vector<int>& light) {
        int n = light.size();
        vector<bool> vec(n, false);
        int count = 0;
        int boder = 0;
        // 遍历light
        for (int k = 0; k < n; k++)
        {
            vec[light[k] - 1] = true;
            bool flag = true;
            if (light[k] > boder) boder = light[k];
            for (int j = 0; j < boder; j++)
            {
                if (!vec[j]) flag = false;
            }
            if (flag) count++;
        }
        return count;
    }
};
```

- 优化暴力法:记录下上次检查数组时,最后一个ture的下一个位置.本轮检查从这个位置开始,而不再从0开始.

```c++
class Solution {
public:
    int numTimesAllBlue(vector<int>& light) {
        int n = light.size();
        vector<bool> vec(n, false);
        int count = 0;
        int start = 0;
        int end = 0;
        // 遍历light
        for (int k = 0; k < n; k++)
        {
            vec[light[k] - 1] = true;
            bool flag = true;
            if (light[k] > end) end = light[k];
            for (int j = start; j < end; j++)
            {
                if (!vec[j])
                {
                    flag = false;
                    break;
                }
                start++;

            }
            if (flag) count++;
        }
        return count;
    }
};
```

**法二: 记录已点亮的最大位置**

记录下已经点亮的灯的最大编号,第k时刻已经点亮了k+1盏灯.如果k时刻点亮的灯数等于最大编号,则说明左侧全部点亮.

```c++
class Solution {
public:
    int numTimesAllBlue(vector<int>& light) {
        int blueNum = 0;
        int maxNum = 0;
        for (int k = 0; k < light.size(); k++)
        {
            maxNum = max(maxNum, light[k]);
            if (maxNum == k + 1)
                blueNum++;
        }
        return blueNum;
    }
};
```







