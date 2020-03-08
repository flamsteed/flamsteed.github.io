---
layout:     post                    # 使用的布局（不需要改）
title:      LeetCode-contest记录    # 标题 
subtitle:   双周赛21&单周赛179       #副标题
date:       2020-03-08              # 时间
author:     ZXY                      # 作者
header-img:  img/post-bg-rwd.jpg   #这篇文章标题背景图片
catalog: true                       # 是否归档
tags:                               #标签
    - LeetCode
    - contest
---

# 写在前面
周末两天leetcode比赛做题整理。

# 双周赛21
## 5336. 上升下降字符串
>给你一个字符串 s ，请你根据下面的算法重新构造字符串：<br/>
从 s 中选出 最小 的字符，将它 接在 结果字符串的后面。<br/>
从 s 剩余字符中选出 最小 的字符，且该字符比上一个添加的字符大，将它 接在 结果字符串后面。<br/>
重复步骤 2 ，直到你没法从 s 中选择字符。<br/>
从 s 中选出 最大 的字符，将它 接在 结果字符串的后面。<br/>
从 s 剩余字符中选出 最大 的字符，且该字符比上一个添加的字符小，将它 接在 结果字符串后面。<br/>
重复步骤 5 ，直到你没法从 s 中选择字符。<br/>
重复步骤 1 到 6 ，直到 s 中所有字符都已经被选过。<br/>
在任何一步中，如果最小或者最大字符不止一个 ，你可以选择其中任意一个，并将其添加到结果字符串。<br/>
请你返回将 s 中字符重新排序后的 结果字符串 。

>示例 1：<br/>
输入：s = "aaaabbbbcccc"<br/>
输出："abccbaabccba"<br/>
解释：第一轮的步骤 1，2，3 后，结果字符串为 result = "abc"<br/>
第一轮的步骤 4，5，6 后，结果字符串为 result = "abccba"<br/>
第一轮结束，现在 s = "aabbcc" ，我们再次回到步骤 1<br/>
第二轮的步骤 1，2，3 后，结果字符串为 result = "abccbaabc"<br/>
第二轮的步骤 4，5，6 后，结果字符串为 result = "abccbaabccba"

解法一：比赛的时候没想太多，用了模拟，很蠢，而且代码量很大。

代码：

    class Solution {
    public:
        int getMinidx(string& s){
            char c = s[0];
            int p = 0;
            for(int i=1; i<s.length(); i++){
                if(c>s[i]){
                    c = s[i];
                    p = i;
                }
            }
            return p;
        }
        int getMaxidx(string& s){
            char c = s[0];
            int p = 0;
            for(int i=1; i<s.length(); i++){
                if(c<s[i]){
                    c = s[i];
                    p = i;
                }
            }
            return p;
        }
        char getMin(string& s){
            char c = s[0];
            for(int i=1; i<s.length(); i++){
                if(c>s[i])
                    c = s[i];
            }
            return c;
        }
        char getMax(string& s){
            char c = s[0];
            for(int i=1; i<s.length(); i++){
                if(c<s[i])
                    c = s[i];
            }
            return c;
        }
        string sortString(string s) {
            string res = "";
            if(s.empty()) return res;
            while(true){
                //1
                char t = getMin(s);
                int p = getMinidx(s);
                res = res + t;
                s.erase(p,1);
                if(s.empty()) break;
                bool k = true;
                while(k){//2 3
                    k = false;
                    char m = s[0];
                    int pp = 0;
                    for(int i=1; i<s.length(); i++){
                        if(s[i]>res[res.length()-1]){
                            k = true;
                            m = s[i];
                            pp = i;
                            break;
                        }
                    }
                    if(k==false) break;
                    for(int i=0; i<s.length(); i++){
                        if(s[i]>res[res.length()-1] && s[i]<m){
                            m = s[i];
                            pp = i;
                        }
                    }
                    res = res + m;
                    s.erase(pp,1);
                    if(s.empty()){
                        break;
                    }
                }
                if(s.empty()) break;
                //4
                t = getMax(s);
                p = getMaxidx(s);
                res = res + t;
                s.erase(p,1);
                if(s.empty()) break;
                k = true;
                while(k){//5 6
                    k = false;
                    char m = s[0];
                    int pp = 0;
                    for(int i=1; i<s.length(); i++){
                        if(s[i]<res[res.length()-1]){
                            k = true;
                            m = s[i];
                            pp = i;
                            break;
                        }
                    }
                    if(k==false) break;
                    for(int i=0; i<s.length(); i++){//5
                        if((s[i]<res[res.length()-1]) && (s[i]>m)){
                            m = s[i];
                            pp = i;
                        }
                    }
                    res = res + m;
                    s.erase(pp,1);
                    if(s.empty()){
                        break;
                    }
                }
                if(s.empty()){
                    break;
                }
            }
            return res;
        }
    };

