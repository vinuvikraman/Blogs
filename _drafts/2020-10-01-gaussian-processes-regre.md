---
layout: post
title: "Gaussion Processes Regression. Step by Step" 
date: 2020-10-01 10:33:00
categories: jekyll update
---

I try to explain a step-by-step process of Gaussion process (GP) regression by a one dimentional numerial example. You might know the maths behind the GP but explain a few equations which is needed for the example.

Lets say we have $$n$$ independent features and one dependent variable. Independent features are $$x_{1} = [x_{11}, x_{12}, x_{13}], x_{2} = [x_{21}, x_{22}, x_{23}] $$ and dependent variable is $$y = [y_{1}, y_{2}, y_{3}]$$. This means we have a multivariable Gaussian of three variables. 
GP regression is an 'interpolation' and gives errors based on a multivariate Gaussion distribution of dependent variable. 

We are considering a function $$f(x_{1})$$ with $$x_{1}$$ as the only one independent variable. If we don't have any errors in f(x) then the covariance in the GP can be written as

$$
k(f(x), f(x')) = \sigma_f^2 \exp\left[\frac{-(f(x) - f(x'))^2}{2 l^2}\right]
$$ 

If the $$f(x)$$ and $$f(x')$$ are closer then we have the covariance is $$\sigma^2$$ and if they are furture away the effect of  $$f(x)$$ and $$f(x')$$ will be negligible. This clearly means that the effect of 'interpolation' is larger as the functions are closer and negligible effect when the functions are furthur apart. 

We can explain this with a very simple example. Lets say $$f(x) = x + b$$. Then the above equation tells that 
$$
k(x, x') = \sigma_f^2 \exp\left[\frac{-(x - x')^2}{2 l^2}\right]
$$ 

When $$x$$ and $$x'$$ are closer then the interpolation effect is larger on $$f(x)$$ and if the distance between $$x$$ and $$x'$$ is larger then the effect of interpolation is negligible on $$f(x)$$.   

However, in the real life we will have errors on the underlying function $$f(x)$$. Here we have observed values $$y = f(x) + N(0, \sigma_n^2)$$. $$N(0, \sigma_n^2)$$ is error on the observed values which is considered as normal. For simplicity we assume $$f(x) = x$$ which gives $$y = x + N(0, \sigma_n^2)$$. Based on this assumption we can write the covariance matrix of the oberved signal as 

$$
k(x, x') = \sigma_n^2 \exp\left[\frac{-(x - x')^2}{2 l^2}\right] + \sigma_n^2\delta(x, x')
$$   

Here $$\sigma_n^2 \delta(x, x')$$ comes based on the Gaussian assumption which has the variance $$\sigma_n^2$$ and $$\delta(x, x')$$ is because of the uncorrelated noise of different observations. 

Now we go to the numerical example. Lets say we have ten observations which is shown in the below table. We also assume that the error on all the points is 0.3. However, the error can be different in different observation but I just assume it is a unique error. For this example we assume that $$\sigma_f=1.27$$ and $$l=1$$.

We want to regress these observations to a point $$y^*$$ at $$x^*=0.2$$. If you remember the GP process the mean of $$y^*$$ is 

$$
\bar{y}^* = K_*K^{-1}y
$$

and variance is 

$$
var(y_*) = K_{∗∗} − K_∗K K_∗^T 
$$

Here $$K$$, $$K_*$$ and $$K_{**}$$ are the covariance matrix of observed data, observed to unknown value, covariance of unknown values which is $$y^*$$. We are going to some data which is in the table below. 

<style>
table {
    width:10%;
}
</style>


|x|y
|---|---|
{% for gp in site.data.gp -%}
|{{ gp.x }} | {{ gp.y}} 
{% endfor %}  

We can estimate the first diagonal element of K from the data 


$$

\begin{eqnarray}
K_{x=-1.5, x=-1.5} &=& 1.27^2 \exp\left[\frac{-(-1.5 - -1.5)^2}{2l^2}\right] + 0.3^2  \nonumber \\
&=& 1.7
\end{eqnarray}

$$

The element in (1, 0) is

$$

K_{x=-1.5, x=-1.0} = 1.27^2 \exp\left[\frac{-(-1.5 - -1.0)^2}{2 l^2}\right] = 1.42

$$

$$\delta(x=-1.5, x=-1.0) = 0$$ because the independency of Gaussian noise. In silimar way we can compute all other elements for observed covariance matrix. 

$$

\begin{bmatrix}

1.70 & 1.42 & 1.21 & 0.81 & 0.72 & 0.51\\
1.42 & 1.70 & 1.56 & 1.34 & 1.21 & 0.97 \\
1.21 & 1.56 & 1.70 & 1.51 & 1.42 & 1.21 \\
0.81 & 1.34 & 1.51 & 1.70 & 1.59 & 1.48 \\
0.72 & 1.21 & 1.42 & 1.59 & 1.70 & 1.56 \\
0.51 & 0.97 & 1.21 & 1.48 & 1.56 & 1.70

\end{bmatrix}

$$

We can estimate $$K_*$$ by replacing the $$x^*=0.2$$ in K. This implice that 

$$

K_*(x=x, x=0.2) = [0.38, 0.79, 1.03, 1.35, 1.46, 1.58] 

$$ 

Similarly, $$K_{**} = 1.27^2 + 0.3^2 = 1.70 $$. Here $$ K_{**} $$ is evaluating at $$x^{*}$$ then $$\delta(x^*, x^*) = 1$$ in the noise term. These values can put in to $$\bar(y)^*$$ which can be estimate as 0.8. The variance at $$x^*=0.2$$ is 0.20


**Reference:** This is mostly taken from https://arxiv.org/abs/1505.02965



Also we assume that $$f(x_{1})$$ has a normal errors which in this case $$ e_{x11}, e_{x12}, e_{x13} $$ in $$x_1$$. There is another variable $$x_{2}$$ and $$f(x_{2})$$ has errors which is different from the errors $$f(x_{1})$$ and so on. What it is useful is that we need these errors to understand the multivariate Gaussion. Eg. we have only two variables and $$y(x_{11}, x_{21})$$ is observed in that surface of $$x_1$$ and $$x_{2}$$. If we have only one variable then as you that the normal error is in the 1D. However, for a


