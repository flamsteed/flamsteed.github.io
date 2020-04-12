---
layout:     post                    # 使用的布局（不需要改）
title:      LeetCode-contest记录    # 标题 
subtitle:   双周赛23&单周赛183               #副标题
date:       2020-03-29              # 时间
author:     ZXY                      # 作者
header-img:  img/post-bg-rwd.jpg   #这篇文章标题背景图片
catalog: true                       # 是否归档
tags:                               #标签
    - LeetCode
    - contest
---

# 写在前面
周末leetcode比赛做题整理。

# 双周赛 23
## 5360. 统计最大组的数目
>给你一个整数 n 。请你先求出从 1 到 n 的每个整数 10 进制表示下的数位和（每一位上的数字相加），然后把数位和相等的数字放到同一个组中。<br/>
请你统计每个组中的数字数目，并返回数字数目并列最多的组有多少个。<br/>

>示例 1：<br/>
输入：n = 13<br/>
输出：4<br/>
解释：总共有 9 个组，将 1 到 13 按数位求和后这些组分别是：<br/>
[1,10]，[2,11]，[3,12]，[4,13]，[5]，[6]，[7]，[8]，[9]。总共有 4 个组拥有的数字并列最多。<br/>

解法：<br/>
遍历求和，然后用哈希表统计和的个数，然后统计最大次数的出现次数。

代码：

    class Solution {
    public:
        int countLargestGroup(int n) {
            vector<int> a(10001,0);
            int mm = 0;
            for(int i=1; i<=n; i++){
                int t = i;
                int s = 0;
                while(t>0){
                    s += t % 10;
                    t/=10;
                }
                mm = max(s,mm);//最大和就是哈希表的范围
                ++a[s]; 
            }
            int m = 0;
            for(int i=1; i<=mm; ++i){//寻找出现最多的次数
                m = max(m,a[i]);
            }
            int res = 0;
            for(int i=1; i<=mm; ++i)//统计该最大次数的出现次数
                if(a[i]==m) ++res;
            return res;
        }
    };

## 5362. 构造 K 个回文字符串
> 给你一个字符串 s 和一个整数 k 。请你用 s 字符串中 所有字符 构造 k 个非空 回文串 。<br/>
如果你可以用 s 中所有字符构造 k 个回文字符串，那么请你返回 True ，否则返回 False 。<br/>

>示例 1：<br/>
输入：s = "annabelle", k = 2<br/>
输出：true<br/>
解释：可以用 s 中所有字符构造 2 个回文字符串。<br/>
一些可行的构造方案包括："anna" + "elble"，"anbna" + "elle"，"anellena" + "b"<br/>

解法：<br/>
统计每个字母出现的个数，然后再统计个数是奇数的字母个数，通过回文字符串的特性可以知道，一个回文字符串最多有一个出现次数为奇数的字母，所以如果奇数的字母个数大于k，那么就一定不能构造。

代码：

    class Solution {
    public:
        bool canConstruct(string s, int k) {
            vector<int> a(30,0);
            if(k>s.length()) return false;
            for(int i=0; i<s.length(); ++i){
                ++a[int(s[i]-'a')];
            }
            int c = 0;
            for(int i=0; i<30; ++i){
                if(a[i]%2==1)
                    ++c;
            }
            if(c>k) return false;
            return true;
        }
    };

## 5361. 圆和矩形是否有重叠
>给你一个以 (radius, x_center, y_center) 表示的圆和一个与坐标轴平行的矩形 (x1, y1, x2, y2)，其中 (x1, y1) 是矩形左下角的坐标，(x2, y2) 是右上角的坐标。<br/>
如果圆和矩形有重叠的部分，请你返回 True ，否则返回 False 。<br/>
换句话说，请你检测是否 存在 点 (xi, yi) ，它既在圆上也在矩形上（两者都包括点落在边界上的情况）。<br/>

