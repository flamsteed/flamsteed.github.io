---
layout:     post                    # 使用的布局（不需要改）
title:      LeetCode-contest记录    # 标题 
subtitle:   单周赛180       #副标题
date:       2020-03-17              # 时间
author:     ZXY                      # 作者
header-img:  img/post-bg-rwd.jpg   #这篇文章标题背景图片
catalog: true                       # 是否归档
tags:                               #标签
    - LeetCode
    - contest
---

# 写在前面
周末leetcode比赛做题整理。

# 周赛 180
## 5356. 矩阵中的幸运数
>给你一个 m * n 的矩阵，矩阵中的数字 各不相同 。请你按 任意 顺序返回矩阵中的所有幸运数。<br/>
幸运数是指矩阵中满足同时下列两个条件的元素：<br/>
&emsp;&emsp;在同一行的所有元素中最小<br/>
&emsp;&emsp;在同一列的所有元素中最大<br/>

解法：比赛的时候没想太多，数据范围很小，找到每行最小的数，判断它是不是列中最大的。

代码：

    class Solution {
    public:
        vector<int> luckyNumbers (vector<vector<int>>& a) {
            vector<int> res;
            if(a.empty()) return res;
            for(int i=0; i<a.size(); i++){
                int m = a[i][0];
                int idx = 0;
                for(int j=1; j<a[i].size(); j++){
                    if(m>a[i][j]){
                        m = a[i][j];
                        idx = j;
                    }
                }
                bool f = true;
                for(int j=0; j<a.size(); j++){
                    if(m<a[j][idx]){
                        f = false;
                        break;
                    }
                }
                if(f) res.push_back(a[i][idx]);
            }
            return res;
        }
    };

## 5357. 设计一个支持增量操作的栈
>请你设计一个支持下述操作的栈。<br/>
实现自定义栈类 CustomStack ：<br/>
&emsp;CustomStack(int maxSize)：用 maxSize 初始化对象，maxSize 是栈中最多能容纳的元素数量，栈在增长到 maxSize 之后则不支持 push 操作。<br/>
&emsp;void push(int x)：如果栈还未增长到 maxSize ，就将 x 添加到栈顶。<br/>
&emsp;int pop()：返回栈顶的值，或栈为空时返回 -1 。<br/>
&emsp;void inc(int k, int val)：栈底的 k 个元素的值都增加 val 。如果栈中元素总数小于 k ，则栈中的所有元素都增加 val 。<br/>

解法：用vector操作就行了，没什么难度。

代码：

    class CustomStack {
    private:
        vector<int> a;
        int l = 0;
    public:
        CustomStack(int maxSize) {
            l = maxSize;
        }
        
        void push(int x) {
            if(a.empty() || a.size()<l){
                a.push_back(x);
            }
        }
        
        int pop() {
            if(a.empty())
                return -1;
            else{
                int p = a.back();
                a.pop_back();
                return p;
            }
        }
        
        void increment(int k, int val) {
            if(a.empty()) return;
            int p = 0;
            int t = k;
            while(p<a.size()&&t>0){
                a[p] += val;
                --t;
                ++p;
            }
            return;
        }
    };

    /**
    * Your CustomStack object will be instantiated and called as such:
    * CustomStack* obj = new CustomStack(maxSize);
    * obj->push(x);
    * int param_2 = obj->pop();
    * obj->increment(k,val);
    */

## 5179. 将二叉搜索树变平衡
>给你一棵二叉搜索树，请你返回一棵 平衡后 的二叉搜索树，新生成的树应该与原来的树有着相同的节点值。<br/>
如果一棵二叉搜索树中，每个节点的两棵子树高度差不超过 1 ，我们就称这棵二叉搜索树是 平衡的 。<br/>
如果有多种构造方法，请你返回任意一种。<br/>

解法：本身就是一棵二叉搜索树了，中序遍历得到一个有序数组，然后变成一个平衡的二叉搜索树。

代码：

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
    private:
        vector<int> a;
    public:
        void preorder(TreeNode* root){//中序遍历
            if(root){
                preorder(root->left);
                a.push_back(root->val);
                preorder(root->right);
            }
        }
        TreeNode* sortedArrayToBST(vector<int>& a) {
            return helper(0,a.size()-1,a);
        }
        TreeNode* helper(int left, int right, vector<int>& a){
            if(left>right) return NULL;
            int m = (left+right)/2;
            TreeNode* root  = new TreeNode(a[m]);
            root->left = helper(left,m-1,a);
            root->right= helper(m+1,right,a);
            return root;
        }
        TreeNode* balanceBST(TreeNode* root) {
            if(root==NULL) return root;
            preorder(root);
            return sortedArrayToBST(a);
        }
    };

## 5359. 最大的团队表现值
>公司有编号为 1 到 n 的 n 个工程师，给你两个数组 speed 和 efficiency ，其中 speed[i] 和 efficiency[i] 分别代表第 i 位工程师的速度和效率。请你返回由最多 k 个工程师组成的 ​​​​​​最大团队表现值 ，由于答案可能很大，请你返回结果对 10^9 + 7 取余后的结果。<br/>
团队表现值 的定义为：一个团队中「所有工程师速度的和」乘以他们「效率值中的最小值」。<br/>

解法：比赛的时候想了一种根据最大乘积组合的贪心，过了一半数据，估计还是有问题。<br/>
&emsp;正解是先按照效率降序排序，然后用优先队列存速度，超过k了就扔掉最小的速度，每一遍记录最大的performance（真是思路清晰Orz）。

代码：

    int maxPerformance(int n, vector<int>& speed, vector<int>& efficiency, int k) {
        const int mod = 1000000007;

        vector<vector<int>> es;
        for (int i = 0; i < efficiency.size(); i++)
        {
            es.push_back({ efficiency[i], speed[i] });
        }
        sort(es.rbegin(), es.rend());//按照效率降序

        long long ans = 0;
        long long sum = 0;
        long long eff = INT_MAX;
        priority_queue<int, vector<int>, greater<int> > que;//小顶优先队列
        for (int i = 0; i < es.size(); i++)
        {
            eff = es[i][0];
            sum += es[i][1];
            que.push(es[i][1]);
            if (--k < 0)//个数打到k了，开始删队头
            {
                sum -= que.top();
                que.pop();
            }
            ans = max(ans, sum * eff);
        }
        return ans % mod;
    }