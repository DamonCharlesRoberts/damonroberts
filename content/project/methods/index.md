---
title: OLS by Hand
summary: Using R
tags:
 - R
 - Statistics
 - Explanations
  
external_link: ""
date: "2020-02-22"


image: 
  caption: ""
  focal_point: ""

links: ""

url_code: ""
url_pdf: ""
url_slides: ""
url_video: ""

# Slides (optional).
#   Associate this project with Markdown slides.
#   Simply enter your slide deck's filename without extension.
#   E.g. `slides = "example-slides"` references `content/slides/example-slides.md`.
#   Otherwise, set `slides = ""`.

slides: ""

---

# Introduction
Here is a break down of finding your OLS equation using matrix algebra. With OLS, you want to find when errors for beta are minimized. First, you need to find your beta for when this is the case. Here is how to calculate OLS by hand.

# Calculating Beta

## Remember that the regression equation is:

$$y = xb + e$$

## Find beta when e is 0:
$$e_0'e_0 = (y-xb_0)'(y-xb_0)$$
This basically is the same as subtracting xb from the regression equation to isolate e.

## Next, use the product rule (like algebra):

$$e_0'e_0 = y'y - y'xb_0 - b_0'xy + x'b_0'xb_0$$

*A.*You can see that you took the $y'$ and multiplied it with $y$. \
*B.*You then took $y'$ and multiplied it with $xb_0$. \
*C.*You then took $b_0'$ and multiplied it by $xy$. \
*D.*You then took $x'$ and $b_0'$ then multiplied it by $xb_0$. \

## Next, you collect terms. 

$$e'_0 e_0 = y'y -2yx'b_0 + x'xb_0'b_0$$

You can think of this as:

$$y^2 - 2yxb_0 + xb_0^2$$

Please not that the matrices for x and by are still transposed. But to remove the scary matrix algebra for more familiar algebra, this is how you can imagine the equation once you've collected the terms.

## Derive Beta