解法：<br/>
#### 边界判断
比赛的时候没有去想数学法，直接画图找矩形和圆的位置关系了，但是因为条件考虑的不全，wa了4次才过。<br/>
代码：

    class Solution {
    public:
        bool checkOverlap(int radius, int x_center, int y_center, int x1, int y1, int x2, int y2) {
            int circleleft = x_center - radius;
            int circleright = x_center + radius;
            int circletop = y_center + radius;
            int circlebot = y_center - radius;
            if(circletop<y1) return false;
            if(circlebot>y2) return false;
            if(circleleft>x2) return false;
            if(circleright<x1) return false;
            
            if(circletop<=y2 && circlebot>=y1 && circleleft>=x1 && circleright<=x2) return true;
            if(circletop>=y2 && circlebot<=y1 && circleleft<=x1 && circleright>=x2) return true;
            
            if(circleright==x1 || circleright==x2) return true;
            if(circleleft==x2 || circleleft==x1) return true;
            if(circlebot==y2 || circlebot==y1) return true;
            if(circletop==y1 || circletop==y2) return true;
            
            if(circleleft>x1 && circleright<x2) return true;
            if(circlebot>y1 && circletop<y2) return true;
            
            
            int d1 = (x1-x_center)*(x1-x_center) + (y1-y_center)*(y1-y_center);
            int d2 = (x1-x_center)*(x1-x_center) + (y2-y_center)*(y2-y_center);
            int d3 = (x2-x_center)*(x2-x_center) + (y1-y_center)*(y1-y_center);
            int d4 = (x2-x_center)*(x2-x_center) + (y2-y_center)*(y2-y_center);
            int r = radius * radius;
            if(r<d1 && r<d2 && r<d3 && r<d4) return false;
            if(r>d1 && r>d2 && r>d3 && r>d4) return false;
            
            
            return true;
        }
    };

#### 数学法
用圆心到矩阵中心距离减去矩阵中心到矩阵一角的距离，跟半径比较。<br/>
代码：

    class Solution {
    public:
        bool checkOverlap(int radius, int x_center, int y_center, int x1, int y1, int x2, int y2) {
            int x=(x1+x2)/2,y=(y1+y2)/2;
            double s=distance(x,y,x_center,y_center);
            double s1=distance(x,y,x1,y1);
            if(radius>=int(s-s1)) return true;
            return false;
        }
        double distance(int a,int b,int c,int d){
            double s;
            s=sqrt((a-c)*(a-c)+(b-d)*(b-d));
            return s;
        }
    };

## 5363. 做菜顺序
>一个厨师收集了他 n 道菜的满意程度 satisfaction ，这个厨师做出每道菜的时间都是 1 单位时间。<br/>
一道菜的 「喜爱时间」系数定义为烹饪这道菜以及之前每道菜所花费的时间乘以这道菜的满意程度，也就是 time[i]*satisfaction[i] 。<br/>
请你返回做完所有菜 「喜爱时间」总和的最大值为多少。<br/>
你可以按 任意 顺序安排做菜的顺序，你也可以选择放弃做某些菜来获得更大的总和。<br/>

>示例 1：<br/>
输入：satisfaction = [-1,-8,0,5,-9]<br/>
输出：14<br/>
解释：去掉第二道和最后一道菜，最大的喜爱时间系数和为 (-1*1 + 0*2 + 5*3 = 14) 。每道菜都需要花费 1 单位时间完成。<br/>

>提示：
>1. n == satisfaction.length
>2. 1 <= n <= 500
>3. -10^3 <= satisfaction[i] <= 10^3

解法：<br/>
数据范围小的吓人，这题贪心就行了。<br/>
先排序，根据喜爱时间的定义，我们要把满意度大的菜放到后面，然后从第一个点开始作为做菜起来，然后起点往后移动，因为数据范围很小，也不需要优化了，每次求出喜爱时间求出最大值就行了。

代码：

    class Solution {
    public:
        int maxSatisfaction(vector<int>& a) {
            if(a.empty()) return 0;
            sort(a.begin(),a.end());//排序
            int p = 0;//起点位置
            int res = 0;
            while(p<a.size()){
                int sat = 0;
                for(int i=p; i<a.size(); ++i){
                    sat += (i-p+1) * a[i];
                } 
                res = max(sat,res);
                ++p;
            }
            return res;
        }
    };

