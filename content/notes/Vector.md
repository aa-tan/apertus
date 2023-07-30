---
tags:
- math
- physics
---
# Vector
A vector has [[magnitude]] and direction. In a 2D plane, this can be represented by a coordinate points. Mathematic notation uses bold to indicate a **vector** and vertical bars to represent ||**magnitude**|| of a vector.

#fix The vector (x,y) has a magnitude and direction relative to coordinate point (0,0).
```chart
type: line
labels: [0,4]
series:
  - title: Vector z
    data: [0,2]
tension: 0
width: 58%
labelColors: false
fill: false
beginAtZero: true
```
The vector **z** (4,3) in the above figure has a direction relative to 0,0. With a magnitude defined by the length. Magnitude can be calculated as:
 $$||\vec{z}|| = \sqrt{\vec{x}^2+\vec{y}^2}$$
> [!example]-
 $||\vec{z}|| = \sqrt{4^2+3^2}$
 >
 $||\vec{z}|| = \sqrt{25}$
 >
 $||\vec{z}|| = 5$
 

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
$\vec{c} = \vec{a}+\vec{b}$
$\vec{c} = (2,5) + (6,7)$
$\vec{c} = (2+6, 5+7)$
$\vec{c} = (8,12)$

### Subtraction
Vector subtraction involves reversing the second vector and then performing addition.
$$(x_1,y_1)-(x_2,y_2) = (x_1,y_1) + (-x_2,-y_2)$$
>[!example]-
$\vec{c} = (2,5)-(6,7)$
$\vec{c} = (2,5)+(-6,-7)$
$\vec{c} = (2-6,5-7)$
$\vec{c} = (-4,-2)$

## Scaling
Scaling a vector can be achieved by multiplying the vector with a [[Scalar]].
$$(x_1,y_1)*||\vec{z}|| = (x_1*||\vec{z}||, y_1*||\vec{z}||)$$
>[!example]-
>Assume scalar $||\vec{z}|| = 3$
>$\vec{c} = \vec{a} * ||\vec{z}||$
>$\vec{c} = (2,5) * 3$
>$\vec{c} = (2*3, 5*3)$
>$\vec{c} = (6, 15)$
>$\vec{c}=\vec{a}$

## Dot Product
The Dot Product is a method of multiplying two vectors to return a scalar. It can be used to calculate a [[Kinematics|reflection angle]] in [[2D Space]].

Assume θ as the angle between vectors $\vec{a}$ and $\vec{b}$
$$\vec{a}·\vec{b} = ||\vec{a}|| * ||\vec{b}|| * cos(θ)$$