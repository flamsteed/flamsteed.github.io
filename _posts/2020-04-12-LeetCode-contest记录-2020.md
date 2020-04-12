---
layout:     post                    # 使用的布局（不需要改）
title:      LeetCode-contest记录    # 标题 
subtitle:   单周赛184               #副标题
date:       2020-03-29              # 时间
author:     ZXY                      # 作者
header-img:  img/post-bg-rwd.jpg   #这篇文章标题背景图片
catalog: true                       # 是否归档
tags:                               #标签
    - LeetCode
    - contest
---

# 写在前面
第二次ak，不容易，虽然还是相对较水的一次。

# 5380. 数组中的字符串匹配
>给你一个字符串数组 words ，数组中的每个字符串都可以看作是一个单词。请你按 任意 顺序返回 words 中是其他单词的子字符串的所有单词。<br/>
如果你可以删除 words[j] 最左侧和/或最右侧的若干字符得到 word[i] ，那么字符串 words[i] 就是 words[j] 的一个子字符串。

>示例 1：<br/>
输入：words = ["mass","as","hero","superhero"]<br/>
输出：["as","hero"]<br/>
解释："as" 是 "mass" 的子字符串，"hero" 是 "superhero" 的子字符串。
["hero","as"] 也是有效的答案。<br/>

>提示：<br/>
1 <= words.length <= 100<br/>
1 <= words[i].length <= 30<br/>
words[i] 仅包含小写英文字母。<br/>
题目数据 保证 每个 words[i] 都是独一无二的。<br/>

解法：<br/>
暴力就行了，数据量很小。

代码：

    class Solution {
    public:
        vector<string> stringMatching(vector<string>& a) {
            vector<string> res;
            if(a.empty()) return res;
            int l = a.size();
            for(int i=0; i<l; ++i){
                for(int j=0; j<l; ++j){
                    if(i!=j && a[j].size()>a[i].size()){
                        auto pos = a[j].find(a[i]);
                        if(pos!=a[j].npos){
                            res.push_back(a[i]);
                            break;
                        }
                    }
                }
            }
            return res;
        }
    };

5381. 查询带键的排列
>给你一个待查数组 queries ，数组中的元素为 1 到 m 之间的正整数。 请你根据以下规则处理所有待查项 queries[i]（从 i=0 到 i=queries.length-1）：<br/>
>- 一开始，排列 P=[1,2,3,...,m]。<br/>
>- 对于当前的 i ，请你找出待查项 queries[i] 在排列 P 中的位置（下标从 0 开始），然后将其从原位置移动到排列 P 的起始位置（即下标为 0 处）。注意， queries[i] 在 P 中的位置就是 queries[i] 的查询结果。<br/><br/>
>请你以数组形式返回待查数组  queries 的查询结果。<br/>

>示例 1：<br/>
输入：queries = [3,1,2,1], m = 5<br/>
输出：[2,1,2,1] <br/>
解释：待查数组 queries 处理如下：<br/>
对于 i=0: queries[i]=3, P=[1,2,3,4,5], 3 在 P 中的位置是 2，接着我们把 3 移动到 P 的起始位置，得到 P=[3,1,2,4,5] 。<br/>
对于 i=1: queries[i]=1, P=[3,1,2,4,5], 1 在 P 中的位置是 1，接着我们把 1 移动到 P 的起始位置，得到 P=[1,3,2,4,5] 。 <br/>
对于 i=2: queries[i]=2, P=[1,3,2,4,5], 2 在 P 中的位置是 2，接着我们把 2 移动到 P 的起始位置，得到 P=[2,1,3,4,5] 。<br/>
对于 i=3: queries[i]=1, P=[2,1,3,4,5], 1 在 P 中的位置是 1，接着我们把 1 移动到 P 的起始位置，得到 P=[1,2,3,4,5] 。 <br/>
因此，返回的结果数组为 [2,1,2,1] 。  <br/>

>提示：<br/>
1 <= m <= 10^3<br/>
1 <= queries.length <= m<br/>
1 <= queries[i] <= m<br/>

解法：<br/>
从m的范围来看，n^2的算法就可以通过了，模拟这个过程就是n^2。

代码：

    class Solution {
    public:
        vector<int> processQueries(vector<int>& a, int m) {
            vector<int> p(m+1,0);
            vector<int> res;
            if(a.empty()) return res;
            for(int i=0; i<m; ++i){
                p[i] = i+1;
            }
            for(int k=0; k<a.size(); ++k){
                int pos;
                for(pos = 0; pos<m; ++pos){
                    if(p[pos]==a[k]){
                        break;
                    }
                }
                res.push_back(pos);
                int tmp = p[pos];
                for(int i=pos; i>=1; --i){
                    p[i] = p[i-1];
                }
                p[0] = tmp;
            }
            return res;
        }
    };

编程最忌讳的其实就是大量的数组移动操作，那么为了减少这种操作，我们可以用map记录数值对应的索引位置，然后修改索引就行了。

