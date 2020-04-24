---
layout: mypost
title: 拉格朗日插值多项式在Python中的实现
categories: [技术,Python,数学建模]
---

## 什么是拉格朗日插值多项式

> 在数值分析中，拉格朗日插值法是以法国十八世纪数学家约瑟夫·拉格朗日命名的一种多项式插值方法。许多实际问题中都用函数来表示某种内在联系或规律，而不少函数都只能通过实验和观测来了解。如对实践中的某个物理量进行观测，在若干个不同的地方得到相应的观测值，拉格朗日插值法可以找到一个多项式，其恰好在各个观测的点取到观测到的值。这样的多项式称为拉格朗日（插值）多项式。数学上来说，拉格朗日插值法可以给出一个恰好穿过二维平面上若干个已知点的多项式函数。拉格朗日插值法最早被英国数学家爱德华·华林于1779年发现，不久后（1783年）由莱昂哈德·欧拉再次发现。1795年，拉格朗日在其著作《师范学校数学基础教程》中发表了这个插值方法，从此他的名字就和这个方法联系在一起。
> 
> ——百度百科

## 数学表示形式

平面上有$(x_1,y_0),(x_2,y_2),\dots,(x_n,y_n)$共$n$个点，构造最高为$n-1$次的多项式函数$L_n(x)$

$$D_n=\{1,2,\dots,n\}，B_k=\{i|i\in D,i\neq k\}$$

$$p_k(x)=\prod_{i\in B_k}\frac{x-x_i}{x_k-x_i}$$

$$L_n(x)=\sum^{n}_{j=1}y_jp_j(x)$$

最终的$L_n(x)$化简后会得出最高不超过$n-1$次的过平面上$n$个点的多项式

## Python中的实现

> 可以使用`Jupyter`方便操作

引用相关科学计算库

```python
import numpy as np
from matplotlib import pyplot as plt
import matplotlib
```
核心代码：实现$p_k(x)$和$L_n(x)$

```python
def p(k,targs):                 # 运用闭包返回p_k(x)
    def rtn_func(x):
        rtn=1
        for i in targs:         # 累乘
            if i==k: continue   # i!=k
            rtn*=x-i[0]
            rtn/=k[0]-i[0]
        rtn*=k[1]
        return rtn
    return rtn_func

def L(*targs):                  # 运用闭包返回L(x)
    funcs=[p(i,targs) for i in targs]   # 获取p_k(x)
    def rtn_func(x):
        rtn=0
        for i in funcs:rtn+=i(x)# 执行累加
        return rtn
    return rtn_func
```

输入待拟合的数据，绘图

```python
data=[[1,1],[2,2],[3,2],[4,6],[5,-1],[0,-1]]
func=L(*data)           # 返回多项式函数
x=np.arange(0,5,0.1)    # 范围[0,5)，间隔0.1
y=[func(i) for i in x]  # 获取值
plt.title("Lagrange Interpolation Polynomial")
plt.plot(x,y)
plt.show()              # 运用matplotlib显示图像
```

结果如图：

![函数图像](plot-image.png)

## Python优化

```python
def p(args,ex):
    def loop(largs):
        if len(largs)==1:return [1,-largs[0]]
        dp=loop(largs[1:])
        return [-largs[0]*i+j for i,j in zip([0]+dp,dp+[0])]
    largs=list(args)
    largs.remove(ex)
    div=1
    for i in largs:
        div*=ex[0]-i[0]
    return [i*ex[1]/div for i in loop(list(zip(*largs))[0])]

def L(*args):
    lists=[p(args,i) for i in args]
    ans=[sum(i) for i in zip(*lists)]
    ans.reverse()
    def rtn_func(x):
        return sum([a*x**i for i,a in enumerate(ans)])
    return rtn_func
```

很短，能用，速度快，直接计算出多项式系数，无需循环

至于快了多少，见测试（测试环境：i5-8650U笔记本，未插电）

```python
print('Time test:')
# data gen
data=[[0,0]]
for i in range(0,63):
    data.append([data[i][0]+np.random.ranf(),np.random.ranf()])
print('#1 func gen')
%time func_new=new_L(*data)
%time func=L(*data)
print('#2 calc')
x=np.arange(0,100,0.01)
%time y1=[func_new(i) for i in x]
%time y2=[func(i) for i in x]
```

输出：

```output
Time test:
#1 func gen
Wall time: 36 ms
Wall time: 0 ns
#2 calc
Wall time: 429 ms
Wall time: 26.7 s
```

虽然函数预处理需要更长的时间，但是运算时快了不止一个量级