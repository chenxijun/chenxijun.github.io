---
layout: mypost
title: 三次样条插值在Python中的实现
categories: [技术,Python,数学建模]
---

## 什么是三次样条插值

> 三次样条插值（Cubic Spline Interpolation）简称Spline插值，是通过一系列形值点的一条光滑曲线，数学上通过求解三弯矩方程组得出曲线函数组的过程。
> 
> 实际计算时还需要引入边界条件才能完成计算。一般的计算方法书上都没有说明非扭结边界的定义，但数值计算软件如Matlab都把非扭结边界条件作为默认的边界条件。
> 
> ——百度百科

这篇博客暂时不进行详解，等待更新

## 在Python中的实现

写的很丑，有很大压缩空间

```python
def cubic(start,end,*args):
    count=4*(len(args)-1)
    mat_ori=np.zeros((count,count))
    mat_ans=np.zeros(count)
    index=0
    for i,j in zip(args[:-2],args[1:-1]):
        mat_ori[4*index,4*index]=i[0]**3
        mat_ori[4*index,4*index+1]=i[0]**2
        mat_ori[4*index,4*index+2]=i[0]
        mat_ori[4*index,4*index+3]=1
        mat_ans[4*index]=i[1]
        mat_ori[4*index+1,4*index]=j[0]**3
        mat_ori[4*index+1,4*index+1]=j[0]**2
        mat_ori[4*index+1,4*index+2]=j[0]
        mat_ori[4*index+1,4*index+3]=1
        mat_ans[4*index+1]=j[1]
        mat_ori[4*index+2,4*index]=3*j[0]**2
        mat_ori[4*index+2,4*index+1]=2*j[0]
        mat_ori[4*index+2,4*index+2]=1
        mat_ori[4*index+2,4*index+4]=-3*j[0]**2
        mat_ori[4*index+2,4*index+5]=-2*j[0]
        mat_ori[4*index+2,4*index+6]=-1
        mat_ans[4*index+2]=0
        mat_ori[4*index+3,4*index]=6*j[0]
        mat_ori[4*index+3,4*index+1]=2
        mat_ori[4*index+3,4*index+4]=-6*j[0]
        mat_ori[4*index+3,4*index+5]=-2
        mat_ans[4*index+3]=0
        index+=1
    mat_ori[4*index,4*index]=args[-2][0]**3
    mat_ori[4*index,4*index+1]=args[-2][0]**2
    mat_ori[4*index,4*index+2]=args[-2][0]
    mat_ori[4*index,4*index+3]=1
    mat_ans[4*index]=args[-2][1]
    mat_ori[4*index+1,4*index]=args[-1][0]**3
    mat_ori[4*index+1,4*index+1]=args[-1][0]**2
    mat_ori[4*index+1,4*index+2]=args[-1][0]
    mat_ori[4*index+1,4*index+3]=1
    mat_ans[4*index+1]=args[-1][1]
    mat_ori[4*index+2,0]=3*args[0][0]**2
    mat_ori[4*index+2,1]=2*args[0][0]
    mat_ori[4*index+2,2]=1
    mat_ans[4*index+2]=start
    mat_ori[4*index+3,4*index]=3*args[-1][0]**2
    mat_ori[4*index+3,4*index+1]=2*args[-1][0]
    mat_ori[4*index+3,4*index+2]=1
    mat_ans[4*index+3]=end
    mat_rg=np.linalg.solve(mat_ori,mat_ans)
    def rtn_func(x):
        def bin_search(left,right):
            if(x<args[left][0]):return left
            if(x>args[right-1][0]):return right-1
            if(right-left<=1):return left
            mid=int((left+right)/2)
            if(x<args[mid][0]):return bin_search(left,mid)
            else:return bin_search(mid,right)
        num=bin_search(0,len(args)-1)
        return mat_rg[4*num]*x**3+mat_rg[4*num+1]*x**2+mat_rg[4*num+2]*x+mat_rg[4*num+3]
    return rtn_func
```

## 边界条件对效果的影响

```python
data=[[0,0],[np.pi/2,1],[np.pi,0],[3*np.pi/2,-1],[2*np.pi,0]]
func1=cubic(-1,-1,*data)
func2=cubic(1,1,*data)
x=np.arange(-1,7,0.1)
y1=[func1(i) for i in x]
y2=[func2(i) for i in x]
plt.plot(x,y1)
plt.plot(x,y2)
plt.plot([i[0] for i in data],[i[1] for i in data],'ob')
plt.plot(x,np.sin(x))
plt.show()
```

![样条曲线](cubic-image.png)

其中蓝线是(-1,-1)的图像，黄线是(1,1)的图像，绿线正弦图像

![残差图](diff-image.png)

残差图中一致

由此可见，边界条件影响颇大

## 对正比例函数的拟合

![正比例](linear-image.png)

完全重合，自然是很好