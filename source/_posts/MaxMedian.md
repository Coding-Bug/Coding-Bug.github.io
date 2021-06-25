---
title: MaxMedian
date: 2021-06-13 11:57:10
categories:
    - div1
tags:
    - 限定长度最大子段和
    - 思维
    - 二分答案
---
# 题目
{% asset_img MaxMedian.png %}

### 题目简述
这题是长度不小于k连续子序列排队后的最大中位数。其中排序后+中位数定义下标为$\lfloor(r-l+1)/2\rfloor$.

###分析
这是一道求长度不小于k的连续子序列问题，但它又不是简单求连续子序列的问题。而是求中位数，而且是子序列排序后的中位数，并且要中位数最大。这种场景很难不让人想到二分答案+问题转化。
通过观察不难发现
- 如果连续子序列长度是偶数，如[1,2,3,4,5,6],那么大于等于中位数的数会比小于中位数的数多两个。
- 如果连续子序列的长度是奇数，如[1,2,3,4,5]那么大于等于中位数的数会比小于中位数的数一两个。
除上面所述两中情况之外不会有其他情况。

## 算法
根据分析可以巧妙设计算法，假设答案ans，如果序列中的值大于等于ans，令其等于1，否等于-1。如果变换后的序列中长度不小于k的最大连续子序列的值小于0，就说明中位数ans设大了，则答案往小取，否则答案往大取，直到二分区间无穷小。

```c++
#include<iostream>
using namespace std;
int a[200002];
int sum[200002];
int main(){
    int n,k;
    //freopen("test/div1C.txt","r",stdin);
    // 关同步流
     ios::sync_with_stdio(false);
     cin.tie(0);
    cin>>n>>k;
    for(int i = 0;i<n;++i){
        cin>>a[i];
    }
    double l=-1e9;   // 答案左区间
    double r=1e9;    // 答案右区间
    while(r-l>1e-5){
        //cout<<l<<" "<<r<<endl;
        double mid=(l+r)/2;
        //求变形之后地前缀和
        if(a[0]>=mid){
            sum[0]=1;
        }else{
            sum[0]=-1;
        }
        for(int j = 1;j<n;++j){
           if(a[j]>=mid){
               sum[j]=sum[j-1]+1;
           }else{
               sum[j]=sum[j-1]-1;
           }
        }

       // 求最大子段和
       int Min=0;
       int Max=-1e9;
       for(int i = k;i<n;++i){
           Min=min(Min,sum[i-k]);
           Max=max(Max,sum[i]-Min);
       }

       Max=max(Max,sum[k-1]);
       if(Max>0){
           l=mid;
       }else{
           r=mid;
       }
       
    }
    cout<<(int)r<<endl;
}
```




