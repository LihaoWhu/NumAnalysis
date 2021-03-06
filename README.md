## 数值分析/NumAnalysis

我们希望将《计算方法》课程中的理论内容通过Python语言进行实现，并将其运用到实际问题之中。
作为一个对计算机稍微有一定了解的学习者，深知将理论转化为实际往往是存在困难的。通常发展了一门新的理论，并将其转化为技术，这期间需要很长的时间来逐步完善并提高效率。

更重要的是，我们认为这门课程中的一些方法是形象生动而有趣的。可能在更高级的模块中集成了比这些先进得多的算法，但是这并不重要，我们希望用这本书中的方法来解决一些适合于我们的需求的问题。


This repository is used to create a project on lesson "Numerical Analysis" and practically use the method we have learned in problem solving.
As a learner who knows only a little about computer and programming, I know that there must exist difficulties in the transformation from theory to practice. It takes long time when the theory generally improves efficiency as technical tools.

From our perspective, some methods in this lesson are really interesting and fantastic. Maybe some much advanced technique has been used in popular modules, however, it is not important while we hope that the method in our textbook can solve the problem we faced and adapted to our demand.

## 目录/Table of Contents
* [非线性方程数值解/Numerical Solution to Nonlinear Equation](##非线性方程数值解/Numerical-Solution-to-Nonlinear-Equation)
	* [迭代法/Recursive Method](###迭代法/Recursive-Method)
	* [牛顿法/Newton Method](###牛顿法/Newton-Method)
	* [双点快速截弦法/](###双点快速截弦法/)


## 非线性方程数值解/Numerical Solution to Nonlinear Equation
### 迭代法/Recursive Method
基本原理
![](http://latex.codecogs.com/gif.latex?f(x)=0\\Rightarrow~x=g(x)~\\Rightarrow~x_{n+1}=g(x_n))
在这个部分中我们将利用循环完成迭代的操作，需要手动输入迭代公式![](http://latex.codecogs.com/gif.latex?g(x))，具体代码如下：
``` python
import numpy as np
import sympy as sp
g = lambda x: x**2-3*x**(1/2)+4 # Here input your recursive function
x0 = 1 # Your initial point
n = 10 # Your iteration times
residual = 0.001 # Your tolerance 

def Recursive_Method(g,x0,n,residual):
    x = np.zeros(n+1)
    x[0] = x0
    for index in range(1,n+1):
        x[index] = g(x[index-1])
        difference = x[index] - x[index-1]
        if np.abs(x[index]-x[index-1])<residual:
            print("Iteration %r: x=%r, difference=%r <= Residual=%r" %(index,x[index],difference,residual))
            break
        else:
            print("Iteration %r: x=%r, difference=%r" %(index,x[index],difference))
    print("Terminal: The final result is %r" % x[index])
    return x[index]

Recursive_Method(g,x0,n,residual)
```
例如我们要使用![](http://latex.codecogs.com/gif.latex?x=\frac{1}{2}(10-x^3)^{\frac{1}{2}})求解![](http://latex.codecogs.com/gif.latex?x^3+4x^2-10=0)在![](http://latex.codecogs.com/gif.latex?[0,1])上的根，迭代10次或两次误差小于![](http://latex.codecogs.com/gif.latex?10^{-6})，则使用以下的代码
``` python
g = lambda x: 0.5*(10-x**3)**0.5
Recursive_Method(g,1.5,10,10e-6)
```
得到的结果为：
```
Iteration 1: x=1.2869537676233751, difference=-0.2130462323766249
Iteration 2: x=1.4025408035395783, difference=0.11558703591620323
Iteration 3: x=1.3454583740232942, difference=-0.057082429516284172
Iteration 4: x=1.3751702528160383, difference=0.029711878792744173
Iteration 5: x=1.3600941927617329, difference=-0.015076060054305396
Iteration 6: x=1.3678469675921328, difference=0.0077527748303998223
Iteration 7: x=1.3638870038840212, difference=-0.0039599637081115802
Iteration 8: x=1.3659167333900399, difference=0.0020297295060187626
Iteration 9: x=1.3648782171936771, difference=-0.0010385161963628597
Iteration 10: x=1.3654100611699569, difference=0.00053184397627981106
Terminal: The final result is 1.3654100611699569
```

**注意**
1. 本部分尚未加入是否发散的判断，因此要根据迭代结果进行发散的判断
2. 请自行计算迭代式![](http://latex.codecogs.com/gif.latex?x_{n+1}=g(x_n))，在输入过程中注意指数与根号的输入规范

### 牛顿法/Newton Method
具有特殊迭代格式的迭代法，对于![](http://latex.codecogs.com/gif.latex?f(x)=0)的求解，采用格式：

![](http://latex.codecogs.com/gif.latex?g(x)=x-\dfrac{f(x)}{f'(x)})

能保证更快的收敛速度。这里，我们为了得到更为精确的结果，避免数值微分带来的误差和符号计算可能碰到的不确定性，要求手动输入函数![](http://latex.codecogs.com/gif.latex?f(x),f'(x))，并同理使用[迭代法](###迭代法/Recursive-Method)中的方法：
``` python
f = lambda x: x**2+3*x+1 # Your function f(x)
f1 = lambda x: 2*x+3 # First order deviation of f(x)
x0 = 1 # The initial point
n = 100 # Iteration time
residual = 10e-6 # Tolerance

def Newton_Recursive(f,f1,x0,n,residual):
    x = np.zeros(n+1)
    x[0] = x0
    for index in range(1,n+1):
        x[index] = x[index-1]-f(x[index-1])/f1(x[index-1])
        difference = x[index] - x[index-1]
        if np.abs(x[index]-x[index-1])<residual:
            print("Iteration %r: x=%r, difference=%r <= Residual=%r" %(index,x[index],difference,residual))
            break
        else:
            print("Iteration %r: x=%r, difference=%r" %(index,x[index],difference))
    print("Terminal: The final result is %r" % x[index])
    return x[index]

Newton_Recursive(f,f1,x0,n,residual)
```
作为对比，我们依旧解决迭代法中10次尚未达到精度的例子：
>求解![](http://latex.codecogs.com/gif.latex?x^3+4x^2-10=0)在![](http://latex.codecogs.com/gif.latex?[0,1])上的根，迭代10次或两次误差小于![](http://latex.codecogs.com/gif.latex?10^{-6})。
此时，在牛顿法中，![](http://latex.codecogs.com/gif.latex?f(x)=x^3+4x^2-10,f'(x)=3x^2+8x)。对应代码为：
``` python
f = lambda x: x**3+4*x**2-10
f1 = lambda x: 3*x**2+8*x
Newton_Recursive(f,f1,1.5,10,10e-6)
```
仅通过三次迭代就得到收敛结果
```
Iteration 1: x=1.3733333333333333, difference=-0.12666666666666671
Iteration 2: x=1.3652620148746266, difference=-0.0080713184587066777
Iteration 3: x=1.3652300139161466, difference=-3.2000958479994068e-05
Iteration 4: x=1.3652300134140969, difference=-5.0204973511824846e-10 <= Residual=1e-05
Terminal: The final result is 1.3652300134140969
```
### 双点快速截弦法/

<!--
	## 线性方程组LU分解/System of Linear Equation——LU Decomposition
	### 杜利特尔分解/Doolittle Decomposition
	### 形态分析/Property Analysis
	## 线性方程组迭代法/Recursive way to solve the System of Linear Equation
	### Jacobi迭代/Jacobi Recursion
	### Gauss-Seidel迭代/Gauss-Seidel Recursion
	## 插值与拟合/Interpolation and Fitting
	### Lagrange插值/Lagrangian Interpolation
	### Newton插值/Newton Interpolation
	### 最小二乘拟合/Least Square
	## 数值积分/Numerical Integral
	### 插值型数值积分
	### Newton-Cotes公式/Newton-Cotes Method
	#### 梯形公式
	#### Sinpson公式
	### 复化求积公式
	#### 复化梯形公式
	#### 复化Sinpson公式
	### Gauss求积公式
	## 常微分方程数值解/Numerical Solution in Ordinary Differential Equation
	### Euler法/Euler Method
	### 改进的Euler法/Improved Euler Method
-->

