---
title: EducationalCodeforcesRound111
date: 2021-07-16 22:26:19
catagories:
  - CF
tags:
  - 暴力
  - 几何
---
# Educational Codeforces Round 111 

## C题 [Manhattan Subarrays](https://codeforces.com/contest/1550/problem/C)  

### 题目大意
题目大概意思是，定义两个点$a(x_1,y_1)$,$b(x_2,y_2)$的距离$d=|x_1-x_2|+|y_1-y_2|$  。对于三个点p，q，r如果$d(p,r)=d(p,q)+d(q,r)$,则认为这三个点是一个“坏三连”。问给定一个数组$a_1,a_2...a_n$每个元素$a_i$与i构成一个点$(a_i,i)$，问有多少个任意三个点都不能构成“坏三连”的连续子串(如果连续子串只有一个或者两个元素，则也为不能构成“坏三连”的子串)
#### 分析

分析题目可以发现，连续子串每个元素$a_i$对应的点横坐标不确定，但是纵坐标是严格递增的。
* 考察可以构成“坏三连”的三个点，由题意可知纵坐标为中间那个点一定在另外两个点中间(包含边边)，如图。、
   ![avata](http://qwc0s7gbm.hn-bkt.clouddn.com/CF111_0.png)
* 先考察高度连续两个点$p(a_i,i)$,$q(a_{i+1},i+1)$
    1.如果$a_{i+1}>a_i$，则后面的点都不能在q右边。图片红色的区域不可再落点第三个点。
    ![avata](http://qwc0s7gbm.hn-bkt.clouddn.com/CF111_1.png)
    2.$a_{i+1}<a_i$，也类似。
    3.如果$a_{i+1}=a_i$，则无论下一个点放在哪里，都是“坏三连”，不能放第三个点。
* 考察三个点$p(a_i,i)$,$q(a_{i+1},i+1)$,$r(a_{i+2},i+2)$，只分析$a_{i+1}>a_i$，并且第三个点以落的情况，$a_{i+1}<a_i$情况类似。
   1.如果$a_{i+2}<a_i$，则r的左上角也不能再放置点
   ![avata](http://qwc0s7gbm.hn-bkt.clouddn.com/CF111_2.png)
   2.如果$a_i<a_{i+2}<a_i+1$,则受到p和q的同时影响，r左上角和右上角都无法再放置点，则不可再放置下一个点
   3.如果$a_{i+2}=a_i$，则不能无法再放置下一个点，r一定会在下一个点到p之间。
*  考察能放下四个点，则不可能再放下第五个点，则最多四个点。只有在放三个点的第一条基础上，才能放置四个点。
    ![avata](http://qwc0s7gbm.hn-bkt.clouddn.com/CF111_3.png)
下面暴力模拟一遍过的，但是有点笨。其实有更好的方法

```c++
#include<iostream>
using namespace std;
int t; // 测试用例数
int n; // 数组长度
long long a[200002];
long long ans;
int main(){
    //freopen("test/C.txt","r",stdin);
    cin>>t;
    while(t--){
       ans=0;
       cin>>n;
       for(int i = 0;i<n;++i){
           cin>>a[i];
       }
       int right;   // 坐标右下线
       int left;    // 坐标左上限
       for(int i = 0;i<n-2;++i){
           right=0;
           left=0;
           // 先判断第二个点
           if(a[i+1]>a[i]){
               right=a[i+1];
           }else if(a[i+1]<a[i]){
               left=a[i+1];
           }else{
               continue;
           }
           
           // 判断第三个点
           if(right!=0){   // 如果第一个点偏右
              if(a[i+2]>=right){
                  continue;
              }else if(a[i+2]<a[i]){
                  left=a[i+2];
                  ans++;
              }else{
                    ans++;
                  continue;
              }
           }else{
               if(a[i+2]<=left){
                  continue;
              }else if(a[i+2]>a[i]){
                  right=a[i+2];
                  ans++;
              }else{
                    ans++;
                  continue;
              }
           }

           // 判断第四个点
           if(i!=n-3&&a[i+3]<right&&a[i+3]>left){
               ans++;
           }
       }

       ans+=n;   // 单独也是答案
       ans+=n-1; // 每两个也是答案
       cout<<ans<<endl;
       
    }
}
```