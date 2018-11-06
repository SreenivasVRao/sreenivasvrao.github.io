---
title: "Affine Transforms"
date: 2018-11-01
permalink: /blog/Affine-Transforms/
tags:
  - November
---

This is part of some work we were doing on estimating Camera Rotation parameters last semester. It was a toy problem that was meant to help us get a sense of how one could go about recovering parameters of an unknown set of transformations applied to data.

We got very comfortable with homogeneous co-ordinates, and affine transforms in 2D space when we solved this problem. It was also helpful in acquainting us with gradient descent as a simple optimization algorithm. Plus, the results look rather cool. (Which is what we're always going for!)

<img src="https://i.imgur.com/od93vbb.gif" alt="drawing" width="100"/>

What are affine transforms?
=======

Consider a sheet of paper on a table in front of you. If you move it around on the table, you're _translating_ its position in the the plane. You can also _rotate_ the sheet, without picking it up. These are simple affine transforms. If the paper was magical, or were you a wizard, you could expand its size, i.e., scale it up. That's also an affine transform. Let's look at the visualization earlier on the page again:

|X-axis Translation| Y-axis Translation | Scale X, Y | Rotate along X|
|:-----:|:-----:|:-----:|:-----:|
|<img src="https://i.imgur.com/od93vbb.gif" alt="drawing" width="100"/>| <img src="https://i.imgur.com/Efg2Msu.gif" alt="drawing" width="100"/>|<img src="https://i.imgur.com/TG9DAGh.gif" alt="drawing" width="100"/>| <img src="https://i.imgur.com/S2PjUdi.gif" alt="drawing" width="100"/>|

But images are too complicated to represent in terms of maths equations. So let's stick to points in 2D space. i.e., $(X, Y)$ co-ordinates. Images are essentially sets of points in 3D space, with an $(X, Y, Color)$ location.

Given a single $(X, Y)$ point, we can write an affine transform as:

$$ ax + by + c = \overline{X} $$

$$ dx + ey + f =\overline{Y} $$

By controlling the  parameters $ a, b, c, d, e, f $, we control how the transform shifts $(X, Y)$ to $(\overline{X}, \overline{Y})$.

For example, the rotation component is given by:
$$
\begin{align*}
A=\begin{pmatrix}  
		1 & a_{11} & a_{12} & \cdots & a_{1m}  \\
        1 & a_{21} & a_{22} & \cdots & a_{2m} \\
        \vdots &\vdots & \vdots & \ddots & \vdots \\
        1 & a_{n1} & a_{n2} & \cdots & a_{nm} \\
  \end{pmatrix}
\quad
b=\begin{pmatrix}  
		b_{1}\\
        b_{2}\\
        \vdots \\
        b_{n}\\
  \end{pmatrix}
\end{align*}
$$

$$\begin{matrix}a=cos\theta \& b = sin\theta\\ d = -sin\theta \& e = cos\theta\end{matrix} $$

And the translation component is given by:

$$ \begin{matrix}c = t_x  \& \& f = t_y \end{matrix} $$

So a single rotation + translation is given by:

$$ X cos\theta  + Y sin\theta +  t_x = \overline{X} $$

$$ - X sin\theta + Y cos\theta + t_y =  \overline{Y} $$

We can represent this pair of equations as a matrix multiplication:

$$ \begin{bmatrix}
a \& b \&  c \\
d \&  e \& f \\
\end{bmatrix}\begin{bmatrix}X\\Y\\ 1\\\end{bmatrix} = \begin{bmatrix}\overline{X}\\ \overline{Y}\\ \end{bmatrix}$$

The advantage of this matrix multiplication is that we can extend it to several more points by simple concatenation :)

$$
\begin{bmatrix}
a & b & c \\
d & e & f \\
\end{bmatrix} 
\begin{bmatrix}
X_1 & X_2 & X_3 & \dots & X_K\\
Y_1 & Y_2 & Y_3 & \dots & Y_K\\
1 & 1 & 1 & \dots & 1
\end{bmatrix}=
\begin{bmatrix}
\overline{X}_1 & \overline{X}_2 & \overline{X}_3 & \dots & \overline{X}_K\\
\overline{Y}_1 & \overline{Y}_2 & \overline{Y}_3 & \dots & \overline{Y}_K\\
\end{bmatrix}
$$

We can write this more simply as:

$$
M P = \overline{P}
$$

Now using these equations, and python magic, we can come up with a neat visualization.

|X-axis Translation| Y-axis Translation |
|:-----:|:-----:|
|<img src="https://i.imgur.com/nbo0Juk.gif" alt="drawing"/>| <img src="https://i.imgur.com/ifGTg1w.gif" alt="drawing"/>|

In my next post, we're going to go backwards- from these frames, to the original points! Code to make the visualization seen above is [here.](https://gist.github.com/SreenivasVRao/d3036982d0ea443e09b10f8a867bac20)