解法二：用26位的数组存放字母个数，然后从前往后，从后往前加减，答案就出来了，比模拟不知道好到哪里去了。

代码：

    class Solution {
    public:
        string sortString(string s) {
            if (s.empty()) return s;
            vector<int> a(26,0);
            for(int i=0; i<s.length(); i++){
                a[int(s[i]-'a')]++;
            }
            string res = "";
            do{
                for(int i=0; i<26; i++){//1 2 3
                    if(a[i]>0){
                        res = res + char(i+int('a'));
                        a[i]--;
                    }
                }
                for(int i=25; i>=0; i--){//4 5 6
                    if(a[i]>0){
                        res = res + char(i+int('a'));
                        a[i]--;
                    }
                }
            }while(res.length()<s.length());
            return res;
        }
    };

## 5338. 二叉树中的最长交错路径
>给你一棵以 root 为根的二叉树，二叉树中的交错路径定义如下：<br/>
选择二叉树中 任意 节点和一个方向（左或者右）。<br/>
如果前进方向为右，那么移动到当前节点的的右子节点，否则移动到它的左子节点。<br/>
改变前进方向：左变右或者右变左。<br/>
重复第二步和第三步，直到你在树中无法继续移动。<br/>
交错路径的长度定义为：访问过的节点数目 - 1（单个节点的路径长度为 0 ）。<br/>
请你返回给定树中最长 交错路径 的长度。<br/>

解法：数据范围是最大50000个点，递归肯定爆，然后我还是写了一个递归，然后超时了，其实递归稍微改一下就行了，自顶向下遍历二叉树，方向是对的就上层深度+1，不对就从1开始重新计算，这样就不会超时了。

代码：

    class Solution {
    public:
        int res = 0;
        void helper(TreeNode* root, int k, int d){
            if(root==NULL) return;
            res = max(res,d);
            if(k){
                helper(root->left,0,d+1);
                helper(root->right,1,1);
            }
            else{
                helper(root->left ,0,1);
                helper(root->right,1,d+1);
            }
        }
        int longestZigZag(TreeNode* root) {
            if(root==NULL) return 0;
            helper(root,0,0);
            helper(root,1,0);
            return res;
        }
    };

# 周赛 179
## 5352. 生成每种字符都是奇数个的字符串
>给你一个整数 n，请你返回一个含 n 个字符的字符串，其中每种字符在该字符串中都恰好出现 奇数次 。<br/>
返回的字符串必须只含小写英文字母。如果存在多个满足题目要求的字符串，则返回其中任意一个即可。

解法：全部填a，偶数的话最后一位填b。

代码：

    class Solution {
    public:
        string generateTheString(int n) {
            string s = "";
            if(n==0) return s;
            if(n%2==1){
                for(int i=1; i<=n; i++)
                    s = s + 'a';
            }
            else{
                for(int i=1; i<n; i++)
                    s = s + 'a';
                s = s + 'b';
            }
            return s;
        }
    };

