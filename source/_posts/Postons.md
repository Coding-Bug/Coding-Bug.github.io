---
title: Postons
date: 2021-06-09 14:44:47
categories:
   - div1
tags:
   - 反悔
   - 贪心
   - 优先队列
---

# 题目
{% asset_img Potions(Hard_version).png %}
#### 题目分析
题目大概意思就是给出n个药水，每种药水会让生命值变化，你从左到右走，经过这个位置的时候可以拿起这瓶药水也可以忽略这瓶药水。你的初始生命值为0，并且整个过程中你的生命值不能低于0，问你最多能拿多少药水。

#### 题解
这个题目和最大连续子序列不同，这个是可以不连续的。

- 贪心：使用贪心的策略，每次只要生命值允许，就拿这瓶药，并且记录自己拿了那些药。

- 反悔：如果走到一个地方，拿了这瓶药之后，自己的生命会比0小，则比较：
     - 若将之前所有药中生命值最小的一瓶比现在碰到的这瓶还小，那么丢弃之前那瓶药，而选择这瓶药，即反悔。
     - 否则跳过这瓶药

- 优先队列：由于每次都要比较之前已经拿了的药最小的，所以要用小根堆来记录之前已经喝过的药。

#### 代码
```c++
#include<iostream>
#include<queue>
using namespace std;
long long Now=0;    // 当前生命值
int ans=0;    // 能够捡起的数量 
int a[200004];
int n;     // position数
int main(){
    //freopen("test/div1A.txt","r",stdin);
    cin>>n;
    priority_queue<int,vector<int>,greater<int>> Q;   // 小根堆
    for(int i = 0;i<n;++i){
        cin>>a[i];
    }

    for(int i =0;i<n;++i){
        if(Now+a[i]<0){
            if(!Q.empty()&&Q.top()<a[i]){
                // 将小根堆顶部替换掉，由于Now本来就大于0，所以反悔后Now也大于0
                Now=Now+a[i]-Q.top();
                Q.pop();
                Q.push(a[i]);
                
            }else{
                continue;
            }
        }else{
            Now+=a[i];
            Q.push(a[i]);    // 把之前要的记录下来
            ans++;
        }
    }
    cout<<ans<<endl;
}
```


