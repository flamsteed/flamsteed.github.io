---
layout:     post                    # 使用的布局（不需要改）
title:      LeetCode-contest记录    # 标题 
subtitle:   单周赛181&双周赛22       #副标题
date:       2020-03-26              # 时间
author:     ZXY                      # 作者
header-img:  img/post-bg-rwd.jpg   #这篇文章标题背景图片
catalog: true                       # 是否归档
tags:                               #标签
    - LeetCode
    - contest
---

# 写在前面
周末leetcode比赛做题整理。

# 周赛 181
## 1389. 按既定顺序创建目标数组
>给你两个整数数组 nums 和 index。你需要按照以下规则创建目标数组：<br/>
目标数组 target 最初为空。<br/>
按从左到右的顺序依次读取 nums[i] 和 index[i]，在 target 数组中的下标 index[i] 处插入值 nums[i] 。<br/>
重复上一步，直到在 nums 和 index 中都没有要读取的元素。<br/>
请你返回目标数组。<br/>
题目保证数字插入位置总是存在。<br/>

>提示：
>1. 1 <= nums.length, index.length <= 100
>2. nums.length == index.length
>3. 0 <= nums[i] <= 100
>4. 0 <= index[i] <= i

解法：范围这么小，模拟一遍就行，一边插入一边移动。

代码：

    class Solution {
    public:
        vector<int> createTargetArray(vector<int>& a, vector<int>& b) {
            if(a.empty() || b.empty()) return {};
            vector<int> res(b.size(),-1);
            for(int i=0; i<b.size(); i++){
                if(res[b[i]]==-1){
                    res[b[i]] = a[i];
                }
                else{
                    for(int j=b.size()-1; j>=b[i]+1; j--)
                        res[j] = res[j-1];
                    res[b[i]] = a[i];
                }
            }
            return res;
        }
    };

## 1390. 四因数
>给你一个整数数组 nums，请你返回该数组中恰有四个因数的这些整数的各因数之和。<br/>
如果数组中不存在满足题意的整数，则返回 0 。

>示例：<br/>
输入：nums = [21,4,7]<br/>
输出：32<br/>
解释：<br/>
21 有 4 个因数：1, 3, 7, 21<br/>
4 有 3 个因数：1, 2, 4<br/>
7 有 2 个因数：1, 7<br/>
答案仅为 21 的所有因数的和。<br/>

>提示：
>1. 1<= nums.length <= 10^4
>2. 1 <= nums[i] <= 10^5

解法：判断因数的复杂度是O(sqrt(n))，再乘上数组长度，不会超时，所以挨个判断就行了。

代码：

    class Solution {
    public:
        bool checkNum(int k){
            int s = 2;
            for(int i=2; i<=sqrt(k); i++)
                if(k%i==0){
                    if(i!=k/i) s+=2;
                    else s++;
                }
            //cout<<s<<endl;
            if(s==4) return true;
            return false;
        }
        int sumFourDivisors(vector<int>& nums) {
            int res = 0;
            if(nums.empty()) return res;
            for(auto x : nums){
                //cout<<x<<endl;
                if(checkNum(x)){
                    res += 1 + x;
                    for(int j=2; j<=x; j++)
                        if(x%j==0){
                            res += j + x/j;
                            break;
                        }
                }
            }
            return res;
        }
    };

## 1391. 检查网格中是否存在有效路径
>给你一个 m x n 的网格 grid。网格里的每个单元都代表一条街道。grid[i][j] 的街道可以是：<br/>
1 表示连接左单元格和右单元格的街道。<br/>
2 表示连接上单元格和下单元格的街道。<br/>
3 表示连接左单元格和下单元格的街道。<br/>
4 表示连接右单元格和下单元格的街道。<br/>
5 表示连接左单元格和上单元格的街道。<br/>
6 表示连接右单元格和上单元格的街道。<br/>
你最开始从左上角的单元格 (0,0) 开始出发，网格中的「有效路径」是指从左上方的单元格 (0,0) 开始、一直到右下方的 (m-1,n-1) 结束的路径。该路径必须只沿着街道走。<br/>
注意：你 不能 变更街道。<br/>
如果网格中存在有效的路径，则返回 true，否则返回 false 。<br/>

>示例：
输入：grid = [[2,4,3],[6,5,2]]
输出：true
解释：如图所示，你可以从 (0, 0) 开始，访问网格中的所有单元格并到达 (m - 1, n - 1) 。