## 5353. 灯泡开关 III
>房间中有 n 枚灯泡，编号从 1 到 n，自左向右排成一排。最初，所有的灯都是关着的。<br/>
在 k 时刻（ k 的取值范围是 0 到 n - 1），我们打开 light[k] 这个灯。<br/>
灯的颜色要想 变成蓝色 就必须同时满足下面两个条件：<br/>
灯处于打开状态。<br/>
排在它之前（左侧）的所有灯也都处于打开状态。<br/>
请返回能够让所有开着的灯都变成蓝色的时刻数目 。<br/>

解法：先用模拟做了一遍，然后爆了，然后细细一想，其实蓝色灯的意思就是：第i个时间，1~i的灯都开了，这时候才是全蓝，求和判断就完事了。

代码：

    class Solution {
    public:
        int numTimesAllBlue(vector<int>& light) {
            if(light.empty()) return 0;
            int res = 0;
            int l = light.size();
            long s = 0;
            for(int i=0; i<l; i++){
                s += light[i];
                if(s == (long(1+i+1))*(long(i+1))/2) res++;
            }
            return res;
        }
    };

## 5355. T 秒后青蛙的位置
>给你一棵由 n 个顶点组成的无向树，顶点编号从 1 到 n。青蛙从 顶点 1 开始起跳。规则如下：<br/>
在一秒内，青蛙从它所在的当前顶点跳到另一个未访问过的顶点（如果它们直接相连）。<br/>
青蛙无法跳回已经访问过的顶点。<br/>
如果青蛙可以跳到多个不同顶点，那么它跳到其中任意一个顶点上的机率都相同。<br/>
如果青蛙不能跳到任何未访问过的顶点上，那么它每次跳跃都会停留在原地。<br/>
无向树的边用数组 edges 描述，其中 edges[i] = [fromi, toi] 意味着存在一条直接连通 fromi 和 toi 两个顶点的边。<br/>
返回青蛙在 t 秒后位于目标顶点 target 上的概率。<br/>

解法：先从target倒着跳，求出跳到根的跳数，同时记录每次跳到的位置，和时间比较一下，如果时间不够就输出0,<br/>
因为青蛙是死脑筋，所以如果target不是叶节点，且时间大于跳数，那青蛙还是不会停在target，输出0。<br/>
完成以上判断之后，剩下的情况都能跳到，求概率就行了，这时候遍历之前记录的位置，统计这些位置的子树个数，然后计算概率就行了。

代码：

    class Solution {
    public:
        double frogPosition(int n, vector<vector<int>>& edges, int t, int target) {
            if(edges.empty()){
                if(t==0) return 0.0;
                if(target==1) return 1.0;
                return 0.0;
            }
            int d = 0;
            int l = 0;
            int r = target;
            vector<int> path;
            for(int i=0; i<edges.size(); i++){
                if(edges[i][0]>edges[i][1]){
                    int t = edges[i][0];
                    edges[i][0] = edges[i][1];
                    edges[i][1] = t;
                }
            }
            bool f = true;
            if(target==1){
                f = true;
                for(int i=0; i<edges.size(); i++){
                    if(edges[i][0]==target){
                        f = false;
                        break;
                    }
                }
                if(!f && t>0) return 0.0;
                return 1.0;
            }
            while(l!=1 && f){
                f = false;
                for(int i=0; i<edges.size(); i++){
                    if(edges[i][1]==r){
                        l = edges[i][0];
                        r = l;
                        path.push_back(l);
                        d++;
                        f = true;
                        break;
                    }
                }
            }
            if(t<d || !f) {
                if(edges[0][0]==target) return 1.0;
                return 0.0;
            }
            else if(t>d){
                f = true;
                for(int i=0; i<edges.size(); i++){
                    if(edges[i][0]==target){
                        f = false;
                        break;
                    }
                }
                if(!f) return 0.0;
            }
            vector<int> num;
            for(int i=0; i<path.size(); i++){
                int s = 0;
                for(int j=0; j<edges.size(); j++){
                    if(edges[j][0]==path[i]){
                        s++;
                    }
                }
                num.push_back(s);
            }
            double res = 1.0;
            for(int i=0; i<num.size(); i++){
                res = res * (1.0)/double(num[i]);
            }
            return res;
        }
    };