Remember, you want to find the a slope for your regression equation when there is no error. Here, you derive beta when it is equal to zero. While this seems somewhat counter intuitive (wouldn't you derive the error term, then?), finding the beta when the sum of squared errors equals 0 will help when you later calculate your expected values and your residuals (that you need for your OLS equation). 

$$\frac{\partial S(b_0)}{\partial b_0} = -2x'y + 2x'xb_0$$

Okay, here you are using the product rule of partial derivatives. Remember that if you find the partial derivative with respect to y of $x^2y$, your result will be $x^2$. Why? X is not being derived. Leave it alone. You are finding the derivative only for your y term. Using the normal product rule $y$ becomes equivalent to 1 ($1y^{1-1} = 1y^0 = 1(1) = 1$). Since we are taking the derivative with respect to $b_0$, we are leaving the other values alone. As I mentioned above, the last peice of the collected terms equation is $xb_0^2$. This is how you get $2x'x'b_0$. The exponent, 2 drops in front and $b_0^2$ becomes $b_0^{2-1}$ or $b_0^{1}$. Since $x'$ and $x$ were not derived, leave them alone. 

## Add $2x'y$

Take the equation that you got from your partial derivative of beta. Add $2x'y$ to both sides. 
This gives you:

$$2x'y - 2x'y + 2x'xb_0 = 2x'y$$

## Simplify
The $2x'y$ on the left side cancel out. This then gives you:

$$2x'xb_0 = 2x'y$$

## Divide both sides by 2

$$\frac{2x'xb_0}{2} = \frac{2x'y}{2}$$
which yields: $x'xb_0 = x'y$ 

## Then isolate $b_0$

$$\frac{x'xb_0}{x'x} = \frac{x'y}{x'x}$$

which yields:

$$b_0 = \frac{x'y}{x'x} $$

$$\equiv (x'x)^{-1}(x'y)$$

If you think of this as normal algebra where you have $(x*x)^{-1}(x*y)$ or $(x^2)^{-1}xy$, you can do some simple math that gives a familiar result:

$$(x^2)^{-1}(xy)$$

$$\equiv\frac{xy}{x^2}$$

which yields $\frac{y}{x}$. This is the slope of a line. Exactly what you'd expect your beta to be. 
Now that you have found your beta when the sum of your squared errors are equal to 0, it is time to estimate your residuals for points along your x axis that are not included in your dataset. That is, since you do not have a dataset where every imaginable value of x has an observation associated with it in your dataset, you must make an estimation. You want to calculate your residuals first so that you have an idea of how far away these expected values will be away from the beta you just calculated. 

# Create a Residual Maker

## Remember what your error is equal to:

$$ e = y-xb $$

## Plug beta into the equation
Use the beta that you just calculated to help you find your error term. So, plug in the long form of beta in to $e = y-xb$
$$e = y - x((x'x)^{-1}xy)$$

## Simplify the equation from above

See that you have two y's? Remember that an identity matrix (I) is effectively equal to 1. So, pull y out of the beta portion of your equation and you will also need to take y from the first part of the equation. The first y in the equation above will become 1 (equal to your Identity matrix). 

$$e = 1 - (x(x'x)^{-1}x)y$$
$$ \equiv e = I -(x(x'x)^{-1}x)y$$
You have found your residual maker for each of your values. Now, to make this easier on the eyes, you define M as $I-(x(x'x)^{-1}x)$
This then gives you:
$$ e = My$$
You have now found your residual maker. 

# Projection Matrix
As mentioned above, not every imaginable value of x will be included in your dataset. Given that, you have to calculate some expected values given your accurate beta (where the sum of squared errors are minimized) and the size of your residuals, you can now calculate your expected values. The projection matrix gives you these expected values.

## Remember that $\hat{y} = y-e$
This is also part of the reason that you calculate e before you calculate your expected values. 
Given that you already know e, simply plug it into your equation. 
$$\hat{y} = y - My$$

## Pull out common terms to simplify
$$\hat{y} = (1-M)y $$
Recall that 1 is the same as an identity matrix:
$$\hat{y} = (I - M)y$$
Recall that $M = (I -x(x'x)^{-1}x)$.

## Plug M into your equation and solve
$$\hat{y} = (I - (I - x(x'x)^{-1}x'))y $$
The I's cancel out here. Think of them as both equalling 1. So it would be $1 - (1 - ....)$

This leaves you with:

$$\hat{y} = (x(x'x)^{-1}x')y $$
This is your projection matrix. To keep it easy on your eyes, set $P = x(x'x)^{-1}x')$
This leaves you with:
$$\hat{y} = Py$$
\newpage

# Doing all of this Using R (taken from Andy Phillips)

## Finding Beta in R
```{r}
library(foreign)

vote <- read.dta("/Users/damonroberts/Dropbox/Methods/Data II 2020/data/fair.dta") 
head(vote)

# recall our model from last time:
res <- lm(VOTE ~ GROWTH + INFLATION , data = vote)
summary(res)

# To perform OLS by hand, we first create a vector for y:
y <- vote$VOTE
y
# and X, noting that we need an intercept:
X <- cbind(1, vote$GROWTH, vote$INFLATION)
X

# the formula for b then is:
b <- solve(t(X)%*%X)%*%t(X)%*%y
b

# do these match?
res$coefficients
b
```

## Residual Maker and Projection Maker in R
```{r}
library(foreign)
vote <- read.dta("/Users/damonroberts/Dropbox/Methods/Data II 2020/data/fair.dta") 
head(vote)

# recall our model from last time:
res <- lm(VOTE ~ GROWTH + INFLATION , data = vote)
summary(res)

# We could run OLS by hand:
y <- vote$VOTE
X <- cbind(1, vote$GROWTH, vote$INFLATION)
b <- solve(t(X)%*%X)%*%t(X)%*%y
b

# The residual maker: My = (I-X(X'X)^-1X')y
diag(length(y)) # note that we can create an identity matrix like this
    
e <- (diag(length(y)) - X%*%solve(t(X)%*%X)%*%t(X))%*%y    
e
res$residuals  

# do these match up?
cbind(e, res$residuals)

# The projection matrix: Py = X(X'X)^-1X'y
y.hat <- X%*%solve(t(X)%*%X)%*%t(X)%*%y  
y.hat
# does this match up?
cbind(y.hat, res$fitted.values)

# here's a plot of our fitted vs. actual:
plot(y, y.hat)
# and our y vs. residuals
plot(y, e)

# We also know that y = Py + My:
y.test <- X%*%solve(t(X)%*%X)%*%t(X)%*%y + (diag(length(y)) - X%*%solve(t(X)%*%X)%*%t(X))%*%y 
cbind(y,y.test)
```
\newpage
# OLS Assumptions in Matrix Algebra

## A1. The Parameters must be linear. 
OLS is a linear equation. If you feel that the parameters have a better functional form with a non-linear equation, think about using something else like a logit or probit (if probabilistic and logarithmic).

## A2. Full Rank. 
If your matrices are not full rank, your vectors are not conformable. You cannot do any sort of manipulation if they are not conformable. Multicollinearity is an example of breaking this assumption. If your variables are perfectly correlated, all the values in your matrix will be equal to 1 - thus not full rank.

## A3. Exogeneity of the IVs. 
The IV's do not have any predictive information about your error ($\epsilon$).

## A4. Spherical Disturbances.

### Homogeneity:
  
$$E[Var(\epsilon_i | X)] = \sigma^2 \forall i = 1,2,...,n$$
    
This is essentially saying that for every value of X_i, the variance is equal to $\sigma^2$.
    
### No Autocorrelation:
  
$$E[Cov(\epsilon_i, \epsilon_j | X)] = 0 \forall i \neq j$$
    
This is telling you that the errors for different observations (i and j) are not covaring with one another (or think of it as being uncorrelated with each other). If observation i is covarring (correlated) with observation j, then there is a problem. For example, think of a recession. If you had the recession start in 2009 and last through 2010, and do not account for that recession starting in 2009 (by not using a lagged DV), then 2010 will have spill-over effects from 2009 show up in its estimate. 

  In a matrix both of these equations would show an identity matrix where $\sigma^2$ runs along the main diagonal. The equation for this would be $E[\epsilon\epsilon' |X] = \sigma^2I = E[Var(\epsilon)]$
  
## A5. Data generation.
  - You make this assumption about your population.
  - You assume that in your population, the data generating process of $x$ and $\epsilon$ are independent from one another
  
## A6. $\epsilon$ is normally distributed.
  - Given all of your x-values, your error is going to be normally distributed. You do not want to have your error to come from one place. 
  - The equation for this would be $\epsilon|x \sim N(0,\sigma^2I)$. This says that your errors are distributed normally when you have a mean of 0 and a variance of $\sigma^2I$. 
