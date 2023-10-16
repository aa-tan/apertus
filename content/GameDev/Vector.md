---
tags:
- math
- physics
title: "Vector"
---
A vector has [[GameDev/Magnitude|magnitude]] and direction. In a 2D plane, this can be represented by a coordinate points. Mathematic notation uses bold or an arrow over the variable to indicate a $\vec{vector}$ and vertical bars to represent $||\vec{magnitude}||$ of a vector.

 Magnitude can be calculated as:
 $$||\vec{z}|| = \sqrt{\vec{x}^2+\vec{y}^2}$$


> [!example]-
> $$
> \begin{aligned}
> ||\vec{z}|| &= \sqrt{4^2+3^2} \\ &= \sqrt{25} \\ &= 5
> \end{aligned}
> $$

### Normalized Vector
A Normalized vector is a vector with the same direction but a magnitude of 1. It is calculated as:
$$Normal(x) = \frac{x}{||x||}$$

# Mathematical Operations
Basic operations can be applied to vectors in order to create new vectors. 
In all examples, assume the following:

$\vec{a} = (2,5)$ and $\vec{b} = (6,7)$

### Addition
Vector addition adds corresponding indices with each other.

$$(x_1,y_1)+(x_2,y_2)=(x_1+x_2,y_1+y_2)$$
>[!example]-
> $$
> \begin{aligned}
> \vec{c} &= \vec{a}+\vec{b}\\
> &= (2,5) + (6,7)\\
> &= (2+6, 5+7)\\
> &= (8,12)\\
> \end{aligned}
> $$

### Subtraction
Vector subtraction involves reversing the second vector and then performing addition.

$$(x_1,y_1)-(x_2,y_2) = (x_1,y_1) + (-x_2,-y_2)$$
>[!example]-
> $$
> \begin{aligned}
> \vec{c} &= (2,5)-(6,7) \\
> &= (2,5)+(-6,-7) \\
> &= (2-6,5-7) \\
> &= (-4,-2) \\
> \end{aligned}
> $$

## Scaling
Scaling a vector can be achieved by multiplying the vector with a [[GameDev/Scalar|Scalar]].

$$(x_1,y_1)*||\vec{z}|| = (x_1*||\vec{z}||, y_1*||\vec{z}||)$$
>[!example]-
> $$
> \begin{aligned}
> ||\vec{z}|| &= 3\\
> \vec{c} &= \vec{a} * ||\vec{z}||\\
> &= (2,5) * 3\\
> &= (2*3, 5*3)\\
> &= (6, 15)\\
> \end{aligned}
> $$
## Dot Product
The Dot Product is a method of multiplying two vectors to return a scalar. It can be used to calculate a [[GameDev/Kinematics|reflection angle]] in [[GameDev/2D Space|2D Space]].

Assume $θ$ as the angle between vectors $\vec{a}$ and $\vec{b}$

$$\vec{a}·\vec{b} = ||\vec{a}|| * ||\vec{b}|| * cos(θ)$$