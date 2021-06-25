---
title: 在O(logn)的复杂度下两数组的混合中位数
date: 2021-04-10 23:32:41
categories:
  -算法
tags:
  -分治法
---
### 题目 
设X[ 0 : n - 1]和Y[ 0 : n – 1 ]为两个数组，每个数组中含有n个已排好序的数。找出X和Y的2n个数的中位数。要求：O(logn)时间内

##### 解析
要在O(logn)时间内求解，则自然想到分治算法。求解思路与如下：
先分别求数组X和Y的两个中位数MedX和MedY。则会有以下情况
* （1）MedX==MedY。则很明显MedX即是2n个数的中位数。因为两个数组是完全等长的，所以两个数组中位数两边的数的个数，也是完全一样的。

* （2）MedX>MedY
  * 如果n是奇数
这种情况下，比X<sub>(n-1)/2+1</sub>,X<sub>(n-1)/2+2</sub>,...,X<sub>n-1</sub>小的数至少有n+1个，所以X<sub>(n-1)/2+1</sub>,X<sub>(n-1)/2+2</sub>,...,X<sub>n-1</sub>一定比中位数大。所以中位数不会在X<sub>(n-1)/2+1</sub>,X(n-1)/2+2</sub>,...,X<sub>n-1</sub>中。同样Y<sub>0</sub>,Y<sub>1</sub>,...,Y<sub>(n-1)/2-1</sub>一定比中位数小。所以中位数在X<sub>0</sub>,X<sub>1</sub>,...,X<sub>(n-1)/2</sub>和Y<sub>(n-1)/2</sub>,Y<sub>n/2+2</sub>,...,Y<sub>n-1</sub>中。则只需要考察X[0:(n-1)/2:n-1]和Y[0:(n-1)/2]。
  * 如果n是偶数
  则X<sub>(n-1)/2+2</sub>,X<sub>(n-1)/2+3</sub>,...,X<sub>n-1</sub>小的数至少有n+1个，所以X<sub>(n-1)/2+2</sub>,X<sub>(n-1)/2+3</sub>,...,X<sub>n-1</sub>一定比中位数大。同样Y<sub>0</sub>,Y<sub>1</sub>,...,Y<sub>(n-)/2-2</sub>一定比中位数小。则只需要考察X[0:(n-1)/2+1]和Y[(n-1)/2-1:n-1]。

由于在中位数两边加上或减去任意等数量的数并不影响中位数的取值,所以上述方法成立。
* （3）MedX<MedY
这种情况和上面的同理

分到最后每个数组只有两个元素，则不可以再分下去，利用X和Y的中位数用直接方法求解。
  
``` c++
#include<iostream>
using namespace std;
/*
函数名：FindMedian
功能：  求数组的某一区间的中位数
参数：  double Z[]-所求数组，int zl-数组的最小下标，int zr-数组的最大下标
返回值：double-中位数
*/
double FindMedian(double Z[],int zl,int zr){
       if((zr-zl)%2==0){
           return Z[(zr+zl)/2];
       }else{
           return (Z[(zr+zl)/2]+Z[(zr+zl)/2+1])/2;
       }
}
/*
函数名：FindMedian
功能：  求分别有序数组X某一区间和有序数组Y某一区间的混合中位数
参数：  int X[]-数组X，int xl-数组X要考察的最小下标，int xr-数组x要考察的最大下标
返回值：double-混合中位数
*/
double FindMixMedian(double X[],int xl,int xr,double Y[],int yl,int yr){
    
    double MedX=FindMedian(X,xl,xr);// 求X考察区域的中位数
    double MedY=FindMedian(Y,yl,yr);// 求y所考察区域的中位数
    // 若两个中位数相等，则所求便是混合区间的中位数
    if(abs(MedX-MedY)<0.00001){
        return MedY;
    }
    // 若两边都只剩一个了，就取平均
    if(xl==xr&&yl==yr){
        return (X[xl]+Y[xl])/2;
    }
    // 若每个数组只剩两个，则讨论求出中位数
    if(xr-xl+1==2&&yr-yl+1==2){
        // 若MedX大，则X[xr]大于X[xl]和Y[YL],则判断X[xr]和Y[yr]
        if(MedX>MedY){    
            if(X[xr]>=Y[yr]){     // 若X[xr]>=Y[yr],则可以确定地关系是Y[yl]<=Y[yr]<=X[xr]
                if(X[xl]<Y[yl]){  // X[xl]<Y[yl]<=Y[yr]<=X[xr] 
                    return ((Y[yl]+Y[yr])/2);
                }else{            // Y[yl]<=Y[yr]<X[xl]<=X[xr]或 Y[yl]<=X[xl]<=Y[yr]<=X[xr]
                    return ((X[xl]+Y[yr])/2);
                }
            }else{ // 如果X[xr]比Y[Yr]小，则X[xl]一定比Y[yl]大,则 Y[yl]<X[xl]<=X[xr]<Y[yr]
             return MedX;
            }
        }
        // 若MedY大，则与上面的对称
        if(MedX<MedY){     
            if(Y[yr]>=X[xr]){
                if(Y[yl]<X[xl]){
                    return ((X[xl]+X[xr])/2);
                }else{    
                    return ((Y[yl]+X[xr])/2);
                }  
            } else{
                return MedY;
            }
         }

    }
    // 区间划分
    if(MedX>MedY){   // MedX大时，X取小的部分，Y取大的部分
        if((xl-xr+1)%2!=0){    // 若区间个数为奇数
           return FindMixMedian(X,xl,(xl+xr)/2,Y,(yl+yr)/2,yr);
        }else{
           return FindMixMedian(X,xl,(xl+xr)/2+1,Y,(yl+yr)/2,yr);
        }
    }else{           // MedX小时，X取大的部分，Y取小的部分
        if((xl-xr+1)%2!=0){    // 若区间个数为奇数
           return FindMixMedian(X,(xl+xr)/2,xr,Y,yl,(yl+yr)/2);
        }else{
           return FindMixMedian(X,(xl+xr)/2,xr,Y,yl,(yl+yr)/2+1);
        }
    }
}
int main(){
    freopen("input1.txt","r",stdin);
    freopen("output1.txt","w",stdout);
    int n;     // 数组X和Y中每个数组所含有的元素个数   
    double X[202]; 
    double Y[202];
    double ans;
    cin>>n;
    for(int i = 0;i<n;++i){
        cin>>X[i];
    }
    for(int i = 0;i<n;++i){
        cin>>Y[i];
    }
    ans = FindMixMedian(X,0,n-1,Y,0,n-1);
    cout<<ans<<endl;
}
```