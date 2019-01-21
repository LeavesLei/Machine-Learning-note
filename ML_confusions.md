### SVM
- - [x] 什么是支持向量

决定超平面的点，叫做支持向量
- - [ ] 凸优化理论中的超平面分离定理(Separating Hyperplane Theorem,SHT)

凸集分离定理，有时间看看凸集相关的书

- - [ ] Explanation about Kernel Function 

### Logistic Regression
- the difference between **logistic regression** and **linear regression**

&emsp;logistic regression is used to solve classification problems, but linear regression solve the regression problem.  
&emsp;linear regression requires that the variables follow a normal distribution and should be continuous, but logistic regression have the typed variabels.  
&emsp;linear regression requires a linear relationship between independent and dependent varialbe, while logistic regression doesn't need this.  
&emsp;(most important)Logistic regression is the relationship between the probability of taking a value of a dependent variable and an independent variable, while linear regression is a direct analysis of the relationship between a dependent variable and an independent variable.

- - [ ] P59，the function： log(p/1-p) = θx？how to get the solution？
- least square method(最小二乘法)

勒让德使用Arithmetic Mean代替标准值

gauss验证了LSM是极大似然估计在y服从正态分布假设下的简化

- - [ ] Gredient Descent

Both logistic regression and linear regression use gredient descent method.

- - [ ]  softmax

- - [ ] case with LR

### Bayes Smoothing
CTR(Click-through Rate,点击率)

r = C(click)/I(impression)

two questions: 

- the rate 0/0 is reasonable?
- a = 5/10, while b = 50/100. a = b is reasonable?

Bayes Smoothing: r = (C+a)/(I+b)  
&emsp;the key to the bayes smooth is how to get the parameters a and b.

