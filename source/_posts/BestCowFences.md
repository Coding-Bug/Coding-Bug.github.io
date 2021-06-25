---
title: BestCowFences
date: 2021-06-13 00:35:27
categories:
    - div1
tags:
    - 限定长度最大子段和
    - 思维
    - 二分答案
---
# 题目
{% asset_img BestCowFence.png %}
### 题目简介
题目大概就是围起连续不小于f的数，使他们的平均值最大，求出这个最大值。
### 分析
 如果是求连续子序列最大和，那么问题就很简单。但是他这里要求的是最大平均数，由于最大平均数不仅和连续子序列的和有关，还和连续子序列的长度有关。即average=sum/length。则事情就变得不那么简单了。

##### 二分答案
这里用到二分答案策略，所谓的二分答案，就是假设所求的值刚开始在一个区间段[l,r]，然后选取这个区间段的中间值mid作为答案，如果算出这个中间值取大了，就从左半区间[l,mid],否则从[mid,r]找最后直到区间缩到无穷小，则此时区间的逼近值即为正确答案。这个其实在数值分析里面学过。

##### 变向思维
我们要求连续区间的最大平均值，很困难，因为这个平均值不仅和连续区间和sum有关，还与区间长度length有关，这就需要我们将问题进行转换。

假设最大平均值为ans，则对于平均值最大的那段区间[i,j]有(a<sub>i</sub>+a<sub>i+1</sub>+...+a<sub>j</sub>)/length=ans。即(a<sub>i</sub>+a<sub>i+1</sub>+...+a<sub>j</sub>)-ans*length=0。

- 如果有大于ans的区间段平均值，则对于最大的区间段[p,q],有(a<sub>p</sub>+a<sub>p+1</sub>+...+a<sub>q</sub>)/length>ans，即(a<sub>p</sub>+a<sub>p+1</sub>+...+a<sub>q</sub>)-ans*length>0。那么我们的ans就偏小了。这就是一个二分的判断偏小依据。

- 如果全部的区间段平均值都小于ans，则对于最大的区间段，有(a<sub>p</sub>+a<sub>p+1</sub>+...+a<sub>q</sub>)/length<ans,即(a<sub>p</sub>+a<sub>p+1</sub>+...+a<sub>q</sub>)-ans*length<0,则此时ans偏大了。得到一个二分偏大的判断。

那么问题就从求连续子序列最大平均值，转换成了求(a<sub>i</sub>+a<sub>i+1</sub>+...+a<sub>j</sub>)-ans * length的值。我们将整个区间段[0,n-1]的值全部减去ans，那么我们在求连续子序列的和时，就相当于求a<sub>i</sub>+a<sub>i+1</sub>+...+a<sub>j</sub>-ans * lengths的值。则问题最终变成了一个连续子序列求和问题。

##### 长度不小于f的最大连续子序列问题
我们可以先求出[0,n]的前缀和sum[i] i=0,1,2...n。由于子序列的长度不能小于f，则以i结尾的连续子序列dp[i] (i>=f)，前f个数一定是要的。先让dp[i]=sum[i],则sum[i]-sum[i-k-1]这段肯定要留在dp[i]里面。所以只需要找到sum[0] ~ sum[i-k]中小于0的最小值sum[m]，然后令dp=sum[i]-sum[m]即为以i结尾的连续子序列的最大值（就是把前面连续的最自己没有正作用的子序列干掉）。则我们要维护从sum[0]~sum[i-k]的最小值，并且每次用dp[i]更新整个区间内连续子序列的最大值即可。
```c++
for(int i=f;i<n;++i){
           Min=min(Min,sum[i-f]);   
           Max=max(Max,sum[i]-Min);   // sum[i]-Min是以i结尾的最大连续子段和，Max是之前求出的最大连续子段和
       }
```

## 算法
先假定二分区间为很大，然后二分假设答案mid，令原数组所有的数减去mid得到新数组。求新数组中长度不小于k的连续子序列的最大和，如果结果大于0，说明ans设小了，区间往右边分，否则区间往左边分。直到最后区间逼近一个数为止。
## 代码

```c++
#include<iostream>
#include<cmath>
using namespace std;
double a[100002];
double b[100002];
double sum[100002]; // 记录第i个数之前的和
int n;
int f;
int main(){
    //freopen("test/div1A.txt","r",stdin);
    ios::sync_with_stdio(false);
    cin.tie(0);
    double mid;       // 假设的答案 
    cin>>n>>f;
    for(int i=0;i<n;++i){
        //scanf("%lf",&a[i]);  // 用cin会套老鹅
        cin>>a[i];
    }
    double l=-1e6;    // 答案区间的左边
    double r=1e6;     // 答案区间的右边
    while(r-l>1e-4){
       mid = (l+r)/2;
       // 先求出减去平均值后的数组
       for(int i=0;i<n;++i){
           b[i]=a[i]-mid;   
       }
       
       sum[0]=b[0];
       for(int i = 1;i<n;++i){
            sum[i]=sum[i-1]+b[i];
       }
       double Min=0;     // 记录以j(0<=j<i-f)结尾大子段和的最小值，初始值应该为0，因为比0小的我们才不要
                         // 如果这里初始化为一个很大的值，那么若sum[0]大于0，就会被认为是要被抛弃的，然而我们只需要抛弃小于0的
       double Max=-1e9;      // 记录长度大于f的最大子段和
       // 求限定长度为f的最大连续子段和
       for(int i=f;i<n;++i){
           Min=min(Min,sum[i-f]);   
           Max=max(Max,sum[i]-Min);   // sum[i]-Min是以i结尾的最大连续子段和，Max是之前求出的最大连续子段和
       }
       Max=max(Max,sum[f-1]);   // 观察可知上面没有判断sum[f-1]的情况
       if(Max>0){  // Max>0说明答案不够大
         l=mid;
       }else{      // 否则就是答案不够小
         r=mid;
       }

    }
    cout<<(int)(1000*r)<<endl;
}
```