代码：

    class Solution {
    public:
        vector<int> processQueries(vector<int>& queries, int m) {
            map<int,int> mp;
            for(int i=0; i<m; ++i){
                mp[i+1] = i;
            }
            vector<int> res;
            for(int i=0; i<queries.size(); ++i){
                int k = queries[i];
                res.push_back(mp[k]);
                for(auto& p : mp){
                    if(p.second>=mp[k])
                        continue;
                    ++p.second;
                }
                mp[k] = 0;
            }
            return res;
        }
    };

1410. HTML 实体解析器
>「HTML 实体解析器」 是一种特殊的解析器，它将 HTML 代码作为输入，并用字符本身替换掉所有这些特殊的字符实体。<br/>
HTML 里这些特殊字符和它们对应的字符实体包括：<br/>
双引号：字符实体为 &quot; ，对应的字符是 " 。<br/>
单引号：字符实体为 &apos; ，对应的字符是 ' 。<br/>
与符号：字符实体为 &amp; ，对应对的字符是 & 。<br/>
大于号：字符实体为 &gt; ，对应的字符是 > 。<br/>
小于号：字符实体为 &lt; ，对应的字符是 < 。<br/>
斜线号：字符实体为 &frasl; ，对应的字符是 / 。<br/>
给你输入字符串 text ，请你实现一个 HTML 实体解析器，返回解析器解析后的结果。<br/>

>示例 1：<br/>
输入：text = "&amp; is an HTML entity but &ambassador; is not."<br/>
输出："& is an HTML entity but &ambassador; is not."<br/>
解释：解析器把字符实体 &amp; 用 & 替换<br/>

解法：<br/>
字符串操作基本功考察咯，比赛的时候switch报了很奇怪的错，只好改成一堆if。<br/>

代码：

    class Solution {
    public:
        string entityParser(string s) {
            string res = "";
            if(s.empty())
                return res;
            int p = 0;
            while(p<s.length()){
                string tmp = "";
                if(s[p]=='&'){
                    while(p<s.length()&&s[p]!=';'){
                        tmp+=s[p];
                        ++p;
                    }
                    if(p==s.length()){
                        res+=tmp;
                    }
                    else{
                        tmp+=s[p];
                        //cout<<tmp<<endl;
                        if(tmp=="&quot;"){
                                res+="\"";
                        }
                        else if(tmp=="&apos;"){
                                res+="\'";
                        }
                        else if(tmp=="&amp;"){
                                res+="&";
                        }
                        else if(tmp=="&gt;"){
                                res+=">";
                        }
                        else if(tmp=="&lt;"){
                                res+="<";
                        }
                        else if(tmp=="&frasl;"){
                                res+="/";
                        }
                        else{
                                res += tmp;
                        }
                    }
                }
                else{
                    res+=s[p];
                }
                ++p;
            }
            return res;
        }
    };

# 5383. 给 N x 3 网格图涂色的方案数
>你有一个 n x 3 的网格图 grid ，你需要用 红，黄，绿 三种颜色之一给每一个格子上色，且确保相邻格子颜色不同（也就是有相同水平边或者垂直边的格子颜色不同）。<br/>
给你网格图的行数 n 。<br/>
请你返回给 grid 涂色的方案数。由于答案可能会非常大，请你返回答案对 10^9 + 7 取余的结果。<br/>

>提示：<br/>
n == grid.length<br/>
grid[i].length == 3<br/>
1 <= n <= 5000<br/>

解法：<br/>
这题才是今天的重点，乍一看是一道数学题，实际上，他就是一道数学题。。。<br/>
至少我水平不济，想不出什么状态压缩dp，只好老老实实找规律。<br/>
其实我们可以发现，一行三个格子，有两种情况：使用两种颜色和使用三种颜色。<br/>
我们假设字母abc分别表示三种颜色，那么就有以下的涂色方式：<br/>
两种颜色：
- aba
- aca
- bab
- bcb
- cac
- cbc<br/>

三种颜色：
- abc
- acb
- bca
- bac
- cab
- cba

这时候我们考虑下一行，如果现在是两种颜色(aba)，那么下一行可能的涂色方式：
- bcb
- bab
- cac
- bac
- cab

如果现在是三种颜色(abc)，那么下一行可能的涂色方式：
- bab
- bcb
- bca
- cab

可以观察到，如果上一行是两种颜色，那么下一行有5种涂色方式，包含2种两色，3种三色；如果上一行是三种颜色，那么下一行有4种涂色方式，包含2种两色，2种三色。<br/>
这时候就可以发现递推规律已经出来了，写就行了。（其实这和状态压缩dp本质差不多）

代码：

    class Solution {
    public:
        int numOfWays(int n) {
            long long a,b,c,d;
            a = 6;
            b = 6;
            long long mod = pow(10,9) + 7;
            if(n==0) return 0;
            if(n==1) return 12;
            for(int i=2; i<=n; ++i){
                c = a*3 + b*2;
                c %= mod;
                d = a*2 + b*2;
                d %= mod;
                a = c;
                b = d;
            }
            long long res = a + b;
            return res%mod;
        }
    };