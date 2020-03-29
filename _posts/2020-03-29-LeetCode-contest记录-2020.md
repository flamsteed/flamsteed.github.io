---
layout:     post                    # 使用的布局（不需要改）
title:      LeetCode-contest记录    # 标题 
subtitle:   单周赛182               #副标题
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

# 周赛 181
## 5368. 找出数组中的幸运数
>在整数数组中，如果一个整数的出现频次和它的数值大小相等，我们就称这个整数为「幸运数」。<br/>
给你一个整数数组 arr，请你从中找出并返回一个幸运数。<br/>
如果数组中存在多个幸运数，只需返回 最大 的那个。<br/>
如果数组中不含幸运数，则返回 -1 。<br/>

>示例 1：<br/>
输入：arr = [2,2,3,4]<br/>
输出：2<br/>
解释：数组中唯一的幸运数是 2 ，因为数值 2 的出现频次也是 2 。<br/>

解法：暴力就完事了，没啥好说的，

代码：

    class Solution {
    public:
        int findLucky(vector<int>& a) {
            if(a.empty()) return -1;
            vector<int> c(501,0);
            for(int i=0; i<a.size(); i++){
                c[a[i]]++;
            }
            int res = -1;
            for(int i=1; i<=500; i++){
                if(c[i]==i)
                    res = i;
            }
            return res;
        }
    };

## 5369. 统计作战单位数
> n 名士兵站成一排。每个士兵都有一个 独一无二 的评分 rating 。<br/>
每 3 个士兵可以组成一个作战单位，分组规则如下：<br/>
从队伍中选出下标分别为 i、j、k 的 3 名士兵，他们的评分分别为 rating[i]、rating[j]、rating[k]<br/>
作战单位需满足： rating[i] < rating[j] < rating[k] 或者 rating[i] > rating[j] > rating[k] ，其中  0 <= i < j < k < n<br/>
请你返回按上述条件可以组建的作战单位数量。每个士兵都可以是多个作战单位的一部分。<br/>

>示例 ：<br/>
输入：rating = [2,5,3,4,1]<br/>
输出：3<br/>
解释：我们可以组建三个作战单位 (2,3,4)、(5,4,1)、(5,3,1) 。<br/>

### 暴力
比赛的时候瞅了一眼范围，只有200，这么小的范围就是让你暴力的（雾），三重循环n^3不会超时，就是有点丑陋<br/>
代码：

    class Solution {
    public:
        int numTeams(vector<int>& a) {
            if(a.empty()||a.size()<=2) return 0;
            int res = 0;
            for(int x1 = 0; x1<a.size(); x1++){
                for(int x2=x1+1; x2<a.size(); x2++){
                    for(int x3 = x2+1; x3<a.size(); x3++){
                        if(a[x1]<a[x2] && a[x2]<a[x3]){
                            ++res;
                        }
                        if(a[x1]>a[x2] && a[x2]>a[x3]){
                            ++res;
                        }
                    }
                }
            }
            return res;
        }
    };

### 统计
其实可以简化一下，因为这个作战单位只有三个人，所以可以先固定中间的那个人，然后统计这个人左边和右边比他大和比他小的人数，可以组成的单位个数为：biggerl * smallerr + biggerr * smallerl

代码：

    class Solution {
    public:
        int numTeams(vector<int>& a) {
            int l = a.size();
            if(l<3) return 0;
            int res = 0;
            for(int i=1; i<l-1; i++){
                int biggerl(0),smallerl(0),biggerr(0),smallerr(0);
                for(int j=0; j<i; j++){
                    if(a[j]<a[i]){
                        smallerl++;
                    }
                    else{
                        biggerl++;
                    }
                }
                for(int j=i+1; j<l; j++){
                    if(a[j]<a[i]){
                        smallerr++;
                    }
                    else{
                        biggerr++;
                    }
                }
                res += biggerl * smallerr + biggerr * smallerl;
            }
            return res;
        }
    };

## 5370. 设计地铁系统
>请你实现一个类 UndergroundSystem ，它支持以下 3 种方法：<br/>
1. checkIn(int id, string stationName, int t)<br/>
编号为 id 的乘客在 t 时刻进入地铁站 stationName 。<br/>
一个乘客在同一时间只能在一个地铁站进入或者离开。<br/>
2. checkOut(int id, string stationName, int t)<br/>
编号为 id 的乘客在 t 时刻离开地铁站 stationName 。<br/>
3. getAverageTime(string startStation, string endStation) <br/>
返回从地铁站 startStation 到地铁站 endStation 的平均花费时间。<br/>
平均时间计算的行程包括当前为止所有从 startStation 直接到达 endStation 的行程。<br/>
调用 getAverageTime 时，询问的路线至少包含一趟行程。<br/>
你可以假设所有对 checkIn 和 checkOut 的调用都是符合逻辑的。也就是说，如果一个顾客在 t1 时刻到达某个地铁站，那么他离开的时间 t2 一定满足 t2 > t1 。所有的事件都按时间顺序给出。<br/>

解法：<br/>
这种设计类题目都不难，就是比较复杂，要记录的东西非常多，站名，人的id，上车时间，下车时间，因为同一个人会多次上下车，所以最好的办法就是一下车就把对应的时间记录，C++里面pair真的是很好用的一个结构，然后对于下车后的记录，这里参考了 @Ikagura 大佬的思路，把startstation和endstation拼接起来作为key，这样就很方便记录了。<br/>
代码：

    class UndergroundSystem {
    private:
        unordered_map<int,pair<string,int>> record;//记录人的id和上车时间
        unordered_map<string,pair<int,int>> count;//记录下车后的时间
    public:
        UndergroundSystem() {
            record.clear();
            count.clear();
        }
        
        void checkIn(int id, string stationName, int t) {
            record[id] = {stationName,t};//上车记录
        }
        
        void checkOut(int id, string stationName, int t) {
            string name = record[id].first + stationName;//拼接
            count[name].first += t - record[id].second;//记录
            count[name].second += 1;
        }
        
        double getAverageTime(string startStation, string endStation) {
            string name = startStation + endStation;
            return double(count[name].first)/double(count[name].second);
        }
    };

    /**
    * Your UndergroundSystem object will be instantiated and called as such:
    * UndergroundSystem* obj = new UndergroundSystem();
    * obj->checkIn(id,stationName,t);
    * obj->checkOut(id,stationName,t);
    * double param_3 = obj->getAverageTime(startStation,endStation);
    */

## 5371. 找到所有好字符串
>给你两个长度为 n 的字符串 s1 和 s2 ，以及一个字符串 evil 。请你返回 好字符串 的数目。<br/>
好字符串 的定义为：它的长度为 n ，字典序大于等于 s1 ，字典序小于等于 s2 ，且不包含 evil 为子字符串。<br/>
由于答案可能很大，请你返回答案对 10^9 + 7 取余的结果<br/>

>示例 1：<br/>
输入：n = 2, s1 = "aa", s2 = "da", evil = "b"<br/>
输出：51 <br/>
解释：总共有 25 个以 'a' 开头的好字符串："aa"，"ac"，"ad"，...，"az"。还有 25 个以 'c' 开头的好字符串："ca"，"cc"，"cd"，...，"cz"。最后，还有一个以 'd' 开头的好字符串："da"。<br/>

我摊牌了，看了题解也没懂，最近学华为题库比较费时间，所以就不研究了，扔个链接：

https://leetcode-cn.com/problems/find-all-good-strings/solution/shu-wei-dp-kmpqian-zhui-shu-zu-java-by-henrylee4/

就酱。

